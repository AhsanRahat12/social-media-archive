# LinkedIn Post 5 — The War Story

---

Implementing PSA restricted on two production apps this week. One was completely clean. The other broke in two ways simultaneously.

App 1: Audiobookshelf. Added warn labels, restarted, got three violations. Fixed all three. Flipped to enforce. Rolling deployment ran clean. Zero issues.

App 2: Linkding. Added warn labels, restarted, got the same three violations. Fixed all three. Then everything broke.

readOnlyRootFilesystem: true crashed the pod immediately. Error message: Linkding's process manager and web server both needed to write to /tmp. The container filesystem was locked. They could not create temp files.

Fix: Mount an emptyDir volume at /tmp. Temporary, isolated, writable space just for that pod.

Then a second problem hit at the same time. A ReadWriteOnce iSCSI volume deadlocked during the rolling restart. The old crashing pod held the volume lock. The new pod sat in ContainerCreating waiting for the volume to detach. Kubernetes kept restarting the old pod in CrashLoopBackOff, never fully releasing the lock.

Two separate failures happening simultaneously. Took me an hour to realize they were not the same problem.

Fix: kubectl scale to zero, wait 15 seconds for iSCSI to fully detach, scale back up.

Both apps now running PSA restricted in production. Clean.

The real takeaway: the warn phase tells you exactly what will fail. But production will still hit edge cases your staging environment never caught. Always test in staging first.

Have you hit unexpected crashes when enabling readOnlyRootFilesystem? What was the culprit? 👇

#Kubernetes #DevOps #CloudNative #PlatformEngineering #KubernetesSecurity
