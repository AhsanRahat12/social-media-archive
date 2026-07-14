# X Thread

**1/**
I commit secrets to GitHub. Cloudflare tokens, NAS passwords, DB credentials. Anyone who clones the repo can see the files.

That's more secure than hiding them. Here's why.

**2/**
Kubernetes Secrets are base64 encoded, not encrypted. Anyone with cluster access decodes them in seconds. Not real security.

**3/**
Fix: SOPS + age encryption. Public key encrypts (safe to publish). Private key decrypts (lives only in the cluster). Flux uses it to decrypt on deploy automatically.

**4/**
I got it working first try, except I forgot to create the sops-age secret inside the cluster. Flux threw "secret not found" for 20 minutes straight until I caught it.

**5/**
The actual lesson: security isn't about hiding data. It's about encrypting it and controlling who holds the key. The encrypted file sitting in GitHub is just noise without it.

**6/**
Full writeup with the exact commands: https://medium.com/@s.rahatahsan/i-store-secrets-in-github-heres-why-that-s-actually-secure-72fdbc3841d3?sharedUserId=s.rahatahsan

