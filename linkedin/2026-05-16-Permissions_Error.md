# LinkedIn Post 4 — The Filesystem Permissions Trap

---

"Permission denied." Two words that cost me 3 hours and 4 failed deployments.

I mounted a fresh iSCSI volume into a Prometheus pod. It wouldn't start. Permission denied on the data directory.

Here's what was happening:

1. The pod originally had no securityContext, so the container ran as root. When Kubernetes formatted the fresh LUN, the filesystem was created with root ownership.
2. When I added a securityContext to run Prometheus properly as non-root (UID 1000), it could no longer write to the root-owned filesystem it had already created.
3. Non-root user + root-owned filesystem = permission denied. Every time.

The fix is simple IF you know it before the first mount. Set fsGroup in your pod's securityContext. Kubernetes will change the volume's group ownership automatically at mount time.

But here's the trap. fsGroup only works at mount time. If you miss it on the first mount, you need an initContainer running as root to manually fix the ownership afterward.

I missed it. Then the pod-level security policy blocked my initContainer from running as root. Setting runAsUser: 0 on the container wasn't enough. The pod-level runAsNonRoot: true overrides everything. I had to explicitly set runAsNonRoot: false on the initContainer itself to make it work.

Set your securityContext before the first mount. Not after.

What's a "simple" Kubernetes setting that's bitten you harder than expected? 👇

#Kubernetes #DevOps #Linux #CloudNative #ContainerSecurity
