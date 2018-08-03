---
title: Register your personal device on your organization's network - Azure Active Directory | Microsoft Docs
description: Learn how to register your personal device on your organization's network.
services: active-directory
author: eross-msft
manager: mtillman

ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.reviewer: jairoc

---
# Register your personal device on your organization's network
Register your personal device, typically a phone or tablet, running Windows 10, iOS, Android, or macOS, on your organization's network.

## What happens when you register your device
While you're registering your device on your organization's network, the following actions will happen:

- Windows registers your device on your organization's network.

- You might be asked to set up two-step verification through either [Multi-Factor Authentication](multi-factor-authentication-end-user-first-time.md) or [security info](user-help-security-info-overview.md), depending on what your organization has set up.

- You'll be automatically enrolled in mobile device management, such as Microsoft Intune, if it's necessary. For more info about enrolling in Microsoft Intune, see [Enroll your device in Intune](https://docs.microsoft.com/en-us/intune-user-help/enroll-your-device-in-intune-all).

- You'll go through the sign-in process, using the username and password for your personal, non-organizational account.

## To register your device

Follow these steps to register your personal device on your network.

1. Open **Settings**, and then select **Accounts**.

    ![Accounts on the Settings screen](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Select **Access work or school**, and then select **Connect**.

    ![Access work or school and Connect links](./media/user-help-register-device-on-network/register-device-access-work-school-connect.png)

3. 













**To register your Windows 10 device:**

1. In the **Start** menu, click **Settings**.

    ![Settings](./media/user-help-register-device-on-network/01.png)

2. Click **Accounts**.

    ![Accounts](./media/user-help-register-device-on-network/02.png)


3. Click **Access work or school**.

    ![Access work or school](./media/user-help-register-device-on-network/03.png)

4. On the **Access work or school** dialog, click **Connect**.

    ![Connect](./media/user-help-register-device-on-network/04.png)


5. On the  **Set up a work or school account** dialog, enter your account name (for example, someone@example.com), and then click **Next**.

    ![Connect](./media/user-help-register-device-on-network/06.png)


6. On the  **Enter password** dialog, enter your password, and then click **Next**.

    ![Connect](./media/user-help-register-device-on-network/05.png)


7. On the **You're all set** dialog, click **Done**.

    ![Connect](./media/user-help-register-device-on-network/07.png)

## Make sure you're registered

To verify whether a device is joined to an Azure AD, you can review the **Access work or school** dialog on your device.

![Register](./media/user-help-register-device-on-network/08.png)

Alternatively, you can review device settings on the Azure AD portal.

![Register](./media/user-help-register-device-on-network/09.png)





## Next steps

- For more information, see the [introduction to device management in Azure Active Directory](../device-management-introduction.md)

- For more information about managing devices in the Azure AD portal, see the [managing devices using the Azure portal ](../device-management-azure-portal.md).




