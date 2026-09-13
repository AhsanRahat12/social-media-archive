1/ One crash-looping pod corrupted six volumes on my K8s cluster.

2/ The bug was harmless. Flux restarting it 100x wasn't. It overloaded iscsid and dropped other apps' iSCSI sessions mid-write.

3/ I wrote the recovery playbook so you don't have to learn this the hard way:

[ARTICLE LINK]
