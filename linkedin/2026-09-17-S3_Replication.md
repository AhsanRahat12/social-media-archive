Thinking of implementing S3 replication? This mental model will start you off in the right direction:

S3 replication is a syncing process between a source and target S3 bucket. It provides resiliency and acts as a means of backup.

Replication works within the same region or across different regions, but both buckets must have versioning enabled, this is a genuine prerequisite, not optional.

An IAM role is required so S3 can interact with both buckets without permission errors. Cross-account replication also needs a bucket policy in addition to that role.

A few things to keep in mind: it's not retroactive, and it's one directional by default (manual changes in the target aren't synced back to the source, though a bidirectional option exists). Delete markers and objects in Glacier are also not replicated by default.

A critical config to think about is object ownership, get this wrong and you might not have access to objects sitting in your own bucket.
