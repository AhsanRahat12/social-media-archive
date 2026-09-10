S3 object versioning is a very common feature that is used to protect against accidental overwrites and deletions. And in some cases it is a prerequisite for other features.

But there a few things that we need to be mindful about when we enable it.

Once enabled we cannot fully turn it off. We can only suspend it and re enable it, but there is no way to return it to a state where it was never turned on. And this has consequences, good and bad. The good, this is exactly how accidental deletes and overwrites are protected against as both versions of the object are still present. The bad, both versions will consume space and cost creep is a very real thing.

We can manually keep the versions clean by deleting a specific version ID, to clean one off mistakes that we dont need. But for buckets with thousands of objects, this does not scale well. The real fix comes with S3 Lifecycle configuration, rules that expire old versions after a set amount of time. This makes sure that object versions that are no longer needed dont tick up S3 storage expenses.

So enable object versioning and have a plan to manage the versions. This is how you keep on top of costs and protect your data.
