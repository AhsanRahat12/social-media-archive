Most people think a firewall is just a list of rules. Allow this, block that.

Whether it understands connections changes everything.

Stateless firewall:
→ Treats every packet as an independent event
→ No memory of what came before
→ Must write rules for both directions manually
→ More rules, more complexity, more room for mistakes

Stateful firewall:
→ Tracks connections
→ Sees a request go out, remembers it
→ Response comes back? Automatically allowed
→ Write the rule once, in one direction

Why it matters:
→ Stateful means unsolicited inbound traffic is dropped automatically
→ Stateless means anything matching a rule gets through, expected or not

In AWS: Security Groups are stateful. Network ACLs are stateless. Knowing which one you are working with determines how you write your rules.

#Networking #DevOps #LearningInPublic
