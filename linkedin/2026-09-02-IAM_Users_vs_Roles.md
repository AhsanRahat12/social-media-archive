When I started my deep dive in AWS, IAM concepts were not too difficult to grasp. Users, policies, groups were easy to understand and implement.

The framework that did take a while for me to grasp was IAM roles vs IAM users. Specifically understanding when to use the one or the other.

IAM users are meant to be used for long term identities for one specific, nameable thing. If we can picture a person or one app that needs access to resources, we should default to users. IAM users are also capped at 5000 per AWS account.

With roles, giving access to multiple services to do something on my behalf, or giving access to millions of app users with identities they already have, are what makes them useful.

Of course having a role does not simply mean we have access to resources, there are more complexities to it. Namely permissions and configurations, but thinking about the two like this (use-case specific thinking) is what made me make sense of them.

What helped you differentiate between IAM users and roles? Would love to know how you use them in production.

#AWS #IAM #CloudSecurity #AWSCertified #DevOps
