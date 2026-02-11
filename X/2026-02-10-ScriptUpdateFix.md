# Twitter Thread: Automated Update Checker Fix

## TWEET 1
Your automated update checker says "no updates available."

But manual checks show updates everywhere.

Here's the subtle bug I spent hours tracking down 🧵

---

## TWEET 2
My check-updates.sh script was running `pacman -Qu` to find updates.

But it was checking against a stale package database.

When I ran `sudo pacman -Syu` manually, the `-y` flag synced the database first. That's why updates would "magically" appear.

---

## TWEET 3
The fix was simple but critical:

✅ Added `sudo pacman -Sy` before checking
✅ Configured passwordless sudo for the sync command
✅ Tested and caught 2 firmware updates (including a Secure Boot patch)

Now it runs every 3 days via anacron with desktop notifications.

---

## TWEET 4
The lesson: Always verify your automation works under real conditions, not just in theory.

What's a subtle automation bug you've discovered?

#Linux #DevOps #Automation #ArchLinux
