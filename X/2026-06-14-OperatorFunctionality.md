# Post 7 — X (Twitter) Thread

**Topic:** The Operator Owns the StatefulSet. Scale It Down First.
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 7**

🧵 I scaled my Prometheus StatefulSet to zero for maintenance.

30 seconds later it was back at 1 replica.

I scaled it to zero again. It came back again.

I was not losing my mind.

---

**Tweet 2 / 7**

The kube-prometheus-stack Operator continuously watches the StatefulSet.

When it sees a deviation from the desired state, it reconciles it back immediately.

You are fighting the Operator. The Operator will win every time.

---

**Tweet 3 / 7**

The correct order:

1. Scale the Operator down first
2. Scale the StatefulSet to zero
3. Do your maintenance
4. Scale the StatefulSet back up
5. Scale the Operator back up last

---

**Tweet 4 / 7**

kubectl scale deployment kube-prometheus-stack-operator -n monitoring --replicas=0

kubectl scale statefulset prometheus-kube-prometheus-stack-prometheus -n monitoring --replicas=0

With the Operator down, nothing is watching. You can work without anything fighting you.

---

**Tweet 5 / 7**

This applies to every Operator-managed workload.

CNPG. CertManager. Strimzi. Rook.

Same pattern everywhere: the Operator is the source of truth for its managed resources. Work with that, not against it.

---

**Tweet 6 / 7**

The Operator is not a bug.

It is doing exactly what it was designed to do.

The mistake is treating an Operator-managed resource like a regular deployment you own directly.

---

**Tweet 7 / 7**

Have you ever fought an Operator reconciliation loop before understanding what was happening?

Follow for the series on Kubernetes abstractions that work exactly as designed and catch you out anyway.

---

## Metadata

- **Source:** ABS Incident and Grafana Incident / June 2026 Incident Report
- **Pillar:** Kubernetes abstractions working as designed
- **Tier:** Solid
- **Target audience:** DevOps Engineers, SREs, Platform Engineers
