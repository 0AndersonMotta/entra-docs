---
title: Review your access to groups & apps in access reviews - Azure AD
description: Learn how to review your own access to groups or applications in Azure Active Directory access reviews.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/21/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
---

# Review access for yourself to groups or applications in Azure AD access reviews

Azure Active Directory (Azure AD) simplifies how enterprises manage access to groups or applications in Azure AD and other Microsoft Online Services with a feature called Azure AD access reviews.

This article describes how to review your own access to a group or an application.

## Open the access review

The first step to perform an access review is to find and open the access review.

1. Look for an email from Microsoft that asks you to review access. Here is an example email to review your access to a group.

    ![Example email from Microsoft to review your access to a group](./media/review-your-access/access-review-email.png)

1. Click the **Review access** link to open the access review.

If you don't have the email, you can find your pending access reviews by following these steps.

1. Sign in to the MyApps portal at [https://myapps.microsoft.com](https://myapps.microsoft.com).

    ![MyApps portal listing apps you have permissions to](./media/review-your-access/myapps-access-panel.png)

1. In the upper-right corner of the page, click the user symbol, which displays your name and default organization. If more than one organization is listed, select the organization that requested an access review.

1. On the right side of the page, click the **Access reviews** tile to see a list of the pending access reviews.

    If the tile isn't visible, there are no access reviews to perform for that organization and no action is needed at this time.

    ![Pending access reviews list for your apps and groups](./media/review-your-access/access-reviews-list.png)

1. Click the **Begin review** link for the access review you want to perform.

## Perform the access review

Once you have opened the access review, you can see your access.

1. Review your access and decide whether you still need access.

    If the request is to review access for others, the page will look different. For more information, see [Review access to groups or applications](perform-access-review.md).

    ![Open access review asking whether you still need access to a group](./media/review-your-access/perform-access-review.png)

1. Click **Yes** to keep your access or click **No** to remove your access.

1. If you click **Yes**, you might need to specify a justification in the **Reason** box.

    ![Completed access review asking whether you still need access to a group](./media/review-your-access/perform-access-review-submit.png)

1. Click **Submit**.

    Your selection is submitted and you returned to the MyApps portal.

    If you want to change your response, re-open the access reviews page and update your response. You can change your response at any time until the access review has ended.

    > [!NOTE]
    > If you indicated that you no longer need access, you aren't removed immediately. You are removed when the review has ended or when an administrator stops the review.

## Review your own access to groups and apps using My Access (Preview)

### Open the access review

The first step to perform an access review is to find and open the access review.

  >[!IMPORTANT]
> There could be delays in receiving email and it some cases it could take up to 24 hours. Whitelist azure-noreply@microsoft.com to make sure that you are receiving all emails.

   1. Look for an email from Microsoft asking you to review access. You can see an example email message below:

   ![Example email from Microsoft to review access to a group](./media/review-your-access/access-review-email-preview.png)

   2. Click the **Start review** link to open the access review.

If you don't have the email, you can find your pending access reviews by following these steps.

ou can also view your pending access reviews by using your browser to open My Access.

1. Sign  in to the My Access at ht<span>tps://<span>myacces<span>s.m<span>icrosof<span>t.<span>com/@**Castelia<span>.<span>onmicrosoft<span>.<span>com**?enableReviews=true#/access-reviews

  >[!IMPORTANT]
  >Replace `Castelia.onmicrosoft.com` with your domain URL between the **@** and **?**.

2. Select **Access reviews** from the menu on the left side bar to see a list of pending access reviews assigned to you.

   ![access reviews in the menu](./media/review-your-access/access-review-menu.png)

3. Under Groups and Apps you can see:
    - **Name** The name of the access review.
    - **Due** The due date for the review. After this date denied users could be removed from the group or app being reviewed. 
    - **Resource** Users access to the resourced named in this column is being evaluated as part of this access review.
    - **Progress** The number of users reviewed over the total number of users part of this access review.

4. Click on the name of an Access review to get started.
 
   ![Pending access reviews list for apps and groups](./media/perform-access-review/access-reviews-list-preview.png)

Once that it opens, you will see the list of users in scope for the access review. If the request is to review your own access, the page will look different. For more information, see [Review access for yourself to groups or applications](review-your-access.md).

### Perform the access review

Once you have opened the access review, you can see your access.

1. Review your access and decide whether you still need access.

    If the request is to review access for others, the page will look different. For more information, see [Review access to groups or applications](perform-access-review.md).

    ![Open access review asking whether you still need access to a group](./media/review-your-access/perform-access-review.png)

1. Click **Yes** to keep your access or click **No** to remove your access.

1. If you click **Yes**, you might need to specify a justification in the **Reason** box.

    ![Completed access review asking whether you still need access to a group](./media/review-your-access/perform-access-review-submit.png)

1. Click **Submit**.

    Your selection is submitted and you returned to the MyApps portal.

    If you want to change your response, re-open the access reviews page and update your response. You can change your response at any time until the access review has ended.

    > [!NOTE]
    > If you indicated that you no longer need access, you aren't removed immediately. You are removed when the review has ended or when an administrator stops the review.

## Next steps

- [Complete an access review of groups or applications](complete-access-review.md)
