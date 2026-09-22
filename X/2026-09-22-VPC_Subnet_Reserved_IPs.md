1/ Ever wondered why you have fewer usable IPs in a subnet than you expected? Common AWS surprise.

2/ Every subnet reserves 5 IPs automatically: the network address, the VPC router, DNS, one for future use, and broadcast.

3/ Barely matters on a large subnet. On a small one, it's a real problem. A /28 looks like 16 addresses on paper, after AWS's reservations, you're left with 11.

4/ Designing a subnet on the smaller side? Make sure you're accounting for this before you run out of room.
