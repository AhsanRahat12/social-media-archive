# LinkedIn Post 1 — One Command, Six Errors

---

One command would have saved me from 6 separate errors during a Kubernetes migration.

I was migrating Prometheus, Grafana, and Alertmanager from ephemeral storage to persistent iSCSI volumes. I jumped straight into writing manifests.

Here's what happened:

1. Created PVCs manually for components that auto-generate their own. Collision.
2. Missed that Prometheus runs as non-root. Permission denied.
3. Added an initContainer with the wrong volume name. Failed.
4. Fixed the name but forgot pod-level security policy blocks container-level overrides. Failed again.
5. Duplicate default datasource crashed Grafana. CrashLoopBackOff.
6. Helm recorded the failure and blocked all future upgrades.

The one command that would have prevented all of this:

helm show values prometheus-community/kube-prometheus-stack > chart-values.yaml

Every answer was already in the chart. Who owns the PVC. What user the container runs as. What datasource is pre-configured. I just never read it.

Rushing cost me an entire day of debugging. Reading the chart values first would have taken a couple hours. Going slower would have meant going faster. The time you "save" by skipping preparation, you pay back tenfold in troubleshooting.

What's a mistake you made that could have been avoided by slowing down and reading the docs first? I know I'm not alone here 👇

#DevOps #Kubernetes #CloudNative #LessonsLearned #HomeLab
