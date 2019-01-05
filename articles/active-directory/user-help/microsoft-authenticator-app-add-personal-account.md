---
title: Add your personal accounts to the Microsoft Authenticator app - Azure Active Directory | Microsoft Docs
description: How to add your personal Microsoft and non-Microsoft accounts, such as for Outlook.com, Xbox LIVE, Google, or Facebook, to the Microsoft Authenticator app for two-factor verification.
services: active-directory
author: eross-msft
manager: mtillman

ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 01/03/2019
ms.author: lizross
ms.reviewer: librown
---

# Add your personal accounts
You can add your personal Microsoft and non-Microsoft accounts, such as for Outlook.com, Xbox LIVE, Google, Facebook, and more to the Microsoft Authenticator app for two-factor verification.

>[!Important]
>Before you can add your account, you have to download and install the Microsoft Authenticator app. If you haven't done that yet, follow the steps in the [Download and install the app](microsoft-authenticator-app-how-to.md) article.

## Add your personal Microsoft account
You can add your personal Microsoft account by first turning on two-factor verification, and then by adding the account to the app.

### To turn on two-factor verification

1. On your PC, go to your [Security basics](https://account.microsoft.com/security) page and sign-in using your personal Microsoft account. For example, alain@outlook.com.

2. At the bottom of the **Security basics** page, choose the **more security options** link.

    ![Security basics screen with the more security options link highlighted](./media/microsoft-authenticator-app-more-security-options-link/microsoft-authenticator-app-more-security-options-link.png)

3. Go to the **Two-step verification** section and choose to turn the feature **On**. You can also turn it off here if you no longer want to use it with your personal account.

### To add your Microsoft account to the app

1. Open the Microsoft Authenticator app on your mobile device.

2. Select **Add account** from the **Customize and control** icon in the upper-right.

    ![Accounts screen, with the Customize and control icon highlighted](./media/microsoft-authenticator-app-customize-and-control-icon/microsoft-authenticator-app-customize-and-control-icon.png)

3. In the **Add account** screen, choose **Personal account**.

4. Sign in to your personal account, using the appropriate email address (such as alain@outlook.com), and then choose **Next**.

    >[!Note]
    >If you don't have a personal Microsoft account, you have the ability to create one here.

5. Enter your password, and then choose **Sign in**.

    Your personal account is added to the Microsoft Authenticator app.

## Add your Google account
You can add your Google account by first turning on two-factor verification and then by adding the account to the app.

### To turn on two-factor verification

1. On your PC, go to https://myaccount.google.com/signinoptions/two-step-verification/enroll-welcome, select **Get Started**, and then verify your identity.

2. Follow the on-screen steps to turn two-step verification on for your personal Google account.

### To add your Google account to the app

1. On your PC, in the **Set up alternative second step** section, choose **Set up** from the **Authenticator app** section.

2. In the **Get codes from the Authenticator app** screen, select either **Android** or **iPhone** based on your phone type, and then select **Next**.

    You're given a QR code that you can use to automatically associate your account with the Microsoft Authenticator app. Do not close this window.

3. Open the Microsoft Authenticator app, select **Add account** from the **Customize and control** icon in the upper-right, and then select **Other account (Google, Facebook, etc)**.

4. Use your device's camera to scan the QR code from the **Set up Authenticator** screen on your PC.

    >[!Note]
    >If your camera isn't working properly, you can [enter the QR code and URL manually](#add-an-account-to-the-app-manually).

5. Review the **Accounts** screen of the Microsoft Authenticator app on your device, to make sure your account information is right and that there's an associated six-digit verification code.

    For additional security, the verification code changes every 30 seconds preventing you from using the same code twice.

6. Select **Next** on the **Set up Authenticator** screen on your PC, type the 6-digit verification code provided in the app for your Google account, and then select **Verify**.

7. Your account is verified and you can select **Done** to close the **Set up Authenticator** screen.

    >[!NOTE]
    >For more information about two-factor verification and your Google account, see [Turn on 2-Step Verification](https://support.google.com/accounts/answer/185839) and [Learn more about 2-Step Verification](https://www.google.com/landing/2step/help.html).

## Add your Facebook account
You can add your Facebook account by first turning on two-factor verification and then by adding the account to the app.

### To turn on two-factor verification

1. On your PC, open Facebook, click in the top-right corner, and go to **Settings** > **Security and Login**.

    The **Security and Login** page appears.

2. Go down to the **Use two-factor authentication** option in the **Two-Factor Authentication** section, and then select **Edit**.

    The **Two-Factor Authentication** page appears.

3. Select **Turn On**.

### To add your Facebook account to the app

1. On your PC, in the **Add a backup** section, choose **Setup** from the **Authentication app** area.

    You're given a QR code that you can use to automatically associate your account with the Microsoft Authenticator app. Do not close this window.

2. Open the Microsoft Authenticator app, select **Add account** from the **Customize and control** icon in the upper-right, and then select **Other account (Google, Facebook, etc)**.

3. Use your device's camera to scan the QR code from the **Two factor authentication** screen on your PC.

    >[!Note]
    >If your camera isn't working properly, you can [enter the QR code and URL manually](#add-an-account-to-the-app-manually).

4. Review the **Accounts** screen of the Microsoft Authenticator app on your device, to make sure your account information is right and that there's an associated six-digit verification code.

    For additional security, the verification code changes every 30 seconds preventing you from using the same code twice.

5. Select **Next** on the **Two factor authentication** screen on your PC, and then type the 6-digit verification code provided in the app for your Facebook account.

    Your account is verified and you can now use the app to verify your account.

    >[!NOTE]
    >For more information about two-factor verification and your Facebook account, see [What is two-factor authentication and how does it work?](https://www.facebook.com/help/148233965247823).

## Add your Apple account
Add steps about how to add your apple account.

## Add other non-Microsoft personal accounts

Add steps here for how to add other accounts, like Instagram, Adobe, and so on.

## Next steps

- After you add your accounts to the app, you can sign in using the Authenticator app on your device. For more information, see [Sign in using the app](microsoft-authenticator-app-phone-signin-faq.md).

- For devices running iOS, you can also back up your account credentials and related app settings, such as the order of your accounts, to the cloud. For more information, see [Backup and recover with Microsoft Authenticator app](microsoft-authenticator-app-backup-and-recovery.md).