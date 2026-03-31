Security groups are stateful. Most people set them up without really knowing what that means.

Here is what it actually means.

When you allow traffic in on port 443, the reply goes out automatically. No extra rule needed. The security group tracks the connection and knows the difference between a new request and a return response.

Stateful (security group):
→ Allow inbound 443. Done. Return traffic handled automatically.

Stateless (NACL):
→ Allow inbound 443.
→ Allow outbound reply.
→ Allow outbound request.
→ Allow inbound response.
→ Every direction. Every time.

A lot of overly open security groups exist because people didn't know this and kept adding rules just in case.

One word. Completely changes how you write your rules.

#Networking #DevOps #LearningInPublic
