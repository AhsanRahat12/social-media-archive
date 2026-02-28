# Post 5 — chezmoi Externals Deep-Dive (X / Twitter)

Most people make their configs portable. Few make the tools that manage their configs portable too.

That's the gap chezmoi externals closes.

The problem:

1. mise manages your tools inside a Docker container.
2. On a fresh bare metal machine, Docker doesn't exist yet. Neither does mise.
3. Without mise, nothing installs.

One config file tells chezmoi to download mise directly to ~/.local/bin/ on any machine. No sudo. No Docker. No manual steps.

Portability isn't just about your configs. It's about the tools that manage them too.

Full setup on my GitHub: github.com/AhsanRahat12

Ever had a hidden dependency break your setup on a fresh machine? 👇

#DevOps #Linux #CloudNative
