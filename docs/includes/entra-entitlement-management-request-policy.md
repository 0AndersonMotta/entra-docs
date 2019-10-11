---
title: include file
description: include file
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 07/31/2019
ms.author: rolyon
ms.custom: include file
---

## For users in your directory

Follow these steps if you want your request policy to be for users in your directory that can request this access package. When defining the request policy, you can specify individual users, or more commonly groups of users. For example, your organization may already have a group such as **All employees**.  If that group is added in the policy for users who can request access, then any member of that group can then request access.

1. In the **Users who can request access** list, select **For users in your directory**.

    When you select this option, new options appear to further refine who in your directory can request this access package.

    ![Access package - Requests - For users in your directory](./media/active-directory-entitlement-management-request-policy/for-users-in-your-directory.png)

1. Select one of the following options:

    |  |  |
    | --- | --- |
    | **Specific users and groups** | Choose this option if you want only the users and groups in your directory that you specify to be able to request this access package. |
    | **All members (excluding guests)** | Choose this option if you want all member users in your directory to be able to request this access package. This option does not include any guest users you might have invited into your directory. |
    | **All users (including guests)** | Choose this option if you want all member users and guest users in your directory to be able to request this access package. |

    Guest users refer to external users that have been invited into your directory with [Azure AD B2B](../articles/active-directory/b2b/what-is-b2b.md). For information about the differences between member users and guest users, see [What are the default user permissions in Azure Active Directory?](../articles/active-directory/fundamentals/users-default-permissions.md).

1. If you selected **Specific users and groups**, click **Add users and groups**.

1. In the Select users and groups pane, select the users and groups you want to add.

    ![Access package - Requests - Select users and groups](./media/active-directory-entitlement-management-request-policy/select-users-groups.png)

1. Click **Select** to add the users and groups.

1. Skip down to the [Approval](#approval) section.

## For users not in your directory

Follow these steps if you want your request policy to be for users not in your directory that can request this access package. The **users not in your directory** refers to users who are in another Azure AD directory or domain, and may not have yet been invited into your directory. Azure AD directories must be configured to be allow invitations in **Collaboration restrictions**. For more information, see [Enable B2B external collaboration and manage who can invite guests](../articles/active-directory/b2b/delegate-invitations.md).

> [!NOTE]
> A guest user account will be created for a user not yet in your directory whose request is approved or auto-approved. The guest will be invited, but will not receive an invite email. Instead, they will receive an email when their access package assignment is delivered. By default, later when that guest user no longer has any access package assignments, because their last assignment has expired or been cancelled, that guest user account will be blocked from sign in and subsequently deleted. If you want to have guest users remain in your directory indefinitely, even if they have no access package assignments, you can change the settings for your entitlement management configuration. For more information about the guest user object, see [Properties of an Azure Active Directory B2B collaboration user](../articles/active-directory/b2b/user-properties.md).

1. In the **Users who can request access** list, select **For users not in your directory**.

    When you select this option, new options appear.

    ![Access package - Requests - For users not in your directory](./media/active-directory-entitlement-management-request-policy/for-users-not-in-your-directory.png)

1. Select one of the following options:

    |  |  |
    | --- | --- |
    | **Specific connected organizations** | Choose this option if you want to select from a list of organizations that your administrator previously added. All users from the selected organizations will be able to request this access package. |
    | **All connected organizations** | Choose this option if you want all users from all your connected organizations to be able to request this access package. |

    A connected organization is an external Azure AD directory or domain that you frequently collaborate with.

1. If you selected **Specific connected organizations**, click **Add directories** to select from a list of connected organizations that your administrator previously added.

1. Enter a domain name to search for a connected organization with that domain name.

    ![Access package - Requests - Select directories](./media/active-directory-entitlement-management-request-policy/select-directories.png)

    If the organization you want to collaborate with is not in the list, you can ask your administrator to add it as a connected organization. For more information, see Add a connected organization. 

1. Once you have selected all your connected organizations, click **Select**.

    > [!NOTE]
    > All users from the selected connected organizations will be able to request this access package. This includes users from all subdomains associated with the connected organizations.

1. Skip down to the [Approval](#approval) section.

## None (administrator direct assignments only)

Follow these steps if you want your request policy to bypass access requests and allow administrators to directly assign specific users to the access package. Users won't have to request the access package. You can still set expiration settings, but there are no request settings.

1. In the **Users who can request access** list, select **None (administrator direct assignments only**.

    ![Access package - Requests - None administrator direct assignments only](./media/active-directory-entitlement-management-request-policy/none-admin-direct-assignments-only.png)

    After you create the access package, you can directly assign specific internal and external users to the access package. If you specify an external user, a guest user account will be created in your directory. For information about directly assigning a user, see [View and change assignments for an access package](../articles/active-directory/governance/entitlement-management-access-package-assignments.md).

1. Skip down to the [Enable requests](#enable-requests) section.

## Approval

In the Approval section, you specify approval settings when users request the access package.

1. To require approval for requests from the selected users, set the **Require approval** toggle to **Yes**. To have requests automatically approved, set the toggle to **No**.

1. If you require approval, in the **Select approvers** section, click **Add approvers**.

1. In the Select approvers pane, select one or more users and/or groups to be approvers.

    Only one of the selected approvers needs to approve a request. Approval from all approvers is not required. The approval decision is based on whichever approver reviews the request first.

    ![Access package - Policy- Select approvers](./media/active-directory-entitlement-management-request-policy/policy-select-approvers.png)

1. Click **Select** to add the approvers.

1. Click **Show advanced request settings** to show additional settings.

    ![Access package - Policy- Select directories](./media/active-directory-entitlement-management-request-policy/policy-advanced-request.png)

1. To require users to provide a justification to request the access package, set **Require justification** to **Yes**.

1. To require the approver to provide a justification to approve a request for the access package, set **Require approver justification** to **Yes**.

1. In the **Approval request timeout (days)** box, specify the amount of time the approvers have to review a request. If no  approvers review it in this number of days, the request expires and the user will have to submit another request for the access package.

## Enable requests

1. If you want the access package to be made immediately available to the users in the policy, click **Yes** to enable the policy.

    You can always enable it in the future after you have finished creating the access package.

    ![Access package - Policy- Enable policy setting](./media/active-directory-entitlement-management-request-policy/policy-enable.png)

1. Click **Next** or **Create**.
