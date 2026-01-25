# WiFi Troubleshooting - Arch Linux

**Platform:** LinkedIn  
**Date:** January 24, 2026  
**Topic:** Linux Troubleshooting, Network Issues, systemd  
**Hashtags:** #Linux #DevOps #ArchLinux #Troubleshooting #SystemAdministration

---

My WiFi kept disconnecting on Arch Linux. Then it died completely.

Here's what I learned from 20 minutes of troubleshooting that changed how I approach network issues:

**The Problem:**
Random disconnects throughout the day, then complete failure. Browser showed "No internet," and my laptop refused to reconnect.

**The Investigation:**
Instead of just restarting (which temporarily worked), I dug into the logs:
```bash
journalctl -u systemd-networkd -b -1
```
This checks systemd-networkd logs from the previous boot session.

Pattern revealed: Disconnects at 19:20, 19:24, 19:35, 20:38, 20:54, 21:39, and finally 23:04 (no recovery).

**The Root Cause:**
WiFi power management. One command confirmed it:
```bash
iw dev wlan0 get power_save
```
This checks if power saving is enabled on the wireless interface.

Result: Power save ON.

**The Permanent Fix:**
Created a systemd service to disable power save on boot. The service runs this command automatically:
```bash
iw dev wlan0 set power_save off
```
This disables WiFi power management, preventing sleep-related disconnects.

**Results:**
✅ No more random disconnects
✅ Stable connection for DevOps work
✅ Worth the 5-10 min battery trade-off

Logs don't just show what failed. They reveal patterns you didn't know existed.

Have you ever solved a recurring tech issue by analyzing logs instead of just restarting? What did you find?

Follow me for real troubleshooting from my Linux journey.

#Linux #DevOps #ArchLinux #Troubleshooting #SystemAdministration
