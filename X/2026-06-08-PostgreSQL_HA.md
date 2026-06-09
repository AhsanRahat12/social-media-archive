# Post 2 — X (Twitter) Thread

**Topic:** HA PostgreSQL on Two Raspberry Pis
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 10**

🧵 Everyone told me to use AWS RDS for PostgreSQL.

I built a 2-instance HA cluster on two Raspberry Pis instead.

Here's every decision I had to make:

---

**Tweet 2 / 10**

The full stack I had to understand before writing a single line of YAML:

CloudNative PG for HA operator management.
iSCSI block storage from a QNAP NAS.
Barman for WAL archiving and backups.
Flux for GitOps.

None of this comes pre-assembled.

---

**Tweet 3 / 10**

Decision 1: Backups live outside the cluster.

If Kubernetes breaks entirely, the data survives on the NAS.

Barman archives WAL files to a Minio instance on the QNAP. S3-compatible. No AWS. Fully recoverable.

---

**Tweet 4 / 10**

Decision 2: Retain reclaim policy on every PV.

Delete the cluster, the data stays.
Recreate from Git, rebind with claimRef.

No coin flips on whether your data survives a cluster reset.

---

**Tweet 5 / 10**

Decision 3: topologySpreadConstraints.

Forces the primary and replica onto different nodes.

HA means nothing if both pods land on the same Raspberry Pi.

---

**Tweet 6 / 10**

Decision 4: prefer-standby backup target.

Barman pulls from the replica so the primary never takes the backup I/O hit.

Small detail. Matters at scale.

---

**Tweet 7 / 10**

Decision 5: one-time bootstrap Job for volume ownership.

mkfs.ext4 creates root-owned filesystems. Postgres runs as UID 26.

A Kubernetes Job running as root chowns the PVC before the cluster starts. Without this, initdb fails.

(Full breakdown in my last post)

---

**Tweet 8 / 10**

The honest part: this cluster is one week old.

I am not going to tell you it is battle-tested.

---

**Tweet 9 / 10**

What I can tell you is that every decision was made deliberately.

Every failure mode was considered before it happened.

The architecture will hold when things break. That is a different kind of confidence than uptime.

---

**Tweet 10 / 10**

What was the hardest architectural decision you made on your last infrastructure project?

Building the full series on production-grade infrastructure on commodity hardware.

Follow if this resonates.

---

## Metadata

- **Source:** CloudNative PG Deployment notes
- **Pillar:** Production-grade on a budget
- **Tier:** Elite (personal brand post)
- **Target audience:** DevOps Engineers, Platform Engineers, Career Switchers
