# Post 07 — ClusterIP vs NodePort vs LoadBalancer (X / Twitter)

---

**Tweet 1**
Most Kubernetes tutorials show you how to deploy an app.

Almost none explain how traffic actually reaches it.

There are 3 Service types and each one does something completely different 🧵

---

**Tweet 2**
ClusterIP is the default.

Gives your pod a stable internal IP. Only reachable from inside the cluster.

This is what your apps use to talk to each other internally.

---

**Tweet 3**
NodePort exposes your app on a port directly on the node.

Reachable from outside the cluster but you need to know the node IP and port number.

Works. Not clean in production.

---

**Tweet 4**
LoadBalancer provisions an external IP for clean outside access.

In cloud environments this spins up an actual load balancer. In my home lab Traefik handles this role.

This is your production external access layer.

---

**Tweet 5**
The simple mental model:

ClusterIP → internal app to app communication.
NodePort → quick external access for testing.
LoadBalancer → production external access with a clean IP.

---

**Tweet 6**
In my home lab Traefik sits as the LoadBalancer and handles all external traffic into the cluster.

Everything else uses ClusterIP internally.

Which Service type trips you up the most?
