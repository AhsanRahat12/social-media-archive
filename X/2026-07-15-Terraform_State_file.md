1/ Terraform being cloud-agnostic is genuinely great. It's also a little dangerous if you don't understand what's happening underneath that flexibility. Here's the part most people skip.

2/ Terraform tracks what it believes exists in a local state file. If you manually change something in the console, that belief goes stale. Next apply, Terraform might try to "fix" the drift by destroying the thing you just touched.

3/ Bicep sidesteps this entirely. No state file. Azure owns it internally through Resource Manager, always current with the latest APIs since Microsoft owns both ends.

4/ CloudFormation makes the same call as Bicep, not Terraform. No local state file, AWS owns the stack. Drift detection is opt-in, and failed deploys auto-rollback, something Terraform doesn't do.

5/ So it's not really 3 different tools. It's a 2-way split on state file ownership. Terraform: you own it, works everywhere. Bicep/CloudFormation: the platform owns it, you're locked to one cloud.

6/ The rule that matters more than which side you're on: if you use IaC, only use IaC. A console change "just this once" is how any of these tools ends up lying to you.
