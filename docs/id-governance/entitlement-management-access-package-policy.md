---
title: Change user, request, and expiration settings for an access package in Azure AD entitlement management (Preview) - Azure Active Directory
description: Learn how to change user, request, and expiration settings for an access package in Azure Active Directory entitlement management (Preview).
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
# Change user, request, and expiration settings for an access package in Azure AD entitlement management (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) entitlement management is currently in public preview.
> This preview version is provided without a service level agreement, and it's not recommended for production workloads. Certain features might not be supported or might have constrained capabilities.
> For more information, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

As an access package manager, you can change the users who can request an access package at any time by adding a new policy or editing an existing policy. You can also change approval and expiration settings. If you change the expiration date for a policy, the expiration date for requests that are already in a pending approval or approved state will not change.

This article describes how to change user, request, and expiration settings for an existing access package.

## Choose between adding a new policy or editing an existing policy

The way you specify who can request an access package is to create a policy. You can create multiple policies for a single access package if you want to allow different sets of users to be granted assignments with different approval and expiration settings. A single policy cannot be used to assign internal and external users to the same access package. However, you can create two policies in the same access package -- one for internal users and one for external users. If there are multiple policies that apply to a user, they will be prompted at the time of their request to select the policy they would like to be assigned to.

![Multiple policies in an access package](./media/entitlement-management-access-package-policy/access-package-policy.png)

### Add a new policy

Follow these steps to start adding a new policy to an existing access package.

**Prerequisite role:** Global administrator, User administrator, Catalog owner, or Access package manager

1. In the Azure portal, click **Azure Active Directory** and then click **Identity Governance**.

1. In the left menu, click **Access packages** and then open the access package.

1. Click **Policies** and then **Add policy**.

1. Type a name and a description for the policy.

    ![Create policy with name and description](./media/entitlement-management-access-package-policy/policy-name-description.png)

1. Perform the steps in one of the following policy sections.

### Edit an existing policy

Follow these steps to start editing a policy in an existing access package.

**Prerequisite role:** Global administrator, User administrator, Catalog owner, or Access package manager

1. In the Azure portal, click **Azure Active Directory** and then click **Identity Governance**.

1. In the left menu, click **Access packages** and then open the access package.

1. Click **Policies** and then click the policy you want to edit.

    The **Policy details** pane opens at the bottom of the page.

    ![Access package - Policy details pane](./media/entitlement-management-access-package-policy/policy-details.png)

1. Click **Edit** to edit the policy.

    ![Access package - Edit policy](./media/entitlement-management-access-package-policy/policy-edit.png)

1. Perform the steps in one of the following policy sections.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## Next steps

- [Change resource roles for an access package](entitlement-management-access-package-resources.md)
- [View requests for an access package](entitlement-management-access-package-requests.md)