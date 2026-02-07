# X (Twitter) Post - Kubernetes Rolling Update

Just watched Kubernetes do a zero-downtime deployment in real-time.

Split terminal: `watch -n 1 "kubectl get pods"` on the left, deployment update on the right.

One pod up. One pod down. Repeat. No interruption. Perfectly orchestrated.

RollingUpdate strategy with maxSurge: 1 and maxUnavailable: 1 kept 9/10 replicas available while updating the image.

This is why we don't schedule maintenance windows anymore.

#Kubernetes #DevOps #CloudNative
