1/ Terraform modules confused me until I stopped thinking about Terraform and started thinking about functions.

2/ A module is a folder of .tf files. variables.tf = the inputs the caller provides. main.tf = builds the resources with those values. outputs.tf = hands back whatever needs to be referenced outside.

3/ The part that took longer to click: a module is a black box. Can't reach inside and reference a resource directly, even if you can see the code. No output, no access. That's the whole point, it's what keeps things from getting accidentally coupled.

4/ Once that clicked, a second customer was 5 lines instead of 40. Third customer, another 5. Add for_each over a list and you can spin up fifty customers from one block.
