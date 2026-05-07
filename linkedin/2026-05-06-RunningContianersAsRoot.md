# Post 06 — Running Containers as Root | LinkedIn

---

I hit four walls trying to fix one volume permissions issue in Kubernetes. Each fix revealed the next problem.

Here is exactly what happened when I migrated my kube-prometheus-stack from SD card to NAS:

**Wall 1: Permission denied.**
Mounted a fresh iSCSI LUN. Prometheus from the Helm chart runs as non-root by default. The volume got formatted as root on first mount. Prometheus could not write to it.

**Wall 2: Security context did not fix it.**
Added fsGroup, runAsUser, runAsGroup. Still permission denied. The LUN was already formatted with root ownership. Those settings do not retroactively change ownership on an already formatted volume.

**Wall 3: Init container clashed with pod security.**
Learned about init containers. They run before everything, do a task, then disappear. Tried to create one that runs as root to chown the volume. It clashed with the pod-level security context which was now enforcing non-root.

**Wall 4: The override.**
Explicitly set runAsNonRoot to false on the init container to override the pod-level security for that one container only. Init container ran as root, chowned the volume to the correct non-root user, then disappeared. Main Prometheus container started normally. Fixed.

Two lessons from this:

✅ The first mount formats the LUN. Whatever security context exists at that moment determines ownership.
✅ Pod-level security and container-level security are separate layers. Understanding when and how to override one from the other is critical.

Full implementation on GitHub: github.com/AhsanRahat12/Homelab

Have you ever had a fix that kept exposing the next wall? What finally solved it? 👇

#Kubernetes #HomeLab #DevOps #CloudNative #Security
