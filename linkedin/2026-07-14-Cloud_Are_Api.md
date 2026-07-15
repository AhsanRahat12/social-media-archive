A few months ago I thought clicking through the Azure portal and writing Terraform were two different skills. They're not.

Every VM you create in the portal is a form. That form gets translated into JSON. That JSON gets sent to Azure Resource Manager, which is just a REST API sitting underneath everything.

Terraform doesn't do anything mystical either. It takes your .tf code, turns it into the same JSON, and fires the same HTTP request at the same API. Bicep does the same thing, just with Microsoft's own syntax.

Portal, Terraform, Bicep. Three doors into the same room.

Once that clicked, IaC stopped feeling like a separate discipline and started feeling like the obvious way to do something I was already doing by hand. The manifest file isn't a new concept if you've ever clicked "Create" in a wizard. It's the same request, just version-controlled and repeatable.

The part that actually surprised me: creating a single VM silently provisions a VNet, a subnet, a NIC, a disk, and an NSG behind it. Click one button, get five resources.

What was the moment IaC actually clicked for you, if it did?
