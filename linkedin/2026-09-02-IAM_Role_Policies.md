AWS's IAM roles have a two tiered policy structure. It has a trust policy, this controls who is allowed to assume the role. And it also has a permissions policy, this is what allows the role to actually execute (assuming that it is trusted).

Both of these has to work together in order for the role to work properly. It is simply not enough if we configure a trust policy without configuring the other.

When identites assume a role it gets temporary, time limited credentials. An important thing to keep in mind is that these credentials gets checked against the underlying policies live. Meaning - if permissions change the role gets effected right away, in the same session.

When using roles to execute in our own AWS account the resource policy or the roles own permission policy has to be configured. But for access to resources in another AWS account, we need both the role's permission policy and the resource policy to work together, both sides have to agree.

Hope this helps some one learing about IAM roles.
