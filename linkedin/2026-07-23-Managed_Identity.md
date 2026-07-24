For a while I assumed "managed identity" just meant Azure handles security for me and left it at that. Not wrong, but not the useful part.

Here's the actual problem it solves. Say your AKS cluster needs to read a secret from Key Vault. Without managed identity, you'd have to create a separate login for the cluster, store its password somewhere the cluster can read it, rotate that password on a schedule, and hope it never leaks. Managed identity skips all of that. Azure gives the cluster its own identity, and when it needs to reach Key Vault, it just asks Azure for a short-lived token instead of using a password at all.

The part that actually trips people up: having an identity by itself doesn't let you do anything. Creating one doesn't hand you access to Key Vault or anything else. You need two things together, the identity, and a permission that says exactly what that identity is allowed to do, and where. Identity by itself is nothing. Identity plus permission is what actually lets the cluster reach the vault.

There's also a difference worth knowing. Some identities are tied to one resource and disappear the moment that resource is deleted, like the cluster's own identity. Others exist on their own, outlive whatever they're attached to, and can be shared across several things, like an identity used by an add-on that reads secrets on the cluster's behalf.

Whenever something fails with "access denied," the fix almost never starts with the identity itself. It starts with figuring out which identity actually made the request, then checking what it's allowed to do, and where.

What's a security concept that sounded simple until you actually had to debug it?
