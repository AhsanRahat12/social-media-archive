Most people use security groups and never touch network ACLs.

But they serve completely different jobs.

→ Network ACL: guards the entire subnet. Stateless. First checkpoint.
→ Security group: guards individual resources inside the subnet. Stateful. Second checkpoint.

Think of it like a building.
ACL = security guard at the front door.
Security group = lock on each office inside.

Miss one and you still have the other. That is why you need both.

#Networking #DevOps #LearningInPublic
