# X Thread — The Filesystem Permissions Trap

---

## Tweet 1 (Hook)

"Permission denied."

Two words that cost me 3 hours and 4 failed deployments on a single Kubernetes volume.

Here's the full cascade 🧵

---

## Tweet 2

I mounted a fresh iSCSI volume into a Prometheus pod. It wouldn't start.

The pod originally had no securityContext, so the container ran as root. When Kubernetes formatted the fresh LUN, the filesystem was created with root ownership.

---

## Tweet 3

Then I added a securityContext to run Prometheus properly as non-root (UID 1000).

Problem: the filesystem was already root-owned. Non-root user + root-owned filesystem = permission denied. Every time.

---

## Tweet 4

The fix is simple IF you know it before the first mount.

Set fsGroup in your securityContext. Kubernetes will change the volume's group ownership automatically at mount time.

I didn't know it. I missed it.

---

## Tweet 5

So I needed an initContainer running as root to manually fix the ownership.

Added runAsUser: 0 to the initContainer. Kubernetes rejected it.

Why? The pod-level runAsNonRoot: true overrides everything. Including your initContainers.

---

## Tweet 6

Setting runAsUser: 0 alone is not enough.

You have to explicitly set runAsNonRoot: false on the initContainer itself to override the pod-level policy.

Three errors cascading from one missed setting.

---

## Tweet 7 (CTA)

The lesson: set your securityContext before the first mount. Not after.

What's a "simple" Kubernetes setting that's bitten you harder than expected? 👇

#Kubernetes #DevOps #Linux #ContainerSecurity
