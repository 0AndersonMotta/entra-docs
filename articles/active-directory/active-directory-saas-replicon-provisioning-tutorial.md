---
title: 'Tutorial: Configure Replicon for automatic user provisioning with Azure Active Directory | Microsoft Docs'
description: Learn how to configure Azure Active Directory to automatically provision and de-provision user accounts to Replicon.
services: active-directory
documentationcenter: ''
author: zhchia-msft
writer: zhchia-msft
manager: beatrizd-msft

ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: v-ant-msft

---

# Tutorial: Configure Replicon for automatic user provisioning

The objective of this tutorial is to demonstrate the steps to be performed in Replicon and Azure Active Directory (Azure AD) to configure Azure AD to automatically provision and de-provision users and/or groups to Replicon.

> [!NOTE]
> This tutorial describes a connector built on top of the Azure AD User Provisioning Service. For important details on what this service does, how it works, and frequently asked questions, see [Automate user provisioning and deprovisioning to SaaS applications with Azure Active Directory](./active-directory-saas-app-provisioning.md).

## Prerequisites

The scenario outlined in this tutorial assumes that you already have the following items:

*   An Azure AD tenant
*   A Replicon tenant with the [Plus](https://www.replicon.com/time-bill-pricing/) plan or better enabled
*   A user account in Replicon with Admin permissions

> [!NOTE]
> The Azure AD provisioning integration relies on the [Replicon API](https://www.replicon.com/help/developers), which is available to Replicon teams on the Plus plan or better.

## Adding Replicon from the gallery
Before configuring Replicon for automatic user provisioning with Azure AD, you need to add Replicon from the Azure AD application gallery to your list of managed SaaS applications.

**To add Replicon from the Azure AD application gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click on the **Azure Active Directory** icon. 

	![The Azure Active Directory button][1]

2. Navigate to **Enterprise applications** > **All applications**.

	![The Enterprise applications Section][2]
	
3. To add Replicon, click the **New application** button on the top of the dialog.

	![The New application button][3]

4. In the search box, type **Replicon**.

	![Replicon Provisioning](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconAppSearch.png)

5. In the results panel, select **Replicon**, and then click the **Add** button to add Replicon to your list of SaaS applications.

	![Replicon Provisioning](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconAppSearchResults.png)

	![Replicon Provisioning](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconAppCreate.png)


## Assigning users to Replicon

Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps. In the context of automatic user provisioning, only the users and/or groups that have been "assigned" to an application in Azure AD are synchronized.

Before configuring and enabling automatic user provisioning, you should decide which users and/or groups in Azure AD need access to Replicon. Once decided, you can assign these users and/or groups to Replicon by following the instructions here:

*   [Assign a user or group to an enterprise app](active-directory-coreapps-assign-user-azure-portal.md)

### Important tips for assigning users to Replicon

*	It is recommended that a single Azure AD user is assigned to Replicon to test the automatic user provisioning configuration. Additional users and/or groups may be assigned later.

*	When assigning a user to Replicon, you must select any valid application-specific role (if available) in the assignment dialog. Users with the **Default Access** role are excluded from provisioning.

## Configuring automatic user provisioning to Replicon 

This section guides you through the steps to configure the Azure AD provisioning service to create, update, and disable users and/or groups in Replicon based on user and/or group assignments in Azure AD.

> [!TIP]
> You may also choose to enable SAML-based single sign-on for Replicon, following the instructions provided in the [Replicon single sign-on tutorial](active-directory-saas-replicon-tutorial.md). Single sign-on can be configured independently of automatic user provisioning, though these two features compliment each other.

### To configure automatic user provisioning for Replicon in Azure AD:

1. Sign in to the [Azure portal](https://portal.azure.com) and browse to **Azure Active Directory > Enterprise applications > All applications**.

2. Select Zendesk from your list of SaaS applications.

	![Replicon Provisioning](./media/active-directory-saas-replicon-provisioning-tutorial/Replicon2.png)

3. Select the **Provisioning** tab.

	![Replicon Provisioning](./media/active-directory-saas-replicon-provisioning-tutorial/RepliconProvisioningTab.png)

4. Set the **Provisioning Mode** to **Automatic**.

	![Replicon Provisioning](./media/active-directory-saas-replicon-provisioning-tutorial/Replicon1.png)

5. Under the **Admin Credentials** section, input the **Admin Username,Admin Password,CompanyId & Domain** of your Replicon's account.

6. Upon populating the fields shown above, click **Test Connection** to ensure Azure AD can connect to your Replicon app. If the connection fails, ensure your Replicon account has Admin permissions and try Step 5 again.

7. Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."

8. Click **Save**.

9. Under the **Mappings** section, select **Synchronize Azure Active Directory Users to Replicon**.

10. Review the user attributes that are synchronized from Azure AD to Replicon. The attributes selected as **Matching** properties are used to match the user accounts in Replicon for update operations. Select the Save button to commit any changes.

11. To enable the Azure AD provisioning service for Replicon, change the **Provisioning Status** to **On** in the **Settings** section

12. Click **Save**.

This starts the initial synchronization of any users and/or groups assigned to Replicon in the Users and Groups section. The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running. You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Replicon app.

For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).


## Additional resources

* [Managing user account provisioning for Enterprise Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## Next steps

* [Learn how to review logs and get reports on provisioning activity](active-directory-saas-provisioning-reporting.md)
