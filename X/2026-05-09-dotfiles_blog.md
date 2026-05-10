# Dotfiles Blog — X Thread Promo

---

**1/**
Every time I SSH'd into my Pis, something was different.

Missing tools. Different configs. That "oh I don't have that here" moment over and over.

So I automated my entire dev setup. New blog post. Here is the summary 🧵

---

**2/**
Started with GNU Stow. Felt like organised manual work, not real automation.

Then I found chezmoi. Scripts that run on changes. Templates that adapt to architecture. One repo, one command, every machine identical.

---

**3/**
Added mise for tool management. Define tools in a config file, mise installs them.

Chezmoi downloads mise automatically. A script detects tool list changes and runs mise install. Zero manual installation across machines.

---

**4/**
Built a two-tier system from the start:

Personal global tools (chezmoi, bat, neovim) → dotfiles repo, follows me everywhere.

Project tools (kubectl, helm) → mise.toml in the project root, inside a DevContainer via DevPod.

---

**5/**
One command on a fresh Raspberry Pi:

chezmoi installed. Repo cloned. Configs applied. Mise downloaded. Tools installed. Everything synced.

Full breakdown on Medium: [LINK]
