Running Hyprland and looking for automatic blue light filtering?

Here's how I set up eye-friendly color temperature adjustments on my Arch Linux setup.

**The Problem:**

My Hyprland environment had no built-in blue light filter. Late-night study sessions needed better eye protection than manual brightness adjustments.

**The Solution:**

Gammastep automatically adjusts screen color temperature based on your location and time of day.

**My Setup:**

✅ Installed gammastep via pacman
✅ Tested temperatures (settled on 6500K day, 3500K night)
✅ Added `exec-once = gammastep -l 49.8:-99.9 -t 6500:3500` to hyprland.conf
✅ Got smooth automatic transitions with zero manual intervention

**The Result:**

Automatic warm tint during evening sessions. Noticeably more comfortable for extended work.

Quick note: Blue light filters change color temperature while keeping brightness. If you need actual dimming, use brightnessctl instead.

Are you using automated blue light filtering on your Wayland setup? What temperature settings work best for you? 👇

Follow me for practical Linux and DevOps insights.

#Linux #DevOps #ArchLinux #Hyprland #TechTips
