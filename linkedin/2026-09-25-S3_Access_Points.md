Are you managing very large S3 buckets with lots of objects, and also different teams accessing them? Try using S3 Access Points.

What is it? It allows you to create multiple access points for the same bucket, with each having its own permissions, letting you control and scope access to whatever subset of the bucket you need.

Each access point can also be accessed by its own DNS address, and can be restricted to only accept requests from a specific VPC.

One critical thing to keep in mind is that an access point can only narrow access, it can never grant more than what the bucket policy allows. If the access point permits something the bucket policy does not, it simply won't work.
