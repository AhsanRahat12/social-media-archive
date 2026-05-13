# LinkedIn Post 3 — Operators Own Things You Think You Own

---

I created 6 Kubernetes manifests. 2 of them fought my own cluster.

During a storage migration, I created PersistentVolumeClaims for Prometheus, Grafana, and Alertmanager. Three components, three PVCs. Makes sense, right?

Wrong.

Prometheus and Alertmanager are managed by the Prometheus Operator. If you haven't worked with one before, an Operator is basically a controller that lives inside your cluster and takes care of an application for you. It handles creating StatefulSets, PVCs, and configuration automatically so you don't have to.

And that's exactly what bit me. The Operator was already creating its own PVCs. When I applied mine through a Helm upgrade, two PVCs tried to claim the same volume. The Operator's PVCs got stuck Pending. Pods wouldn't start.

The rule I learned the hard way:

1. Check if a component is a StatefulSet managed by an Operator.
2. If yes, you only create the PersistentVolume. The Operator owns the PVC.
3. If it's a plain Deployment like Grafana, Helm manages it directly, so you create both the PV and the PVC.

One grep of the Helm chart values would have told me all of this before I wrote a single manifest.

This applies far beyond kube-prometheus-stack. Any time you're working with Helm charts that deploy Operator-managed workloads, ask one question first: who owns the PVC lifecycle, Helm or the Operator?

Have you ever had an Operator fight your manual configuration? What happened? 👇

#Kubernetes #DevOps #CloudNative #Helm #InfrastructureAsCode
