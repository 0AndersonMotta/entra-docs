---
title: Group name policy settings for Office 365 groups in Azure Active Directory preview | Microsoft Docs
description: How to set up expiration for Office 365 groups in Azure Active Directory (preview)
services: active-directory
documentationcenter: ''
author: curtand
manager: michael.tillman
editor: ''

ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: curtand                   
ms.reviewer: kairaz.contractor
ms.custom: it-pro

---

# Enforce a naming convention for groups in Azure Active Directory (preview)

Azure Active Directory (Azure AD) provides group naming policy that you can use to enforce consistent naming conventions for Office 365 groups created by users in your organization. A naming policy can help you and your users identify the function of the group, membership, geographic region, or who created the group. The naming policy can also help categorize groups in the address book. You can use the policy to block specific words from being used in group names and aliases.

> [!IMPORTANT]
> Using group naming policy requires Azure Active Directory Premium P1 licenses for each unique user that is a member of Office 365 groups.

The naming policy is applied to groups that are created across all workloads (such as Outlook, Microsoft Teams, SharePoint, Exchange, or Planner). It is applied to both the group name and group alias. It gets applied when a user creates a group and when group name or alias is edited for an existing group. If you set up naming policy In Azure AD and you have an existing Exchange group naming policy, the Azure AD naming policy is applied.

## Naming convention feature
You can use the following features to enforce group naming conventions:

-   **Prefix-suffix naming policy** You can use prefixes or suffixes to define the naming convention of groups (for example: “GRP\_US\_My Group\_Engineering”). 

-   **Custom blocked words** You can upload a set of blocked words specific to their organization to be blocked in groups created by users; for example, “CEO, Payroll, HR.”

### Prefix-suffix naming policy

The prefixes or suffixes can be either fixed strings or user attributes such as \[Department\] that are substituted based on the user who is creating the group. The total prefixed and suffixed string length is restricted to 53 characters. Prefixes and suffixes can contain special characters supported in group name and group alias. When the prefixes and suffixes contain characters that aren't allowed in the group alias, they are removed and applied to the group name. Because of this restriction, the prefixes and suffixes applied to group name might be different from the ones applied to the group alias.

#### Fixed strings

You can use strings to make it easier to scan and differentiate groups in the global address list and in the left navigation links of group workloads. Some of the common prefixes or suffixes are keywords like ‘Grp\_Name’ , ‘\#Name’, ‘\_Name’

#### Attributes

You can use attributes that can help identify who created the group, such as \[Department\], or the location of group members, such as \[CountryCode\]. For example, if you define your naming policy as `Policy = “GRP [GroupName] [Department]”`, and `User’s department = Engineering`, then an enforced group name might be “GRP My Group Engineering." Supported Azure AD attributes are \[Department\], \[Company\], \[Office\], \[StateOrProvince\], \[CountryOrRegion\], \[Title\], \[CountryCode\]. Unsupported user attributes are treated as fixed strings; for example, “\[postalCode\]”. Extension attributes and custom attributes aren't supported.

We recommend that you use attributes that have values filled in for all users in your organization and don't use attributes that have very long values.

### Custom blocked words

A blocked word list is a comma separated list of words to be blocked in group names and aliases. The entire group name/alias with the prefixes and suffixes is checked for blocked words. Blocked words are checked after appending prefixes and suffixes to the user entered group name. So if user enters ‘s\*\*t’ and ‘Prefix\_’ is the naming policy, ‘Prefix\_s\*\*t’ will pass. But if the naming policy is ‘Prefix&lt;space&gt;‘, ‘Prefix s\*\*t’ will fail. Consider this while choosing your prefixes and suffixes. In other words, no sub-string searches are performed. An exact match between the final name and the custom blocked words is required to trigger a failure. Sub-string search isn't performed so that users can use common words like ‘Class’ even if ‘ass’ is a blocked word.

Blocked word list rules:
- Blocked words are not case sensitive.
- When a user enters a blocked word as part of a group name, they see an error message with the blocked word.
- There are no character restrictions on blocked words.
- There is no upper limit to the number of words in the blocked word list.

### Administrator override

Selected administrators can be exempted from these policies, across all group workloads and endpoints, so that they can create groups using blocked words and with their desired naming conventions. The following are the list of administrator roles exempted from the group naming policy.

- Global administrator
- Partner Tier 1 Support
- Partner Tier 2 Support
- User account administrator
- Directory writers

## Install PowerShell cmdlets for naming policy

Uninstall any older module version the Graph version of the Azure Active Directory Module for Windows PowerShell and [Azure Active Directory PowerShell for Graph - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) before you run the PowerShell commands. 

1. Open the Windows PowerShell app as an administrator.
2. Uninstall any previous version of AzureADPreview.
  
  ````
  Uninstall-Module AzureADPreview
  ````
3. Install the latest version of AzureADPreview.
  
  ````
  Install-Module AzureADPreview
  ````
If you are prompted about accessing an untrusted repository, type **Y**. It might take few minutes for the new module to install.

## How to set up naming policy in Azure AD PowerShell

1. Open a Windows PowerShell window on your computer. You can open it without elevated privileges.

2. Run the following commands to prepare to run the cmdlets.
  
  ````
  Import-Module AzureADPreview
  Connect-AzureAD
  ````
In the **Sign in to your Account** screen that opens, enter your Office 365 admin account and password to connect you to your service, and select **Sign in**.

### View the current settings

1. Fetch the current naming policy to view the current settings.
  
  ````
  \$Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
  ````
  
2. Display the current group settings.
  
  ````
  \$Setting.Values
  ````
  
### Set the naming policy and custom blocked words

1. Set the group name prefixes and suffixes in Azure AD PowerShell.
  
  ````
  \$Setting\["PrefixSuffixNamingRequirement"\] =“Grp\_\[Department\]\_\[GroupName\]\_\[CountryCode\]"
  ````
  
2. Set the custom blocked words that you want to restrict. The following example illustrates how you can add your own custom words.
  
  ````
  \$Setting\["CustomBlockedWordsList"\]=“Payroll,CEO,HR"
  ````
  
3. Save the settings for the new policy to be effective, such as in the following example.
  
  ````
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting \$Setting
  ````
  
That's it. You've set your naming policy and added your blocked words.

## Naming policy experiences across Office 365 apps

After you set a group naming policy in Azure AD, Office 365 apps then show a preview of the name according to your naming policy (with prefixes and suffixes) when the user types in the group name and alias. When the user enters blocked words, they'll see an error message so they can remove the blocked words.

Workload | Compliance
----------- | -------------------------------
Outlook Web Access (OWA) | Outlook Web Access shows the naming policy enforced name when the user types a group name or group alias. When an user enters a custom blocked word, an error message is shown in the UI along with the blocked word so that the user can remove it.
Outlook Desktop | Groups created in Outlook desktop are compliant with naming policy. Outlook desktop app doesn't yet show the preview of the naming policy and doesn't return the custom blocked word errors when the user enters the group name. However, naming policy is automatically applied on clicking create/edit and users see error messages if there are custom blocked words in the group name or alias.
Microsoft Teams | Microsoft Teams shows the group naming policy enforced name when the user types a team name. When a user enters a custom blocked word, an error message is shown along with the blocked word so that the user can remove it.
SharePoint  |  SharePoint shows the naming policy enforced name when the user types a site name or group email address. When an user enters a custom blocked word, an error message is shown, along with the blocked word so that the user can remove it.
Microsoft Stream | Microsoft Stream shows the the naming policy enforced name when the user types a group name or group email alias. When an user enters a custom blocked word, an error message is shown with the blocked word so the user can remove it.
Outlook iOS and Android App | Groups created in Outlook apps are compliant with naming policy. Outlook mobile app doesn't yet show the preview of the naming policy and doesn't return the custom blocked word errors, when the user enters the group name. However, naming policy is automatically applied on clicking create/edit and users see error messages if there are custom blocked words in the group name or alias.
Groups mobile app | Groups created in Groups mobile app are compliant with naming policy. Groups mobile app does not show the preview of the naming policy and does not return the custom blocked word errors, when the user enters the group name. But the naming policy is automatically applied on clicking create/edit and users is presented with appropriate errors if there are custom blocked words in the group name or alias.
Planner | Planner is compliant with naming policy. Planner shows the naming policy preview when entering the Plan name. When a user enters a custom blocked word, an error message is shown when creating the plan.
Dynamics 365 for Customer Engagement | Dynamics 365 for Customer Engagement is compliant with naming policy. Dynamics 365 shows the naming policy enforced name when the user types a group name or group email alias. When the user enters a custom blocked word, an error message is shown with the blocked word so the user can remove it.
School Data Sync (SDS) | Groups created through SDS comply with naming policy, but the naming policy isn't applied automatically. SDS administrators have to append the prefixes and suffixes to class names for which groups need to be created and then uploaded to SDS. Group create or edit would fail otherwise.
Outlook Customer Manager (OCM) | Outlook Customer Manager is compliant with naming policy. The naming policy gets automatically applied to the group created in Outlook Customer Manager. If any of the words is defined as a custom blocked word, group creation in OCM is blocked, and the user is blocked from using the OCM app.
Classroom app | Groups created in classroom app comply with naming policy, but the naming policy isn't applied automatically, and the naming policy preview isn't shown to the users while entering a classroom group name. Users must enter the enforced classroom group name with prefixes and suffixes. If not, the classroom group create or edit operation fails with errors.
Power BI | Power BI workspaces aren't compliant with naming policy yet. Power BI workspaces created in your organization with naming policies won't work.
Yammer | Yammer connected groups don't enforce naming policy. For organizations with naming policy enabled, Yammer creates legacy Yammer groups that aren't connected to O365 for groups that don't conform to naming policy.
StaffHub  | StaffHub teams do not follow the naming policy, but the underlying Office 365 group does. StaffHub team name does not apply the prefixes and suffixes and does not check for custom blocked words. But StaffHub does apply the prefixes and suffixes and removes blocked words from the underlying Office 365 group.
Exchange PowerShell | Exchange PowerShell cmdlets are compliant with naming policy. Users receive appropriate error messages with suggested prefixes and suffixes and for custom blocked words if they don't follow the naming convention in group names and group alias.
Azure Active Directory PowerShell cmdlets | Azure Active Directory PowerShell cmdlets are compliant with naming policy. Users receive appropriate error messages with suggested prefixes and suffixes and for custom blocked words if they don't follow the naming convention in group names and group alias.
Exchange admin center | Exchange admin center is compliant with naming policy. Users receive appropriate error messages with suggested prefixes and suffixes and for custom blocked words if they don't follow the naming convention in group names and group alias.
Office 365 admin center | Office 365 Admin center is compliant with naming policy. When adding or changing group names, naming policy is automatically applied, and users receive appropriate errors when they enter custom blocked words. Office 365 Admin center doesn't yet show the preview of the naming policy and doesn't return custom blocked word errors when the user enters the group name.
