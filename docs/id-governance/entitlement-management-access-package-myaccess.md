---
title: Share My Access portal link for an access package in Azure AD entitlement management (Preview) - Azure Active Directory
description: Learn how to share My Access portal link for an access package in Azure Active Directory entitlement management (Preview).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 09/26/2019
ms.author: ajburnle
ms.reviewer: 
ms.collection: M365-identity-device-management


#Customer intent: As an administrator, I want detailed information about how I can edit an access package so that requestors have the resources they need to perform their job.

---
# Share My Access portal link for an access package in Azure AD entitlement management (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) entitlement management is currently in public preview.
> This preview version is provided without a service level agreement, and it's not recommended for production workloads. Certain features might not be supported or might have constrained capabilities.
> For more information, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Most users in your directory can sign in to the My Access portal and automatically see a list of access packages they can request. However, for external business partner users that are not yet in your directory, you will need to send them a link that they can use to request an access package. 

It is important that you copy the entire My Access portal link when sending it to an internal business partner. This ensures that the partner will get access to your directory's portal to make their request. 

The link will start with "myaccess", include a directory hint, and end with an access package id. Make sure the link includes all of the following:

 `https://myaccess.microsoft.com/@<directory_hint>#/access-packages/<access_package_id>`

As long as the access package is enabled for external users and you have a policy for the external user's directory, the external user can use the My Access portal link to request the access package.

**Prerequisite role:** Global administrator, User administrator, Catalog owner, or Access package manager

1. In the Azure portal, click **Azure Active Directory** and then click **Identity Governance**.

1. In the left menu, click **Access packages** and then open the access package.

1. On the Overview page, copy the **My Access portal link**.

    ![Access package overview - My Access portal link](./media/entitlement-management-shared/my-access-portal-link.png)

1. Email or send the link to your external business partner. They can share the link with their users to request the access package.

## Next steps


