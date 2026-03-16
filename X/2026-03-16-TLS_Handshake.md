Every https:// connection starts with this. Most people never think about it.

The TLS handshake in 6 steps:

→ Browser sends supported encryption methods
→ Server picks the strongest match, sends its certificate
→ Browser verifies the certificate. Trusted? Not expired? Domain matches?
→ Browser generates a key, encrypts it with server's public key, sends it over
→ Server decrypts it with its private key. Only it can do this.
→ Both sides have the same key. Everything from here is encrypted.

✅ Authentication: server proves who it is
✅ Key exchange: happens safely via asymmetric encryption
✅ Speed: everything after uses fast symmetric encryption

Six steps. Milliseconds. Every secure connection you have ever made.

#Networking #DevOps #Security #LearningInPublic
