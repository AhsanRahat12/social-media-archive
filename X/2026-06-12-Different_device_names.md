# Post 6 — X (Twitter) Thread

**Topic:** Device Letters Lie. Always Identify Storage by UUID.
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 7**

🧵 This morning the corrupted disk was /dev/sdc.

By that evening it was /dev/sdg.

Same disk. Same data. Different name.

---

**Tweet 2 / 7**

This is how iSCSI works and it will catch you off guard if you don't know to expect it.

Every time an iSCSI session reconnects, the kernel can assign a different device letter. Not stable across reboots, reconnects, or some routine operations.

---

**Tweet 3 / 7**

If you write down "the corrupted disk is sdc" and come back later, you might run fsck against a completely different disk.

Best case, the command does nothing useful.

Worst case, you run a destructive operation on a disk actively in use by something else.

---

**Tweet 4 / 7**

The fix: never reference storage by device letter. Use the UUID.

sudo fsck.ext4 -y /dev/disk/by-uuid/827b7df8-0017-4239-b60d-fcfbc21e5fca

---

**Tweet 5 / 7**

The UUID is generated when the filesystem is created and does not change.

Survives reboots, reconnects, device letter reshuffling. Same disk every time, no matter what the kernel calls it this session.

---

**Tweet 6 / 7**

Find your UUIDs with:

ls -la /dev/disk/by-uuid/

or

blkid

---

**Tweet 7 / 7**

This applies to anything where storage can be reattached dynamically. iSCSI, USB, CSI drivers that reprovision on the fly.

If the device name can change between writing the command and running it, the device name is not safe to use.

Small habit. Saves you from a very bad day.

---

## Metadata

- **Source:** ABS Incident and Grafana Incident / June 2026 Incident Report
- **Pillar:** Storage is where Kubernetes breaks
- **Target audience:** DevOps Engineers, SREs, Linux Administrators
