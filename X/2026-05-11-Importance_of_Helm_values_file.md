# X Thread — One Command, Six Errors

---

## Tweet 1 (Hook)

One command would have saved me from 6 separate errors during a Kubernetes migration.

I skipped `helm show values` and jumped straight into writing manifests.

Here's the cascade 🧵

---

## Tweet 2

Error 1: Created PVCs for Prometheus and Alertmanager manually.

The Prometheus Operator creates its own PVCs automatically. Mine collided with the Operator's. Pods stuck Pending.

Lesson: Check who owns the PVC lifecycle before creating anything.

---

## Tweet 3

Error 2: Prometheus couldn't write to the new volume. Permission denied.

Fresh iSCSI LUNs are root-owned. Prometheus runs as non-root (UID 1000) by default.

Lesson: Set fsGroup in your securityContext before the first mount. Not after.

---

## Tweet 4

Error 3: Added an initContainer to fix permissions. Wrong volume name. Failed.

Error 4: Fixed the name. Pod-level runAsNonRoot: true blocked it from running as root.

Had to explicitly set runAsNonRoot: false on the container to override the pod policy.

---

## Tweet 5

Error 5: Grafana crashed on startup. CrashLoopBackOff.

I added a Prometheus datasource manually. The chart already ships with one as default. Two defaults = crash.

Lesson: Check what the chart already provides before adding anything.

---

## Tweet 6

Error 6: Fixed the values. Pushed. Flux refused to deploy.

Helm stores upgrade state as Kubernetes secrets. It recorded the failure and blocked all future attempts. Had to delete the state secret manually.

---

## Tweet 7

Every single one of these errors was answered in the chart values file.

`helm show values prometheus-community/kube-prometheus-stack > chart-values.yaml`

Reading it would have taken a couple hours. Skipping it cost me a full day.

Going slower would have meant going faster.

---

## Tweet 8 (CTA)

What's a mistake you made that slowing down would have prevented?

Drop it below, I know I'm not alone here 👇

#DevOps #Kubernetes #CloudNative
