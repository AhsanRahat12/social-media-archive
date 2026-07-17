Terraform modules confused me until I stopped thinking about Terraform and started thinking about functions.


A module is just a folder of .tf files. variables.tf declares what the caller has to hand in, main.tf uses those values to build the actual resources, outputs.tf hands back whatever the outside world needs to reference.


The part that took me longer to get: a module is a black box. You cannot reach inside it and reference a resource directly from outside, even if you can see the code. If something inside the module needs to be used elsewhere, it has to come out through an explicit output. No output, no access. That's not a limitation, it's the entire point. It's what makes the module safe to reuse without accidentally coupling two things together.


Once that clicked, calling the module for a second customer was five lines instead of forty. Third customer, another five. A list plus for_each and you can spin up fifty customers from one block.


What repetitive block of code was your first real module?
