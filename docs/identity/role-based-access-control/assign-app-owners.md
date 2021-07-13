---
title: Assign application owners - Azure AD | Microsoft Docs
description: Assign owners to applications in Azure Active Directory
services: active-directory
documentationcenter: ''
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: how-to
ms.date: 11/04/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
#Customer intent: As an Azure AD administrator, I want to assign owners to applications.

---

# Assign application owners

Assigning owners is a simple way to grant the ability to manage all aspects of Azure AD configuration for a specific application registration or enterprise application. As a system behavior, when a user creates a new application registration they are automatically added as the first owner. Ownership permissions give the user the ability to manage all aspects of an application registration or enterprise application that they own. For example, when a user leaves an organization, they can be removed as an owner and additional owners can be added.

## Enterprise application owners

As an owner, a user can manage the organization-specific configuration of the enterprise application, such as the single sign-on configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the enterprise applications they own.

Users and service principals can be owners of application registrations. Only users can be owners of enterprise applications. Groups cannot be assigned as owners of either. Owners can add credentials to an application and use those credentials to impersonate the application’s identity. The application may have more permissions than the owner, and thus would be an elevation of privilege over what the owner has access to as a user or service principal. An application owner could potentially create or update users or other objects while impersonating the application, depending on the application's permissions.

In some cases, enterprise applications created from the application gallery include both an enterprise application and an application registration. When this is true, adding an owner to the enterprise application automatically adds the owner to the corresponding application registration as an owner.

## To assign an owner to an enterprise application

1. Sign in to [your Azure AD organization](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) with an account that is eligible for the **Application Administrator** role or the **Cloud Application Administrator** role for the organization.
2. Select **Enterprise applications**, and then select the application that you want to add an owner to.
3. Select **Owners**, and then select **Add** to get a list of user accounts that you can choose an owner from.
4. Search for and select the user account that you want to be an owner of the application.
5. Click **Select** to add the user account that you chose as an owner of the application.

> [!NOTE]
> If the user setting **Restrict access to Azure AD administration portal** is set to `Yes`, non-admin users will not be able to use the Azure portal to manage the applications they own. For more information about the actions that can be performed on owned enterprise applications, see [Owned enterprise applications](../fundamentals/users-default-permissions#owned-enterprise-applications). 

## Next steps

- [Delegate app registration permissions in Azure Active Directory](delegate-app-roles.md)
- [Azure AD built-in roles](permissions-reference.md)
