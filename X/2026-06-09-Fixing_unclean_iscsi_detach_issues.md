# Post 3 — X (Twitter) Thread

**Topic:** Journal Replay is NOT Filesystem Repair
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 8**

🧵 My app started cleanly after a storage failure.

I thought it was fixed.

Six hours later it crashed again with the exact same error.

Here is what I missed:

---

**Tweet 2 / 8**

After an unclean iSCSI detach, my ext4 filesystem came back dirty.

On remount, the kernel automatically ran journal replay. The app started. No errors.

I closed my laptop.

---

**Tweet 3 / 8**

Journal replay does one specific thing.

It replays in-flight write transactions to restore consistency after an unclean shutdown.

That is it.

---

**Tweet 4 / 8**

What journal replay does NOT do:

Check filesystem metadata.
Validate inode tables and block allocation bitmaps.
Fix corruption that exists outside the journal itself.

---

**Tweet 5 / 8**

The corruption in my case was in the filesystem metadata, not the journal.

Journal replay cleaned the journal. The filesystem mounted read-write. Everything looked healthy.

Until writes hit the corrupted metadata six hours later.

---

**Tweet 6 / 8**

The only fix was fsck.

A full filesystem check that validates everything journal replay ignores.

Not glamorous. But it is the only thing that actually repairs the filesystem.

---

**Tweet 7 / 8**

The lesson: an apparent recovery is not a fix.

Always identify the actual mechanism behind a recovery before you trust it.

If you cannot explain why it is working, you cannot trust that it will keep working.

---

**Tweet 8 / 8**

Have you ever had a system appear to recover on its own, only to break again later?

Full series on storage failures and what they actually teach you.

Follow if this resonates.

---

## Metadata

- **Source:** Audiobookshelf Incident / June 2026 Incident Report
- **Pillar:** Storage is where Kubernetes breaks
- **Tier:** Elite
- **Target audience:** DevOps Engineers, SREs, Platform Engineers
