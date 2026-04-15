# Post 07 — ClusterIP vs NodePort vs LoadBalancer (LinkedIn)

---

Most Kubernetes tutorials show you how to deploy an app.

Almost none of them explain how traffic actually reaches it.

There are three Service types in Kubernetes and each one serves a completely different purpose.

1. ClusterIP is the default. It gives your pod a stable internal IP address. Only reachable from inside the cluster. This is what your apps use to talk to each other.
2. NodePort exposes your app on a port directly on the node. Reachable from outside the cluster but you have to know the node IP and port number. Works but not clean in production.
3. LoadBalancer provisions an external IP so traffic can reach your app cleanly from outside. In cloud environments this spins up an actual load balancer. In my home lab Traefik handles this role.

The way I think about it:

✅ ClusterIP: internal communication between your apps.
✅ NodePort: quick external access, mostly for testing.
✅ LoadBalancer: production external access with a clean IP.

In my home lab setup Traefik sits as the LoadBalancer and handles all external traffic coming into the cluster. Everything else uses ClusterIP to talk internally.

Which Service type trips people up the most in your experience? 👇

Follow me, I am documenting everything I build and learn in my home lab.

`#Kubernetes` `#DevOps` `#CloudNative` `#Networking` `#Homelab`
