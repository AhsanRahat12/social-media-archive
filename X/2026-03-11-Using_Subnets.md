Most people learn /24 and stop there. Subnetting is where it gets useful.

The mechanic is simple: steal bits from the host side, give them to the network side.

Start with 192.168.1.0/24. 8 host bits. 254 usable addresses.

Go to /25: steal 1 bit.
→ That bit is 0 or 1. Two subnets.
→ 192.168.1.0/25 (.0 to .127)
→ 192.168.1.128/25 (.128 to .255)

Go to /26: steal 2 bits.
→ Four subnets. 62 hosts each.
→ Start at 0, 64, 128, 192.

The rule every time:
→ +1 bit = double the subnets
→ +1 bit = half the hosts per subnet

In cloud infrastructure you do this constantly. Public subnets, private subnets, database subnets. Knowing what you are actually doing when you pick a prefix is what separates guessing from designing.

#Networking #DevOps #LearningInPublic
