# X Thread — Operators Own Things You Think You Own

---

## Tweet 1 (Hook)

I created 6 Kubernetes manifests. 2 of them fought my own cluster.

Here's how an Operator silently undid my work 🧵

---

## Tweet 2

I was migrating Prometheus, Grafana, and Alertmanager to persistent storage.

Three components, three PVCs. Makes sense, right?

Wrong.

---

## Tweet 3

Prometheus and Alertmanager are managed by the Prometheus Operator.

If you haven't worked with one before, an Operator is basically a controller that lives inside your cluster and takes care of an application for you. It creates StatefulSets, PVCs, and configuration automatically.

---

## Tweet 4

And that's exactly what bit me.

The Operator was already creating its own PVCs. When I applied mine through a Helm upgrade, two PVCs tried to claim the same volume.

The Operator's PVCs got stuck Pending. Pods wouldn't start.

---

## Tweet 5

The rule I learned the hard way:

1. Check if a component is a StatefulSet managed by an Operator.
2. If yes, you only create the PersistentVolume. The Operator owns the PVC.
3. If it's a plain Deployment like Grafana, Helm manages it directly. You create both.

---

## Tweet 6

One grep of the Helm chart values would have told me all of this before I wrote a single manifest.

helm show values prometheus-community/kube-prometheus-stack | grep -A10 "storageSpec"
helm show values prometheus-community/kube-prometheus-stack | grep -A10 "existingClaim"

---

## Tweet 7 (CTA)

This applies far beyond kube-prometheus-stack.

Any time you're working with Helm charts that deploy Operator-managed workloads, ask one question first: who owns the PVC lifecycle, Helm or the Operator?

Have you ever had an Operator fight your manual config? What happened? 👇

#Kubernetes #DevOps #CloudNative
