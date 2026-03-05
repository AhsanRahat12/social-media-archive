Every time data crosses the internet, two addressing systems are working together. Most people only know one of them.

MAC addresses handle local delivery. IP addresses handle the end-to-end journey. The moment a packet leaves your network, MACs become useless.

Here is what happens at every router hop:

→ Router strips the old Layer 2 frame. Its job is done.
→ Reads the IP packet inside to find the destination.
→ Checks its routing table for the next hop.
→ Wraps the packet in a brand new frame for the next segment.
→ Sends it on its way.

The key insight:
→ IP is end-to-end. Never changes.
→ MAC is hop-by-hop. Rebuilt at every router.

One packet. Many frames. This is how data crosses the internet.

#Networking #DevOps #LearningInPublic
