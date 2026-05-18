# X Thread — Kubernetes Won't Rebind Your Storage

---

## Tweet 1 (Hook)

I deleted a PVC. Created a new one with the exact same spec.

Kubernetes refused to bind it.

Here's why and how to fix it 🧵

---

## Tweet 2

During a migration, I needed to replace manually created PVCs with Operator managed ones.

Deleted the old PVCs. The PersistentVolumes entered "Released" state. New PVCs stayed Pending forever.

---

## Tweet 3

The reason: PVs with a Retain reclaim policy are designed to protect your data.

When a PVC is deleted, Kubernetes holds the PV hostage on purpose. It keeps a reference to the deleted PVC and refuses to bind anyone new.

---

## Tweet 4

"Just scale the deployment to zero first."

Nope. Doesn't help.

Scaling down removes the pod, which releases the volume mount. But the PVC still exists as a Kubernetes object bound to the PV. Those are two separate things.

The Retain policy triggers when the PVC is deleted, not when the pod stops using it.

---

## Tweet 5

The fix is manual. Patch the PV to clear the old reference:

kubectl patch pv <pv-name> -p '{"spec":{"claimRef":null}}'

That clears the stale reference. PV goes from Released to Available. New PVCs can bind.

---

## Tweet 6 (CTA)

Quick summary:

1. Retain policy protects data after PVC deletion.
2. PV enters Released, not Available.
3. Scaling to zero doesn't prevent it.
4. You must manually clear the claimRef to rebind.

What Kubernetes behavior surprised you most when you first encountered it? 👇

#Kubernetes #DevOps #CloudNative #Storage
