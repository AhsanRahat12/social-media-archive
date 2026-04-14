# Post 05 — SOPS + Age Secrets (X / Twitter)

---

**Tweet 1**
Kubernetes secrets are not actually encrypted.

They are base64 encoded. Anyone with cluster access can decode them instantly.

Here is how I actually secure secrets in my cluster 🧵

---

**Tweet 2**
Early in my home lab journey I knew this, so I never committed secrets to Git.

Seemed safe. Until I needed to rebuild my cluster.

No record of what secrets existed. No record of what values they held. Everything was gone.

---

**Tweet 3**
That is when I learned about SOPS with age encryption.

You generate an age key pair. Public key encrypts. Private key decrypts. Nobody can read your secrets without that private key.

The private key never touches Git. Ever.

---

**Tweet 4**
The workflow:

1. Create secret manifest with `--dry-run=client -o yaml`
2. Encrypt only the sensitive fields with SOPS
3. Commit the encrypted file to Git safely
4. Flux decrypts automatically at deploy time

---

**Tweet 5**
Here is the part that really clicked for me.

When your cluster goes down, give Flux the private key and point it at your repo. It reads the encrypted secrets, decrypts them, and rebuilds everything automatically.

Your entire cluster state recovers itself from Git.

---

**Tweet 6**
Secrets that are version controlled, auditable, and genuinely encrypted.

All it takes to recover is one private key.

How do you manage secrets in your setup?
