Every connection you make online starts with this. Most people never think about it.

Before any data moves, TCP runs a 3-way handshake:

→ SYN. "I want to connect. My starting sequence is X."
→ SYN-ACK. "Got you. My sequence is Y. Expecting X+1 from you."
→ ACK. "Received. Expecting Y+1. Let's go."

Connection established. Now data flows.

Those sequence numbers matter:
→ Packets arrive out of order? Resequenced.
→ Packet goes missing? Requested again.
→ Both sides confirmed ready before anything is sent.

This is why TCP is reliable. UDP skips all of this and just fires data.

Every reliable connection on the internet starts with three messages. SYN. SYN-ACK. ACK.

#Networking #DevOps #LearningInPublic
