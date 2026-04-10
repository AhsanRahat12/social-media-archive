# Post 03 — Traefik + Cloudflare Tunnel (LinkedIn)

---

Ingress NGINX was retired in March 2026.

No more releases. No bug fixes. No security patches. Done.

For a component that was sitting at the edge of thousands of clusters, that is a significant shift.

I already run Traefik and Cloudflare Tunnel. Here is why that combination works better:

Traefik handles all internal traffic routing inside the cluster. It reads your ingress rules and routes requests to the right service. Clean, fast, and actively maintained.

Cloudflare Tunnel handles external access entirely differently to the old approach.

1. A cloudflared pod runs inside your cluster and opens an outbound connection to Cloudflare's network.
2. External traffic comes in through Cloudflare and is forwarded through that tunnel.
3. Your router never receives a direct inbound connection.

The result:

✅ Home IP never exposed.
✅ No open ports on your router.
✅ TLS handled at the Cloudflare edge automatically.
✅ Two replicas running so if one pod crashes, the tunnel stays up.

Two separate concerns. Two dedicated tools. Much cleaner than the old approach.

If you are still running Ingress NGINX, now is the time to move. What are you switching to? 👇

Follow me, I am documenting everything I build and learn in my home lab.

`#Kubernetes` `#DevOps` `#CloudNative` `#Homelab` `#Security`
