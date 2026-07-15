1/ Spent months thinking the Azure portal and Terraform were different skills. They're the same skill wearing different clothes.

2/ Every resource in Azure is a JSON object. The portal wizard builds that JSON for you by hand. Terraform builds it from code. Bicep builds it from its own syntax. All three hit the same REST API: Azure Resource Manager.

3/ Click "Create VM" in the portal and you're not getting one resource. You're getting a VNet, subnet, NIC, disk, and NSG, all auto-provisioned as dependencies.

4/ Once I saw that, Terraform stopped looking like a new language to learn and started looking like the same click, just written down and repeatable.
