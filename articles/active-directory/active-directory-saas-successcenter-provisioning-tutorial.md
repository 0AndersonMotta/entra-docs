---
title: 'Tutorial: Configure Cornerstone OnDemand for automatic user provisioning with Azure Active Directory | Microsoft Docs'
description: Learn how to configure Azure Active Directory to automatically provision and de-provision user accounts to Cornerstone OnDemand.
services: active-directory
documentationcenter: ''
author: zhchia-msft
writer: zhchia-msft
manager: beatrizd-msft

ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant-msft
---

# Tutorial: Configure Cornerstone OnDemand for automatic user provisioning


The objective of this tutorial is to show you the steps you need to perform in Cornerstone OnDemand and Azure AD to automatically provision and de-provision user accounts from Azure AD to Cornerstone OnDemand. 

## Prerequisites

The scenario outlined in this tutorial assumes that you already have the following items:

*   An Azure Active Directory tenant
*   A Cornerstone OnDemand tenant with the [Standard](https://www.cornerstoneondemand.com/)  plan or better enabled 
*   A user account in Cornerstone OnDemand with Admin permissions


> [!NOTE]
> The Azure AD provisioning integration relies on the [Cornerstone OnDemand Webservice](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), which is available to Cornerstone OnDemand teams on the Standard plan or better.

## Assigning users to Cornerstone OnDemand

Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps. In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.

Before configuring and enabling the provisioning service, you should decide what users and/or groups in Azure AD represent the users who need access to Cornerstone OnDemand. Once decided, you can assign these users to your Cornerstone OnDemand app by following the instructions here:

*	[Assign a user or group to an enterprise app](active-directory-coreapps-assign-user-azure-portal.md)

### Important tips for assigning users to Cornerstone OnDemand

*	It is recommended that a single Azure AD user is assigned to Cornerstone OnDemand to test the provisioning configuration. Additional users and/or groups may be assigned later.

*	When assigning a user to Cornerstone OnDemand, you must select either the **User** role, or another valid application-specific role (if available) in the assignment dialog.Users with the **Default Access** role are excluded from provisioning.

> [!NOTE]
> As an added feature, the provisioning service reads any custom roles defined in Cornerstone OnDemand, and imports them into Azure AD where they can be selected in the Select Role dialog. These roles will be visible in the Azure portal after the provisioning service is enabled and one synchronization cycle has completed.

## Configuring user provisioning to Cornerstone OnDemand 

This section guides you through connecting your Azure AD to Cornerstone OnDemand's user roster using Cornerstone OnDemand's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Cornerstone OnDemand based on user and group assignment in Azure AD.


###To configure automatic user provisioning to Cornerstone OnDemand in Azure AD:


1. Sign in to the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.

2. If you have already configured Cornerstone OnDemand for single sign-on, search for your instance of Cornerstone OnDemand using the search field. Otherwise, select **Add** and search for **Cornerstone OnDemand** in the application gallery. Select Cornerstone OnDemand from the search results, and add it to your list of applications.

	![Cornerstone OnDemand Provisioning](./media/active-directory-saas-successcenter-provisioning-tutorial/Successcenter2.png)

3. Select your instance of Cornerstone OnDemand, then select the **Provisioning** tab.

4. Set the **Provisioning Mode** to **Automatic**.

	![Cornerstone OnDemand Provisioning](./media/active-directory-saas-successcenter-provisioning-tutorial/Successcenter1.png)

5. Under the **Admin Credentials** section, input the **Admin Username, Admin Password & Domain** of your Cornerstone OnDemand's account. 

6. Upon populating the fields shown above, click **Test Connection** to ensure Azure AD can connect to your Cornerstone OnDemand app. If the connection fails, ensure your Cornerstone OnDemand account has Admin permissions and try Step 5 again.

7. Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."

8. Click **Save**.

9. Under the **Mappings** section, select **Synchronize Azure Active Directory Users to Cornerstone OnDemand**.

10. Review the user attributes that are synchronized from Azure AD to Cornerstone OnDemand. The attributes selected as **Matching** properties are used to match the user accounts in Cornerstone OnDemand for update operations. Select the Save button to commit any changes.

11. To enable the Azure AD provisioning service for Cornerstone OnDemand, change the **Provisioning Status** to **On** in the **Settings** section

12. Click **Save**. 

This starts the initial synchronization of any users and/or groups assigned to Cornerstone OnDemand in the Users and Groups section. The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running. You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Cornerstone OnDemand app.

For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).


## Additional resources

* [Managing user account provisioning for Enterprise Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## Next steps

* [Learn how to review logs and get reports on provisioning activity](active-directory-saas-provisioning-reporting.md)
