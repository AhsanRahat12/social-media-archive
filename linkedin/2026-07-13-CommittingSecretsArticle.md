# LinkedIn Post

I commit secrets to GitHub. Cloudflare tunnel tokens, NAS passwords, database credentials, all visible to anyone who clones the repo.

Sounds reckless. It's actually the opposite.

Kubernetes Secrets are base64 encoded by default, not encrypted. Anyone with cluster access can decode them in seconds. So I switched to SOPS with age encryption instead.

The setup is basically a mailbox. The public key encrypts, anyone can use it safely. The private key decrypts, and it only lives inside my cluster. Flux uses it to decrypt secrets automatically when it deploys.

I got the whole flow working on the first pass, except I forgot one step: creating the sops-age secret inside the cluster itself. Flux kept throwing "secret not found" errors. Took me 20 minutes to realize the private key just wasn't there yet.

Once I created it, everything decrypted and deployed instantly.

The lesson: encryption isn't hiding. It's locking something and controlling who holds the key. GitHub can store the encrypted file in plain sight because without that key, it's useless.

If you're running GitOps, how are you handling secrets right now? SOPS, sealed-secrets, external vault, something else?

(Full breakdown in the comments 👇)

#DevOps #Kubernetes #GitOps #HomeLab #Security

---

# First Comment (post immediately after, carries the link)

Full writeup with the exact commands and the config that broke: https://medium.com/@s.rahatahsan/i-store-secrets-in-github-heres-why-that-s-actually-secure-72fdbc3841d3?sharedUserId=s.rahatahsan


