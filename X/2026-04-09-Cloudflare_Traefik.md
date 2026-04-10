# Post 03 — Traefik + Cloudflare Tunnel (X / Twitter)

---

**Tweet 1**
Ingress NGINX was retired in March 2026.

No releases. No bug fixes. No security patches.

I already moved on. Here is the combination I run instead 🧵

---

**Tweet 2**
First, understand the problem with Ingress NGINX.

It was doing two jobs at once. Routing traffic inside the cluster AND sitting at the edge handling external access.

That is too much responsibility for one component maintained by one or two volunteers.

---

**Tweet 3**
I split those concerns into two dedicated tools.

Traefik handles all internal routing inside the cluster. It reads ingress rules and sends requests to the right service.

Actively maintained. Lightweight. Works perfectly on a Raspberry Pi.

---

**Tweet 4**
Cloudflare Tunnel handles external access.

A cloudflared pod runs inside your cluster and opens an outbound connection to Cloudflare's network. External traffic comes in through Cloudflare and gets forwarded through that tunnel.

Your router never receives a direct inbound connection.

---

**Tweet 5**
What that means in practice:

Home IP never exposed.
No open ports on your router.
TLS handled at the Cloudflare edge automatically.
Two replicas running so if one pod crashes the tunnel stays up.

---

**Tweet 6**
Two separate concerns. Two dedicated tools.

Traefik owns internal routing.
Cloudflare owns external access.

Much cleaner than relying on a single retired controller for everything.

What did you switch to after Ingress NGINX?
