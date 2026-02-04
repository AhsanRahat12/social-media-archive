# LinkedIn Post - Kubernetes Deployments

Today I learned why Kubernetes uses Deployments instead of just running pods directly.

Here's what clicked for me:

Individual pods don't restart automatically. If a pod crashes, it's gone. That's not production-ready.

Deployments solve this by managing replicas:

kubectl create deploy test --image=httpd --replicas=3

One command gives you three identical containers that Kubernetes automatically maintains.

What I found most valuable:

✅ Automatic recovery when pods fail
✅ Built-in scaling without manual intervention
✅ The kubectl describe deployment Events section for troubleshooting
✅ Understanding why production systems need this resilience layer

The --replicas flag isn't just about scaling, it's about reliability. That's the mindset shift I needed.

What concept in Kubernetes took you the longest to understand? I'd love to hear what finally made it click for you 👇

Follow me for daily DevOps learning insights.

#DevOps #Kubernetes #CloudNative #KubeCraft #LearningInPublic
