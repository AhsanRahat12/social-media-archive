# LinkedIn Post 7 — Kubernetes Won't Rebind Your Storage

---

I deleted a PVC. Created a new one with the exact same spec. Kubernetes refused to bind it.

During the migration, I needed to replace manually created PVCs with Operator managed ones. Deleted the old PVCs. The PersistentVolumes entered "Released" state. New PVCs stayed Pending forever.

The reason: PVs with a Retain reclaim policy are designed to protect your data. When the PVC is deleted, Kubernetes holds the PV hostage on purpose. It keeps a reference to the deleted PVC and refuses to bind anyone new.

And no, scaling the deployment to zero first doesn't help. Scaling down removes the pod, which releases the volume mount. But the PVC still exists as a Kubernetes object bound to the PV. Those are two separate things. The Retain policy triggers when the PVC is deleted, not when the pod stops using it.

The fix is manual. You patch the PV to clear the old reference:

kubectl patch pv <pv-name> -p '{"spec":{"claimRef":null}}'

Then it becomes Available again and new PVCs can bind.

1. Retain policy protects data after PVC deletion.
2. PV enters Released state, not Available.
3. Scaling to zero doesn't prevent it. Pod mount and PVC binding are separate.
4. You must manually clear the old claim reference to rebind.

What Kubernetes behavior surprised you most when you first encountered it? 👇

#Kubernetes #DevOps #CloudNative #Storage #SRE
