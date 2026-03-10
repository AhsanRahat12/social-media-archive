You have seen /24 everywhere. Cloud configs, Docker networks, every networking tutorial. But if someone asked you to explain what it actually means, could you?

Most people cannot. They just copy it and move on.

Here is what is actually happening.

An IPv4 address is 32 bits with two parts: a network part that identifies which network you are on, and a host part that identifies your specific device within it. The subnet mask defines where that split happens.

255.255.255.0 in binary is:

11111111.11111111.11111111.00000000

The 1s are the network part. The 0s are the host part. Count the 1s. There are 24. That is where /24 comes from. CIDR notation is just counting the 1s in the subnet mask.

So 192.168.1.0/24 means:

✅ First 24 bits identify the network. That is 192.168.1
✅ Last 8 bits identify the device. That is the final number.
✅ 8 bits of host space gives 256 addresses. Minus 2 reserved. 254 usable hosts.

Once this clicks, everything else in networking starts to make sense. Subnetting, routing tables, cloud VPC design. It all builds on this one idea.

Did /24 finally click for you at some point, or is it still something you just copy from examples? 👇

Follow me for practical networking and DevOps content from someone learning it hands-on.

#Networking #DevOps #Linux #LearningInPublic #KubeCraft
