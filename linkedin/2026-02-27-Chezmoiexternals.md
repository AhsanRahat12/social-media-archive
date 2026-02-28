# Post 5 — chezmoi Externals Deep-Dive (LinkedIn)

Most people make their dev environment portable. Few make the tools that manage their environment portable too.

That's the gap chezmoi externals closes.

Here's the problem it solves:

1. mise manages all your tool binaries inside a container built from your Dockerfile.
2. On a fresh bare metal machine, Docker doesn't exist yet. Neither does mise.
3. Without mise, nothing installs. Without chezmoi externals, mise never arrives.

chezmoi externals fixes this with a single config file that downloads the mise binary directly to ~/.local/bin/ on any machine, no sudo required, no Docker needed.

The result? Your full personal environment bootstraps identically on a brand new laptop or inside a container.

Portability isn't just about your configs. It's about the tools that manage your configs too.

Full setup on my GitHub: github.com/AhsanRahat12

Have you ever had a hidden dependency break your environment setup on a fresh machine? 👇

#DevOps #Linux #CloudNative #KubeCraft #LearningInPublic
