You have seen /24 everywhere. Most people just copy it and move on.

Here is what it actually means:

An IPv4 address is 32 bits. Two parts: network and host. The subnet mask defines where the split happens.

255.255.255.0 in binary:
11111111.11111111.11111111.00000000

Count the 1s. There are 24. That is where /24 comes from.

So 192.168.1.0/24 means:
→ First 24 bits = network (192.168.1)
→ Last 8 bits = device
→ 254 usable hosts

Once this clicks, subnetting, routing tables, and VPC design all start to make sense.

#Networking #DevOps #LearningInPublic
