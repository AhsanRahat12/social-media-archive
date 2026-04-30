# Post — Project Reveal (X / Twitter)

---

**Tweet 1**
For the past few weeks every post I shared has been building toward this.

Today I want to show you what I actually built 🧵

---

**Tweet 2**
A production grade self hosted infrastructure running on two Raspberry Pi 5s on my desk.

Two nodes. One QNAP NAS. Everything managed through GitOps. Nothing deployed manually.

Nodes are named Zoro and Luffy. Yes, One Piece fans will understand.

---

**Tweet 3**
Two apps running in production:

Linkding, my self hosted bookmark manager.
links.rahatahsan.com

Audiobookshelf, my self hosted audiobook and podcast server.
audiobooks.rahatahsan.com

Both publicly accessible. Zero open ports on my router.

---

**Tweet 4**
The full stack under the hood:

K3S + Flux GitOps
SOPS + Age encrypted secrets
Cloudflare Tunnel for external access
Traefik for internal routing
democratic-csi for NAS backed storage
Prometheus + Grafana for observability
Renovate for automated image updates

---

**Tweet 5**
The storage architecture alone went through three stages to get right.

SD card → static iSCSI pinned to one node → democratic-csi with full attach/detach lifecycle management.

Node failure is now survivable. Pod reschedules. Storage follows. No manual intervention.

I tested it. It works.

---

**Tweet 6**
Audiobookshelf runs four volumes with a deliberate split.

Config and metadata on iSCSI LUNs. Block storage, exclusive access, right tool for a live database.

Audiobooks and podcasts on NFS. ReadWriteMany, mounts on any node, no detach needed during failover.

Every decision has a reason.

---

**Tweet 7**
Each project has its own roadmap for what comes next.

PostgreSQL to replace SQLite. cert-manager for automated TLS. Readiness and liveness probes. Secrets provider upgrades from SOPS to AWS KMS or Vault.

The work of making something production grade never really stops.

---

**Tweet 8**
This is not a tutorial cluster.

Every architectural choice is documented. Every problem solved is written up. Every stage of the storage evolution is explained.

Full repo here:
github.com/AhsanRahat12/Homelab

This is just the beginning. A lot more is coming.

What would you like me to go deeper on?
