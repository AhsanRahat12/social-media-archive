Ever wonder why you have fewer usable IP's in a subnet than expected? This is a common surprise encountered when using AWS.

Every subnet in AWS reserves 5 IP addresses:

- The network address itself
- The VPC router
- The DNS server
- One reserved for future use
- The broadcast address

This is not a big deal on a large subnet, but for smaller it can cause issues. For example if using /28, instead of having 16 available addresses you only have 11.

So next time when designing a subnet on the smaller side make sure you are accounting for this.
