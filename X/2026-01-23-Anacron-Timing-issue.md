# Anacron Timing Issue - X (Twitter) Post

## Metadata
- **Date Created:** 2026-01-23
- **Platform:** X (Twitter)
- **Topic:** Anacron Troubleshooting, Timing Configuration
- **Character Count:** ~470 characters
- **Status:** Ready to Post

---

## Post Content

My anacron job was 3 days overdue but never ran.

The culprit? A timing mismatch I didn't expect.

My work hours: 7pm - midnight
Anacron's START_HOURS_RANGE: 3am - 10pm

When anacron checked at 12:01am, my system was active but outside the allowed window. Logs showed: "Normal exit (0 jobs run)"

**The fix:**
Changed START_HOURS_RANGE to 19-23 (7pm - 11pm)

**Lesson:**
Anacron only runs during its configured hours. If your computer is only on outside that window, jobs will NEVER execute even when due.

Ever had cron/anacron silently fail on you? 

#Linux #DevOps #Troubleshooting

---
