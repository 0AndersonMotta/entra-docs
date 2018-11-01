---
title: What are Azure AD access reviews? | Microsoft Docs
description: Using Azure Active Directory access reviews, you can control group membership and application access to meet governance, risk management, and compliance initiatives in your organization.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 10/31/2018
ms.author: rolyon
ms.reviewer: mwahl
---

# What are Azure AD access reviews?

Azure Active Directory (Azure AD) access reviews enable organizations to efficiently manage group memberships, access to enterprise applications, and privileged role assignments. User's access can be reviewed on a regular basis to make sure only the right people have continued access.

Here's a video that provides a quick overview of access reviews.

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## Why are access reviews important?

With Azure AD, companies collaborate with partners outside of their organizations, invite guests to internal groups, connect to cloud apps that IT doesn't have control over, and grant users to use personal devices for work. Productivity has increased significantly, but new challenges around access management also arise. As new employees join, how do you ensure they have the right access to be productive? As people move teams or leave the company, how do you ensure their old access is removed, especially when it involves guests? Excessive access rights can lead to audit findings and compromises as they indicate a lack of control over access. As a result, you have to proactively engage with resource owners to ensure they are regularly reviewing who has access to their resources.

## What can I do with access reviews?

- **Set up recurring reviews**: With Azure AD access reviews, you can set up recurring access reviews of all users, employees, or guests in groups and applications, and users in administrative roles. Assigned reviewers will be notified and approve or deny access with a friendly interface and with the help of smart recommendations.
- **Recertify administrative users**: You can recertify the role assignment of administrative users who are assigned to Azure AD directory roles such as Global Administrator or Azure resource roles. It's a good idea to check how many users have administrative access and if there are any invited guests or partners that have not been removed. This capability is included in Azure AD Privileged Identity Management (PIM).
- **Review access to groups**: If you have enabled Office groups in your organization to leverage the power of self service, anyone in the organization can create groups and invite guests into groups. It's convenient that guests can easily gain access to groups, but you also want to make sure that there's oversight over this process. Access reviews can be created on Office and security groups and assign the group owners to confirm who should have continued access, and if the guests are still needed. It is useful to set up recurring reviews on groups that involve business critical data or customer information, and if a group is used for a new purpose.
- **Review users assigned to applications**: Users assigned to Azure AD-connected applications can also be reviewed, and reviewers will receive recommendations based on the users' sign-in information to make an informative decision.

## Where do you create reviews?

Depending on what you want to review, you will create your access review in [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-how-to-start-security-review.md), Azure AD access reviews, or Azure AD enterprise apps (in preview).

| Access rights of users | Reviewers can be | Review created in | Reviewer experience |
| --- | --- | --- | --- |
| Azure AD directory role | Users themselves</br>Specified reviewers | Azure AD PIM | Azure portal |
| Azure resource role | Users themselves</br>Specified reviewers | Azure AD PIM | Azure portal |
| Members of security group</br>Members of Office group | Users themselves</br>Specified reviewers</br>Group owners | Azure AD access reviews</br>Azure AD groups | Access panel |
| Assigned to a connected app | Users themselves</br>Specified reviewers | Azure AD access reviews</br>Azure AD enterprise apps (in preview) | Access panel |

## Prerequisites

To use access reviews, you must have one of the following:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5 license

For information about a free trial, see [Enterprise Mobility + Security E5 Trial](http://aka.ms/emse5trial).

## Get started with access reviews

To get started with access reviews, click the onboard button in the Azure portal. You can learn more about creating and performing access reviews, or watch a short demo here:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

If you are ready to deploy access reviews in your organization, follow these steps in the video to onboard, train your admins and create your first access review!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## Create reviews via APIs

Microsoft Graph APIs are available for administrators. What you do to manage access reviews of groups, apps, and Azure AD roles in the Azure portal can also be done via APIs. For more information, see the [Azure AD access reviews API reference](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/accessreviews_root). For a code sample, see [Example of retrieving Azure AD access reviews via Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## Next steps

- [Create an access review for members of a group or access to an application](create-access-review.md)
- [Create an access review of users in an Azure AD administrative role](../privileged-identity-management/pim-how-to-start-security-review.md)
