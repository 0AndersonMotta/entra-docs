---
title: Use managed identities from a virtual machine to access Cosmos DB  | Microsoft Docs 
description: Learn how to use managed identities with Windows VMs using the Azure portal, CLI, PowerShell, Azure Resource Manager template  
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''

ms.service: active-directory
ms.subservice: msi
ms.workload: integration
ms.topic: how-to
ms.date: 08/18/2021
ms.author: barclayn
ms.custom: ep-msia
#Customer intent: As an administrator I want to know how to access Cosmos DB from a virtual machine using a managed identity

---

# How to use managed identities to connect to Cosmos DB from an Azure virtual machine


In this article we set up a virtual machine to use managed identities to connect to Cosmos. [Azure Cosmos DB](../../cosmos-db/introduction.md) is a fully managed NoSQL database for modern app development. [Managed identities](overview.md) for Azure resources allow your applications to authenticate when accessing resources while using an automatically managed identity. 

## Prerequisites

 - If you're unfamiliar with managed identities for Azure resources, check out the [overview section](overview.md).
- Before you begin, you must have an Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/).
- A [Comos DB account](../../cosmos-db/create-cosmosdb-resources-portal.md).
- You may need either [PowerShell](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-6.3.0) or the [CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).
- A resource group that we can use to create all resources.


## Create a virtual machine with a managed identity

Before we proceed, we need a virtual machine with a managed identity. You have a few options: 
- You may choose to create a virtual machine with a system assigned managed identity enabled.
- You may enable system assigned managed identities on an existing virtual machine.
- You could create a virtual machine with a user-assigned managed identity enabled.
- You can assign a user-assigned managed identity to an existing VM.

In addition, to the options listed above you may also choose between Linux and Windows for your virtual machine's operating system.

### System assigned

To create an Azure VM with the system-assigned managed identity enabled, your account needs the [Virtual Machine Contributor](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) role assignment.  No additional Azure AD directory role assignments are required.

# [Portal](#tab/azure-portal)

- From the **Azure portal** search for **virtual machines**.
- Choose **Create**
- In the Basics tab, provide the required information.
- Choose **Next: Disks >**
- Continue filling out information as needed and in the **Management** tab find the **Identity** section and check the box next to **System assigned managed identity**


:::image type="content" source="media/how-to-manage-identities-vm-cosmos/create-vm-system-assigned-managed-identities.png" alt-text="Image showing how to enable system assigned managed identities while creating a VM":::

For more information, review the Azure virtual machines documentation:

- [Linux](../../virtual-machines/linux/quick-create-portal.md)
- [Windows](../../virtual-machines/windows/quick-create-portal.md)

# [PowerShell](#tab/azure-powershell)

[New-AZVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm?view=azps-6.3.0) creates resources you reference if they don't exist. To create a VM with a system assigned managed identity enabled pass the parameter **-SystemAssignedIdentity** as shown below. 


```powershell

New-AzVm `
    -ResourceGroupName "My VM" `
    -Name "My resource group" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -SystemAssignedIdentity
    -OpenPorts 80,3389
```

- [Quickstart: Create a Windows virtual machine in Azure with PowerShell](../..//virtual-machines/windows/quick-create-powershell.md)
- [Quickstart: Create a Linux virtual machine in Azure with PowerShell](../../virtual-machines/linux/quick-create-powershell.md)


# [Azure CLI](#tab/azure-cli)

Create a VM using [az vm create](/cli/azure/vm/#az_vm_create). The following example creates a VM named *myVM* with a system-assigned managed identity, as requested by the `--assign-identity` parameter. The `--admin-username` and `--admin-password` parameters specify the administrative user name and password account for virtual machine sign-in. Update these values as appropriate for your environment: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

- [Create a Linux virtual machine with a system assigned managed identity](../../virtual-machines/linux/quick-create-cli.md)
- [Create a Windows virtual machine with a system assigned managed identity](../../virtual-machines/windows/quick-create-cli.md)

# [Resource Manager Template](#tab/azure-resource-manager)

To enable system-assigned managed identity, load the template into an editor, locate the `Microsoft.Compute/virtualMachines` resource of interest within the `resources` section and add the `"identity"` property at the same level as the `"type": "Microsoft.Compute/virtualMachines"` property. Use the following syntax:

   ```json
   "identity": {
       "type": "SystemAssigned"
   },
   ```

When you're done, the following sections should be added to the `resource` section of your template and it should resemble the following:

   ```json
    "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
                }                        
        }
    ]
   ```

---

### User-assigned managed identities

User-assigned managed identities may be used with multiple resources. For information on how to create or delete user-assigned managed identities you can review [Manage user-assigned managed identities](how-manage-user-assigned-managed-identities.md)

To assign a user-assigned identity to a VM, your account needs the Virtual Machine Contributor and Managed Identity Operator role assignments. No additional Azure AD directory role assignments are required.

#### Create a virtual machine with a user-assigned managed identity assigned

# [Portal](#tab/azure-portal)

Currently, the Azure portal does not support assigning a user-assigned managed identity during the creation of a VM. You should create a virtual machine and then assign a user assigned managed identity to it.

[Configure managed identities for Azure resources on a VM using the Azure portal](qs-configure-portal-windows-vm.md#user-assigned-managed-identity)

# [PowerShell](#tab/azure-powershell)

Create a Windows virtual machine with a user assigned managed identity specified.

```powershell
New-AzVm `
    -ResourceGroupName "<Your resource group>" `
    -Name "<Your VM name>" `
    -Location "East US" `
    -VirtualNetworkName "<myVnet>" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -UserAssignedIdentity "/subscriptions/<Your subscription>/resourceGroups/<Your resource group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<Your user assigned managed identity>" `
    -OpenPorts 80,3389

```

Create a Linux virtual machine with a user assigned managed identity specified.

```powershell
New-AzVm `
    -Name "<Linux VM name>" `
    -image CentOS
    -ResourceGroupName "<Your resource group>" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -Linux `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -UserAssignedIdentity "/subscriptions/<Your subscription>/resourceGroups/<Your resource group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<Your user assigned managed identity>" `
    -OpenPorts 22


```

The user assigned managed identity should be specified using its [resourceID](how-manage-user-assigned-managed-identities.md
). 

# [Azure CLI](#tab/azure-cli)

```azurecli
az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY NAME>
```

[Configure managed identities for Azure resources on a VM using the Azure CLI](qs-configure-cli-windows-vm.md#user-assigned-managed-identity)

# [Resource Manager Template](#tab/azure-resource-manager)


---

#### Assign a user-assigned managed identity to a Virtual machine



# [Portal](#tab/azure-portal)

1. Sign in to the [Azure portal](https://portal.azure.com) using an account associated with the Azure subscription that contains the VM.
2. Navigate to the desired VM and click **Identity**, **User assigned** and then **\+Add**.
3. Click the user-assigned identity you want to add to the VM and then click **Add**.


# [PowerShell](#tab/azure-powershell)

To assign a user-assigned identity to a VM, your account needs the [Virtual Machine Contributor](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) and [Managed Identity Operator](../../role-based-access-control/built-in-roles.md#managed-identity-operator) role assignments. No additional Azure AD directory role assignments are required.

Create a user-assigned managed identity using the [New-AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/new-azuserassignedidentity) cmdlet.  Write down the `Id` in the output because you will need this information in the next step.

> [!IMPORTANT]
> Creating user-assigned managed identities only supports alphanumeric, underscore and hyphen (0-9 or a-z or A-Z, \_ or -) characters. Additionally, name should be limited from 3 to 128 character length for the assignment to VM/VMSS to work properly. For more information, see [FAQs](managed-identities-faq.md) and [known issues](known-issues.md)

```azurepowershell-interactive
New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```

Retrieve the VM properties using the `Get-AzVM` cmdlet. Then to assign a user-assigned managed identity to the Azure VM, use the `-IdentityType` and `-IdentityID` switch on the [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet.  The value for the`-IdentityId` parameter is the `Id` you noted in the previous step.  Replace `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, and `<USER ASSIGNED IDENTITY NAME>` with your own values.

> [!WARNING]
> To retain any previously user-assigned managed identities assigned to the VM, query the `Identity` property of the VM object (for example, `$vm.Identity`).  If any user assigned managed identities are returned, include them in the following command along with the new user assigned managed identity you would like to assign to the VM. 


```azurepowershell-interactive
$vm = Get-AzVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
Update-AzVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
```

# [Azure CLI](#tab/azure-cli)

Create a VM using [az vm create](/cli/azure/vm/#az_vm_create). The following example creates a VM associated with a user-assigned managed identity, as specified by the `--assign-identity` parameter. Replace the `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>`, and `<USER ASSIGNED IDENTITY NAME>` parameter values with your own values. 

```azurecli-interactive 
az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY NAME>
```


# [Resource Manager Template](#tab/azure-resource-manager)

Depending on your API version you have to take [different steps](qs-configure-template-windows-vm.md#user-assigned-managed-identity). If your apiVersion is 2018-06-01, your user-assigned managed identities are stored in the userAssignedIdentities dictionary format and the <identityName> value is the name of a variable that you define in the variables section of your template. In the variable you point to the user assigned managed identity that you want to assign to the managed identity.

```json
    "variables": {
	 "identityName": "my-user-assigned"	
		
	},
```

Under the resources element, add the following entry to assign a user-assigned managed identity to your VM. Be sure to replace <identityName> with the name of the user-assigned managed identity you created.

```json

"resources": [
     {
         //other resource provider properties...
         "apiVersion": "2018-06-01",
         "type": "Microsoft.Compute/virtualMachines",
         "name": "[variables('vmName')]",
         "location": "[resourceGroup().location]",
         "identity": {
             "type": "userAssigned",
             "userAssignedIdentities": {
                "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<identityName>'))]": {}
             }
         }
     }
 ]


```

---

## Grant access 

Now that you have a virtual machine configured with a managed identity we need to [grant the managed identity access](../../cosmos-db/how-to-setup-rbac.md) to Cosmos.




### Grant access to a managed identity

Cosmos DB uses RBAC roles to grant access to either data plane or management plane operations. Access to management plane operations is controlled using [Azure RBAC roles](../../cosmos-db/role-based-access-control.md). Data plane access control is managed using Azure Cosmos DB [data plane RBAC](../../cosmos-db/how-to-setup-rbac.md) helps you manage access to data plane operations. In this example we will grant reader access to the vm's managed identity.

- Azure Cosmos DB exposes two built-in role definitions. We will use the **Cosmos DB Built-in Data Reader** role. To grant access, you need to associate the role definition with the identity. In our case, the managed identity associated with our virtual machine.



### [Portal](#tab/azure-portal)

**At this time there is no role assignment option available in the Azure portal**


### [PowerShell](#tab/azure-powershell)

```powershell
$resourceGroupName = "<myResourceGroup>"
$accountName = "<myCosmosAccount>" 
$readOnlyRoleDefinitionId = "00000000-0000-0000-0000-000000000001" # This is the ID of the Cosmos DB Built-in Data Reader role definition
$principalId = "1111111-1111-11111-1111-11111111" # This is the object ID of the managed identity.
New-AzCosmosDBSqlRoleAssignment -AccountName $accountName `
    -ResourceGroupName $resourceGroupName `
    -RoleDefinitionId $readOnlyRoleDefinitionId `
    -Scope "/" `
    -PrincipalId $principalId
```

When the role assignment step completes you can see results like the ones below

results-role-assignment.png

:::image type="content" source="media/how-to-manage-identities-vm-cosmos/results-role-assignment.png" alt-text="This shows the results of the role assignment":::

### [Azure CLI](#tab/azure-cli)

```azurecli

resourceGroupName='<myResourceGroup>'
accountName='<myCosmosAccount>'
readOnlyRoleDefinitionId = '00000000-0000-0000-0000-000000000001' # This is the ID of the Cosmos DB Built-in Data Reader role definition
principalId = "1111111-1111-11111-1111-11111111" # This is the object ID of the managed identity.
az cosmosdb sql role assignment create --account-name $accountName --resource-group $resourceGroupName --scope "/" --principal-id $principalId --role-definition-id $readOnlyRoleDefinitionId

```

### [Resource Manager Template](#tab/azure-resource-manager)

TBD what we would show here


---


## Get an access token


### Windows

1. In the Azure portal, navigate to **Virtual Machines**, go to your Windows virtual machine, then from the **Overview** page click **Connect** at the top. 
2. Enter in your **Username** and **Password** for which you added when you created the Windows VM. 
3. Now that you have created a **Remote Desktop Connection** with the virtual machine, open PowerShell in the remote session.
4. Using Powershell’s Invoke-WebRequest, make a request to the local managed identities for Azure resources endpoint to get an access token for Azure Resource Manager.

   ```powershell
   $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
   ```

   > [!NOTE]
   > The value of the "resource" parameter must be an exact match for what is expected by Azure AD. When using the Azure Resource Manager resource ID, you must include the trailing slash on the URI.
    
   Next, extract the "Content" element, which is stored as a JavaScript Object Notation (JSON) formatted string in the $response object. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```
   Next, extract the access token from the response.
    
   ```powershell
   $ArmToken = $content.access_token

### Linux

For the remainder of the tutorial, work from the virtual machine.

To complete these steps, you need an SSH client. If you need assistance configuring your SSH client's keys, see [How to create and use an SSH public and private key pair for Linux VMs in Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. In the Azure portal, navigate to **Virtual Machines**, go to your Linux virtual machine, then from the **Overview** page click **Connect** at the top. Copy the string to connect to your VM. 
2. Connect to your VM using your SSH client.  
3. Next, you are prompted to enter in your **Password** you added when creating the **Linux VM**. You should then be successfully signed in.  
4. Use CURL to get an access token for Azure Resource Manager: 
     
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true   
    ```
 
    > [!NOTE]
    > In the previous request, the value of the "resource" parameter must be an exact match for what is expected by Azure AD. When using the Azure Resource Manager resource ID, you must include the trailing slash on the URI.
    > In the following response, the access_token element as been shortened for brevity.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```

## Clean up steps

### [Portal](#tab/azure-portal)

1. In the [portal](https://portal.azure.com), select the resource you want to delete.

1. Select **Delete**. 

1. When prompted, confirm the deletion.


### [PowerShell](#tab/azure-powershell)


```azurepowershell-interactive
Remove-AzResource `
  -ResourceGroupName ExampleResourceGroup `
  -ResourceName ExampleVM `
  -ResourceType Microsoft.Compute/virtualMachines
```


### [Azure CLI](#tab/azure-cli)

```azurecli-interactive
az resource delete \
  --resource-group ExampleResourceGroup \
  --name ExampleVM \
  --resource-type "Microsoft.Compute/virtualMachines"
```

### [Resource Manager Template](#tab/azure-resource-manager)

TBD what we would show here

---

## Next steps

Learn more about managed identities for Azure resources:

* [What are managed identities for Azure resources?](overview.md)
* [Azure Resource Manager templates](https://github.com/Azure/azure-quickstart-templates)

