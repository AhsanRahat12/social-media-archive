Every time you open a website, stream a video, or push code to GitHub, something happens before a single byte of real data moves. Most people never think about it.

TCP will not let two devices exchange data until they have formally agreed to talk. This agreement is the 3-way handshake. And it happens in milliseconds, every single time.

Here is what is actually going on:

1. SYN. Your machine sends a signal to the server. "I want to connect. My starting sequence number is X."
2. SYN-ACK. The server responds. "Got you. I am ready. My starting sequence number is Y. I am expecting your next message to be X+1."
3. ACK. Your machine confirms. "Received. I am expecting Y+1 from you. Let's go."

Connection established. Data starts flowing.

Those sequence numbers are not just formalities. They are how TCP guarantees everything that follows:

✅ If packets arrive out of order, sequence numbers put them back in the right sequence.
✅ If a packet goes missing, the receiver knows exactly which one to ask for again.
✅ Both sides know the connection is alive and the other end is ready before sending anything.

This is the difference between TCP and UDP in one idea. UDP just fires data and moves on. TCP makes sure both sides are ready, tracks every segment, and guarantees delivery.

Every reliable connection on the internet starts with these three messages. SYN. SYN-ACK. ACK.

Did understanding the handshake change how you think about network connections? 👇

Follow me for practical networking and DevOps content from someone learning it hands-on.

#Networking #DevOps #Linux #LearningInPublic #KubeCraft
