# Post 07 — Live Data Migration with rsync | LinkedIn

---

I needed to migrate database volumes from an SD card to iSCSI on my NAS. The PVCs were in two different namespaces. Kubernetes would not let me mount both in a single pod.

Here is how I worked around it.

The problem: my staging deployment lived in one namespace with data on the SD card. My production deployment lived in a different namespace with iSCSI storage on the NAS. A pod can only mount PVCs from its own namespace. I needed both accessible at the same time.

The workaround: I created a migration pod using hostPath volumes instead of PVC mounts. Both the SD card path and the iSCSI mount path were accessible directly on the node's filesystem. The pod mounted them as /old and /new.

The migration:

1. Scaled the app to zero.
2. Deployed the migration pod with two hostPath mounts.
3. Exec'd into the pod.
4. Ran rsync -az /old /new.
5. Data moved cleanly. Zero loss.

This only worked because everything was still on a single node at that point. All storage was local to Pi Zoro. hostPath was safe because there was nowhere else for the pod to schedule.

The real lesson: staging and production are very different. When I first deployed these apps I put real data into my base level staging deployment because I did not understand the gap yet. That migration was the moment the difference between staging and production became real to me.

Full implementation on GitHub: github.com/AhsanRahat12/Homelab

Have you had to migrate data between namespaces in Kubernetes? How did you handle the cross namespace limitation? 👇

#Kubernetes #HomeLab #DevOps #CloudNative #Linux
