# Dotfiles Blog — LinkedIn Promo Post

---

Every time I SSH'd into one of my Raspberry Pis, something was different. Missing aliases. Missing tools. A different shell config on every machine.

It sounds small. It isn't. When you're managing a Kubernetes cluster across multiple nodes, inconsistency is friction that slows you down at the worst moments.

So I automated my entire dev setup. Here is what I built:

1. chezmoi manages all my dotfiles from a single Git repo. One command bootstraps any machine.
2. mise handles tool versions and binaries. When the tool list changes, every machine catches up automatically.
3. A two-tier system separates personal global tools from project-specific tools inside DevContainers powered by DevPod.

The result: one command on a fresh Raspberry Pi and my entire environment appears. bashrc, vimrc, tools, configs. Everything identical. Every time.

I wrote the full breakdown on Medium. Link in the comments 👇

#DevOps #Linux #HomeLab #Kubernetes #CloudNative
