---
title: Create and manage a catalog in Azure AD entitlement management (Preview)
description: #Required; article description that is displayed in search results.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: HANKI
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/15/2019
ms.author: rolyon
ms.reviewer: hanki
ms.collection: M365-identity-device-management


#Customer intent: As a < type of user >, I want < what? > so that < why? >.

---
# Create and manage a catalog in Azure AD entitlement management (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) entitlement management is currently in public preview.
> This preview version is provided without a service level agreement, and it's not recommended for production workloads. Certain features might not be supported or might have constrained capabilities.
> For more information, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## Create a catalog

You create a catalog when you want to group related resources and access packages. Whoever creates the catalog becomes the Catalog Owner. A Catalog Owner can add additional Catalog Owners.

**Prerequisite role:** User administrator or Catalog creator

1. Sign in to the [Azure portal](https://portal.azure.com) and open the **Identity Governance** page.

1. Click **Catalogs**.

1. Click **New catalog**.

1. Enter a unique name for the catalog and provide a description.

    Users will see this information in an access package's details.

1. Click **Create**.

## Add resources to a catalog

To include resources in an access package, you must first add the resources to the catalog. The types of resources you can add include groups, applications, and SharePoint Online sites.

**Prerequisite role:** User administrator or Catalog owner

1. On the **Catalogs** page, click to open the catalog you want to add resources to.

1. Click **Resources**.

1. Click **Add resources**.

1. Select a resource type.

1. Select one or more resources of the type that you would like to add to the catalog.

1. When finished, click **Add**.

    These resources can now be included in access packages within the catalog.

## Remove resources from a catalog

You can remove resources from a catalog. A resource can only be removed from a catalog if it is not being used in any of the catalog's access packages.

**Prerequisite role:** User administrator or Catalog owner

1. Open the **Resources** page for the catalog that has the resources you want to remove.

1. Select the resources you want to remove.

1. Click **Remove** (or open the context menu for the resource and click **Remove**).

## Edit a catalog

You can edit the name and description for a catalog. Users see this information in an access package's details.

**Prerequisite role:** User administrator or Catalog owner

1. On the **Catalogs** page, open the catalog you want to edit.

1. On the catalog's **Overview** page, click **Edit**.

1. Edit the catalog's name or description.

1. Click **Save**.

## Delete a catalog

You can delete a catalog, but only if it does not have any access packages.

**Prerequisite role:** User administrator or Catalog owner

1. On the **Catalogs** page, open the catalog you want to delete.

1. On the catalog's **Overview**, click **Delete**.

1. In the message box that appears, click **Yes**.

## Next steps

- [Create and manage an access package](entitlement-management-create-access-package.md)
