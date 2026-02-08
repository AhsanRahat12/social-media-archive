# LinkedIn Post: Kubernetes Networking Basics

---

Kubernetes networking clicked for me when I realized it happens at the pod level, not the container level.

This one insight changed everything.

Here's what every DevOps engineer needs to understand about Kubernetes networking:

**Pod IP Addresses**

Each pod gets its own unique IP address on the cluster. You can view them using:

```
kubectl get pods -o wide
kubectl get pods --all-namespaces -o wide
```

Critical insight: IP addresses aren't unlimited. When you create a cluster, you define a pod CIDR (IP address range). This creates a pool of available addresses. When pods complete their work, delete them to free up IPs. Be thoughtful about IP allocation.

**Pod-to-Pod Communication**

By default, any pod can connect to any pod across all nodes. This is Kubernetes' standard networking model. While you can restrict this with Network Policies, the default is open communication across the entire cluster.

**Container-to-Container Communication (Within a Pod)**

Containers in the same pod communicate through localhost. In your deployment YAML, define unique containerPorts for each container:

✅ Container 1 listens on port 8080
✅ Container 2 listens on port 9000
✅ Both access each other via localhost

This localhost communication is powerful for sidecar patterns and tightly coupled services.

Understanding these fundamentals makes troubleshooting networking issues significantly easier.

What Kubernetes networking concept took you the longest to understand? Share your experience 👇

Follow me for practical Kubernetes and DevOps insights.

#Kubernetes #DevOps #CloudNative #KubeCraft #Networking
