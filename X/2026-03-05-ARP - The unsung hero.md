Your machine knows the IP it wants to reach. But Layer 2 only speaks MAC addresses.

So before any frame can be sent, ARP runs silently in the background:

→ Check ARP cache. Know the MAC already? Use it.
→ If not, broadcast. "Who has this IP?"
→ Owner responds. "That is me. Here is my MAC."
→ Cache it. Build the frame. Send it.

IP gets the credit. ARP does the quiet work that makes it possible.

#Networking #DevOps #LearningInPublic
