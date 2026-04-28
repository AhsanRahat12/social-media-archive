# Post — Full Request Path: Cloudflare + Traefik (LinkedIn)

---

One of the best things about running a home lab is that you get to make real architectural decisions.

Nobody tells you what to use. You research, evaluate, and choose. And the choices you make force you to understand things at a level you never would have otherwise.

One decision I made early on was to use Cloudflare Tunnel to expose my apps externally.

That one choice taught me more about how requests and packets actually travel across a network than anything else I have studied. Not because I read about it. Because I had to make it work and debug it when it did not.

Here is what I now understand end to end:

Requests from the internet hit Cloudflare DNS. Cloudflare forwards them into my cluster through the tunnel. Once inside, Traefik reads the Ingress rules and routes the request to the right pod.

Every layer has its own logs. Every layer has its own failure modes. Knowing the full path means knowing exactly where to look when something breaks.

Before I built this I could describe networking concepts from a textbook. After building it I understand them from experience. There is a big difference between the two.

The home lab did not just give me a place to run apps. It gave me a reason to actually understand the infrastructure underneath them.

If you want to see how I implemented this in my cluster, the full repo is here:
github.com/AhsanRahat12/Homelab

What architectural decision in your home lab taught you the most? 👇

Follow me, I am documenting everything I build and learn in my home lab.

`#Kubernetes` `#DevOps` `#Networking` `#CloudNative` `#Homelab`
