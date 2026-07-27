# Zero Open Ports. HTTPS by Default. Here's How.

My Raspberry Pis sit on a home network behind a Netgear switch.

No open ports on the router. No port forwarding. No DDoS surface. But my applications are publicly accessible at rahatahsan.com with HTTPS.

Most people think that's impossible. It's not. Cloudflare Tunnel makes it normal.

> **[INSERT DIAGRAM: Complete architecture - internet users, Cloudflare DNS/edge, tunnel to home cluster, apps with no exposed ports]**

---

## The Problem With Port Forwarding

Before Cloudflare Tunnel, your choices were limited.

Option 1: Open a port on your router (e.g., 8080 → your cluster). Your home IP is now a target. Any vulnerability in your app becomes a direct attack vector. Your ISP can see the traffic. DDoS becomes a real threat.

Option 2: Keep everything private and lose public access. Your services stay internal.

Option 3: Use a VPN. Solves security but adds complexity. Every user needs a client. Every connection needs setup.

I wanted public services. But I didn't want to open ports or run a VPN.

Cloudflare Tunnel solved it.

> **[INSERT DIAGRAM: Port forwarding vs Cloudflare Tunnel comparison]**

---

## What Cloudflare Tunnel Actually Does

Cloudflare Tunnel is a daemon that runs inside your cluster. It opens an encrypted, outbound-only connection to Cloudflare's edge network.

That's the entire security model: outbound connection, never inbound.

> **[INSERT DIAGRAM: How Cloudflare Tunnel works - outbound connection, no exposed ports]**

Here's how it works:

Your application lives in a pod (Linkling, Audiobookshelf, whatever). Cloudflare Tunnel daemon also runs in the same cluster, usually in the same namespace as your app.

The daemon connects to Cloudflare's edge and says: "route traffic for audiobooks.rahatahsan.com to me."

Someone visits audiobooks.rahatahsan.com. They hit Cloudflare's servers. Cloudflare looks up the route and finds your tunnel. Cloudflare sends the request through the tunnel to your daemon. Your daemon forwards it to the app. The response travels back through the tunnel. HTTPS is handled automatically by Cloudflare.

No ports open. No inbound traffic. No exposure.

---

## The Setup

Each application gets its own Cloudflare Tunnel. Each tunnel is a separate Kubernetes deployment.

My tunnel deployment runs 2 replicas for high availability and includes strict security contexts (non-root user, dropped capabilities, no privilege escalation):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cloudflared
spec:
  selector:
    matchLabels:
      app: cloudflared
  replicas: 2
  template:
    metadata:
      labels:
        app: cloudflared
    spec:
      securityContext:
        runAsNonRoot: true
        seccompProfile:
          type: RuntimeDefault
      containers:
      - name: cloudflared
        image: cloudflare/cloudflared:latest
        args:
        - tunnel
        - --config
        - /etc/cloudflared/config/config.yaml
        - run
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop: ["ALL"]
          runAsNonRoot: true
          runAsUser: 65532
        livenessProbe:
          httpGet:
            path: /ready
            port: 2000
          failureThreshold: 1
          initialDelaySeconds: 10
          periodSeconds: 10
        volumeMounts:
        - name: config
          mountPath: /etc/cloudflared/config
          readOnly: true
        - name: creds
          mountPath: /etc/cloudflared/creds
          readOnly: true
      volumes:
      - name: creds
        secret:
          secretName: tunnel-credentials
      - name: config
        configMap:
          name: cloudflared
          items:
          - key: config.yaml
            path: config.yaml
```

The daemon runs with a config file, not just a token. The config file specifies which domains route where.

The tunnel credentials (the actual authentication to Cloudflare) are stored in a Kubernetes secret, encrypted with SOPS.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cloudflared
data:
  config.yaml: |
    tunnel: pi-zoro
    credentials-file: /etc/cloudflared/creds/credentials.json
    metrics: 0.0.0.0:2000
    no-autoupdate: true
    ingress:
    - hostname: rahatahsan.com
      service: http://linkding.linkding-prod.svc.cluster.local:9090
    - service: http_status:404
```

The config file is the routing brain. It says: "when someone requests links.rahatahsan.com, forward to the Linkding service in the linkding-prod namespace on port 9090. If the hostname doesn't match anything, return 404."

You can add multiple hostname rules. Each domain gets its own ingress entry. The tunnel daemon reads this config and knows exactly where to send traffic.

---

## The DNS Piece (The Gotcha)

Here's where most people get stuck.

You create the tunnel. You run the daemon. You test it locally. Nothing works.

The reason: you haven't told Cloudflare's DNS where your tunnel is.

This assumes you're using Cloudflare for DNS (not just the tunnel). If you're using another DNS provider, the process is slightly different, but the idea is the same.

You need to create a CNAME record in Cloudflare DNS pointing your domain to your tunnel's endpoint.

Example:

```
rahatahsan.com CNAME rahatahsan.com.cfargotunnel.com
```

That dot-cfargotunnel-dot-com suffix is Cloudflare's tunnel endpoint. It's the signal to Cloudflare: "this domain routes through a tunnel, not to a direct IP."

Once that record exists, requests for rahatahsan.com hit Cloudflare DNS, Cloudflare sees the tunnel CNAME, looks up your tunnel by ID (from the config file), and routes the request through.

No CNAME record? Requests go nowhere. You see nothing in your logs. It just fails silently.

It's simple once you understand it. But if you skip this step, you spend an hour debugging a problem that doesn't exist.

---

## Real Example: Linkling

Here's how Linkling (my bookmarks app) works end-to-end.

I create a Cloudflare Tunnel called "pi-zoro" (named after my control plane node). The tunnel credentials are stored in a Kubernetes secret, encrypted by SOPS.

I run the tunnel daemon as a deployment with 2 replicas (for high availability):

```bash
kubectl apply -f cloudflared-deployment.yaml
```

The daemon starts. It reads the config file. It sees that `rahatahsan.com` should route to the Linkding service. It connects to Cloudflare with the credentials. It says: "I'm online, ready for traffic from rahatahsan.com."

I add a DNS record in Cloudflare:

```
rahatahsan.com CNAME rahatahsan.com.cfargotunnel.com
```

> **[INSERT DIAGRAM: Full request flow from browser to app through tunnel]**

Now when someone visits rahatahsan.com:

1. Their browser resolves the domain via Cloudflare DNS
2. Cloudflare sees the CNAME pointing to the tunnel
3. Cloudflare looks up the tunnel by ID
4. The request is routed through the tunnel to one of my daemon replicas
5. The daemon reads the config, sees `rahatahsan.com → http://linkding.linkding-prod.svc.cluster.local:9090`
6. The daemon forwards the request to the Linkding service
7. Linkding responds
8. Response travels back through the tunnel
9. Cloudflare handles HTTPS (automatic, no cert management needed)
10. Browser gets HTTPS response

No port forwarding. No open ports. No exposure. Two replicas mean if one daemon pod crashes, the other keeps serving traffic.

---

## Why This Beats Everything Else

Port forwarding exposes your home IP and invites attack.

A VPN works but requires setup on every client. Every connection needs configuration. It's friction.

Cloudflare Tunnel is set-and-forget. Once it's deployed, it runs. New traffic just works. HTTPS is automatic. You never worry about certificate renewal because Cloudflare handles it.

More importantly: there's no exposed surface. Your router has no open ports. Your home IP is not public. An attacker can't probe your network looking for vulnerabilities.

The only way to reach your services is through Cloudflare. If Cloudflare is compromised, that's a different problem. But your home network is not a target.

---

## The Mental Model

Cloudflare Tunnel inverts the network security model.

Traditional: your server sits on the internet. Firewall blocks bad requests. Open ports = surface area for attacks.

Cloudflare Tunnel: your server sits behind your home network. No ports open. Your server reaches out to Cloudflare. Cloudflare forwards legitimate traffic back through the tunnel. All traffic flows through Cloudflare's infrastructure first.

It's zero-trust networking: don't trust the network perimeter, assume no ports are open, trust the encrypted tunnel instead.

---

## Public vs Private

Not everything needs to be public.

Grafana (monitoring) is LAN-only. No CNAME record. No tunnel. It sits behind my internal network and I access it via other means when needed.

My bookmarks and media apps are public. They have CNAME records. They have tunnel daemons. They're accessible from anywhere with HTTPS.

The choice is per-service. Some things are public. Some things stay private. You control the boundary.

---

## The Lesson That Stuck

Security often feels like it costs something. Open ports feel necessary. VPNs feel like complexity. Certificates feel like overhead.

Cloudflare Tunnel taught me that security can actually be simpler than the alternative.

No port forwarding means I never have to think about home IP exposure. No VPN means I never have to manage client configs. No certificate management means I never have to debug renewal failures.

Simpler and more secure. That's the win.

---

*This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to DevOps engineer.*

💼 LinkedIn: [Rahat Ahsan](https://linkedin.com/in/rahatahsan)
🐦 X: [@RahatAhsan20](https://x.com/RahatAhsan20)
🐙 GitHub: [AhsanRahat12](https://github.com/AhsanRahat12)
