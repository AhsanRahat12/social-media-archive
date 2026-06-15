# Post 7 — LinkedIn

**Topic:** The Operator Owns the StatefulSet. Scale It Down First.
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

I scaled my Prometheus StatefulSet to zero for maintenance.

30 seconds later it was back at 1 replica.

I scaled it to zero again. It came back again.

I was not losing my mind. The Operator was doing its job.

The kube-prometheus-stack Operator continuously watches the Prometheus StatefulSet and reconciles it back to the desired state whenever something deviates. Scale the StatefulSet to zero directly and the Operator sees a drift from what it expects and immediately corrects it.

You are fighting the Operator. The Operator will win every time.

The correct order is:

1. Scale the Operator down first
2. Then scale the StatefulSet to zero
3. Do your maintenance
4. Scale the StatefulSet back up
5. Scale the Operator back up last

```bash
kubectl scale deployment kube-prometheus-stack-operator -n monitoring --replicas=0
kubectl scale statefulset prometheus-kube-prometheus-stack-prometheus -n monitoring --replicas=0
```

With the Operator down, nothing is watching the StatefulSet. You can scale it, leave it at zero, do your maintenance, and bring it back without anything fighting you.

This applies to every Operator-managed workload. CNPG, CertManager, Strimzi, Rook. The pattern is the same everywhere: the Operator is the source of truth for its managed resources. Work with that, not against it.

The Operator is not a bug. It is doing exactly what it was designed to do. The mistake is treating an Operator-managed resource like a regular deployment you own directly.

Have you ever fought an Operator reconciliation loop before understanding what was happening?

Follow for the series on Kubernetes abstractions that work exactly as designed and catch you out anyway.

#Kubernetes #DevOps #SRE #Prometheus #Infrastructure

---

## Metadata

- **Source:** ABS Incident and Grafana Incident / June 2026 Incident Report
- **Pillar:** Kubernetes abstractions working as designed
- **Tier:** Solid
- **Target audience:** DevOps Engineers, SREs, Platform Engineers
