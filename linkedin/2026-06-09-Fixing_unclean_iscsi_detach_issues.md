# Post 3 — LinkedIn

**Topic:** Journal Replay is NOT Filesystem Repair
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

My app started cleanly after a storage failure. I thought it was fixed.

Six hours later it crashed again with the exact same error.

Here is what I missed.

After an unclean iSCSI detach, my ext4 filesystem came back dirty. When the volume remounted, the kernel automatically ran journal replay and the app started without errors. I closed my laptop thinking the problem was gone.

It was not.

Journal replay does one specific thing: it replays in-flight write transactions to restore consistency after an unclean shutdown. That is it.

What journal replay does NOT do:

1. Check or repair filesystem metadata
2. Validate inode tables and block allocation bitmaps
3. Fix corruption that exists outside the journal itself

The corruption in my case was in the filesystem metadata, not the journal. Journal replay cleaned up the journal, the filesystem mounted read-write, and everything looked healthy.

Until writes started hitting the corrupted metadata area six hours later. Then EIO errors came back and the app crashed again.

The only fix was fsck. A full filesystem check that validates everything journal replay ignores.

The lesson: an apparent recovery is not a fix. Always identify the actual mechanism behind a recovery before you trust it. If you cannot explain why it is working, you cannot trust that it will keep working.

Have you ever had a system appear to recover on its own, only to break again later?

Follow for the full series on storage failures and what they actually teach you.

#Kubernetes #Linux #DevOps #Infrastructure #SRE

---

## Metadata

- **Source:** Audiobookshelf Incident / June 2026 Incident Report
- **Pillar:** Storage is where Kubernetes breaks
- **Tier:** Elite
- **Target audience:** DevOps Engineers, SREs, Platform Engineers
