1/ S3 replication is a syncing process between a source and target bucket. Provides resiliency and acts as a backup mechanism.

2/ Works same-region or cross-region, but both buckets need versioning enabled first, genuine prerequisite, not optional.

3/ Needs an IAM role so S3 can interact with both buckets without permission errors. Cross-account replication also needs a bucket policy on top of that role.

4/ Keep in mind: not retroactive, one directional by default (target changes don't sync back to source, bidirectional option exists though).

5/ Delete markers and Glacier objects aren't replicated by default either.

6/ Critical config to think through: object ownership. Get it wrong and you might lose access to objects in your own bucket.
