---
title: How to add or remove users in Azure Active Directory | Microsoft Docs
description: Learn how to add new users or remove existing users using the Azure Active Directory portal.
services: active-directory
author: eross-msft
manager: mtillman

ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/04/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
---

# How to: Add new or remove users using the Azure Active Directory portal
Add new users or remove existing users from your Azure Active Directory (Azure AD) tenant, using the Azure AD portal.

## Add a new user
You can create a new user using the Azure AD portal.

### To add a new user
1. Sign in to the [Azure AD portal](https://portal.azure.com/) using a Global administrator account for the directory.

2. Select **Azure Active Directory**, select **Users**, and then select **New user**.

    ![Users - All users blade with New user highlighted](media/add-users-azure-active-directory/new-user-all-users-blade.png)

3. On the **User** blade, fill out the required information.

    ![Add new user, User blade with user info](media/add-users-azure-active-directory/new-user-user-blade.png)

    - **Name (required).** The first and last name of the new user. For example, Mary Parker.

    - **User name (required).** The user name of the new user. For example, marpa@contoso.com. 
    
        The domain part of the user name must use either the initial default domain name, <_yourdomainname_>.onmicrosoft.com, or a custom domain name, such as contoso.com. For more information about how to create a custom domain name, see [How to add a custom domain name to Azure Active Directory](add-custom-domain.md).

    - **Profile.** Optionally, you can add more information about the user. You can also do this after you've finished adding the user. For more information about adding user info, see [How to add or change user profile information](active-directory-users-profile-azure-portal.md).

    - **Properties.** Optionally, you can change the **Source of authority** for the user. The default value for this is _Azure Active Directory_. For more information about changing a user's source of authority, see [How to change a user's source of authority information](active-directory-users-properties-azure-portal.md).

    - **Groups.** Optionally, you can add the user to an existing group. You can also do this after you've finished adding the user. For more information about adding users to groups, see [How to create a basic group and add members](active-directory-groups-create-azure-portal.md).

    - **Directory role.** Optionally, you can change the user's role. The available options are User, Global administrator, and Limited administrator. For more information about each of these roles, see [How to assign roles to users](active-directory-users-assign-role-azure-portal.md).

4. Copy the auto-generated password provided in the **Password** box. You'll need to give this to the user for the intial sign-in process.

5. Select **Create**.

    The user is created and added to your Azure AD tenant.

## Add a new user within a hybrid environment
If you have an environment with both Azure Active Directory (cloud) and Windows Server Active Directory (on-premises), you can add new users by syncing the existing user account data. For more information about hybrid environments and users, see [Azure Active Directory B2B collaboration for hybrid organizations](../b2b/hybrid-organizations.md).

## Delete a user
You can delete an existing user using the Azure AD portal.

### To delete a user
1. Sign in to the [Azure AD portal](https://portal.azure.com/) using a Global administrator account for the directory.

2. Select **Azure Active Directory**, select **Users**, and then search for and select the user you want to delete from your Azure AD tenant. For example, _Mary Parker_.

3. Select **Delete user**.

    ![Users - All users blade with Delete user highlighted](media/add-users-azure-active-directory/delete-user-all-users-blade.png)

    The user is deleted and no longer appears on the **Users - All users** blade.

## Next steps

- [Add guest users from another directory](../b2b/what-is-b2b.md)

- [Restore a deleted user](active-directory-users-restore.md)