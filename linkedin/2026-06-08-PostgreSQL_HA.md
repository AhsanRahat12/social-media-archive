# Post 2 — LinkedIn

**Topic:** HA PostgreSQL on Two Raspberry Pis
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

Everyone told me to use AWS RDS for PostgreSQL.

I built a 2-instance HA cluster on two Raspberry Pis instead. Here's every decision I had to make.

Before writing a single line of YAML, I had to understand the full stack:

1. CloudNative PG handles PostgreSQL HA. Primary and replica managed by an operator. Automatic failover, streaming replication, health checks built in.

2. iSCSI over the network for persistent storage. Block devices served from a QNAP NAS to both nodes. One LUN per workload, exclusive access per pod.

3. Barman for backups. WAL archiving to a Minio instance on the NAS. Point-in-time recovery without touching AWS.

4. Flux for GitOps. Every resource lives in Git. The cluster is fully reproducible from a clean state.

The decisions that actually mattered:

Backups live outside the cluster. If Kubernetes breaks entirely, the data survives on the NAS. This was non-negotiable from day one.

Retain reclaim policy on every PV. Delete the cluster, the data stays. Recreate from Git, rebind with claimRef. No coin flips.

topologySpreadConstraints force the primary and replica onto different nodes. HA means nothing if both pods land on the same Pi.

prefer-standby backup target. Barman pulls from the replica so the primary never takes the backup I/O hit.

The honest part: this cluster is one week old. I am not going to tell you it is battle-tested.

What I can tell you is that every decision was made deliberately, every failure mode was considered before it happened, and the architecture will hold when things break.

That is a different kind of confidence than uptime. And it is the one worth building.

What was the hardest architectural decision you made on your last infrastructure project?

Follow for the full series on building production-grade infrastructure on commodity hardware.

#Kubernetes #PostgreSQL #CloudNativePG #DevOps #Infrastructure

---

## Metadata

- **Source:** CloudNative PG Deployment notes
- **Pillar:** Production-grade on a budget
- **Tier:** Elite (personal brand post)
- **Target audience:** DevOps Engineers, Platform Engineers, Career Switchers
