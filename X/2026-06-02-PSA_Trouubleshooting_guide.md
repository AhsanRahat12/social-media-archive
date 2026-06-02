# X Thread 5 — The War Story

---

**Tweet 1 (Hook)**
Implementing PSA restricted on two production apps this week.

One was completely clean. The other broke in two ways simultaneously. Here is what happened 🧵

---

**Tweet 2**
App 1: Audiobookshelf.

Added warn labels. Got three violations. Fixed all three. Flipped to enforce. Rolling deployment ran clean.

Zero issues.

---

**Tweet 3**
App 2: Linkding.

Added warn labels. Got the same three violations. Fixed all three. Then everything broke.

---

**Tweet 4**
Problem #1: readOnlyRootFilesystem: true crashed the pod immediately.

Linkding's process manager and web server both needed to write to /tmp. The container filesystem was locked. They could not create temp files.

---

**Tweet 5**
Fix: Mount an emptyDir volume at /tmp.

Temporary, isolated, writable space just for that pod. Allows the app to write to /tmp without exposing the container filesystem.

---

**Tweet 6**
Problem #2: A ReadWriteOnce iSCSI volume deadlocked during rolling restart.

The old crashing pod held the lock. The new pod sat in ContainerCreating waiting to detach. Kubernetes restarted the old pod over and over, never releasing the lock.

---

**Tweet 7**
Two separate failures at the same time. Took me an hour to realize they were not the same problem.

Fix: kubectl scale to zero, wait 15 seconds for iSCSI to fully detach, scale back up.

---

**Tweet 8 (CTA)**
Both apps now running PSA restricted in production. Clean.

Real lesson: the warn phase tells you what will fail. But production hits edge cases staging never caught. Always test in staging first.

Have you hit readOnlyRootFilesystem crashes? Reply and let me know 👇
