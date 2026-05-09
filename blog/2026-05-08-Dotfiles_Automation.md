# I Automated My Entire Dev Setup. One Command, Any Machine.

> **[INSERT GRAPHIC: Introduction — Before/After graphic]**

Every time I SSH'd into one of my Raspberry Pis, something was different. Missing aliases. Different shell configs. A tool I use every day on one machine that didn't exist on the other. That quiet "oh, I don't have that here" moment over and over again.

It sounds small. It isn't. When you're managing a Kubernetes cluster across multiple nodes, inconsistency isn't just annoying — it's friction that slows you down at the worst moments.

I decided to fix it properly.

## The First Attempt: GNU Stow

I started with GNU Stow. It's a common recommendation for managing dotfiles, and I can see why — symlink your configs into place and you're done.

But it never felt like automation. It felt like organised manual work. I was still the one making sure everything was set up right on each machine. There was no awareness of architecture. No scripts that could run differently based on the OS or the hardware. No lifecycle — just symlinks.

It worked, technically. But it wasn't what I wanted.

## Finding Chezmoi

Then I found chezmoi, and it clicked immediately.

Chezmoi doesn't just place files. It manages them. It has a source of truth — a Git repo — and everything flows from there. But the thing that sold me was the scripting.

I can write scripts that run automatically when something changes. I can template configs based on the architecture — so the same repo works on my ARM-based Pis and my x86 laptop without any manual adjustment. I can have scripts that behave differently depending on the OS. One repo, one command, every machine ends up in the same state.

The moment I ran `chezmoi apply` on one of my Pis and watched my entire environment appear — bashrc, vimrc, tools, everything — in one command, I knew Stow was never coming back.

> **[INSERT GRAPHIC: Chezmoi sync flow — GitHub → chezmoi update → Machine matches repo]**

## Adding Mise for Tool Management

Chezmoi handles configs. But what about the actual tools?

That's where mise came in. Mise manages tool versions and binaries — think of it as a universal version manager. I define the tools I want in a config file, and mise installs them.

The integration with chezmoi is clean. Chezmoi's externals download the mise binary automatically on any machine. A chezmoi script detects when the tool list changes and runs `mise install`. So when I add a new tool to my global config and push to GitHub, every machine picks it up on the next `chezmoi update`.

No manual installation. No "did I install that on this machine?" No version mismatches.

## The Two-Tier System

Here's where it gets intentional.

From the start, I wanted two separate layers of tooling:

**Personal global tools** — things I want everywhere, regardless of what project I'm working on. Chezmoi, bat, neovim. These live in my dotfiles repo and follow me to every machine.

**Project-specific tools** — things a whole team needs, but only inside a specific project directory. kubectl, helm, terraform. These live in a `mise.toml` in the project root.

> **[INSERT GRAPHIC: Two-tier tooling system — Personal global vs Project scoped]**

The separation matters because I wanted a portable workflow. Project tools live inside a DevContainer powered by DevPod. When the container starts, mise installs the project tools. Then chezmoi applies my personal dotfiles on top. When the container is removed, it's gone — nothing installed on the host, nothing left behind, no mess.

It's like a sealed workspace with everything I need inside it, and my personal preferences layered on top. The idea for this portable workflow came from the KubeCraft internship program, and it changed how I think about development environments entirely.

## The Container Boot Flow

When I spin up a DevContainer, this is what actually happens:

The Dockerfile builds with the mise binary available. The post-create script runs `mise trust` and `mise install`, which installs all project-scoped tools. Then DevPod clones my dotfiles repo and runs the chezmoi setup script, which applies my personal configs and installs my global tools. By the time the shell starts, everything is on PATH and ready.

Two repos. Two layers. One seamless environment.

> **[INSERT GRAPHIC: DevContainer boot flow — 3 steps to ready]**

## The Payoff

The first real test was on my Raspberry Pi. One command:

```
sh -c "$(curl -fsLS get.chezmoi.io)" -- -b $HOME/.local/bin init --apply git@github.com:AhsanRahat12/dotfiles.git
```

Chezmoi installed. Repo cloned. Configs applied. Mise downloaded. Tools installed. Everything synced. One command.

That was the moment it stopped being a project and started being infrastructure I rely on.

Now when I SSH into either Pi, the environment is identical. When I spin up a DevContainer, my tools are there. When I add something new, I push it once and every machine catches up with `chezmoi update`.

The direction is always the same: GitHub is the source of truth, chezmoi pulls it down, the machine matches the repo.

## What I'd Tell Anyone Managing Dotfiles

Start with whatever works. GNU Stow is fine for learning the concept. But if you want real automation — scripts that run on changes, templates that adapt to architecture, a system that bootstraps a fresh machine from nothing — chezmoi is worth the investment.

And if you're managing tools across machines or inside containers, mise pairs with it perfectly. The two together give you a setup that is genuinely portable and genuinely reproducible.

Both repos are on my GitHub:

- [dotfiles](https://github.com/AhsanRahat12/dotfiles) — personal global tooling and configs
- [DevContainer](https://github.com/AhsanRahat12/DevContainer) — project-specific environment setup

---

This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to full-time DevOps engineer.

💼 LinkedIn: [Rahat Ahsan](https://linkedin.com/in/rahatahsan)
🐦 X: [@RahatAhsan20](https://x.com/RahatAhsan20)
🐙 GitHub: [AhsanRahat12](https://github.com/AhsanRahat12)
