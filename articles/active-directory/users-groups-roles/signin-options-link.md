---
title: More sign-in options for Azure Active Directory accounts - Microsoft 365 | Microsoft Docs
description: How on-screen messaging reflects username lookup during sign-in 
services: active-directory
author: curtand
manager: mtillman

ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/01/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro

ms.collection: M365-identity-device-management
---

# More sign-in options for Microsoft accounts in Microsoft 365

In the near future, we're updating the Microsoft 365 sign-in page for Azure Active Directory (Azure AD) to add a new **Sign-in options** link. This link will be added only to sign-in pages that accept Microsoft accounts, also called "personal accounts."

## Sign-in pages that support Microsoft accounts

The Microsoft 365 sign-in page supports one or both of work or school accounts and Microsoft accounts, depending on the situation, to support:

* Apps that accept sign-ins from both types of account
* Organizations that accept guests

You can tell if the sign-in page your organization uses supports Microsoft accounts by looking at the hint text in the username field. If the hint text says "Email, phone, or Skype", the sign-in page supports Microsoft accounts.

![Difference between account sign-in pages](./media/signin-options-link/ui-prompt.png)

## New sign-in options link

The **Sign-in options** link will be added only to sign-in pages which support Microsoft accounts. If the sign-in page your organization uses doesn't support Microsoft accounts, this change doesn't affect you.
  
After this change takes place, your users will see a new link that says "Sign-in options" on the sign-in page. Clicking the link will bring the user to a new screen that will show additional sign-in options that only work for personal Microsoft accounts. The additional sign-in options can't be used for signing in to work or school account resources.

![sign-in options opens choices for Microsoft accounts](./media/signin-options-link/options-link.png)

## Deployment schedule

The new options will begin gradually rolling out to Azure AD organizations in early May 2019, and roll out is targeted to be complete worldwide by the middle of May 2019.

## Next steps

[Customize your sign-in branding](../fundamentals/add-custom-domain.md)