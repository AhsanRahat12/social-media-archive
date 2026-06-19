# Post 10 — X (Twitter) Thread

**Topic:** Backups Must Live Outside the Cluster
**Format:** Numbered Steps
**Length:** Short and punchy per tweet
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 7**

🧵 If your Kubernetes cluster dies, your backups need to survive it.

Mine live on my NAS. Here's how I set it up:

---

**Tweet 2 / 7**

1. Minio runs on the NAS as an S3-compatible object store.

No AWS. No monthly bill. Just a bucket on hardware I already owned.

---

**Tweet 3 / 7**

2. Barman handles PostgreSQL WAL archiving.

Every transaction gets shipped to Minio continuously. Not just daily snapshots — continuous archiving.

---

**Tweet 4 / 7**

3. Backups run from the replica, not the primary.

The primary never takes the backup I/O hit. Small detail, matters under load.

---

**Tweet 5 / 7**

4. 30 day retention. Point-in-time recovery to any moment in that window.

Managed by the CNPG operator. One config block in the cluster spec.

---

**Tweet 6 / 7**

The key decision: backups live completely outside Kubernetes.

If the cluster is gone, the data is still there.

That was non-negotiable from day one.

---

**Tweet 7 / 7**

What does your PostgreSQL backup setup look like?

Follow for more on building production-grade infrastructure on a budget.

---

## Metadata

- **Source:** CloudNative PG Deployment notes / iSCSI Fix and Barman Backups
- **Format:** Numbered Steps
- **Pillar:** Production-grade on a budget
- **Target audience:** DevOps Engineers, Platform Engineers, anyone running self-hosted PostgreSQL
