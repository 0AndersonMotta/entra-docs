---
title: Remove role assignments from a group in Azure Active Directory | Microsoft Docs
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

# Remove role assignments from a group in Azure Active Directory

This section describes how an IT admin can remove Azure AD role assigned to group.

## Using Azure admin center

1. Sign in to the [Azure AD admin center](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) with Privileged role administrator or Global administrator permissions in the Azure AD organization.

1. Select **Roles and administrators** > ***role name***.

1. Select the group from which you want to remove the role assignment and select **Remove assignment**.

1. When asked to confirm your action, select **Yes**.

## Using PowerShell

### Create a group that can be assigned to role

    $group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $true -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true

### Get the role definition you want to assign the group to

    $roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Helpdesk Administrator'"

### Create a role assignment

    $roleAssignment = New-AzureADMSRoleAssignment -ResourceScope '/' -RoleDefinitionId $roleDefinition.Id -PrincipalId $group.objectId

### Remove the role assignment

    Remove-AzureAdMSRoleAssignment -Id $roleAssignment.Id 

## Using Microsoft Graph API

    //Create a group that can be assigned Azure AD role. POST https://graph.microsoft.com/beta/groups
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

### Get the role definition

    GET https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions?$filter = displayName eq ‘Helpdesk Administrator’

### Create the role assignment

    POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments 
    { 
    "principalId":"<Object Id of Group>", 
    "roleDefinitionId":"<Id of role definition>", 
    "resourceScope":"/" 
    }

### Delete role assignment

    DELETE https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/<Id of role assignment>

## Next steps

- [Create a role-eligible group](roles-groups-create-eligible.md)
- [Assign a role to a group](roles-groups-assign-role.md)
- [View a group's role assignments](roles-groups-view-assignments.md)
