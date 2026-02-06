# X/Twitter Thread - Rancher Desktop Polkit Authentication Issue

## Tweet 1 (Hook)

Rancher Desktop wouldn't accept my password on Arch Linux.

The issue? My window manager was missing a critical authentication component.

Here's what I learned debugging this 🧵

---

## Tweet 2 (Problem)

I ran: `rdctl set --application.admin-access=true`

Expected: Password prompt
Got: Silent failure

No prompt. No error. Nothing.

---

## Tweet 3 (Root Cause)

Hyprland (minimal Wayland compositor) doesn't include a polkit authentication agent by default.

Polkit daemon was running, but without an agent, there's no way to display password prompts.

---

## Tweet 4 (Solution)

The fix:

Add to ~/.config/hypr/hyprland.conf:

exec-once = /usr/lib/polkit-kde-authentication-agent-1

Reload Hyprland, run the command again.

Password prompt appeared. Problem solved.

---

## Tweet 5 (Key Lesson)

Desktop environments (GNOME/KDE) handle auth agents automatically.

Minimal window managers require manual setup of system services: polkit, notifications, network managers, screenshots, etc.

Trade-off: control vs. convenience.

---

## Tweet 6 (Engagement)

What system service caught you off guard when running a minimal Linux setup?

#Linux #DevOps #ArchLinux
