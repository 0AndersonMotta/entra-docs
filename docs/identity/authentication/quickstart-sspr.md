---
title: Quickstart Azure AD self-service password reset
description: In this quickstart, you will quickly configure Azure AD self-service password reset to allow users to reset their own passwords

services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: quickstart
ms.date: 04/27/2018

ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry

#Customer intent: As an Azure AD Administrator, I want to protect user authentication so I deploy SSPR so that when users have trouble signing-in they can reset their passwords using something they know.
---
# Quickstart: Self-service password reset

Self-service password reset (SSPR) offers a simple means for IT administrators to empower users to reset or unlock their passwords or accounts. The system includes detailed reporting that tracks when users access the system, along with notifications to alert you to misuse or abuse.

## Prerequisites

A working Azure AD tenant with at least a trial license enabled.
An account with Global Administrator privileges.
A standard non-administrator user with a password you know for testing, if you need to create a user see the article [Quickstart: Add new users to Azure Active Directory](../add-users-azure-active-directory.md)
A pilot group to test with that the non-administrator user is a member of, if you need to create a group see the article [Create a group and add members in Azure Active Directory](../active-directory-groups-create-azure-portal.md)

## Enable SSPR for your Azure AD tenant

> [!VIDEO https://www.youtube.com/embed/Pa0eyqjEjvQ]

1. From your existing Azure AD tenant, on the **Azure portal** under **Azure Active Directory** select **Password reset**.

2. From the **Properties** page, under the option **Self Service Password Reset Enabled**, choose **Selected**.
    * From **Select group**, choose your pilot group created as part of the prerequisites section of this article.
    * Click **Save**.

3. From the **Authentication methods** page, choose the following:
   * **Number of methods required to reset**: 1
   * **Methods available to users**:
      * **Mobile phone**
      * **Office phone**
   * Click **Save**.

    ![Authentication][Authentication]

4. From the **Registration** page, choose the following:
   * Require users to register when they sign in: **Yes**
   * Set the number of days before users are asked to reconfirm their authentication information: **365**

## Test self-service password reset

Open a new browser window in InPrivate mode, and browse to [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup).
Log in with a standard user, and register your authentication phone.
Once complete, click the button marked **looks good** and close the browser window.

> [!TIP]
> Test SSPR with a user rather than an administrator, because Microsoft enforces strong authentication requirements for Azure administrator accounts. For more information regarding the administrator password policy, see our [password policy](concept-sspr-policy.md#administrator-password-policy-differences) article.

Open a new browser window in InPrivate mode, and browse to [https://aka.ms/sspr](https://aka.ms/sspr).
Enter your User ID, the characters from the CAPTCHA, and then click **Next**.
Follow the verification steps to reset your password

## Clean up resources

It's easy to disable self-service password reset. Open your Azure AD tenant and go to **Password Reset** > **Properties**, and then select **None** under **Self Service Password Reset Enabled**.

## Next steps

In this quickstart, you’ve learned how to configure self-service password reset for your users. To complete these steps, continue to the Azure portal:

> [!div class="nextstepaction"]
> [Enable self-service password reset](howto-sspr-deployment.md)

[Authentication]: ./media/quickstart-sspr/sspr-authentication-methods.png "Azure AD authentication methods available and the quantity required"
