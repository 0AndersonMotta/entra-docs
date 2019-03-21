---
title: Admin consent for LinkedIn account connections - Azure Active Directory | Microsoft Docs
description: Explains how to enable or disable LinkedIn integration account connections in Microsoft apps in Azure Active Directory
services: active-directory
author: curtand
manager: mtillman

ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/21/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
---

# Consent to LinkedIn account connections for an Azure Active Directory organization

In this article, you'll see how to allow users to access their LinkedIn connections within some Microsoft apps. No data is shared until users consent to connect their accounts. You can integrate your organization in the Azure Active Directory (Azure AD) [admin center](https://aad.portal.azure.com).

> [!IMPORTANT]
> The LinkedIn integration setting is currently being rolled out to Azure AD organizations. When it is rolled out to your organization, it is enabled by default.
> 
> Exceptions:
> * The setting is not available for customers using Microsoft Cloud for US Government, Microsoft Cloud Germany, or Azure and Office 365 operated by 21Vianet in China.
> * The setting is off by default for tenants provisioned in Germany. Note that the setting is not available for customers using Microsoft Cloud Germany.
> * The setting is off by default for tenants provisioned in France.
>
> Integrating LinkedIn account connections works only if you have it enabled *and* after users consent to apps accessing company data on their behalf. For information about the user consent setting, see [How to remove a user’s access to an application](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## Enable or disable LinkedIn integration for your users in the Azure portal

You can integrate LinkedIn account connections for your entire organization or for only selected users in your organization.

1. Sign in to the [Azure AD admin center](https://aad.portal.azure.com/) with an account that's a global admin for the Azure AD organization.
2. Select **Users**.
3. On the **Users** blade, select **User settings**.
4. Under **LinkedIn account connections**, allow users to connect their accounts to access their LinkedIn connections within some Microsoft apps. No data is shared until users consent to connect their accounts.

  * Select **Yes** to consent to the service for all users in the organization
  * Select **Selected** to consent for only selected users in the organization
  * Select **No** to withdraw consent for users in your organization

    ![Integrate LinkedIn account connections in the organization](./media/linkedin-integration/linkedin-integration.png)

5. When you're done, select **Save** to save your settings.
     
## Enable or disable LinkedIn integration for your users in Group Policy

1. Download the [Office 2016 Administrative Template files (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extract the **ADMX** files and copy them to your central store.
3. Open Group Policy Management.
4. Create a Group Policy Object with the following setting: **User Configuration** > **Administrative Templates** > **Microsoft Office 2016** > **Miscellaneous** > **Show LinkedIn features in Office applications**.
5. Select **Enabled** or **Disabled**.
  
   State | Effect
   ------ | ------
   **Enabled** | The **Show LinkedIn features in Office applications** setting in Office 2016 Options is enabled. Users in your organization can use LinkedIn features in their Office applications.
   **Disabled** | The **Show LinkedIn features in Office applications** setting in Office 2016 Options is disabled and end users can't change this setting. Users in your organization can't use LinkedIn features in their Office 2016 applications.

This group policy affects only Office 2016 apps for a local computer. Users can see LinkedIn features in profile cards throughout Office 365 even if they disable LinkedIn in their Office 2016 apps.

## Next steps

* [User consent and data sharing for LinkedIn](linkedin-user-consent.md)

* [LinkedIn information and features in your Microsoft apps](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn help center](https://www.linkedin.com/help/linkedin)

## Next steps

Use the following link to see your current LinkedIn integration setting in the Azure portal:

[View your current LinkedIn integration setting in the Azure portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
