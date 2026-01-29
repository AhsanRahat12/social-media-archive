# Kubernetes Request Flow - X/Twitter Thread

## Tweet 1
What actually happens when you run `kubectl run nginx --image=nginx`?

Here's the flow that finally made Kubernetes click for me 🧵

## Tweet 2
1. kubectl reads ~/.kube/config to find your cluster
2. Sends request to API server (port 6443)
3. Scheduler analyzes available CPU/memory across nodes
4. Pod gets created on optimal node with networking + storage auto-configured

## Tweet 3
The game-changer: Read commands (kubectl get) hit the API server and stop. Create commands (kubectl run) go through the scheduler for intelligent placement.

This is why checking your context matters. Wrong context = different cluster entirely.

## Tweet 4
Day 1 of Kubernetes and this architecture makes way more sense than I expected.

What concept finally made K8s click for you?

#Kubernetes #DevOps #CloudNative #LearningInPublic
