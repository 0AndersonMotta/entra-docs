---
title: Grant the ability to create an unlimited number of app registrations - Azure Active Directory | Microsoft Docs
description: You can assign a custom Azure AD administrator role to create any number of app registrations in the Azure AD admin center.
services: active-directory
author: curtand
manager: mtillman

ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 06/30/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro

ms.collection: M365-identity-device-management
---
# Quickstart: Grant the ability to create an unlimited number of app registrations

In this quickstart, you will create a custom role with permission to create an unlimited number of app registrations, and then assign that role to a user. The assigned user can then use the Azure AD portal, Azure AD PowerShell, Azure AD Graph API, or Microsoft Graph API to create application registrations. Unlike the built-in Application Developer role, this custom role grants the ability to create an unlimited number of application registrations. The Application Developer role grants the ability, but the total number of created objects is limited to 250 to prevent hitting the directory-wide object quota.

If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.

## Prerequisite

The least privileged role required to create and assign Azure AD custom roles is the Privileged role administrator.

## Create a new custom role using the Azure AD portal

1. Sign in to the [Azure AD admin center](https://aad.portal.azure.com) with Privileged role administrator or Global administrator permissions in the Azure AD organization.
1. Select **Azure Active Directory**, select **Roles and administrators**, and then select **New custom role**.

    ![Create or edit roles from the Roles and administrators page](./media/roles-create-assignments/new-custom-role.png)

1. On the **Basics** tab, provide "Application Registration Creator" for the name of the role and "Can create an unlimited number of application registrations." for the role description.

    ![provide a name and description for a custom role on the Basics tab](./media/roles-quickstart-app-registrations-unlimited/basics-tab.png)

1. On the **Permissions** tab, enter "microsoft.directory/applications/create" into the filter box, and then select it by checking the box next to the permission.

    ![Select the permissions for a custom role on the Permissions tab](./media/roles-quickstart-app-registrations-unlimited/permissions-tab.png)

1. On the **Review + create** tab, review the permissions and select **Create**.

### Assign the role to a user using the Azure AD portal

1. Sign in to the [Azure AD admin center](https://aad.portal.azure.com) with Privileged role administrator or Global administrator permissions in your Azure AD organization.
1. Select **Azure Active Directory** and then select **Roles and administrators**.
1. Select the Application Registration Creator role and select **Add assignment**.
1. Select the desired user and click **Select** to add the user to the role.

Done! In this quickstart, you successfully created a custom role with permission to create an unlimited number of app registrations, and then assign that role to a user.

There are two permissions available for granting the ability to create application registrations, each with different behaviors.

- microsoft.directory/applications/createAsOwner: this permission will result in the creator being added as the first owner of the created app registration, and the created app registration will count against the creator's 250 created objects quota.
- microsoft.directory/applicationPolicies/create: this permission will result in the creator not being added as the first owner of the createa app registration, and the created app registration will not count against the creator's 250 created objects quota. Use this permission carefully, as there is nothing preventing the assignee from creating app registrations until the directory-level quota is hit. If both permissions are assigned, this permission will take precedent.

> [!TIP]
> To assign the role to an application using the Azure AD portal, type the name of the application into the search box of the assignment picker. Applications are not shown in the picker list by default, but will appear when searched for.

## Create a custom role using Azure AD PowerShell

Create a new role using the following PowerShell script:

``` PowerShell
# Basic role information
$description = "Application Registration Creator"
$displayName = "Can create an unlimited number of application registrations."
$templateId = (New-Guid).Guid

# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/createAsOwner"
)
$resourceActions = @{'allowedResourceActions'= $allowedResourceAction}
$rolePermission = @{'resourceActions' = $resourceActions}
$rolePermissions = $rolePermission

# Create new custom admin role
$customRole = New-AzureAdRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### Assign the custom role using Azure AD PowerShell
Assign the role using the below PowerShell script:

``` PowerShell
# Scopes to scope granted permissions to
$resourceScopes = @('/')

# IDs of principal and role definition you want to link

$principalId = "<ObjectId of the user to assign role to>"
$roleDefinitionId = $customKeyCredAdmin.ObjectId

# Create a scoped role assignment

$roleAssignment = New-AzureADRoleAssignment -ResourceScopes $resourceScopes -RoleDefinitionId $customRole.ObjectId -PrincipalId $principalId
```

### Create a custom role using Microsoft Graph API

HTTP request to create the custom role.

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions
```

Body

```HTTP
{
    "description":"Can create an unlimited number of application registrations.",
    "displayName":"Application Registration Creator",
    "isEnabled":true,
    "rolePermissions":
    [
        {
            "resourceActions":
            {
                "allowedResourceActions": 
                [
                    "microsoft.directory/applications/createAsOwner"
                ]
            },
            "condition":null
        }
    ],
    "templateId":"<PROVIDE NEW GUID HERE>",
    "version":"1"
}
```

### Assign the custom role using Microsoft Graph API

HTTP request to assign a custom role.

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Body

``` HTTP
{
    "principalId":"<PROVIDE OBJECTID OF USER TO ASSIGN HERE>",
    "roleDefinitionId":"<PROVIDE OBJECTID OF ROLE DEFINITION HERE>",
    "resourceScopes":["/"]
}
```

## Clean up resources

### To remove the expiration policy

1. Ensure that you are signed in to the [Azure portal](https://portal.azure.com) with an account that is the Global Administrator for your tenant.
2. Select **Azure Active Directory** > **Groups** > **Expiration**.
3. Set **Enable expiration for these Office 365 groups** to **None**.

### To turn off user creation for groups

## Next steps

- Feel free to share with us on the [Azure AD administrative roles forum](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
- For more about roles and Administrator role assignment, see [Assign administrator roles](directory-assign-admin-roles.md).
- For default user permissions, see a [comparison of default guest and member user permissions](../fundamentals/users-default-permissions.md).
