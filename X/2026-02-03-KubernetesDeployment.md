# X (Twitter) Posts - Kubernetes Deployments

## Tweet 1 (Thread Starter)

Today I learned why Kubernetes uses Deployments instead of just running pods directly.

Individual pods don't restart automatically. If a pod crashes, it's gone.

Deployments manage replicas that Kubernetes automatically maintains. That's the reliability layer production needs.

---

## Tweet 2 (Reply to Tweet 1)

Creating a deployment with 3 replicas:

kubectl create deploy test --image=httpd --replicas=3

One command = three identical containers with:
• Automatic recovery
• Built-in scaling
• Self-healing infrastructure

The --replicas flag isn't just about scaling, it's about resilience.

---

## Tweet 3 (Reply to Tweet 2)

What Kubernetes concept took you the longest to understand?

#DevOps #Kubernetes #CloudNative
