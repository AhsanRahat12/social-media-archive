# WiFi Troubleshooting Thread - Arch Linux

**Platform:** X (Twitter)  
**Date:** January 24, 2026  
**Format:** Thread (6 tweets)  
**Topic:** Linux Troubleshooting, Network Issues, systemd  
**Hashtags:** #Linux #DevOps #ArchLinux

---

**Tweet 1 (Hook):**
My Arch Linux WiFi kept dying randomly. Then it completely failed.

20 minutes of log analysis revealed something I didn't expect 🧵

---

**Tweet 2 (Problem):**
Random disconnects all day: 19:20, 19:24, 19:35, 20:38, 20:54, 21:39

At 23:04, it died completely. No reconnection.

Restart fixed it temporarily, but I needed the root cause.

---

**Tweet 3 (Investigation):**
Checked previous boot logs:
```bash
journalctl -u systemd-networkd -b -1
```

Pattern: WiFi power management causing brief disconnects. Eventually, one failed to recover.

---

**Tweet 4 (Root Cause):**
Confirmed with:
```bash
iw dev wlan0 get power_save
```

Power save: ON ← there's the culprit

---

**Tweet 5 (Solution):**
Permanent fix: Created systemd service to run on boot:
```bash
iw dev wlan0 set power_save off
```

Trade-off: 5-10 min battery life for stable WiFi

Worth it for DevOps work 💯

---

**Tweet 6 (Lesson + CTA):**
Lesson: Logs reveal patterns you don't see in real-time.

Don't just restart. Investigate.

Ever found a hidden pattern in your logs? Reply with what you discovered 👇

#Linux #DevOps #ArchLinux
