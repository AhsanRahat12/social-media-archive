# Post 9 — X (Twitter) Thread

**Topic:** PV Stuck in "Released" — Pre-bind It With claimRef
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 8**

🧵 I deleted my PostgreSQL cluster to recreate it cleanly.

The PersistentVolume went into Released state and refused to bind to the new PVC.

I spent an hour thinking something was broken.

Nothing was broken.

---

**Tweet 2 / 8**

When a PVC is deleted, the PV moves to Released.

It holds the reference to the old claim and will not automatically bind to a new one.

Kubernetes will not hand a volume with existing data to a new workload without explicit instruction.

---

**Tweet 3 / 8**

At first that frustrated me.

Then I understood why it exists.

If Kubernetes rebound Released volumes automatically, a new PVC of the same size could silently get access to someone else's data.

The behaviour that frustrated me is protecting me from something worse.

---

**Tweet 4 / 8**

The fix is claimRef.

Pre-bind the PV to a specific PVC by name in the spec:

spec:
  claimRef:
    name: cnpg-data-pvc
    namespace: cnpg

Now Kubernetes knows exactly which claim this volume belongs to before that claim even exists.

---

**Tweet 5 / 8**

Pair this with a Retain reclaim policy and you have a pattern that survives cluster deletions cleanly:

1. Delete the cluster. PV moves to Released, data intact.
2. Recreate from Git. New PVC is created.
3. Kubernetes binds via claimRef. Data available immediately.

---

**Tweet 6 / 8**

No coin flips. No manual patching. Fully reproducible from a clean state.

---

**Tweet 7 / 8**

The pattern works because you designed for it upfront.

claimRef in the PV spec. Retain as the reclaim policy.

Two deliberate choices that make cluster recreation a non-event instead of a recovery operation.

---

**Tweet 8 / 8**

Have you ever been frustrated by a Kubernetes behaviour that turned out to be protecting you from something worse?

Follow for the series on stateful Kubernetes patterns that make sense once you understand the intention behind them.

---

## Metadata

- **Source:** CloudNative PG Deployment notes
- **Pillar:** GitOps for stateful infrastructure
- **Tier:** Solid
- **Target audience:** DevOps Engineers, Platform Engineers, anyone running stateful workloads
