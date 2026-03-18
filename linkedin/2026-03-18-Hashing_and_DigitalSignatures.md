I used to download software and just run it. Never thought twice about where it came from or whether someone had messed with it along the way.

Then I learned about hashing and digital signatures. Now I actually understand what is being verified and why it matters.

Here is how it works.

When you hash a file, you run it through an algorithm that spits out a fixed string of characters. A fingerprint. The same file always produces the same fingerprint. Change even one byte and the fingerprint changes completely.

That solves tampering. But not identity. What if someone swaps the file AND the hash?

Digital signatures fix that. Before sending a file, the sender hashes it and encrypts that hash with their private key. That encrypted hash is the signature. Only one person on earth could have created it: whoever holds that private key.

When you receive it:

1. You decrypt the signature with the sender's public key. You get the original hash back.
2. You hash the file yourself.
3. You compare the two.

Match? The file is genuine and untouched. No match? Something is wrong.

✅ Hash proves nothing was changed.
✅ Signature proves it came from the right person.
✅ Together they give you trust without having to just hope for the best.

This is how software releases are verified, how code signing works, how your machine trusts system updates. It is running quietly in the background of things you do every day.

Do you verify file signatures before installing things? Honestly, I did not until I understood this. 👇

Follow me for practical networking and DevOps content from someone learning it hands-on.

#Networking #DevOps #Security #LearningInPublic #KubeCraft
