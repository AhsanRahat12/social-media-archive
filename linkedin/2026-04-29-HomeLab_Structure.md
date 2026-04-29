# Post — Monorepo Structure (LinkedIn)

---

Early iterations of my home lab were unorganized.

I had everything in one folder. Apps, infrastructure, cluster config all mixed together. The moment I started adding more it became impossible to navigate.

That is when I looked at how serious teams structure their Kubernetes repos and applied the same pattern.

Four folders. Each with a clear purpose:

clusters handles the Flux entrypoint and wires everything together.

apps holds my workloads. audiobookshelf and linkding, each with base manifests and environment specific patches.

infrastructure manages the controllers everything depends on. democratic-csi for storage, Renovate for automated updates.

monitoring runs kube-prometheus-stack and Grafana for full cluster visibility.

The deploy order matters. Infrastructure first. Monitoring second. Apps last. Flux enforces this automatically through dependsOn.

Everything has a place now. I can rebuild the entire cluster from scratch just by pointing Flux at the repo.

Want to see the full structure? github.com/AhsanRahat12/Homelab

How are you structuring your Kubernetes repos? 👇

Follow me, I am documenting everything I build and learn in my home lab.

#Kubernetes #DevOps #GitOps #CloudNative #Homelab
