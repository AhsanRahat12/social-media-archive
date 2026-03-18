I used to just download software and run it. Never thought about whether someone had tampered with it.

Hashing and digital signatures are what actually verify it.

Hashing: run a file through an algorithm, get a fingerprint. Same file, same fingerprint every time. Change one byte, completely different fingerprint.

But what if someone swaps the file AND the hash? Digital signatures fix that.

The sender:
→ Hashes the file
→ Encrypts that hash with their private key
→ That encrypted hash is the signature

You receiving it:
→ Decrypt the signature with their public key. Get the original hash.
→ Hash the file yourself.
→ Compare the two. Match? File is genuine and untouched.

✅ Hash proves nothing was changed.
✅ Signature proves it came from the right person.

This runs quietly behind every software release, code signing, and system update you have ever installed.

#Networking #DevOps #Security #LearningInPublic
