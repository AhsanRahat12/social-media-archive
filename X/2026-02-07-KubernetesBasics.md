# X (Twitter) Thread: Kubernetes Networking Basics

---

## Tweet 1

Kubernetes networking clicked for me when I realized: it happens at the POD level, not the container level.

This one insight changed everything.

Here's what you need to know: 🧵

---

## Tweet 2

Each pod gets its own IP address. View them with:

kubectl get pods -o wide

Critical insight: IP addresses aren't unlimited. You define a pod CIDR when creating your cluster. Delete completed pods to free up IPs.

---

## Tweet 3

By default, ANY pod can connect to ANY pod across all nodes.

This is Kubernetes' standard networking model. You can restrict this with Network Policies, but the default is open communication.

---

## Tweet 4

Containers within the same pod communicate through LOCALHOST.

Define unique containerPorts in your YAML:
✅ Container 1: port 8080
✅ Container 2: port 9000
✅ Both access each other via localhost

Powerful for sidecar patterns.

---

## Tweet 5

Understanding these fundamentals makes troubleshooting networking issues significantly easier.

What Kubernetes concept took you longest to understand?

#Kubernetes #DevOps #CloudNative
