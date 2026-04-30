# Post — Project Reveal (LinkedIn)

---

For the past few weeks every post I have shared has been building toward this.

Kubernetes distros. GitOps. SOPS secrets. Cloudflare Tunnel. Traefik. Prometheus. Grafana. Kustomize overlays. Renovate. Storage architecture.

I was not just learning these things. I was applying them.

Today I want to show you what I actually built.

A production grade self hosted infrastructure running on two Raspberry Pi 5s on my desk.

Two nodes. Two apps. One QNAP NAS for storage. Everything managed through GitOps. Nothing deployed manually.

The two apps running in production:

Linkding, my self hosted bookmark manager, accessible at links.rahatahsan.com

Audiobookshelf, my self hosted audiobook and podcast server, accessible at audiobooks.rahatahsan.com

Both exposed publicly through Cloudflare Tunnel with zero open ports. Both running on NAS backed storage via democratic-csi so if a node fails the pod reschedules and storage follows automatically. Both fully encrypted secrets committed safely to Git with SOPS. Both monitored with Prometheus and Grafana. Both receiving automated image update PRs from Renovate.

I tested failover on both. Cordoned a node, deleted the pod, watched it reschedule to the other node with all data intact. It worked.

Each project also has its own roadmap for what comes next. PostgreSQL to replace SQLite for Linkding. cert-manager for automated TLS. Readiness and liveness probes. Secrets provider upgrades from SOPS to AWS KMS or Vault. The work of making something truly production grade never really stops.

This is not a tutorial cluster. This is a real system solving real problems with real architectural decisions behind every choice.

The full repo with docs for every decision made is here:
github.com/AhsanRahat12/Homelab

This is just the beginning. There is a lot more coming.

What would you like me to go deeper on? 👇

#Kubernetes #DevOps #GitOps #CloudNative #Homelab
