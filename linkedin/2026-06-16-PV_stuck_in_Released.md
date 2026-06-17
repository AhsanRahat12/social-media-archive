# Post 9 — LinkedIn

**Topic:** PV Stuck in "Released" — Pre-bind It With claimRef
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

I deleted my PostgreSQL cluster to recreate it cleanly.

The PersistentVolume went into Released state and refused to bind to the new PVC.

I spent an hour thinking something was broken. Nothing was broken.

When a PVC is deleted, the PV it was bound to moves to Released. It holds onto the reference of the old claim and will not automatically bind to a new one. Kubernetes will not hand a volume with existing data to a new workload without explicit instruction.

At first that frustrated me. Then I understood why it exists.

If Kubernetes rebound Released volumes automatically, deleting a PVC and creating a new one of the same size could silently give your new workload access to someone else's data. In a multi-tenant cluster that is a serious security and data integrity problem. The behaviour that frustrated me is protecting me from something worse.

The fix is claimRef. You pre-bind the PV to a specific PVC by name directly in the spec:

```yaml
spec:
  claimRef:
    name: cnpg-data-pvc
    namespace: cnpg
```

Now when the new PVC is created with a matching name in the matching namespace, Kubernetes binds them immediately. No ambiguity. No manual intervention. The PV knows exactly which claim it belongs to before that claim even exists.

Pair this with a Retain reclaim policy and you have a stateful storage pattern that survives cluster deletions cleanly:

1. Delete the cluster. PV moves to Released, data intact.
2. Recreate from Git. New PVC is created.
3. Kubernetes binds PV to PVC via claimRef. Data available immediately.

No coin flips. No manual patching. Fully reproducible from a clean state.

The pattern works because you designed for it upfront. claimRef in the PV spec, Retain as the reclaim policy. Two deliberate choices that make cluster recreation a non-event instead of a recovery operation.

Have you ever been frustrated by a Kubernetes behaviour that turned out to be protecting you from something worse?

Follow for the series on stateful Kubernetes patterns that make sense once you understand the intention behind them.

#Kubernetes #PostgreSQL #DevOps #Infrastructure #CloudNativePG

---

## Metadata

- **Source:** CloudNative PG Deployment notes
- **Pillar:** GitOps for stateful infrastructure
- **Tier:** Solid
- **Target audience:** DevOps Engineers, Platform Engineers, anyone running stateful workloads on Kubernetes
