---
title: Bulk download group membership list - Azure Active Directory portal | Microsoft Docs
description: Add users in bulk in the Azure admin center. 
services: active-directory 
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 08/15/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
---

# Bulk download members of a group (preview) in Azure Active Directory

Azure Active Directory (Azure AD) supports bulk group list download, bulk import for group members, and bulk removal of group members.

## To bulk download group membership

1. [Sign in to your Azure AD organization](https://aad.portal.azure.com) with a User administrator account in the organization. Group owners can also download, import, or remove members.
1. In Azure AD, select **Groups** > **All groups**.
1. Open the group from which you want to dowlonad membership, and then select **Members**.
1. On the **Members** page, select **Download members** to download a CSV file listing the group members.

   ![The Download Members command is on the profile page for the group](./media/groups-bulk-download-members/download-panel.png)

## Check download status

You can see the status of all of your pending bulk requests in the **Bulk operation results (preview)** page.

   ![The Bulk operations results page shows you bulk request status](./media/groups-bulk-download-members/bulk-center.png)

## Next steps

- [Bulk import group members](groups-bulk-import-members.md)
- [Bulk remove group members](groups-bulk-download-members.md)
