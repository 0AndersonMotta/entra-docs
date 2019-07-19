---
title: Enable passwordless sign-in with the Microsoft Authenticator app (preview) - Azure Active Directory
description: Enable passwordless sign in to Azure AD using the Microsoft Authenticator app (preview)

services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/19/2019

ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown

ms.collection: M365-identity-device-management
---
# Enable passwordless sign-in with the Microsoft Authenticator app (preview)

The Microsoft Authenticator app can be used to sign in to any Azure AD account without using a password. Similar to the technology of [Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-identity-verification), the Microsoft Authenticator uses key-based authentication to enable a user credential that is tied to a device and uses a biometric or PIN.

![Example of a browser sign-in asking for user to approve the sign-in](./media/howto-authentication-passwordless-phone/phone-sign-in-microsoft-authenticator-app.png)

Instead of seeing a prompt for a password after entering a username, a person who has enabled phone sign-in in the Microsoft Authenticator app will see a message telling them to tap a number in their app. In the app, the user must match the number, choose Approve, then provide their PIN or biometric, then the authentication will complete.

> [!NOTE]
> This capability has been in the Microsoft Authenticator app since March of 2017, so there is a possibility that when the policy is enabled for a directory, users may encounter this flow immediately. Be aware and prepare your users for this change.

## Requirements

- Azure Multi-Factor Authentication enabled in directory
- WebAuthN requires Microsoft Edge on Windows 10 version 1809 or higher
- Latest version of Microsoft Authenticator installed on devices running iOS 8.0 or greater, or Android 6.0 or greater.

> [!NOTE]
> If you enabled the previous Microsoft Authenticator app passwordless sign-in preview, using Azure AD PowerShell, it was enabled for your entire directory. If you enable using this new method, you have the ability to scope to specific users. Be aware that doing so may impact users from the previous preview.

## Enable passwordless authentication methods

### Enable the combined registration experience

Registration features for passwordless authentication methods rely on the combined registration preview. Follow the steps in the article [Enable combined security information registration (preview)](howto-registration-mfa-sspr-combined.md), to enable the combined registration preview.

### Enable new passwordless authentication methods

1. Sign in to the [Azure portal](https://portal.azure.com)
1. Browse to **Azure Active Directory** > **Authentication methods** > **Authentication method policy (Preview)**
1. Under each **Method**, choose the following options
   1. **Enable** - Yes or No
   1. **Target** - All users or Select users
1. **Save** each method

> [!NOTE]
> You don’t need to opt in to both of the passwordless methods (if you want to preview only one passwordless method, you can enable only that method). We encourage you try out both methods since they both have their own benefits.

## User registration and management of Microsoft Authenticator app

1. Browse to [https://myprofile.microsoft.com](https://myprofile.microsoft.com)
1. Sign in if not already
1. Click **Security Info**
1. Add an authenticator app by clicking **Add method**, choosing **Authenticator app**, and clicking **Add**
1. Follow the instructions to install and configure the Microsoft Authenticator app on your device
1. Click **Done** to complete the process

Organizations can point their users to the article [Sign in with your phone, not your password](../user-help/microsoft-authenticator-app-phone-signin-faq.md) for further assistance setting up in the Microsoft Authenticator app and enabling phone sign-in.

## Sign in with passwordless credential

For public preview, there is no way to enforce users to create or use this new credential. A user will only encounter passwordless sign-in once an admin has enabled their tenant and the user has updated their Microsoft Authenticator app to enable phone sign-in.

After typing your username on the web and selecting **Next**, users are presented with a number and are prompted in their Microsoft Authenticator app to select the appropriate number to authenticate instead of using their password. 

![Example of a browser sign-in using the Microsoft Authenticator app](./media/howto-authentication-passwordless-phone/web-sign-in-microsoft-authenticator-app.png)

## Known Issues

### AD FS integration

When a user has enabled the Microsoft Authenticator passwordless credential, authentication for that user will always default to sending a notification for approval. This logic prevents users in a hybrid tenant from being directed to ADFS for sign in verification without the user taking an additional step to click “Use your password instead.” This process will also bypass any on-premises Conditional Access policies, and Pass-through authentication flows. The exception to this process is if a login_hint is specified, a user will be autoforwarded to AD FS, and bypass the option to use the passwordless credential.

### Azure MFA server

End users who are enabled for MFA through an organization’s on-premises Azure MFA server can still create and use a single passwordless phone sign in credential. If the user attempts to upgrade multiple installations (5+) of the Microsoft Authenticator with the credential, this change may result in an error.  

### Device registration

One of the prerequisites to create this new, strong credential, is that the device where it resides is registered within the Azure AD tenant, to an individual user. Due to device registration restrictions, a device can only be registered in a single tenant. This limit means that only one work or school account in the Microsoft Authenticator app can be enabled for phone sign in.

## Next steps

[What is passwordless?](concept-authentication-passwordless.md)

[Learn about device registration](../devices/overview.md#getting-devices-in-azure-ad)

[Learn about Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
