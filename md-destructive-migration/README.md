# Migrate an ARM resource group with virtual machines to Azure Managed Disks service using a destructive remove-recreate approach.

Built by: [colincole](https://github.com/colincole)

The EnableManagedDisks.ps1 PowerShell script allows someone to migrate an entire resource group and all contained resources to use the Azure Managed Disks service. The script will export the resource group to a template, modify the resource group's virtual machines and availability sets to use Managed Disks, remove the VM's and availability sets, then redeploy the incremental changes in the modified template. The Managed Disks service will import each disk on redeploy. No data will be lost but there will be some downtime to remove and recreate each VM.

One of the scenarios that is supported with this script is upgrading from unmanaged "standard" blob storage directly to managed "premium" storage with Managed Disks. To do this, set the AllowTemplateChanges flag on the script. When prompted after running the script, find the named newly generated json template file and open it up in an editor. There's two properties that will need to be manually changed in the template. First, change all of the Microsoft.Compute/disks resources to use premium storage. Do this by changing the "accountType" property from "Standard_LRS" to "Premium_LRS".  Second, for each Virtual Machine resource, change the "vmSize" property to an S-Series VM. For example, change "vmSize" from "Standard_D2_v2" to "Standard_DS2_v2". Make these changes for each disk and each VM, then save the json template, and press Yes to continue to the script. Storage will be migrated to premium on redeployment.
