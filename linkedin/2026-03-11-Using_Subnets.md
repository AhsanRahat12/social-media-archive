Most people learn what /24 means and stop there. Subnetting is where it gets actually useful.

The idea is simple. You have a network. You need to divide it into smaller pieces. Subnetting is how you do that, and it comes down to one mechanic: stealing bits.

Start with 192.168.1.0/24. That is your whole network. 8 host bits. 254 usable addresses.

Now go to /25. You just stole 1 bit from the host side and gave it to the network side. That stolen bit can only be 0 or 1. Two possibilities. Two subnets.

✅ Subnet 1: 192.168.1.0/25 — covers .0 to .127 — 126 usable hosts
✅ Subnet 2: 192.168.1.128/25 — covers .128 to .255 — 126 usable hosts

Go to /26 and you steal 2 bits. Two bits can be 00, 01, 10, or 11. Four subnets, 62 hosts each.

The pattern holds every single time:

1. Every extra bit stolen doubles the number of subnets.
2. Every extra bit stolen halves the hosts per subnet.
3. The starting address of each subnet is always a multiple of the block size.

That block size is just 256 minus the last octet of the subnet mask. For /26 that is 256 minus 192 which equals 64. So subnets start at 0, 64, 128, 192.

This is not just theory. In cloud infrastructure you split VPCs into subnets constantly. Public subnets, private subnets, database subnets. Understanding what you are actually doing when you pick a prefix length is what separates guessing from designing.

Have you ever had to subnet a network in a real project? What was the use case? 👇

Follow me for practical networking and DevOps content from someone learning it hands-on.

#Networking #DevOps #Linux #LearningInPublic #KubeCraft
