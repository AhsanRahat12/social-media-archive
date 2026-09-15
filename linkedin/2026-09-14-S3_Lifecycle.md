One of the more simpler and popular features in S3, that should be part of your storage setup is the S3 lifecycle. What is it?

At its core, a set of rules applied to a bucket that transitions objects across different, cheaper, classes of storage. It's often confused with S3 Intelligent-Tiering but they are different. Lifecycle rules depend on time, where as Intelligent-Tiering depends on frequency of access.

The transitions flow in one direction, towards cheaper and colder storage, and can never flow back up.

A few things to keep in mind: objects in S3 Standard must stay there for at least 30 days. After that waiting period it can be moved to Standard-IA or One Zone-IA. Other classes also have similar time scopes within a single rule, but this can be worked around by splitting the transition with a separate rule. As objects move to cheaper storage classes, the time needed to access them also increases (except Glacier Instant Retrieval). And the cost to access them is also higher.

Thus designing and configuring lifecycle rules should be done carefully!
