---
title: Authentication methods and features - Azure Active Directory
description: Learn about the different authentication methods and features available in Azure Active Directory to help improve and secure sign-in events

services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/03/2020

ms.author: iainfou
author: iainfoulds
manager: daveba

ms.collection: M365-identity-device-management
ms.custom: contperfq4

# Customer intent: As an identity administrator, I want to understand what authentication options are available in Azure AD and how or why I can use them to improve and secure user sign-in events.
---
# What authentication and verification methods are available in Azure Active Directory?

As part of the sign-in experience for accounts in Azure Active Directory (Azure AD), there are different ways that a user can authenticate themselves. A username and password is the most common way a user would historically provide credentials. With modern authentication and security features in Azure AD, that basic password should be supplemented or replaced with more secure authentication methods.

![Table of the strengths and preferred authentication methods in Azure AD](media/concept-authentication-methods/authentication-methods.png)

Passwordless authentication methods such as Windows Hello, FIDO2 security keys, and the Microsoft Authenticator app provide the most secure sign-in events.

Azure Multi-Factor Authentication adds additional security over only using a password when a user signs in. The user can be prompted for additional forms of authentication, such as to respond to a push notification, enter a code from a software or hardware token, or respond to an SMS or phone call.

Many accounts in Azure AD are also enabled for self-service password reset (SSPR). Again, when a user tries to reset their password, they're prompted for an additional form of verification.

It's recommended that you require users to register multiple authentication methods. When one method isn't available for a user during sign-in or SSPR, they can choose to authenticate with another method.

To simplify the user on-boarding experience and register for both MFA and SSPR, you can use [enable combined security information registration](howto-registration-mfa-sspr-combined.md).

## Available authentication methods

The following table outlines what methods are available for primary or secondary authentication. These authentication methods provide flexibility for your organization and the user experience.

| Method                         | Primary authentication | Secondary authentication  |
|--------------------------------|------------------------|---------------------------|
| FIDO2 security keys (preview)  | Yes                    | MFA                       |
| Microsoft Authenticator app    | Yes (preview)          | MFA and SSPR              |
| OATH hardware tokens (preview) | No                     | MFA                       |
| OATH software tokens           | No                     | MFA                       |
| SMS                            | Yes (preview)          | MFA and SSPR              |
| Voice call                     | No                     | MFA and SSPR              |
| Password                       | Yes                    |                           |

All of these authentication methods can be configured in the Azure portal, and increasingly using the [Microsoft Graph REST API beta](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

These authentication methods have different levels of security and convenience. Where possible, use authentication methods with the highest level of security.

| Authentication method | Security | Convenience | Satisfy Strong authentication requirements? | Phisable? |  Channel jackable? | Availability |
|---------|---------|---------|---------|---------|---------|---------|
| Windows Hello for Business | High | High | Strong primary and secondary | No | No | High |
| FIDO2 security key | High | High | Strong authentication | No | No | High |
| Microsoft Authenticator app | High | High | High  Strong authentication<br /><br />Can satisfy secondary authentication when used with a password. | Yes | No  | High |
| Hardware OATH tokens | Medium | Medium | Secondary authentication when used with a password. | Yes | No | High |
| Software OATH tokens | Medium | Medium | Secondary authentication when used with a password. | Yes | No | High |
| SMS | Medium | High | Primary or secondary authentication when used with a password. | Yes | Yes | Medium |
| Voice | Medium | Medium | Secondary authentication when used with a password | Yes | Yes | Medium |
| Password | Low | High | Primary authentication | Yes | Yes | High |

## How each authentication method works

To learn more about how each authentication method works, see the following separate conceptual articles:

* [Microsoft Authenticator app](concept-authentication-authenticator-app.md)
* [FIDO2 security keys (preview)](concept-authentication-passwordless.md#fido2-security-keys)
* [OATH software tokens](concept-authentication-oath-tokens.md#oath-software-tokens)
* [OATH hardware tokens (preview)](concept-authentication-oath-tokens.md#oath-hardware-tokens-preview)
* [SMS](concept-authentication-phone-options.md#mobile-phone-verification)
* [Voice call](concept-authentication-phone-options.md)
* Password

In Azure AD, a password is often one of the primary authentication methods. You can't disable the password authentication method. Increase the security of sign-in events using Azure Multi-Factor Authentication.

Even if you use an authentication method such as [SMS-based sign-in](howto-authentication-sms-signin.md) when the user doesn't use their password to sign, a password remains as an available authentication method.

If you have old applications and configure per-user Azure Multi-Factor Authentication, you can secure sign-in events using [app passwords](#app-passwords).

For SSPR, the following methods can also be used to confirm your identity:

* [Security questions](concept-authentication-security-questions.md)
* [Email address](#email-address)

### App passwords

Certain older, non-browser apps don't understand pauses or breaks in the authentication process. If a user is enabled for multi-factor authentication and attempts to use one of these older, non-browser apps, they usually can't successfully authenticate. An app password allows users to continue to successfully authenticate with older, non-browser apps without interruption.

By default, users can't create app passwords. If you need to allow users to create app passwords, select the **Allow users to create app passwords to sign into non-browser apps** under *Service settings* for user's Azure Multi-Factor Authentication properties.

![Screenshot of the Azure portal that shows the service settings for multi-factor authentication to allow the user of app passwords](media/concept-authentication-methods/app-password-authentication-method.png)

If you enforce Azure Multi-Factor Authentication using Conditional Access policies and not through per-user MFA, you can't create app passwords. Modern applications that use Conditional Access policies to control access don't need app passwords.

For more information, see [Enable and use Azure Multi-Factor Authentication with legacy applications using app passwords](howto-mfa-app-passwords.md).

### Email address

An email address can't be used as a direct authentication method. Email address is only available as a verification option for self-service password reset (SSPR). When email address is selected during SSPR, an email is sent to the user to complete the authentication / verification process.

During registration for SSPR, a user provides the email address to use. It's recommended that they use a different email account than their corporate account to make sure they can access it during SSPR.

## Next steps

To get started, see the [tutorial for self-service password reset (SSPR)][tutorial-sspr] and [Azure Multi-Factor Authentication][tutorial-azure-mfa].

To learn more about SSPR concepts, see [How Azure AD self-service password reset works][concept-sspr].

To learn more about MFA concepts, see [How Azure Multi-Factor Authentication works][concept-mfa].

Learn more about configuring authentication methods using the [Microsoft Graph REST API beta](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
