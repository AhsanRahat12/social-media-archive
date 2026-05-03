# Post 03 — What Deploying democratic-csi Taught Me | LinkedIn

---

Deploying democratic-csi taught me more about how Kubernetes handles storage than any tutorial ever did.

Here is exactly what I had to build and why each piece exists:

1. Create the iSCSI LUN manually on the QNAP NAS. Thick provisioned. The only step democratic-csi does not touch.
2. Deploy democratic-csi via Helm. Values file contains only what the driver needs to run. No NAS config lives there.
3. Create a StorageClass pointing to the node-manual driver. Without it Kubernetes has no idea which driver to hand the volume to.
4. Create the PV with all iSCSI details in volumeAttributes. Portal, IQN, LUN number. CHAP credentials passed via a SOPS encrypted Kubernetes secret in nodeStageSecretRef.
5. PVC binds. Everything else is automatic.

The only manual steps are creating the LUN on the NAS and writing the PV manifest. iSCSI attach when the pod starts and detach when the pod moves to another node, all handled automatically by democratic-csi.

That last part is the whole point. The node management you never have to think about is exactly what makes the pod portable.

Full implementation on GitHub: github.com/AhsanRahat12/Homelab

Have you worked with CSI drivers in your cluster? What was the piece that finally made storage click for you? 👇

#Kubernetes #HomeLab #DevOps #CloudNative #Linux
