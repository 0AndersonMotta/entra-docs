---
title: Review access to groups or applications in Azure AD Access Reviews | Microsoft Docs
description: Learn how to review access of group members or application access in Azure Active Directory Access Reviews.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 02/19/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
---

# Review access to groups or applications in Azure AD Access Reviews

Azure Active Directory (Azure AD) simplifies how enterprises manage access to groups and applications in Azure AD and other Microsoft Online Services with a feature called Azure AD Access Reviews.

This article describes how to perform an access review for members of a group or users with access to an application.

## Open the access review

The first step to perform an access review is to find and open the access review.

1. Open your inbox and look for an email from Microsoft that asks you to review access. Here is an example email to review the access for a group.

    ![Review access email](./media/perform-access-review/access-review-email.png)

1. Click the **Start review >** link to open the access review.

If you don't have the email, you can open your access reviews by following these steps.

1. Sign in to the MyApps portal at [https://myapps.microsoft.com](https://myapps.microsoft.com).

    ![MyApps portal](./media/perform-access-review/myapps-access-panel.png)

1. In the upper-right corner of the page, click the user symbol, which displays your name and default organization. If more than one organization is listed, select the organization that requested an access review.

1. On the right side of the page, click the **Access reviews** tile to see a list of the pending access reviews.

    If the tile isn't visible, there are no access reviews to perform for that organization and no action is needed at this time.

    ![Access reviews list](./media/perform-access-review/access-reviews-list.png)

1. Click the **Begin review** link for the access review you want to perform.

## Perform the access review

Once you have opened the access review, you see the names of users who need to be reviewed.

If the request is to review your own access, the page will look different. For more information, see [Review access for yourself](review-your-access.md).

![Perform access review](./media/perform-access-review/perform-access-review.png)

There are two ways that you can approve or deny access:

- You can approve or deny each request individually, or
- You can accept the system recommendations, which is the easiest and quickest way.

### Approve or deny access for each request

1. Review the list of users to decide whether to approve or deny their continued access.

1. To approve or deny each request, click the row to open the window to specify the action to take.

1. Click **Approve** or **Deny**. (If you don't know the user, you can indicate that too.)

    ![Perform access review](./media/perform-access-review/approve-deny.png)

    The reviewer might require that you supply a reason for approving continued access or group membership.

1. Once you have specified the action to take, click **Save**.

    If you want to change your response, for example, and approve a previously denied user or deny a previously approved user, select the row and update the response. You can do this step until the access review is finished. If there are multiple reviewers, the last submitted response would be recorded.

    > [!NOTE]
    > If a user is denied access, they aren't removed immediately. They are removed when the review is finished or when an administrator stops the review.

### Approve or deny access based on recommendations

To make access reviews easier and faster for you, we also provide recommended actions that you can accept with a single click. The recommended actions are generated based on your sign-in and the user's activity, so you can have confidence in the recommendations.

1. In the blue bar at the bottom of the page, click **Accept recommendations**.

    ![Accept recommendations](./media/perform-access-review/accept-recommendations.png)

    You see a summary of the recommended actions.

    ![Accept recommendations summary](./media/perform-access-review/accept-recommendations-summary.png)

1. Click **Ok** to accept the recommendations.

## Next steps

- [Complete an access review of groups or applications](complete-access-review.md)
