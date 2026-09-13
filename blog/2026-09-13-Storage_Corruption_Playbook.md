# One Crash-Looping Pod Corrupted Six Volumes. Now I Have a Playbook.

On June 20th I was debugging a brand new app on my Raspberry Pi cluster. It had a volume permission bug — wrong UID, harmless on its own. The pod crash-looped while I worked on it.

By the time I looked up, six volumes across two nodes were corrupted. Prometheus, Grafana, Alertmanager, Audiobookshelf, and both of my Postgres LUNs. Apps that had nothing to do with what I was debugging.

That incident, and the two that followed, turned into a written playbook. This is the story of what actually breaks, and the procedure that came out of it.

---

## The Disease Behind Every Symptom

My cluster stores app data on iSCSI LUNs served by a QNAP NAS. iSCSI is block storage over the network: the node sees a network volume as a local disk, formats it with ext4, and the app writes to it like any other filesystem.

The weak point is a single daemon called `iscsid`. One process per node, managing the network sessions for *every* LUN on that node. It's a shared resource, and it was never designed for the session churn Kubernetes generates.

A crash-looping pod mounts and unmounts its volume on every restart. Each cycle is a login and logout against the NAS. Hammer that fast enough and `iscsid` starts dropping sessions — including sessions belonging to *other* apps, mid-write.

When a session drops mid-write, the ext4 journal on that LUN is left half-written. On the next mount, the filesystem sees a broken journal and starts returning I/O errors on everything. Postgres reports it as query failures. SQLite says "database disk image is malformed." Prometheus fails WAL writes. Same disease, four different symptoms.

> **[INSERT DIAGRAM: the cascade — one crash-looping pod to six corrupted volumes]**

That's how one harmless UID bug corrupted six unrelated volumes. The bug didn't do it. The bug *plus Flux continuously restarting the pod* did it.

---

## The One Command That Would Have Prevented Everything

```bash
flux suspend kustomization actual-budget
```

That's it. If I had suspended Flux before debugging, the pod would have stayed down, `iscsid` would have seen zero churn, and the other six volumes would never have been touched.

This became rule one of the playbook, and it's bigger than it looks: **before you repair anything, silence everything that can fight you.** In my cluster, up to three separate controllers can undo your work while you're mid-repair:

Flux's Kustomization re-applies manifests from Git. Flux's HelmRelease drift-corrects Helm-managed apps independently — suspending the Kustomization alone does not stop it. And operators like the Prometheus Operator or CNPG reconcile their workloads back to whatever their custom resource says, no matter what you do to the underlying StatefulSet.

I learned the second one the hard way during a later recovery: Kustomization suspended, and Grafana kept scaling itself back up mid-repair because the HelmRelease was still active. Fighting reconcilers during a storage repair doesn't just slow you down — the churn it creates is the exact mechanism that spread the corruption in the first place.

---

## The Playbook

Three incidents distilled into one procedure. Condensed:

**1. Recognize the signature.** "Database disk image is malformed," `Input/output error`, or a pod that's `Running` with zero restarts while every request fails — go straight to storage, don't debug the app.

**2. Suspend every reconciler.** Kustomization, HelmRelease, and know which operator owns the workload:

```bash
flux suspend kustomization <app> -n flux-system
flux suspend helmrelease <app> -n <namespace>   # Helm-managed apps drift-correct independently
kubectl get pods -n <namespace> -w              # confirm the count actually holds, doesn't bounce back
```

**3. Identify the exact block device on the node.** Never guess from the PVC name — confirm by mountpoint, which embeds the volume name. SSH into the Pi, then:

```bash
mount | grep <pv-name>
sudo lsblk -o NAME,SIZE,TYPE,MOUNTPOINT
```

**4. Scale to zero — but patch the right object.** Operator-managed workloads won't stay down if you scale the StatefulSet; the operator puts it back. Patch the custom resource instead:

```bash
# Operator-managed (Prometheus, CNPG): patch the CR
kubectl patch prometheus <name> -n monitoring --type=merge -p '{"spec":{"replicas":0}}'

# Plain Deployments (Grafana, Linkding, Audiobookshelf): scale directly
kubectl scale deployment <app> -n <namespace> --replicas=0
```

**5. Wait for the real unmount.** My CSI driver is known to leave stale VolumeAttachment objects that never clear on their own:

```bash
kubectl get volumeattachments | grep <pv-name>
kubectl delete volumeattachment <id>            # once no pod references the PVC
```

**6. Repair the filesystem — on the raw, unmounted device, never a mounted one.** Two writers on the same device is guaranteed corruption. SSH into the Pi, then:

```bash
findmnt | grep sdXX               # no output = safe to proceed
sudo fsck.ext4 -y /dev/sdXX
```

"Recovering journal" in the output means it's working.

**7. Scale back up.** The stale-attachment trap can hit again on the way up — a `Multi-Attach` error means back to step 5:

```bash
kubectl patch prometheus <name> -n monitoring --type=merge -p '{"spec":{"replicas":1}}'
kubectl scale deployment <app> -n <namespace> --replicas=1
```

**8. Verify the data, not the pod status.** A green pod proves nothing — open the app and actually use it. Log in, load pages, write something. For Prometheus, confirm the WAL errors have actually stopped:

```bash
kubectl logs -n monitoring prometheus-<pod> -c prometheus --tail=100 | grep -iE "error|i/o"
```

**9. Resume the reconcilers.** Both of them. Watch one clean reconciliation before walking away:

```bash
flux resume helmrelease <app> -n <namespace>
flux resume kustomization <app> -n flux-system
```

**10. Write it down.** Dated notes: what broke, root cause, exact commands, what surprised you. Future-you is the audience.

---

## Every Rule Has a Scar

The playbook isn't theory — each line exists because skipping it cost me something.

The stale VolumeAttachment rule exists because it has now bitten three separate recoveries. The "patch the CR, not the StatefulSet" rule exists because the Prometheus Operator silently reverted my scale-downs until I figured out who actually owned the replica count.

The most expensive lesson: **a running pod is not a healthy pod.** My Postgres primary sat fully broken for two days — every query failing with I/O errors — while showing zero restarts, because its liveness probe only checked that the process was alive, not that it could read or write. Prometheus does the same thing: it will run green while silently failing every write to disk. The playbook's first step is recognizing that signature precisely because the dashboard won't show it.

---

## The Takeaway

Storage incidents on Kubernetes are rarely about the app that's screaming. Mine were about a shared daemon two layers below Kubernetes, and a GitOps controller that kept "helping."

If you run stateful workloads on iSCSI, three things transfer directly: suspend your reconcilers before debugging anything that touches a volume, never trust pod status as proof of health, and write the incident down the same day — because the third time it happens, the notes become a playbook, and the playbook turns a lost evening into twenty minutes.

---

*This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to DevOps engineer.*

💼 LinkedIn: [Rahat Ahsan](https://linkedin.com/in/rahatahsan)
🐦 X: [@RahatAhsan20](https://x.com/RahatAhsan20)
🐙 GitHub: [AhsanRahat12](https://github.com/AhsanRahat12)
