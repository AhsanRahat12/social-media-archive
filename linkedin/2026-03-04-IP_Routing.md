Every time data crosses the internet, two completely different addressing systems are working together. Most people only know about one of them.

Layer 2 is great at moving data between devices on the same local network. But it hits a hard wall the moment you need to reach a device on a different network. MAC addresses mean nothing outside your local segment. A completely different system has to take over.

That system is IP. And this is where routing begins.

When a packet needs to travel from your machine to a server on the other side of the world, it does not take one straight path. It hops through a chain of routers. And at every single hop, something interesting happens:

1. The router strips off the existing Layer 2 frame entirely. Its job is done.
2. It reads the Layer 3 IP packet inside to find the destination.
3. It checks its routing table to find the next hop.
4. It wraps the packet in a brand new frame addressed to the next router.
5. It sends it on its way.

This repeats at every router until the packet reaches its destination network.

Here is the key insight that makes this click:

✅ IP addresses are end-to-end. They never change across the journey.
✅ MAC addresses are hop-by-hop. They get swapped at every single router.

Your IP packet stays intact the entire trip. The frame carrying it gets rebuilt from scratch at every hop.

This is also why routing works across completely different network types. Ethernet to WiFi to fibre. Each hop gets a fresh frame suited to that segment. The IP packet inside never cares.

Did this distinction between IP and MAC ever trip you up when you were learning networking? 👇

Follow me for practical networking and DevOps content from someone learning it hands-on.

#Networking #DevOps #Linux #LearningInPublic #KubeCraft
