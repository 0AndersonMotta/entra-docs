---
title: Create an access review for members of a group or users with access to an application with Azure AD| Microsoft Docs
description: Learn how to create an access review for members of a group or users with access to an application. 
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 11/12/2018
ms.author: rolyon
ms.reviewer: mwahl
---

# Create an access review of group members or application access with Azure AD

Access assignments become stale when users have access they don't need any more. To reduce the risk associated with stale access assignments, administrators can use Azure Active Directory (Azure AD) to create an access review for group members or users assigned to an application. Creating recurring access reviews can be time-saving. If you need to routinely review users who have access to an application or are members of a group, you can define the frequency of those reviews. For more information on these scenarios, see [Manage user access](manage-user-access-with-access-reviews.md) and [Manage guest access](manage-guest-access-with-access-reviews.md).

## Prerequisites

- If this is first time using access reviews, make sure have [enabled access reviews](access-reviews-overview.md).

## Create an access review

1. As a Global Administrator or User Account Administrator, open the [Access Reviews page](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) in the Azure portal.

1. Click **Controls**.

    ![Access review - Controls](./media/create-access-review/controls.png)

1. Click **New access review** to create a new access review.

    ![Create an access review](./media/create-access-review/create-an-access-review.png)

1. Name the access review. Optionally, give the review a description. The name and description are shown to the reviewers.

1. Set the **Start date**. By default, an access review occurs once, starts the same time it's created, and it ends in one month. You can change the start and end dates to have an access review start in the future and last however many days you want.

1. To make the access review recurring, change the **Frequency** setting from **One time** to **Weekly**, **Monthly**, **Quarterly** or **Annually**, and use the **Duration** slider or text box to define how many days each review of the recurring series will be open for input from reviewers. For example, the maximum duration that you can set for a monthly review is 27 days, to avoid overlapping reviews.

1. Use the **End** setting to specify how to end the recurring access review series. The series can end in three ways: it runs continuously to start reviews indefinitely, until a specific date, or after a defined number of occurrences has been completed. You, another User Account Administrator, or another Global Administrator can stop the series after creation by changing the date in **Settings**, so that it ends on that date.

1. In the **Users** section, specify the users that access review applies to. Access reviews can be for the members of a group or for users who were assigned to an application. You can further scope the access review to review only the guest users who are members (or assigned to the application), rather than reviewing all the users who are members or who have access to the application.

1. In the **Reviewers** section, select either one or more people to review all the users in scope. Or you can select to have the members review their own access. If the resource is a group, you can ask the group owners to review. You also can require that the reviewers supply a reason when they approve access.

1. In the **Programs** section, select the program you want to use. You can simplify how to track and collect access reviews for different purposes by organizing them into programs. **Default Program** is always present, or you can create a different program. For example, you can choose to have one program for each compliance initiative or business goal.

### Upon completion settings

1. To specify what happens after a review completes, expand the **Upon completion settings** section.

    ![Upon completion settings](./media/create-access-review/upon-completion-settings.png)

1. If you want to automatically remove access for users that were denied, set **Auto apply results to resource** to **Enable**. If you want to manually apply the results when the review completes, set the switch to **Disable**.

1. Use the **Should reviewer not respond** list to specify what happens for users that are not reviewed by the reviewer within the review period. This setting does not impact users who have been reviewed by the reviewers manually. If the final reviewer's decision is Deny, then the user's access will be removed.

    - **No change** - Leave user's access unchanged
    - **Remove acccess** - Remove user's access
    - **Approve access** - Approve user's access
    - **Take recommendations** - Take the system's recommendation on denying or approving the user's continued access

### Advanced settings

1. To specify additional settings, expand the **Advanced settings** section.

    ![Advanced settings](./media/create-access-review/advanced-settings.png)

1. Set **Show recommendations** to **Enable** to show the reviewers the system recommendations based the user's access information.

1. Set **Require reason on approval** to **Enable** to require the reviewer to supply a reason for approval.

1. Set **Mail notifications** to **Enable** to have Azure AD send email notifications to reviewers when an access review starts, and to administrators when a review completes.

1. Set **Reminders** to **Enable** to have Azure AD send reminders of access reviews in progress to reviewers who have not completed their review.

## Start the access review

Once you have specified the settings for an access review, click **Start**.

By default, Azure AD sends an email to reviewers shortly after the review starts. If you choose not to have Azure AD send the email, be sure to inform the reviewers that an access review is waiting for them to complete. You can show them the instructions for how to [review access](perform-access-review.md). If your review is for guests to review their own access, show them the instructions for how to [review your own access](perform-access-review.md).

If some of the reviewers are guests, guests are notified via email only if they've already accepted their invitation.

## Manage the access review

You can track the progress as the reviewers complete their reviews in the Azure AD dashboard in the **Access Reviews** section. No access rights are changed in the directory until [the review is completed](complete-access-review.md).

If this is a one-time review, then after the access review period is over or the administrator stops the access review, follow the steps in [Complete an access review](complete-access-review.md) to see and apply the results.  

To manage a series of access reviews, navigate to the access review from **Controls**, and you will find upcoming occurrences in Scheduled reviews, and edit the end date or add/remove reviewers accordingly. 

Based on your selections in Upon completion settings, auto-apply will be executed after the review's end date or when you manually stop the review. The status of the review will change from Completed through intermediate states such as Applying and finally to state Applied. You should expect to see denied users, if any, being removed from the group membership or application assignment in a few minutes.

## Create reviews via APIs

Microsoft Graph APIs are available for administrators. What you do to manage access reviews of groups and application users in the Azure portal can also be done via APIs. For more information, see the [Azure AD Access Reviews API reference](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/accessreviews_root). For a code sample, see [Example of retrieving Azure AD Access Reviews via Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## Next steps

- [Perform an access review with Azure AD Access Reviews](perform-access-review.md)
- [Complete an access review of members of a group or users' access to an application in Azure AD](complete-access-review.md)
