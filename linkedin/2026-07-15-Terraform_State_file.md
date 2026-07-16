Terraform being cloud-agnostic is genuinely great. It's also a little dangerous if you don't understand what's actually happening underneath that flexibility.

Terraform's biggest weakness is a file most people don't think about until it burns them: the state file. It tracks what Terraform believes exists versus what's actually in Azure. If someone changes something manually in the portal, that file drifts from reality. Run terraform apply after that and it can try to "reconcile" by destroying the resource it thinks is wrong. That's not a hypothetical, that's a real production incident pattern.

Bicep doesn't have this problem. No state file to manage or corrupt. Azure tracks it internally through Resource Manager, always in sync with the newest APIs since Microsoft owns both ends.

Worth knowing if you ever touch AWS: CloudFormation makes the same choice as Bicep, not Terraform. No local state file, AWS owns the stack internally. Drift detection is opt-in, you run it, it doesn't silently reconcile on your next deploy. Failed deployments auto-rollback too, which Terraform doesn't do.

So the real split isn't three different tools. It's state file ownership. Terraform makes you own it, in exchange for working across every cloud. Bicep and CloudFormation hand that ownership to the platform, in exchange for staying locked to one.

The rule that matters more than which side you're on: if you're using IaC, only use IaC. The moment someone clicks around in a console "just this once," any of these tools can lie to you about what's real.

Have you ever had state drift bite you, on any of these?
