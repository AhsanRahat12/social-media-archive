1/ For a while I thought "managed identity" just meant Azure handles security for me. Not wrong, but not the useful part.

2/ The problem it solves: say your AKS cluster needs to read a secret from Key Vault. Without it, you'd create a separate login for the cluster, store its password somewhere, rotate it on a schedule, hope it never leaks. Managed identity skips all of that with a short-lived token instead.

3/ The part that trips people up: an identity alone doesn't let you do anything. Creating one doesn't hand you access to Key Vault. You need the identity plus a permission saying exactly what it's allowed to do, and where.

4/ Some identities are tied to one resource and disappear when it's deleted. Others exist on their own, outlive what they're attached to, and get shared across several things.

5/ When something fails with "access denied," the fix starts with figuring out which identity made the request, then checking what it's actually allowed to do, and where.
