# I Lost All My Prometheus Metrics 28 Times. Here's Why.

I didn't delete the pod.

The storage ran out of space.

Prometheus was running on my Raspberry Pi's SD card. 30GB of total space. The metrics database kept growing. One day it filled the disk. Prometheus crashed. Kubernetes restarted the pod. Everything inside — metrics, dashboards, alert rules — got wiped.

Then it filled up again. And crashed again. And again.

This happened 28 times.

Each restart meant losing the entire metrics database. I'd have days of data one moment, then the pod would crash and I'd lose everything. The TSDB would try to recover, fail, and start fresh. Rinse, repeat.

By the time I understood what was happening, I'd lost months of potential observability. The system was designed to fail, and I didn't even realize it.

---

## The Three Stages of Failure

My storage journey was painful because I didn't completely understand what I was doing initially. Each stage taught me something fundamental.

### Stage 1: The SD Card Illusion

At first, I stored everything on the Raspberry Pi's SD card. Linkding bookmarks, Audiobookshelf metadata, Prometheus metrics. Everything local, nothing to worry about.

But that 30GB SD card wasn't just storage for my apps. It was running the entire Pi. The OS, Kubernetes, every component, every workload — all of it lived on that one drive.

Prometheus metrics kept growing. But so did Kubernetes logs. So did application data. The disk filled constantly. Not over months. Over weeks. Sometimes over days.

When the disk hit 100%, Prometheus crashed. Kubernetes restarted the pod. But the restart meant the metrics database files got corrupted. On the next start, Prometheus would try to recover the TSDB, fail, and start fresh. Everything lost.

This happened 28 times. 28 separate restarts caused by disk exhaustion. And each restart meant losing all historical metrics.

The problem wasn't just that the SD card was too small. The problem was mainly that I'd built a system designed to fail. Everything running on the same drive. No separation between the OS, the container runtime, the applications, and the data. No monitoring of disk usage. No retention policies. No idea when the disk would fill up until the pod crashed.

By the time I understood what was happening, I realized I'd been flying blind the entire time. Observability requires history. I had none because the disk ran out so often.

### Stage 2: The Node-Pinned Storage

I discovered iSCSI on the QNAP NAS. Problem solved, right?

I created an iSCSI LUN on the NAS and connected it to my Kubernetes cluster. Created a PersistentVolume pointed at that LUN. Pinned Linkding to only run on Zoro (the control plane) so it always had access to the volume.

This worked — until it didn't. When I restarted Zoro for an OS update, Luffy (the worker node) couldn't run Linkding because it was nodeSelector'd to Zoro. The app went down. The LUN was unreachable from any other node.

More fundamentally, I'd created a single point of failure. If Zoro (the controller) failed, Linkding failed. No recovery. No failover.

### Stage 3: Storage That Survives Failures

That's when I discovered democratic-csi, a CSI driver that manages iSCSI and NFS mounts automatically.

Instead of pinning workloads to specific nodes, the CSI driver handles the attach/detach lifecycle. A pod can run on any node, and its storage follows it.

When Zoro restarts, Linkding reschedules to Luffy. The iSCSI LUN detaches from Zoro and reattaches to Luffy. Same data, different node. The pod comes back with everything intact.

That's when I stopped worrying about node failures. My cluster became more resilient.

---

## Why iSCSI AND NFS

Once I understood that the QNAP NAS was the reliable backend, the next question was: why do I have both iSCSI and NFS?

They're not redundant. They're different tools for different problems.

**iSCSI provides block storage.** Think of it like a hard drive you plug into a server. Low-level, high performance, exclusive access. Only one pod can use an iSCSI LUN at a time (ReadWriteOnce). Perfect for databases, configuration files, anything that needs reliable single-access storage.

Audiobookshelf stores configs and metadata on iSCSI LUNs. The database that tracks which audiobooks you have, which ones you're currently listening to, your bookmarks and progress — all of it runs on iSCSI.

**NFS provides file storage.** Think of it like a network hard drive that multiple computers can mount simultaneously. Network-level, slower than iSCSI, but allows multiple pods to access the same data (ReadWriteMany). Perfect for shared files that different workloads need to read.

Audiobookshelf's actual audiobooks and podcasts live on NFS shares. The media library is just files on a network mount. The app reads from them, but nothing else needs exclusive access.

---

## The Permissions Nightmare

Understanding iSCSI vs NFS was one thing. Getting permissions right was another.

When I first mounted iSCSI LUNs, containers couldn't write to them. Permission denied.

The problem was that containers were running as root, but the mounted LUNs and NFS shares had different ownership and permissions. I had to learn about securityContext in Kubernetes — specifying the UID/GID that a container runs as, and the filesystem group that owns mounted volumes.

Here's what I use for Audiobookshelf:

```yaml
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
    runAsGroup: 1000
    fsGroup: 1000
    seccompProfile:
      type: RuntimeDefault

  containers:
  - name: audiobookshelf
    volumeMounts:
    - mountPath: /config
      name: config-volume
    - mountPath: /metadata
      name: metadata-volume
    - mountPath: /audiobooks
      name: audiobooks-volume
    - mountPath: /podcasts
      name: podcasts-volume
    
    securityContext:
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      capabilities:
        drop:
        - ALL

  volumes:
  - name: config-volume
    persistentVolumeClaim:
      claimName: config-pvc      # iSCSI LUN
  - name: metadata-volume
    persistentVolumeClaim:
      claimName: metadata-pvc    # iSCSI LUN
  - name: audiobooks-volume
    persistentVolumeClaim:
      claimName: audiobooks-pvc  # NFS share
  - name: podcasts-volume
    persistentVolumeClaim:
      claimName: podcasts-pvc    # NFS share
```

The pod-level securityContext sets the UID/GID for all containers. The fsGroup: 1000 makes the container's user own any mounted volumes. The container-level securityContext hardens it further: no privilege escalation, read-only filesystem (except the volumes), no Linux capabilities.

But that still wasn't enough. The NAS has its own user and group system for iSCSI and NFS shares. The LUNs and shares have to be owned by a UID/GID that matches what the container expects (1000/1000 in this case).

I had to understand the full stack: Kubernetes securityContext → container user/group → NAS mount permissions. All three had to align.

The fix was tedious but the understanding was massive. Security contexts in Kubernetes aren't optional extras. They're the bridge between your container's user model and your storage backend's permission model. Get one wrong and the pod fails silently.

---

## What Actually Survives Node Failure

Let me walk through what happens when a node fails in production.

A pod running Linkding on Zoro has an iSCSI LUN mounted.

Zoro dies (hardware failure, restart, whatever).

Kubernetes scheduler immediately reschedules the pod to Luffy.

The democratic-csi driver sees the new pod needs the same LUN.

The driver detaches the LUN from Zoro and attaches it to Luffy.

The pod starts on Luffy with the exact same mounted volume.

Linkding is back up. No data loss. No manual recovery. Just Kubernetes doing its job.

This is what I spent months building toward. A system where storage follows the workload, not the other way around.

---

## The Real Cost of Prometheus Losing Data

Those 28 pod restarts that wiped my Prometheus database weren't just annoying. They were preventing me from learning anything.

Metrics are only useful if they persist. Dashboards without history are useless. I was flying blind.

The moment I migrated Prometheus, Grafana, and Alertmanager to dedicated iSCSI LUNs, I could actually see what was happening in my cluster over time. I could spot trends. I could see when things degraded. I could understand failures after they happened.

Before: data lost every restart, learning impossible.

After: 30 days of retention, actual observability. I was on my way to implementing resource limits and requests using the base data that was now persisting.

---

## The Lesson That Stuck

Storage in Kubernetes looks simple from the outside. Create a PVC, mount it in a pod, done.

But understanding where that storage lives, how it survives failures, why you choose one backend over another — that's where the real learning is.

My Prometheus metrics database taught me this the hard way. 28 times.

Now every storage decision is intentional. Block storage for exclusive-access workloads that need performance. File storage for shared data. Nothing on the nodes themselves. Everything on the NAS, which survives node failures because it exists outside the cluster.

That's production thinking. That's what separates "I got it working" from "it actually survives failure."

---

*This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to DevOps engineer.*

💼 LinkedIn: [Rahat Ahsan](https://linkedin.com/in/rahatahsan)
🐦 X: [@RahatAhsan20](https://x.com/RahatAhsan20)
🐙 GitHub: [AhsanRahat12](https://github.com/AhsanRahat12)
