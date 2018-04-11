---

title: Allow or block invitations to B2B users from specific domains - Azure Active Directory | Microsoft Docs
description: Shows how an admin can add guest users to their directory from a partner organization using Azure Active Directory (Azure AD) B2B collaboration.
services: active-directory
documentationcenter: ''
author: twooley
manager: mtillman
editor: ''
tags: ''

ms.assetid:
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/13/2018
ms.author: twooley
ms.reviewer: sasubram

---

# Allow or block invitations to B2B users from specific domains (Preview)

You can use an allow list or a deny list to allow or block invitations to B2B users from specific domains. For example, if you want to block personal email address domains, you can set up a deny list that contains domains like Gmail.com and Outlook.com. Or, if your business has a partnership with other businesses like Contoso.com, Fabrikam.com and Litware.com, and you want to restrict invitations to only these organizations, you can add Contoso.com, Fabrikam.com and Litware.com to your allow list.
  
> [!NOTE]
> Currently, you can only use deny lists. The ability to use allow lists is coming very soon.

## Important considerations

 - You can create either an allow list or a block list. You can't set up both types of lists. By default, whatever domains are not in the allow list are on the block list, and visa versa. 
- You can create only one policy per organization. You can update the policy to include more domains, or you can delete the policy to create a new one. 
- This list works independently from OneDrive for Business and SharePoint Online allow/block lists. You would need to set up an allow or deny list for OneDrive for Business and SharePoint Online if you want to restrict individual file sharing in SharePoint Online. For more information, see [Restricted domains sharing in SharePoint Online and OneDrive for Business](https://support.office.com/article/restricted-domains-sharing-in-sharepoint-online-and-onedrive-for-business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9).
- This list doesn’t apply to already-added external users. The list will be enforced for all external users that are added after the list is set up. If an invitation is in a pending state when you set the policy, the user's attempt to redeem the invitation will fail.

## Set the allow or deny list policy in the portal

To create an allow or deny list: 
1. Sign in to the [Azure portal](https://portal.azure.com).
2. Select **Azure Active Directory**> **Users** > **User settings**.
3. Under **External users**, select **Manage external collaboration settings**.
4. Under **Collaboration restrictions**, choose the desired setting.

By default, the **Allow invitations to be sent to any domain (most inclusive)** setting is enabled. In this case, you can invite B2B users from any organization.

### Add a deny list

This is the most typical scenario, where your organization wants to work with any organization, but wants to prevent users from specific organizations and domains to be invited to your organization.

To add a deny list:

1. Sign in to the [Azure portal](https://portal.azure.com).
2. Select **Azure Active Directory**> **Users** > **User settings**.
3. Under **External users**, select **Manage external collaboration settings**.
4. Under **Collaboration restrictions**, select **Deny invitations to the specified domains**.
6. Under **TARGET DOMAINS**, enter the name of one of the domains that you want to block. For multiple domains, enter each domain on a new line.

   ![Shows the deny option with added domains](/media/active-directory-b2b-allow-deny-list/DenyListSettings.png)
 
9. When you're done, click **Save**.

After you set the policy, if you try to invite a user from a blocked domain, you receive a message saying that the user is currently blocked by your invitation policy.
 
### Add an allow list

> [!NOTE]
> Currently, the **Allow invitations only to the specified domains (most restrictive)** setting is unavailable. The ability to use allow lists is coming very soon.

This is a more restrictive configuration, where you can set specific domains in the allow list and restrict invitations to any other organizations or domains that aren't specifically mentioned. 

If you want to use an allow list, make sure that you first spend time to fully evaluate what your business needs. If you make this policy too restrictive, your users may choose to send documents over email or find other non-IT sanctioned ways of collaborating.

### Switch from allow to deny list and visa versa 

If you switch from one policy to the other, this discards the existing policy configuration. Make sure to back up details of your configuration before you perform the switch. 

## Set the allow or deny list policy using PowerShell

### Prerequisite
To set the allow or deny list by using PowerShell, you must install the preview version of the Azure Active Directory Module for Windows PowerShell. Specifically, install the AzureADPreview module version 2.0.0.98 or later.

To check the version of the module (and see if it's installed):
 
1. Open Windows PowerShell as an elevated user (Run as Administrator). 
2. Run the following command to see if you have any versions of the Azure Active Directory Module for Windows PowerShell installed on your computer:

   ````powershell  
   Get-Module -ListAvailable AzureAD*
   ````
If the module is not installed, or you don't have a required version, do one of the following:

- If no results are returned, run the following command to install the latest version of the AzureADPreview module:
  
   ````powershell  
   Install-Module AzureADPreview
   ````
- If only the AzureAD module is shown in the results, run the following commands to install the AzureADPreview module: 

   ````powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ````
- If only the AzureADPreview module is shown in the results, but the version is less than 2.0.0.98, run the following commands to update it: 

   ````powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
   ````

- If both the AzureAD and AzureADPreview modules are shown in the results, but the version of the AzureADPreview module is less than 2.0.0.98, run the following commands to update it: 

   ````powershell 
   Uninstall-Module AzureAD 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
    ````

### Create and set the policy through PowerShell

To create an allow or deny list, use the [New-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview) cmdlet.

The following example shows how to set an example deny list that blocks the "live.com" domain.

Giving the policyValue inline.

````powershell  
New-AzureADPolicy -Definition @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}") -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 

To set the allow or deny list policy, use the [Set-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview) cmdlet. For example:

````powershell   
Set-AzureADPolicy -Definition $policyValue -Id $currentpolicy.Id 
````

To get the policy, use the [Get-AzureADPolicy](https://docs.microsoft.com/en-us/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview) cmdlet. For example:

````powershell
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy'} | select -First 1 
````

To remove the policy use the [Remove-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview) cmdlet. For example:

````powershell
Remove-AzureADPolicy -Id $currentpolicy.Id 
````

## Next steps





