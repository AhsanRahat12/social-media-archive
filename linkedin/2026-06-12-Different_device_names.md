# Post 6 — LinkedIn

**Topic:** Device Letters Lie. Always Identify Storage by UUID.
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

This morning the corrupted disk was /dev/sdc.

By that evening it was /dev/sdg.

Same disk. Same data. Different name.

This is how iSCSI works and it will catch you off guard if you don't know to expect it.

Every time an iSCSI session reconnects, the kernel can assign a different device letter. /dev/sdc, /dev/sdd, /dev/sdg, whatever is next in line. The assignment is not stable across reboots, reconnects, or even some routine operations.

If you write down "the corrupted disk is sdc" and come back to it later, you might be running fsck against a completely different disk. Best case, the command does nothing useful. Worst case, you run a destructive operation against a disk that is actively in use by something else.

The fix is simple: never reference storage by device letter. Use the UUID.

```
sudo fsck.ext4 -y /dev/disk/by-uuid/827b7df8-0017-4239-b60d-fcfbc21e5fca
```

The UUID is generated when the filesystem is created and does not change. It survives reboots, reconnects, and device letter reshuffling. It points to the same disk every time, no matter what the kernel decides to call it this session.

Find your UUIDs with:

```
ls -la /dev/disk/by-uuid/
```

or

```
blkid
```

This applies to any system where storage can be reattached dynamically. iSCSI, USB drives, anything backed by a CSI driver that reprovisions on the fly. If the device name can change between when you write the command and when you run it, the device name is not safe to use.

Small habit. Saves you from a very bad day.

What naming convention do you trust for identifying storage in your environment?

Follow for the series on storage problems nobody warns you about.

#Linux #Kubernetes #DevOps #Infrastructure #SRE

---

## Metadata

- **Source:** ABS Incident and Grafana Incident / June 2026 Incident Report
- **Pillar:** Storage is where Kubernetes breaks
- **Target audience:** DevOps Engineers, SREs, Linux Administrators
