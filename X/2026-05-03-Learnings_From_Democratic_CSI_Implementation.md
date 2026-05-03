# Post 03 — What Deploying democratic-csi Taught Me | X Thread

---

**1/**
Deploying democratic-csi taught me more about Kubernetes storage than any tutorial ever did.

Here is exactly what I had to build 🧵

---

**2/**
Step 1: Create the iSCSI LUN manually on the QNAP NAS. Thick provisioned.

This is the only step democratic-csi does not touch. The actual storage block is yours to create and own.

---

**3/**
Step 2: Deploy democratic-csi via Helm.

Values file contains only what the driver needs to run. No NAS config lives there. That all lives in the PV.

---

**4/**
Step 3: Create a StorageClass pointing to the node-manual driver.

Without it Kubernetes has no idea which driver to hand the volume to.

---

**5/**
Step 4: Create the PV with iSCSI details in volumeAttributes.

Portal, IQN, LUN number. CHAP credentials passed via a SOPS encrypted Kubernetes secret in nodeStageSecretRef.

---

**6/**
Step 5: PVC binds. Done.

iSCSI attach when the pod starts. iSCSI detach when the pod moves. All handled automatically.

The node management you never have to think about is exactly what makes the pod portable.

Full implementation: github.com/AhsanRahat12/Homelab 👇
