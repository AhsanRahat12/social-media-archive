# Post 05 — SOPS + Age Secrets (LinkedIn)

---

Kubernetes secrets are not actually encrypted.

They are base64 encoded. That is just encoding, not encryption. Anyone with cluster access can decode them instantly.

Early in my home lab journey I knew this, so I never committed secrets to Git. Seemed like the safe move. But the problem showed up when I needed to rebuild my cluster. No record of what secrets existed. No record of what values they held. Everything was gone.

That is when I learned about SOPS with age encryption. The idea is simple. You generate an age key pair. The public key lives in your repo and is used to encrypt. The private key lives only inside your cluster and is used to decrypt. Nobody can read your secrets without that private key.

The workflow looks like this:

1. Create your secret manifest with `--dry-run=client -o yaml` so nothing is applied yet.
2. Encrypt only the sensitive fields with SOPS using your public key. The YAML structure stays readable. Only the values are locked.
3. Commit the encrypted file to Git. Safe to push. Safe to share.
4. Flux decrypts automatically at deploy time using the private key stored in the cluster.

The part that really clicked for me was what this means for rebuilding. When your cluster goes down, all you do is give Flux the private key and point it at your repo. It reads the encrypted secrets, decrypts them, and rebuilds everything automatically. No manual recreation. No trying to remember values.

Your entire cluster state, including secrets, recovers itself from Git.

The private key never touches Git. Ever.

How do you manage secrets in your setup? 👇

Follow me, I am documenting everything I build and learn in my home lab.

`#Kubernetes` `#DevOps` `#Security` `#GitOps` `#CloudNative`
