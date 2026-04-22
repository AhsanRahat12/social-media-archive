# Post — Kustomize Base + Overlay (LinkedIn)

---

When I first started my home lab I had one environment. Everything ran in the same place. It worked until I wanted to test something new without breaking what was already running.

Then I learned about how enterprises use different environment settings for different stages of development. So I applied the same pattern in my home lab.

Base holds the bare bones of what the app needs to run. The deployment and the core config. Just the foundation, nothing environment specific.

Staging and production each have their own namespace. That is how I keep them separated.

The problem was I was still manually copying everything from base into each environment and then adding things on top. Any change to base meant updating everything everywhere. It got messy fast.

That is where Kustomize patches came in.

Instead of copying the base manifest, the overlay just points to base and patches only what needs to differ. The namespace, a specific value, an image tag. Everything else is inherited automatically.

Staging became where I test everything before it goes live. New features, volume migrations, database updates. If something breaks it breaks there, not in production.

Production is the final implementation. Only what has been tested and validated makes it here.

That one pattern saved me from chasing down inconsistencies between environments and wondering which version of a config was actually running where.

The structure sounds like overhead when you first set it up. After the first time it saves you from a mistake you would have made, you never question it again.

Want to see how I structured this in practice? The full repo is here:
github.com/AhsanRahat12/Homelab

Are you running environment separation in your setup or are you still on a single environment? 👇

Follow me, I am documenting everything I build and learn in my home lab.

`#Kubernetes` `#DevOps` `#GitOps` `#CloudNative` `#Homelab`
