﻿---

title: Restore a deleted user in Azure Active Directory | Microsoft Docs
description: How to restore a deleted user, view restorable users, and permamnently delete a user in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: femila
editor: ''

ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/292017
ms.author: curtand                       
ms.reviewer: jeffsta
ms.custom: it-pro

---

# Restore a deleted user in Azure Active Directory

When you delete a user in the Azure Active Directory (Azure AD), the deleted group is retained but not visible for 30 days from the deletion date. This is so that the user and its properties can be restored if needed. 

## Reuired permissions
The permissions required to restore a group can be any of the following:

Role  | Permissions 
--------- | ---------
Company Administrator, Partner Tier2 support, and InTune Service Admins | Can restore any deleted user 
User Account Administrator and Partner Tier1 support | Can restore any deleted user except those assigned to the Company Administrator role

## How to restore a deleted user
In the Azure portal, you can now
* Restore a deleted user 
* Permanently delete a user 

1. In the [Azure AD admin center](https://aad.portal.azure.com), select **Users and groups** &gt; **All users**. 
2. Under **Show**, filter the page to show **Recently deleted users**. 
3. Select one or more recently deleted users.
4. Select **Restore user** or **Delete permanently**.

## Next steps
These articles provide additional information on Azure Active Directory user management.

* [Quickstart: Add or delete users to Azure Active Directory](add-users-azure-active-directory.md)
* [Manage user profiles](active-directory-users-profile-azure-portal.md)
* [Add guest users from another directory](active-directory-b2b-what-is-azure-ad-b2b.md) 
* [Assign a user to a role in your Azure AD](active-directory-users-assign-role-azure-portal.md)
