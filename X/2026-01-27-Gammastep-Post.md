Running Hyprland and need automatic blue light filtering?

Here's my setup for eye-friendly late-night sessions 🧵

---

**The Problem:**

No built-in blue light filter in Hyprland. Manual brightness adjustments weren't cutting it for late-night study sessions.

---

**The Solution:**

Gammastep - auto-adjusts screen color temperature based on location and time.

Wayland-compatible. Set it and forget it.

---

**My Setup:**

1. Installed: `sudo pacman -S gammastep`
2. Tested temps: 6500K day, 3500K night
3. Added to hyprland.conf: `exec-once = gammastep -l 49.8:-99.9 -t 6500:3500`

Done. Smooth automatic transitions all day.

---

**Result:**

Zero manual work. Automatic warm tint at night. Way more comfortable for extended coding sessions.

Pro tip: Blue light filter ≠ brightness dimming. Use brightnessctl if you need actual dimming.

---

Using automated blue light filtering on your Wayland setup? What temps work for you?

#Linux #DevOps #ArchLinux #Hyprland
