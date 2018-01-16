---
title: Configure LinkedIn integration for a tenant in the Azure Active Directory portal | Microsoft Docs
description: Explains how to enable or disable LinkedIn integration for Microsoft apps in Azure Active Directory
services: active-directory
author: curtand
manager: mtillman

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/16/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
---

# Enabling LinkedIn integration in Azure Active Directory
This article tells you how to enable LinkedIn integration for your users. Enable LinkedIn integration to provide users with access public LinkedIn data within Microsoft apps. They can also choose to share their personal LinkedIn network information. Each user can independently choose to connect their work account to their LinkedIn account.

### LinkedIn integration from the end users perspective
When end users in your organization connect their LinkedIn accounts to their work accounts, their LinkedIn connections and profile data can be used in your organization's Microsoft apps. They can opt out by removing permission for LinkedIn to share that data. The integration also uses publicly available profile information, and users can choose whether to share their public profile and network information with Microsoft applications through LinkedIn privacy settings.

>[!NOTE]
> Enabling LinkedIn integration in Azure AD enables apps and services to access some of your end users' information. Read the [Microsoft Privacy Statement](https://privacy.microsoft.com/privacystatement/) to learn more about the privacy considerations when enabling LinkedIn integration in Azure AD. 

## Enable LinkedIn integration
LinkedIn integration for enterprises is enabled by default in Azure AD. So, when this feature is made available for your tenant, all users in your organization can see LinkedIn features and profiles. Enabling LinkedIn integration allows users in your organization to use LinkedIn features within Microsoft services, such as viewing profiles when receiving an email in Outlook. Disabling this feature prevents access to LinkedIn features and stops user account connections and data sharing between LinkedIn and your organization via Microsoft services.

> [!IMPORTANT]
> This feature is not available for go-local, sovereign, and government tenants. In addition, Azure AD service updates, such as LinkedIn integration capabilities, are not deployed to all Azure tenants at the same time. You cannot enable LinkedIn integration with Azure AD until this capability has been rolled out to your Azure tenant.

### Enable LinkedIn integration in the Azure portal

1. Sign in to the [Azure Active Directory admin center](https://aad.portal.azure.com/) with an account that's a global admin for the Azure AD tenant.
2. Select **Users and groups**.
3. On the **Users and groups** blade, select **User settings**.
4. Under **LinkedIn Integration**, select Yes or No to enable or disable LinkedIn integration.
   ![Enabling LinkedIn integration](./media/linkedin-integration/LinkedIn-integration.PNG)

### Disable LinkedIn integration using Group Policy

1. Download the [Office 2016 Administrative Template files (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extract the **ADMX** files and copy them to your **central repository**.
3. Open Group Policy Management.
4. Create a GPO with the following setting: User Configuration > Administrative Templates > Microsoft Office 2016 > Miscellaneous > Allow LinkedIn Integration
5. Select **Enabled** or **Disabled**.
  * **Enabled**: The **Show LinkedIn features in Office applications** setting found in Office Options is set to the enabled state.   
  * **Disabled**: The **Show LinkedIn features in Office applications** setting found in Office Options is defaulted to the disabled state. End users cannot change this setting. All LinkedIn Features related to profiles and activity data are disabled in the product. 

### Learn more 
* [LinkedIn information and features in your Microsoft apps](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn help center](https://www.linkedin.com/help/linkedin)

## Next steps
Use the following link to see your current LinkedIn integration setting in the Azure portal:

[Configure LinkedIn integration](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 