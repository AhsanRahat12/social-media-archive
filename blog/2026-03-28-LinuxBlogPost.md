# Why I Ditched Windows and Mac for Arch Linux — and What It Taught Me About DevOps

I'll be honest — I didn't plan to go this deep.

I just wanted to learn DevOps. But somewhere between setting up my first server and joining the KubeCraft internship program, a group of engineers pointed me toward a rabbit hole I'm still happily falling through. That rabbit hole was Linux.

---

## Why Linux in the First Place

If you're learning DevOps, Linux isn't optional — it's the foundation. Most servers run Linux. Most of the tooling you'll use as a DevOps engineer lives natively on Linux. Docker, Kubernetes, shell scripting, networking — all of it makes more sense when Linux is your daily environment, not just something you SSH into occasionally.

So I made the switch. Not just as a learning exercise — as my daily driver.

---

## Starting With Ubuntu

I started where most people start — Ubuntu. Clean, stable, beginner friendly. It got me comfortable with the terminal, with package managers, with navigating a filesystem without a GUI holding my hand.

Four months in, I felt like I had a handle on it. I could get things done. But I kept feeling like I was still just using a system someone else had configured for me. I wanted to go deeper.

So I installed Arch.

---

## Why Arch?

Arch Linux is not beginner Linux. There's no hand-holding — you build it from scratch using the **Arch Linux wiki** as your guide. Partitioning the drive, setting up the bootloader, configuring everything piece by piece.

It's slow and frustrating, especially the first time. But that's kind of the point.

I didn't want an operating system that abstracted everything away. I wanted to understand what was underneath. Arch doesn't give you a choice — you either figure it out or it doesn't work.

I documented my entire setup on GitHub if you want to see how it came together: [arch-linux-setup](https://github.com/AhsanRahat12/arch-linux-setup)

---

## The Rabbit Hole Gets Real

The hardest part wasn't the installation — it was everything after it.

I installed **Hyprland**, a tiling window manager, and suddenly my entire relationship with my computer changed. No more clicking around. Everything runs on keybindings — switching workspaces, opening terminals, moving windows — all configured by me, for me.

![Tiled terminals screenshot](screenshot-tiled-terminals.png)

The productivity gain was real. But getting there meant learning things I'd never thought about before.

Things like:

- Installing **brightnessctl** just to be able to adjust my screen brightness — something that was always just a key press on Windows or Mac, handled invisibly by the OS
- Finding and configuring a screen dimming app that automatically reduces eye strain when it gets dark outside
- Writing **shell scripts to randomize my wallpaper** — something so small, but something that made me actually understand how scripts interact with the system

Every single one of these felt like pulling back a curtain. Things that Windows and Mac had always silently handled for me suddenly had a name, a package, a config file. Nothing was magic anymore. Everything was understandable — and that was addictive.

---

## How KubeCraft Pushed Me Deeper

I wouldn't have gone this far alone. At KubeCraft I was surrounded by engineers who lived and breathed Linux — people who got excited talking about dotfiles and window managers and shell configurations the same way I was starting to.

That environment matters more than people realize. When the people around you treat deep Linux knowledge as normal and valuable, you stop feeling like you're overcomplicating things and start feeling like you're building real foundations.

They pointed me into the rabbit hole. I jumped in willingly.

---

## What Linux Taught Me About DevOps

Learning Linux this way — really learning it, not just using it — changed how I think about systems entirely.

When you've manually configured every component of your operating system, when you've written scripts to automate your own environment, when you've debugged why your brightness keys stopped working after an update — you stop being intimidated by infrastructure. You start seeing it as just more systems to understand.

![Vim and tmux screenshot](screenshot-vim-tmux.png)

That mindset is everything in DevOps. Servers, pipelines, containers — they're all just systems. And systems can be understood, configured, and fixed.

Arch Linux taught me to stop taking systems for granted. That lesson goes way beyond the desktop.

---

## Should You Try It?

If you're learning DevOps — yes, get comfortable with Linux. Ubuntu is a great place to start. You don't need to jump straight to Arch.

But if you're someone who loves understanding how things work under the hood, who gets a quiet satisfaction from configuring something exactly the way you want it — Arch is worth the patience. The things it teaches you can't be learned any other way.

Start with Ubuntu. Get comfortable. Then, when you're ready to go deeper — you'll know.

---

## Follow the Journey

This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to full-time DevOps engineer. If you're on a similar road, or just curious what this kind of learning actually looks like day to day, come follow along:

- 💼 **LinkedIn:** [Rahat Ahsan](https://www.linkedin.com/in/rahatahsan/)
- 🐦 **X:** [@RahatAhsan20](https://x.com/RahatAhsan20)
- 🐙 **GitHub:** [AhsanRahat12](https://github.com/AhsanRahat12)

---

*Data analyst at Green World. DevOps practitioner in progress. Arch Linux user. Currently in the KubeCraft internship program.*
