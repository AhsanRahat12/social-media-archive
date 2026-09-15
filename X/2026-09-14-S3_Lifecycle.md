1/ One of the simpler, more popular features in S3 that should be part of your storage setup: S3 Lifecycle. What is it?

2/ At its core, a set of rules applied to a bucket that transitions objects across different, cheaper classes of storage.

3/ Often confused with S3 Intelligent-Tiering, but they're different. Lifecycle rules depend on time. Intelligent-Tiering depends on frequency of access.

4/ Transitions flow in one direction only, toward cheaper, colder storage. Never back up.

5/ Objects in S3 Standard must stay at least 30 days before moving to Standard-IA or One Zone-IA. Other classes have similar time scopes within a single rule, but this can be worked around by splitting the transition into a separate rule.

6/ As objects move to cheaper storage classes, access time and cost both go up (except Glacier Instant Retrieval). Design and configure lifecycle rules carefully.
