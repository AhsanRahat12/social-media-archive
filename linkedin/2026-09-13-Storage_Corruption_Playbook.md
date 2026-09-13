One crash-looping pod corrupted six volumes across two nodes.

The bug was harmless. A wrong UID on a volume mount. But Flux kept restarting the pod, which overloaded iscsid, the one daemon managing every iSCSI session on the node. It dropped other apps' sessions mid-write. Six ext4 journals, corrupted at once.

Three incidents later, I have a written playbook. The rules that earned their place:

1. Suspend every reconciler before debugging anything that touches a volume.
2. A running pod is not a healthy pod. Verify the data, not the status.
3. Never fsck a mounted filesystem.

Full article with the failure chain and the 10-step recovery procedure: [ARTICLE LINK]

Has a "harmless" bug ever cascaded on you? Tell me how you traced it 👇
