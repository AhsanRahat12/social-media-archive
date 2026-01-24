# Anacron Timing Issue - LinkedIn Post

## Metadata
- **Date Created:** 2026-01-23
- **Platform:** LinkedIn
- **Topic:** Anacron Troubleshooting, Timing Configuration
- **Status:** Ready to Post

---

## Post Content

My Anacron job was 3 days overdue. The timestamp confirmed it should have run. But it never did.

Here's what I discovered while troubleshooting this frustrating issue:

**The Problem:**
Configured a job to run every 3 days for system update checks. Everything looked correct in /etc/anacrontab. The timestamp showed the job was due. Yet, nothing happened.

**My Troubleshooting Process:**

1. Verified anacron installation and cronie service (both active)
2. Checked anacrontab configuration (syntax looked correct)
3. Examined job timestamps in /var/spool/anacron/ (confirmed job was DUE)
4. Analyzed anacron logs with journalctl

**The Critical Discovery:**
The logs showed: "Normal exit (0 jobs run)" at 12:01am.

**Root Cause:**
Time window mismatch. My work hours are 7pm to midnight. Anacron's START_HOURS_RANGE was set to 3am to 10pm. When anacron checked at 12:01am, I was actively using my system, but it was outside the allowed execution window.

**The Solution:**
Changed START_HOURS_RANGE from 3-22 to 19-23 (7pm to 11pm) to match my actual computer usage hours.

**Key Lesson:**
Anacron only executes jobs during START_HOURS_RANGE. Even if your job is overdue, it will never run if your computer is only powered on outside this configured window.

Have you encountered similar timing issues with cron or anacron? What troubleshooting approaches worked for you? Share your experiences 👇

Follow me for practical Linux troubleshooting and DevOps insights.

#Linux #DevOps #Troubleshooting #SystemAdministration #Anacron

---

