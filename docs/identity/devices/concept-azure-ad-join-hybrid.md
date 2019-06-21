---
title: What is a hybrid Azure AD joined device?
description: Learn how device identity management can help you to manage devices that are accessing resources in your environment.

services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/04/2019

ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo

ms.collection: M365-identity-device-management
---
# Hybrid Azure AD joined devices

For more than a decade, many organizations have used the domain join to their on-premises Active Directory to enable:

- IT departments to manage work-owned devices from a central location.
- Users to sign in to their devices with their Active Directory work or school accounts.

Typically, organizations with an on-premises footprint rely on imaging methods to provision devices, and they often use **System Center Configuration Manager (SCCM)** or **group policy (GP)** to manage them.

If your environment has an on-premises AD footprint and you also want benefit from the capabilities provided by Azure Active Directory, you can implement hybrid Azure AD joined devices. These are devices that are joined to your on-premises Active Directory and registered with your Azure Active Directory.

|   | Hybrid Azure AD Join |
| --- | --- |
| **Definition** | Joined to on-premises AD and Azure AD requiring organizational account to sign in to the device |
| **Primary audience** | Suitable for hybrid organizations with existing on-premises AD infrastructure |
|   | Applicable to all users in an organization |
| **Device ownership** | Organization |
| **Operating Systems** | Windows 10, 8.1 and 7 |
|   | Windows Server 2008/R2, 2012/R2, 2016 and 2019 |
| **Provisioning** | Windows 10, Windows Server 2016/2019 |
|   | Domain join by IT and auto-join via Azure AD Connect or ADFS config |
|   | Domain join by Windows Autopilot and auto-join via Azure AD Connect or ADFS config |
|   | Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 and Windows Server 2008 R2 - Require MSI |
| **Device sign in options** | Organizational accounts using: |
|   | Password |
|   | Windows Hello for Business for Win10 |
| **Device management** | Group Policy |
|   | System Center Configuration Manager standalone or co-management with Microsoft Intune |
| **Key capabilities** | SSO to both cloud and on-premises resources |
|   | Conditional Access through Domain join or through Intune if co-managed |
|   | Self-service Password Reset and Windows Hello PIN reset on lock screen |
|   | Enterprise State Roaming across devices |


![Azure AD registered devices](./media/overview/01.png)

You should use Azure AD hybrid joined devices if:

- You have Win32 apps deployed to these devices that rely on Active Directory machine authentication.
- You require GP to manage devices.
- You want to continue to use imaging solutions to configure devices for your employees.

You can configure Hybrid Azure AD joined devices for Windows 10 and down-level devices such as Windows 8 and Windows 7.

## Azure AD joined devices

The goal of Azure AD joined devices is to simplify:

- Windows deployments of work-owned devices
- Access to organizational apps and resources from any Windows device
- Cloud-based management of work-owned devices
- Users to sign in to their devices with their Azure AD or synced Active Directory work or school accounts.

![Azure AD registered devices](./media/overview/02.png)

Azure AD Join can be deployed by using any of the following methods:

- [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot)
- [Bulk deployment](https://docs.microsoft.com/intune/windows-bulk-enroll)
- [Self-service experience](azuread-joined-devices-frx.md)

## Next steps

- [What is Microsoft Intune?](https://docs.microsoft.com/intune/what-is-intune)
- [What is co-management?](https://docs.microsoft.com/sccm/comanage/overview)
- [Windows Hello for Business](https://docs.microsoft.com/indows/security/identity-protection/hello-for-business/hello-identity-verification)