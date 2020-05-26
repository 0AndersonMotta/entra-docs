---
title: Create a group for assigning roles in Azure Active Directory | Microsoft Docs
description: Preview custom Azure AD roles for delegating identity management. Manage Azure roles in the Azure portal, PowerShell, or Graph API.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/07/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro

ms.collection: M365-identity-device-management
---

# Create a role-assignable group in Azure Active Directory

You can only assign a role to a group that was created with the ‘isAssignableToRole’ property set to True, or was created in the Azure AD portal with **Azure AD roles can be assigned to the group** turned on. This group attribute makes the group one that can be assigned to a role in Azure Active Directory (Azure AD). This article describes how to create this special kind of group.

## Using Azure AD admin center

1. Sign in to the [Azure AD admin center](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) with Privileged role administrator or Global administrator permissions in the Azure AD organization.
1. Select **Groups** > **All groups** > **New group**.

   ![Open Azure Active Directory and create a new group](./media/roles-groups-create-eligible/new-group.png)

1. On the **New Group** tab, provide group type, name and description.
1. Turn on **Azure AD roles can be assigned to the group**. This switch is visible to only Privileged Role Administrators and Global Administrators because these are only two roles that can set the switch.

   ![Make the new group eligible for role assignment](./media/roles-groups-create-eligible/eligible-switch.png)

1. Select the members and roles for the group. You don't have to assign roles when you create the group.

   ![Add members at the role-assignable group and assign roles](./media/roles-groups-create-eligible/specify-members.png)

1. After members and roles are selected, select **Create**.

   ![The Create button is at the bottom of the page](./media/roles-groups-create-eligible/create-button.png)

The group is created with any selected roles assigned to it. You can also choose not to assign roles during creation and assign them later on.

## Using PowerShell

To install the preview module:

    install-module azureadpreview
    import-module azureadpreview

To verify that the module is ready to use, issue the following command:

    get-module azureadpreview

### Create a group that can be assigned to role

    $group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $true -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true

For this type of group, `isPublic` will always be false and `isSecurityEnabled` will always be true.

### Copy one group's users and service principals into a role-assignable group

    #Basic set up
    install-module azureadpreview
    import-module azureadpreview
    get-module azureadpreview
    
    #Connect to Azure AD. Sign in as Privileged Role Administrator or Global Administrator. Only these two roles can create a role-assignable group.
    Connect-AzureAD
    
    #Input variabled: Existing group
    $idOfExistingGroup = "14044411-d170-4cb0-99db-263ca3740a0c"
    
    #Input variables: New role-assignable group
    $groupName = "Contoso_Bellevue_Admins"
    $groupDescription = "This group is assigned to Helpdesk Administrator built-in role in Azure AD."
    $mailNickname = "contosobellevueadmins"
    
    #Create new security group which is a role assignable group. For creating O365 group, set GroupTypes="Unified" and MailEnabled=$true
    $roleAssignablegroup = New-AzureADMSGroup -DisplayName $groupName -Description $groupDescription -MailEnabled $false -MailNickname $mailNickname -SecurityEnabled $true -IsAssignableToRole $true
    
    #Get details of existing group
    $existingGroup = Get-AzureADMSGroup -Id $idOfExistingGroup
    $membersOfExistingGroup = Get-AzureADGroupMember -ObjectId $existingGroup.Id

    #Copy users and service principals from existing group to new group
    foreach($member in $membersOfExistingGroup){
    if($member.ObjectType -eq 'User' -or $member.ObjectType -eq 'ServicePrincipal'){
    Add-AzureADGroupMember -ObjectId $roleAssignablegroup.Id -RefObjectId $member.ObjectId
    }
    }

## Using Microsoft Graph API

### Create a role-assignable group in Azure AD

    POST https://graph.microsoft.com/beta/groups
    {
    "description": "This group is assigned to Helpdesk Administrator built-in role of Azure AD.",
    "displayName": "Contoso_Helpdesk_Administrators",
    "groupTypes": [
    "Unified"
    ],
    "mailEnabled": true,
    "securityEnabled": true
    "mailNickname": "contosohelpdeskadministrators",
    "isAssignableToRole": true,
    }
For this type of group, `isPublic` will always be false and `isSecurityEnabled` will always be true.

## Next steps

- [Use cloud groups to manage role assignments](roles-groups-concept.md)
- [Troubleshooting roles assigned to cloud groups](roles-groups-faq-troubleshooting.md)
