---
title: Create an Azure AD Domain Services resource forest using Azure PowerShell | Microsoft Docs
description: In this article, learn how to create and configure an Azure Active Directory Domain Services resource forest and outbound forest to an on-premises Active Directory Domain Services environment using Azure PowerShell.
author: iainfoulds
manager: daveba

ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: iainfou

#Customer intent: As an identity administrator, I want to create an Azure AD Domain Services resource forest and one-way outbound forest from an Azure Active Directory Domain Services resource forest to an on-premises Active Directory Domain Services forest using Azure PowerShell to provide authentication and resource access between forests.

---

# Create an Azure Active Directory Domain Services resource forest and outbound forest trust to an on-premises domain using Azure PowerShell (preview)

In environments where you can't synchronize password hashes, or you have users that exclusively sign in using smart cards so they don't know their password, you can use a resource forest in Azure Active Directory Domain Services (AD DS). A resource forest uses a one-way outbound trust from Azure AD DS to one or more on-premises AD DS environments. This trust relationship lets users, applications, and computers authenticate against an on-premises domain from the Azure AD DS managed domain. Azure AD DS resource forests are currently in preview.

![Diagram of forest trust from Azure AD DS to on-premises AD DS](./media/concepts-resource-forest/resource-forest-trust-relationship.png)

In this article, you learn how to:

> [!div class="checklist"]
> * Create an Azure AD DS resource forest using Azure PowerShell
> * Create a one-way outbound forest trust in Azure AD DS using Azure PowerShell
> * Configure DNS in an on-premises AD DS environment to support Azure AD DS connectivity
> * Create a one-way inbound forest trust in an on-premises AD DS environment
> * Test and validate the trust relationship for authentication and resource access

If you don’t have an Azure subscription, [create an account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## Prerequisites

To complete this article, you need the following resources and privileges:

* An active Azure subscription.
    * If you don’t have an Azure subscription, [create an account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* An Azure Active Directory tenant associated with your subscription, either synchronized with an on-premises directory or a cloud-only directory.
    * If needed, [create an Azure Active Directory tenant][create-azure-ad-tenant] or [associate an Azure subscription with your account][associate-azure-ad-tenant].

* Install and configure Azure PowerShell.
    * If needed, follow the instructions to [install the Azure PowerShell module and connect to your Azure subscription](/powershell/azure/install-az-ps).
    * Make sure that you sign in to your Azure subscription using the [Connect-AzAccount][Connect-AzAccount] cmdlet.
* Install and configure Azure AD PowerShell.
    * If needed, follow the instructions to [install the Azure AD PowerShell module and connect to Azure AD](/powershell/azure/active-directory/install-adv2).
    * Make sure that you sign in to your Azure AD tenant using the [Connect-AzureAD][Connect-AzureAD] cmdlet.
* You need *global administrator* privileges in your Azure AD tenant to enable Azure AD DS.
* You need *Contributor* privileges in your Azure subscription to create the required Azure AD DS resources.

## Sign in to the Azure portal

In this article, you create and configure the outbound forest trust from Azure AD DS using the Azure portal. To get started, first sign in to the [Azure portal](https://portal.azure.com).

## Networking considerations

The virtual network that hosts the Azure AD DS resource forest needs network connectivity to your on-premises Active Directory. Applications and services also need network connectivity to the virtual network hosting the Azure AD DS resource forest. Network connectivity to the Azure AD DS resource forest must be always on and stable otherwise users may fail to authenticate or access resources.

Before you configure a forest trust in Azure AD DS, make sure your networking between Azure and on-premises environment meets the following requirements:

* Use private IP addresses. Don't rely on DHCP with dynamic IP address assignment.
* Avoid overlapping IP address spaces to allow virtual network peering and routing to successfully communicate between Azure and on-premises.
* An Azure virtual network needs a gateway subnet to configure a site-to-site (S2S) VPN or ExpressRoute connection
* Create subnets with enough IP addresses to support your scenario.
* Make sure Azure AD DS has its own subnet, don't share this virtual network subnet with application VMs and services.
* Peered virtual networks are NOT transitive.
    * Azure virtual network peerings must be created between all virtual networks you want to use the Azure AD DS resource forest trust to the on-premises AD DS environment.
* Provide continuous network connectivity to your on-premises Active Directory forest. Don't use on-demand connections.
* Make sure there's continuous name resolution (DNS) between your Azure AD DS resource forest name and your on-premises Active Directory forest name.

The following example diagram shows an Azure ExpressRoute connection from the *Gateway* subnet for the Azure AD DS virtual network to the on-premises datacenter. An Azure Site-to-Site VPN connection could be used instead of ExpressRoute:

![Diagram of example ExpressRoute connectivity for Azure AD DS resource forest](./media/create-resource-forest-powershell/expressroute-connectivity.png)

The hybrid network configuration is beyond the scope of this documentation, and may already exist in your environment. For details on specific scenarios, see the following articles:

* [Azure Site-to-Site VPN](/vpn-gateway/vpn-gateway-about-vpngateways).
* [Azure ExpressRoute Overview](/vpn-gateway/vpn-gateway-about-vpngateways).
* [Connect AWS VPC to Azure using Site-to-Site VPN](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)

### Virtual Network

Your Azure AD DS resource forest needs an Azure virtual network for network connectivity. The `New-AaddsForest` PowerShell cmdlet can create the needed virtual network for you, or you can provide the name of an existing subnet. If you create a virtual networking using the `New-AaddsForest` cmdlet, make sure the previous network considerations are included.

### Subnets

The virtual network for Azure AD DS should include a minimum of two or three subnets:

| Purpose     | Description |
|:------------|:------------|
| Azure AD DS | This subnet exclusively hosts the Azure AD DS resource forest. Don't create your own VMs or attach other services to this subnet. |
| Workload    | This subnet hosts your own Azure VMs for virtualized applications and resources. |
| Gateway     | The subnet hosts the termination point for Azure Site-to-Site VPN or Azure ExpressRoute connection. In larger environments, this Site-to-Site VPN or ExpressRoute connection already exists in another virtual network. If so, use [Azure virtual network peering][network-peering] between this Azure AD DS virtual network and your Site-to-Site VPN or ExpressRoute virtual network.|

## Choose an Azure AD DS forest name

The Azure AD DS resource forest is a separate forest from your on-premises Active Directory. The Azure AD DS resource forest is only used to host resources and users that manage or connect to the forest in Azure. Users and groups are not replicated to the Azure AD DS resource forest from the on-premises AD DS. DNS resource records and zones also aren't replicated.

To establish a trust requires network connectivity and name resolution. The resource forest works like traditional Active Directory - it locates domains and domain controllers using DNS. For this name resolution to work correctly, the Azure AD DS resource forest name must be different from your on-premises Active Directory forest, including any child domains and other trusted forest names.

> [!NOTE]
> In public preview, secure LDAP over the Internet to the Azure AD DS domain isn't supported.

It's recommended to prefix your existing forest name with either *aadds* or *rf* and then optionally add another label to the left of the domain for additional uniqueness.

![Forest Name relationship](media/create-resource-forest-powershell/resource-forest-name-resolution.png)

## Deployment process

It's a multi-part process to create an Azure AD DS resource forest and the trust relationship to an on-premises AD DS. The following high-level steps build your trusted, hybrid environment:

1. Create an Azure AD DS service principal.
1. Create Azure AD DS resource forest.
1. Create hybrid network connectivity using site-to-site VPN or Express Route.
1. Create the Azure AD DS side of the trust relationship.
1. Create the on-premises AD DS side of the trust relationship.

## Create the Azure AD service principal

Azure AD DS requires a service principal synchronize data from Azure AD. This principal must be created in your Azure AD tenant before you created the Azure AD DS resource forest.

Create an Azure AD service principal for Azure AD DS to communicate and authenticate itself. A specific application ID is used named *Domain Controller Services* with an ID of *2565bd9d-da50-47d4-8b85-4c97f669dc36*. Don't change this application ID.

Create an Azure AD service principal using the [New-AzureADServicePrincipal][New-AzureADServicePrincipal] cmdlet:

```powershell
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
```

## Create an Azure AD DS resource forest


1. First, create a resource group using the [New-AzResourceGroup][New-AzResourceGroup] cmdlet. In the following example, the resource group is named *myResourceGroup* and is created in the *westus* region. Use your own name and desired region:

    ```azure-powershell
    New-AzResourceGroup `
      -Name "myResourceGroup" `
      -Location "westus"
    ```
1. Review the following parameters needed for the `New-AaddsForest` cmdlet. Make sure you also have the prerequisite **Azure PowerShell** and **Azure AD PowerShell** modules. Make sure you have planned the virtual network requirements to provide application and on-premises connectivity.

    | Name                         | Cmdlet parameter          | Description |
    |:-----------------------------|---------------------------|:------------|
    | Subscription                 | *-azureSubscriptionId*    | Subscription ID used for Azure AD DS billing. You can get the list of subscriptions using the [Get-AzureRMSubscription][Get-AzureRMSubscription] cmdlet. |
    | Resource Group               | *-aaddsResourceGroupName* | Name of the resource group for the Azure AD DS instance and associated resources. |
    | Location                     | *-aaddsLocation*          | The Azure region to host your Azire AD DS instance. For available regions, see [supported regions for Azure AD DS.](https://azure.microsoft.com/global-infrastructure/services/?products=active-directory-ds&regions=all) |
    | Azure AD DS administrator    | *-aaddsAdminUser*         | The user principal name of the first Azure AD DS administrator. This account must be an existing cloud user account in your Azure Active Directory. The user, and the user running the cmdlet, is added to the *AAD DC Administrators* group. |
    | Azure AD DS domain name      | *-aaddsDomainName*        | The FQDN of the Azure AD DS instance, based on the previous guidance on how to choose a forest name. |

    The `New-AaddsForest` cmdlet can create the Azure virtual network and Azure AD DS subnet if these resources don't already exist. The cmdlet can optionally create the workload subnets, when specified:

    | Name                              | Cmdlet parameter                  | Description |
    |:----------------------------------|:----------------------------------|:------------|
    | Virtual network name              | *-aaddsVnetName*                  | Name of the virtual network for the Azure AD DS instance.|
    | Address space                     | *-aaddsVnetCIDRAddressSpace*      | Virtual network's address range in CIDR notation (if creating the virtual network).|
    | Azure AD DS subnet name           | *-aaddsSubnetName*                | Name of the subnet of the *aaddsVnetName* virtual network hosting Azure AD DS. Don't deploy your own VMs and workloads into this subnet. |
    | Azure AD DS address range         | *-aaddsSubnetCIDRAddressRange*    | Subnet address range in CIDR notation for the AAD DS instance, such as *192.168.1.0/24*. Address range must be contained by the address range of the virtual network, and different from other subnets. |
    | Workload subnet name (optional)   | *-workloadSubnetName*             | Optional name of a subnet in the *aaddsVnetName* virtual network to create for your own application workloads. VMs and applications and also be connected to a peered Azure virtual network instead. |
    | Workload address range (optional) | *-workloadSubnetCIDRAddressRange* | Optional subnet address range in CIDR notation for application workload, such as *192.168.2.0/24*. Address range must be contained by the address range of the virtual network, and different from other subnets.|

1. Create an Azure AD DS resource forest using the `New-AaaddsForest` cmdlet. The following example creates a forest named *rf.addscontoso.com* and creates a workload subnet. Provide your own parameter names and IP address ranges or existing virtual networks:

    ```azure-powershell
    New-AaddsForest `
        -azureSubscriptionId <subscriptionId> `
        -aaddsResourceGroupName "myResourceGroup" `
        -aaddsLocation "West US" `
        -aaddsAdminUser "contosoadmin@contoso.com" `
        -aaddsDomainName "rf.aaddscontoso.com" `
        -aaddsVnetName "myVnet" `
        -aaddsVnetCIDRAddressSpace "192.168.0.0/16" `
        -aaddsSubnetName "AzureADDS" `
        -aaddsSubnetCIDRAddressRange "192.168.1.0/24" `
        -workloadSubnetName "myWorkloads" `
        -workloadSubnetCIDRAddressRange "192.168.2.0/24"
    ```

    It takes quite some time to create the Azure AD DS resource forest and supporting resources. Allow the cmdlet to complete. Continue on to the next section to configure your on-premises network connectivity while the Azure AD resource forest provisions in the background.

## Configure and validate network settings

1. Create the hybrid connectivity to your on-premises network to Azure using an Azure VPN or Azure ExpressRoute connection. The hybrid network configuration is beyond the scope of this documentation, and may already exist in your environment. For details on specific scenarios, see the following articles:

* [Azure Site-to-Site VPN](/vpn-gateway/vpn-gateway-about-vpngateways).
* [Azure ExpressRoute Overview](/vpn-gateway/vpn-gateway-about-vpngateways).

    > [!NOTE]
    > If you create the connection directly to your Azure AD DS virtual network, use a separate gateway subnet. Don't create the gateway in the Azure AD DS subnet.

1. To administer an Azure AD DS domain, you create a management VM, join it to the managed domain, and install the required AD DS management tools.

    While the Azure AD DS resource forest is being deployed, [create a Windows Server VM](https://docs.microsoft.com/azure/active-directory-domain-services/join-windows-vm) then [install the core AD DS management tools](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-management-vm) to install the needed management tools. Wait to join the management VM to the Azure AD DS managed domain until one of the following steps after the domain is successfully deployed.

1. Validate network connectivity between your on-premises network and the Azure virtual network.

    * Confirm that your on-premises domain controller can connect to the managed VM using `ping` or remote desktop, for example.
    * Verify that your management VM can connect to your on-premises domain controllers, again using a utility such as `ping`.

1. In the Azure portal, search for and select **Azure AD Domain Services**. Choose your managed domain, such as *rf.aaddscontoso.com* and wait for the status to report as **Running**.

    When running, [update DNS settings for the Azure virtual network](tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network) and then [enable user accounts for Azure AD DS](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds) to finalize the configurations for your Azure AD DS resource forest.

1. Make a note of the DNS addresses shown on the overview page. You need these addresses when you configure the on-premises Active Directory side of the trust relationship in a following section.
1. Restart the management VM for it to receive the new DNS settings, then [join the VM to the Azure AD DS managed domain](join-windows-vm.md#join-the-vm-to-the-azure-ad-ds-managed-domain).
1. After the management VM is joined to the Azure AD DS managed domain, connect again using remote desktop.

    From a command prompt, use `nslookup` and the Azure AD DS resource forest name to validate name resolution for the resource forest.

    ```console
    nslookup rf.aaddscontoso.com
    ```

    The command should return two IP addresses for the resource forest.

## Create the forest trust

The forest trust has two parts - the one-way outbound forest trust in the Azure AD DS resource forest, and the one-way inbound forest trust in the on-premises AD DS forest. You manually create both sides of this trust relationship. When both sides are created, users and resources can successfully authenticate using the forest trust.

### Create the Azure AD DS side of the trust relationship

Use the `Add-AaddsResourceForestTrust` cmdlet to create the Azure AD DS side of the trust relationship. Provide the cmdlet the following information:

| Name                               | Cmdlet parameter     | Description |
|:-----------------------------------|:---------------------|:------------|
| Azure AD DS domain name            | *-ManagedDomainFqdn* | FQDN of the managed domain, such as *rf.aaddscontoso.com* |
| On-premises AD DS domain name      | *-TrustFqdn*         | The FQDN of the trusted forest, such as *corp.contoso.com* |
| Trust friendly name                | *-TrustFriendlyName* | Friendly name of the trust relationship. |
| On-premises AD DS DNS IP addresses | *-TrustDnsIPs*       | A comma-delimited list DNS server IPv4 addresses for the trusted domain listed. |
| Trust password                     | *-TrustPassword*     | A complex password for the trust relationship. This password is also entered when creating the one-way inbound trust in the on-premises AD DS. |
| Credentials                        | *-Credentials*       | The credentials used to authenticate to Azure. The user must be in the *AAD DC Administrators group*. If not provided, the script prompts for authentication. |

The following example creates a trust relationship named *myAzureADDSTrust* to *corp.contoso.com*. Use your own parameter names and passwords:

```azure-powershell
Add-AaddsResourceForestTrust `
    -ManagedDomainFqdn "rf.aaddscontoso.com" `
    -TrustFqdn "corp.contoso.com" `
    -TrustFriendlyName "myAzureADDSTrust" `
    -TrustDnsIPs "10.0.1.10,10.0.1.11" `
    -TrustPassword <complexPassword>
```

> [!IMPORTANT]
> Remember your trust password. You must use the same password when your create the on-premises side of the trust.

## Configure DNS in the on-premises domain

To correctly resolve the Azure AD DS managed domain from the on-premises environment, you may need to add forwarders to the existing DNS servers. If you haven't configure the on-premises environment to communicate with the Azure AD DS managed domain, complete the following steps from a management workstation for the on-premises AD DS domain:

1. Select **Start | Administrative Tools | DNS**
1. Right-select DNS server, such as *myAD01*, select **Properties**
1. Choose **Forwarders**, then **Edit** to add additional forwarders.
1. Add the IP addresses of the Azure AD DS managed domain, such as *10.0.1.4* and *10.0.1.5*.
1. From a local command prompt, validate name resolution using **nslookup** of the Azure AD DS resource forest domain name. For example, `Nslookup rf.aaddscontoso.com` should return the two IP addresses for the Azure AD DS resource forest.

## Create inbound forest trust in the on-premises domain

The on-premises AD DS domain needs an incoming forest trust for the Azure AD DS managed domain. This trust must be manually created in the on-premises AD DS domain, it can't be created from the Azure portal.

To configure inbound trust on the on-premises AD DS domain, complete the following steps from a management workstation for the on-premises AD DS domain:

1. Select **Start | Administrative Tools | Active Directory Domains and Trusts**
1. Right-select domain, such as *onprem.contoso.com*, select **Properties**
1. Choose **Trusts** tab, then **New Trust**
1. Enter name on Azure AD DS domain name, such as *aadds.contoso.com*, then select **Next**
1. Select the option to create a **Forest trust**, then to create a **One way: incoming** trust.
1. Choose to create the trust for **This domain only**. In the next step, you create the trust in the Azure portal for the Azure AD DS managed domain.
1. Choose to use **Forest-wide authentication**, then enter and confirm a trust password. This same password is also entered in the Azure portal in the next section.
1. Step through the next few windows with default options, then choose the option for **No, do not confirm the outgoing trust**. You can't validate the trust relation because your delegated admin account to the Azure AD DS resource forest doesn't have the required permissions. This behavior is by design.
1. Select **Finish**

## Validate resource authentication

The following common scenarios let you validate that forest trust correctly authenticates users and access to resources:

* [On-premises user authentication from the Azure AD DS resource forest](#on-premises-user-authentication-from-the-azure-ad-ds-resource-forest)
* [Access resources in the Azure AD DS resource forest using on-premises user](#access-resources-in-the-azure-ad-ds-resource-forest-using-on-premises-user)
    * [Enable file and printer sharing](#enable-file-and-printer-sharing)
    * [Create a security group and add members](#create-a-security-group-and-add-members)
    * [Create a file share for cross-forest access](#create-a-file-share-for-cross-forest-access)
    * [Validate cross-forest authentication to a resource](#validate-cross-forest-authentication-to-a-resource)

### On-premises user authentication from the Azure AD DS resource forest

You should have Windows Server virtual machine joined to the Azure AD DS resource domain. Use this virtual machine to test your on-premises user can authenticate on a virtual machine.

1. Connect to the Windows Server VM joined to the Azure AD DS resource forest using Remote Desktop and your Azure AD DS administrator credentials. If you get a Network Level Authentication (NLA) error, check the user account you used is not a domain user account.

    > [!NOTE]
    > To securely connect to your VMs joined to Azure AD Domain Services, you can use the [Azure Bastion Host Service](https://docs.microsoft.com/azure/bastion/bastion-overview) in supported Azure regions.

1. Open a command prompt and use the `whoami` command to show the distinguished name of the currently authenticated user:

    ```console
    whoami /fqdn
    ```

1. Use the `runas` command to authenticate as a user from the on-premises domain. In the following command, replace `userUpn@trusteddomain.com` with the UPN of a user from the trusted on-premises domain. The command prompts you for the user’s password:

    ```console
    Runas /u:userUpn@trusteddomain.com cmd.exe
    ```

1. If the authentication is a successful, a new command prompt opens. The title of the new command prompt includes `running as userUpn@trusteddomain.com`.
1. Use `whoami /fqdn` in the new command prompt to view the distinguished name of the authenticated user from the on-premises Active Directory.

### Access resources in the Azure AD DS resource forest using on-premises user

Using the Windows Server VM joined to the Azure AD DS resource forest, you can test the scenario where users can access resources hosted in the resource forest when they authenticate from computers in the on-premises domain with users from the on-premises domain. The following examples show you how to create and test various common scenarios.

#### Enable file and printer sharing

1. Connect to the Windows Server VM joined to the Azure AD DS resource forest using Remote Desktop and your Azure AD DS administrator credentials. If you get a Network Level Authentication (NLA) error, check the user account you used is not a domain user account.

    > [!NOTE]
    > To securely connect to your VMs joined to Azure AD Domain Services, you can use the [Azure Bastion Host Service](https://docs.microsoft.com/azure/bastion/bastion-overview) in supported Azure regions.

1. Open **Windows Settings**, then search for and select **Network and Sharing Center**.
1. Choose the option for **Change advanced sharing** settings.
1. Under the **Domain Profile**, select **Turn on file and printer sharing** and then **Save changes**.
1. Close **Network and Sharing Center**.

#### Create a security group and add members

1. Open **Active Directory Users and Computers**.
1. Right-select the domain name, choose **New**, and then select **Organizational Unit**.
1. In the name box, type *LocalObjects*, then select **OK**.
1. Select and right-click **LocalObjects** in the navigation pane. Select **New** and then **Group**.
1. Type *FileServerAccess* in the **Group name** box. For the **Group Scope**, select **Domain local**, then choose **OK**.
1. In the content pane, double-click **FileServerAccess**. Select **Members**, choose to **Add**, then select **Locations**.
1. Select your on-premises Active Directory from the **Location** view, then choose **OK**.
1. Type *Domain Users* in the **Enter the object names to select** box. Select **Check Names**, provide credentials for the on-premises Active Directory, then select **OK**.

    > [!NOTE]
    > You must provide credentials because the trust relationship is only one way. This means users from the Azure AD DS can't access resources or search for users or groups in the trusted (on-premises) domain.

1. The **Domain Users** group from your on-premises Active Directory should be a member of the **FileServerAccess** group. Select **OK** to save the group and close the window.

#### Create a file share for cross-forest access

1. On the Windows Server VM joined to the Azure AD DS resource forest, create a folder and provide name such as *CrossForestShare*.
1. Right-select the folder and choose **Properties**.
1. Select the **Security** tab, then choose **Edit**.
1. In the *Permissions for CrossForestShare* dialog box, select **Add**.
1. Type *FileServerAccess* in **Enter the object names to select**, then select **OK**.
1. Select *FileServerAccess* from the **Groups or user names** list. In the **Permissions for FileServerAccess** list, choose *Allow* for the **Modify** and **Write** permissions, then select **OK**.
1. Select the **Sharing** tab, then choose **Advanced Sharing…**
1. Choose **Share this folder**, then enter a memorable name for the file share in **Share name** such as *CrossForestShare*.
1. Select **Permissions**. In the **Permissions for Everyone** list, choose **Allow** for the **Change** permission.
1. Select **OK** two times and then **Close**.

#### Validate cross-forest authentication to a resource

1. Sign in a Windows computer joined to your on-premises Active Directory using a user account from your on-premises Active Directory.
1. Using **Windows Explorer**, connect to the share you created using the fully qualified host name and the share such as `\\fs1.aadds.contoso.com\CrossforestShare`.
1. To validate the write permission, right-select in the folder, choose **New**, then select **Text Document**. Use the default name **New Text Document**.

    If the write permissions are set correctly, a new text document is created. The following steps will then open, edit, and delete the file as appropriate.
1. To validate the read permission, open **New Text Document**.
1. To validate the modify permission, add text to the file and close **Notepad**. When prompted to save changes, choose **Save**.
1. To validate the delete permission, right-select **New Text Document** and choose **Delete**. Choose **Yes** to confirm file deletion.

## Next steps

In this article, you learned how to:

> [!div class="checklist"]
> * Create an Azure AD DS resource forest using Azure PowerShell
> * Create a one-way outbound forest trust in Azure AD DS using Azure PowerShell
> * Configure DNS in an on-premises AD DS environment to support Azure AD DS connectivity
> * Create a one-way inbound forest trust in an on-premises AD DS environment
> * Test and validate the trust relationship for authentication and resource access

For more conceptual information about forest types in Azure AD DS, see [What are resource forests?][concepts-forest] and [How do forest trusts work in Azure AD DS?][concepts-trust]

<!-- INTERNAL LINKS -->
[concepts-forest]: concepts-resource-forest.md
[concepts-trust]: concepts-forest-trust.md
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance-advanced]: tutorial-create-instance-advanced.md
[Connect-AzAccount]: /powershell/module/Az.Accounts/Connect-AzAccount
[Connect-AzureAD]: /powershell/module/AzureAD/Connect-AzureAD
[New-AzResourceGroup]: /powershell/module/Az.Resources/New-AzResourceGroup
[network-peering]: ../virtual-network/virtual-network-peering-overview.md
[New-AzureADServicePrincipal]: /powershell/module/AzureAD/New-AzureADServicePrincipal
[Get-AzureRMSubscription]: /powershell/module/AzureRM.Profile/Get-AzureRmSubscription
