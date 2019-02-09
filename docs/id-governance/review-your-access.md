---
title: Review your own access with Azure AD Access Reviews | Microsoft Docs
description: Learn how to review your own access by using Azure Active Directory Access Reviews.
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
ms.date: 02/09/2019
ms.author: rolyon
ms.reviewer: mwahl
---

# Review your own access with Azure AD Access Reviews

Azure Active Directory (Azure AD) simplifies how enterprises manage access to applications and members of groups in Azure AD and other Microsoft Online Services with a feature called access reviews. Perhaps you received an email from Microsoft that asks you to review access for members of a group or users with access to an application.

This article describes how to review your own access.

## Open the access review

The first step to perform an access review is to find and open the access review.

1. Open your email and look for an email from Microsoft that asks you to review access. Here is an example email to review your access to a group.

    ![Review access email](./media/review-your-access/access-review-email.png)

1. Click the **Review access >** link to open the access review.

If you don't have the email, you can open your access reviews by following these steps.

1. Sign in to the [Azure AD access panel](https://myapps.microsoft.com).

    ![Azure AD access panel](./media/review-your-access/myapps-access-panel.png)

1. In the upper-right corner of the page, click the user symbol, which displays your name and default organization. If more than one organization is listed, select the organization that requested an access review.

1. On the right side of the page, click the **Access reviews** tile to see a list of the pending access reviews.

    If the tile isn't visible, there are no access reviews to perform for that organization and no action is needed at this time.

    ![Access reviews list](./media/review-your-access/access-reviews-list.png)

1. Click the **Begin review** link for the access review you want to perform.

## Perform the access review

Once you have opened the access review, you can see your access.

1. Review your access and decide whether you still need access.

    If the request was to review access for others, see [Start an access review](perform-access-review.md).

    ![Perform access review](./media/review-your-access/perform-access-review.png)

1. Click **Yes** to keep your access or click **No** to remove your access.

1. If you click **Yes**, you might need to specify a justification in the **Reason** box.

    ![Perform access review](./media/review-your-access/perform-access-review-submit.png)

1. Click **Submit**.

## Next steps

- [Complete an access review of members of a group or users' access to an application in Azure AD](complete-access-review.md)
