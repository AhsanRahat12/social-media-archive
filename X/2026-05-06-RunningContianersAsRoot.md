# Post 06 — Running Containers as Root | X Thread

---

**1/**
I hit four walls trying to fix one volume permissions issue in Kubernetes.

Each fix revealed the next problem. Here is exactly what happened 🧵

---

**2/**
Migrating kube-prometheus-stack from SD card to NAS.

Mounted a fresh iSCSI LUN. Prometheus runs as non-root by default. Volume got formatted as root on first mount. Permission denied.

---

**3/**
Wall 2: Added fsGroup, runAsUser, runAsGroup.

Still permission denied. The LUN was already formatted with root ownership. Those settings do not retroactively change ownership on an already formatted volume.

---

**4/**
Wall 3: Learned about init containers. They run before everything, do a task, then disappear.

Tried to create one that runs as root to chown the volume. Clashed with the pod-level security context which was now enforcing non-root.

---

**5/**
Wall 4: Explicitly set runAsNonRoot to false on the init container only.

Init container ran as root, chowned the volume, disappeared. Main Prometheus container started normally. Fixed.

---

**6/**
Two lessons:

The first mount formats the LUN. Whatever security context exists at that moment determines ownership.

Pod-level security and container-level security are separate layers. Knowing how to override one from the other is critical.

Full implementation: github.com/AhsanRahat12/Homelab 👇
