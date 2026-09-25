1/ Managing a very large S3 bucket with thousands of objects, accessed by lots of different teams? Try S3 Access Points.

2/ What is it? Create multiple access points for the same bucket, each with its own policy, letting you control and scope access to whatever subset of the bucket it's meant to serve.

3/ Each access point gets its own unique DNS address, and can be restricted to only accept requests from a specific VPC.

4/ Critical thing to know: an access point can only narrow access, it can never grant more than what the bucket policy allows. If the access point permits something the bucket policy doesn't, it simply won't work.
