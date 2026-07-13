# I Store Secrets in GitHub. Here's Why That's Actually Secure.

It sounds insane.

Storing production secrets in a public GitHub repository.

But that's exactly what I do. Cloudflare Tunnel credentials, QNAP NAS passwords, database admin tokens — all committed to Git, visible to anyone who clones the repo.

And it's more secure than keeping them hidden.

---

## The Problem With Kubernetes Secrets

When I started building my home lab, I had nowhere to store secrets.

Kubernetes has a built-in Secret resource, but it's only base64-encoded. That's encoding, not encryption. It's like writing something in a simple code that anyone can decode in seconds. Anyone with access to the cluster can read it.

So I had two bad choices:

**Option 1:** Store secrets outside Git, manage them manually. Create passwords by hand, hope I remember them, rebuild the cluster and realize I have no idea what secrets should exist.

**Option 2:** Store secrets in Git as plain text. Safe from being deleted, but anyone reading the repo sees everything.

I needed Option 3: secrets in Git that are actually encrypted with real security.

---

## Enter SOPS and Age Encryption

SOPS is a tool that encrypts just the sensitive parts of your YAML files. Age is the encryption method it uses.

Think of it like a mailbox with a public key and a private key:

**The public key** is like the mailbox slot. Anyone can put something in (encrypt a secret). It's safe to publish.

**The private key** is like the mailbox key. Only you can open it and read what's inside (decrypt the secret). You keep this locked away.

Here's how it works for your secrets:

The public key lives in your Git repo (next to your code). Flux uses it to encrypt secrets.

The private key lives only in your cluster. Flux uses it to decrypt secrets when deploying.

GitHub can store the encrypted secrets. Nobody can read them without the private key. Nobody needs to because only your cluster has the key.

> **[INSERT DIAGRAM: Public key encrypts, private key decrypts - mailbox visual]**

---

## The Setup

Here's how to actually do this. Three steps.

Step 1: Generate the encryption keys.

Step 2: Store the private key in your cluster.

Step 3: Tell Flux how to use the key.

After that, every time you create a new secret, you encrypt it with step 4 before committing.

> **[INSERT DIAGRAM: Setup flow - key generation, private key to cluster, Flux config]**

```bash
age-keygen -o age.agekey
```

This creates a file called age.agekey with two keys: public and private.

You'll need the public key for encrypting. It's in the comments at the top of the file, looks something like `age1abc...xyz`.

Save it as an environment variable so you can use it in the next step:

```bash
export AGE_PUBLIC=age1abc...xyz
```

(Replace `age1abc...xyz` with the actual key from your file.)

**Step 2: Store the private key in the cluster**

The private key (the whole age.agekey file) needs to live somewhere in your cluster so Flux can use it to decrypt secrets.

```bash
cat age.agekey | \
  kubectl create secret generic sops-age \
    --namespace=flux-system \
    --from-file=age.agekey=/dev/stdin
```

This command creates a secret in your cluster called `sops-age` that contains your private key. Think of it like locking the key in a safe inside your cluster. Flux knows where the safe is and can open it when needed.

**Step 3: Tell Flux how to decrypt**

In your Flux configuration, you need to tell Flux: "when you see encrypted files, use this secret to decrypt them."

Here's what that looks like:

```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: apps
  namespace: flux-system
spec:
  sourceRef:
    kind: GitRepository
    name: homelab
  path: ./apps/production
  prune: true
  wait: true
  decryption:
    provider: sops
    secretRef:
      name: sops-age
```

The `decryption` block at the bottom is what matters. It says: "this Kustomization uses SOPS for decryption, and the key is stored in the `sops-age` secret."

That's it. Flux now knows how to decrypt your secrets.

---

## Encrypting Secrets (Step 4)

Now that the setup is done, here's how you create and encrypt a new secret when you need one.

> **[INSERT DIAGRAM: Secret encryption workflow - create → encrypt → commit → Flux deploys]**

**Create the secret manifest**

Write your secret normally:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: cloudflare-tunnel-token
  namespace: networking
type: Opaque
data:
  token: base64-encoded-token-here
```

Don't commit it yet.

**Encrypt it**

```bash
sops --age=$AGE_PUBLIC \
  --encrypt \
  --encrypted-regex '^(data|stringData)$' \
  --in-place secret.yaml
```

This command encrypts the sensitive parts of your secret file using the public key.

The `--encrypted-regex` flag is important. It says: "only encrypt the values in the `data` fields, not the whole file." This keeps the metadata (name, namespace) readable, so you can still see what secret this is in GitHub.

Now the secret is encrypted. It's safe to commit.

The encrypted file looks like this:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: cloudflare-tunnel-token
  namespace: networking
type: Opaque
data:
  token: ENC[AES256_GCM,data:j7+P...=]
sops:
  kms: []
  gcp_kms: []
  azure_kv: []
  hc_vault: []
  age:
    - recipient: age1abc...xyz
      enc: |
        -----BEGIN AGE ENCRYPTED FILE-----
        ...encrypted...
        -----END AGE ENCRYPTED FILE-----
  version: 3.7.3
```

The important parts: the `token` value is now encrypted (that `ENC[...]` part). The metadata (name, namespace) is still readable. The `sops` section at the bottom tells whoever reads this file: "this was encrypted with the age1abc...xyz key, and you'll need that key to decrypt it."

Anyone can see the file. Nobody can read the actual token value without the private key.

**Commit to Git**

```bash
git add secret.yaml
git commit -m "add cloudflare tunnel secret"
git push
```

The encrypted secret is now safe in the repo.

---

## Flux Decrypts Automatically

When you push the encrypted secret to GitHub, Flux notices the change automatically.

Flux reads the encrypted secret.yaml file from GitHub.

Flux looks at the metadata and sees "this file needs to be decrypted."

Flux uses the private key stored in the cluster to decrypt the token value.

Flux applies the decrypted secret to the cluster.

Done. Your secret is deployed. You didn't have to do anything special. Flux handled the decryption behind the scenes.

---

## The Problem I Hit

I got almost everything right on my first try. But I missed one critical step.

I encrypted the secrets and committed them to Git. I set up the Kustomization with the decryption block. Everything was in place.

But I forgot to create the `sops-age` secret in the cluster. Flux tried to decrypt and couldn't find the key.

Flux kept failing with "secret not found" errors. It took me twenty minutes of debugging to realize the private key was never in the cluster.

Once I ran that kubectl create secret command, Flux immediately decrypted and applied everything.

The lesson: the private key must exist in the cluster before Flux tries to reconcile encrypted secrets. If you rebuild your cluster, recreate the secret immediately.

---

## Why This Actually Works

The security is simple and solid.

GitHub stores the encrypted files. Anyone can read them. But without the private key, it's just gibberish.

The private key lives only in your cluster. Only Flux has access to it. Flux uses it to decrypt secrets at deploy time.

Here's what this means in practice:

**Your team can review pull requests with secret changes.** They see the encrypted values, can't read them, approve anyway.

**Rebuilding your cluster is reproducible.** Push the same code to Flux, it decrypts with the same private key, everything comes back identically.

**Secrets are auditable.** GitHub shows who changed what, when, and why.

Compare this to the old way (secrets stored somewhere separate):
- No idea what secrets should exist after rebuilding
- Secrets passed via Slack or email (terrible)
- One person knows the password, they leave, chaos
- No history of who changed what

With SOPS, your secrets are safe, auditable, and part of your version control system.

---

## What This Enables

Once SOPS was working, my entire approach changed.

I could now commit the complete cluster definition to GitHub. Every resource, every secret. Everything needed to rebuild from scratch.

Pull requests with secret changes? No problem. The team reviews the encrypted values (they can't read them), approves, and merges.

Need to rotate a secret? Update the YAML file and push. Flux automatically applies the new value.

Cluster disaster? Point Flux at the same repo, it decrypts with the existing private key, and everything rebuilds identically.

The cluster became reproducible. Secrets stopped being special or scary.

---

## The Lesson That Stuck

Security isn't about hiding things. It's about locking them properly and controlling who has the key.

SOPS + Age doesn't make secrets disappear from GitHub. It encrypts them. Encryption is different from hiding. Encrypted data is useless without the key.

The public key is worthless. The private key is everything. And only your cluster has it.

That's how real infrastructure teams do it.

---

*This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to DevOps engineer.*

💼 LinkedIn: [Rahat Ahsan](https://linkedin.com/in/rahatahsan)
🐦 X: [@RahatAhsan20](https://x.com/RahatAhsan20)
🐙 GitHub: [AhsanRahat12](https://github.com/AhsanRahat12)
