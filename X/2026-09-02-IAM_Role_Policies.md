1/ AWS IAM roles have a two tiered policy structure. Trust policy: who's allowed to assume the role. Permission policy: what it can actually do once assumed. Both have to be configured, one without the other doesn't work.

2/ Assuming a role goes through STS via sts:AssumeRole. What comes back: temporary, time limited credentials.

3/ These get checked live against the underlying policy. Revoke a permission mid-session, the next API call gets denied, even if earlier calls in that same session succeeded. No need to reassume the role.

4/ Same AWS account: resource policy OR the role's own permission policy is enough, either one works alone.

5/ Different AWS account: both sides have to agree, the role's permission policy AND the resource policy.

6/ Ever assumed a role fine but had nothing actually work once inside? Curious what it turned out to be.
