Every time you visit a website with https://, something happens before the page loads. It takes milliseconds. Most people never think about it.

Your browser and the server are negotiating a secure connection from scratch. This is the TLS handshake.

Here is what actually happens:

1. Your browser connects and sends a list of encryption methods it supports.
2. The server picks the strongest method they both support and sends back its certificate.
3. Your browser verifies the certificate. Is it signed by a trusted authority? Is it expired? Does the domain match?
4. Your browser generates a key, encrypts it with the server's public key, and sends it over.
5. The server decrypts it with its private key. Only the server can do this.
6. Both sides now have the same key. Everything from here is encrypted.

That certificate check in step 3 is what prevents someone from pretending to be your bank. Anyone can set up a server. Not anyone can get a trusted certificate authority to vouch for it.

The key exchange in step 4 is what makes the connection private. The key travels encrypted. Even if someone intercepts it, they cannot decrypt it without the server's private key.

✅ Authentication: the server proves it is who it claims to be.
✅ Encryption: the key exchange happens safely using asymmetric encryption.
✅ Speed: everything after the handshake uses fast symmetric encryption.

Six steps. Milliseconds. Every https:// connection you have ever made.

Did understanding TLS change how you think about web security? 👇

Follow me for practical networking and DevOps content from someone learning it hands-on.

#Networking #DevOps #Security #LearningInPublic #KubeCraft
