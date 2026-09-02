1/ Deep diving into AWS, learning IAM. Users, policies, groups clicked fast. What took longer: knowing when to use IAM roles vs IAM users.

2/ IAM users = long term identity for one specific, nameable thing. Picture one person or one app, that's a user. Capped at 5,000 per AWS account.

3/ IAM roles = useful when you're giving access to multiple services acting on your behalf, or to millions of app users signing in with identities they already have.

4/ Caveat worth knowing: having a role doesn't automatically mean you have access. Permissions and configuration still matter underneath. This framework just helped it click.

5/ What helped you differentiate users vs roles? And curious, how are you using them in production?
