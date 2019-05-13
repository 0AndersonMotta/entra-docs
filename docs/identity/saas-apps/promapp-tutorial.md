---
title: 'Tutorial: Azure Active Directory integration with Promapp | Microsoft Docs'
description: In this tutorial, you'll learn how to configure single sign-on between Azure Active Directory and Promapp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess

ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Promapp

In this tutorial, you'll learn how to integrate Promapp with Azure Active Directory (Azure AD).
This integration provides these benefits:

* You can use Azure AD to control who has access to Promapp.
* You can enable your users to be automatically signed in to Promapp (single sign-on) with their Azure AD accounts.
* You can manage your accounts in one central location: the Azure portal.

To learn more about SaaS app integration with Azure AD, see [Single sign-on to applications in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.

## Prerequisites

To configure Azure AD integration with Promapp, you need to have:

* An Azure AD subscription. If you don't have an Azure AD environment, you can sign up for a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).
* A Promapp subscription that has single sign-on enabled.

## Scenario description

In this tutorial, you'll configure and test Azure AD single sign-on in a test environment.

* Promapp supports SP-initiated and IdP-initiated SSO.

* Promapp supports just-in-time user provisioning.

## Add Promapp from the gallery

To set up the integration of Promapp into Azure AD, you need to add Promapp from the gallery to your list of managed SaaS apps.

1. In the [Azure portal](https://portal.azure.com), in the left pane, select **Azure Active Directory**:

	![Select Azure Active Directory](common/select-azuread.png)

2. Go to **Enterprise applications** > **All applications**:

	![Enterprise applications blade](common/enterprise-applications.png)

3. To add an application, select **New application** at the top of the window:

	![Select New application](common/add-new-app.png)

4. In the search box, enter **Promapp**. Select **Promapp** in the search results and then select **Add**.

	 ![Search results](common/search-new-app.png)

## Configure and test Azure AD single sign-on

In this section, you'll configure and test Azure AD single sign-on with Promapp by using a test user named Britta Simon.
To enable single sign-on, you need to establish relationship between an Azure AD user and the corresponding user in Promapp.

To configure and test Azure AD single sign-on with Promapp, you need to complete these steps:

1. **[Configure Azure AD single sign-on](#configure-azure-ad-single-sign-on)** to enable the feature for your users.
2. **[Configure Promapp single sign-on](#configure-promapp-single-sign-on)** on the application side.
3. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** to test Azure AD single sign-on.
4. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** to enable Azure AD single sign-on for the user.
5. **[Create a Promapp test user](#create-a-promapp-test-user)** that's linked to the Azure AD representation of the user.
6. **[Test single sign-on](#test-single-sign-on)** to verify that the configuration works.

### Configure Azure AD single sign-on

In this section, you'll enable Azure AD single sign-on in the Azure portal.

To configure Azure AD single sign-on with Promapp, take these steps:

1. In the [Azure portal](https://portal.azure.com/), on the **Promapp** application integration page, select **Single sign-on**:

    ![Select single sign-on](common/select-sso.png)

2. In the **Select a single sign-on method** dialog box, select **SAML/WS-Fed** mode to enable single sign-on:

    ![Select a single sign-on method](common/select-saml-option.png)

3. On the **Set up Single Sign-On with SAML** page, select the **Edit** icon to open the **Basic SAML Configuration** dialog box:

	![Edit icon](common/edit-urls.png)

4. In the **Basic SAML Configuration** dialog box, if you want to configure the application in IdP-initiated mode, complete the following steps.

    ![Basic SAML Configuration dialog box](common/idp-intiated.png)

    1. In the **Identifier** box, enter a URL in this pattern:

       | |
    	|--|
	    | `https://go.promapp.com/TENANTNAME/`|
	    | `https://au.promapp.com/TENANTNAME/`|
	    | `https://us.promapp.com/TENANTNAME/`|
	    | `https://eu.promapp.com/TENANTNAME/`|
	    | `https://ca.promapp.com/TENANTNAME/`|
	    |   |

	   > [!NOTE]
	   > Azure AD integration with Promapp is currently configured only for service-initiated authentication. (That is, going to a Promapp URL initiates the authentication process.) But the **Reply URL** field is a required field.

    1. In the **Reply URL** box, enter a URL in this pattern:

       `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate.aspx`

5. If you want to configure the application in SP-initiated mode, select **Set additional URLs**. In the **Sign on URL** box, enter a URL in this pattern:

      `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate`

    ![Promapp Domain and URLs single sign-on information](common/metadata-upload-additional-signon.png)

   

	> [!NOTE]
	> These values are placeholders. You need to use the actual identifier, reply URL, and sign-on URL. Contact the [Promapp support team](https://www.promapp.com/about-us/contact-us/) to get the values. You can also refer to the patterns shown in the **Basic SAML Configuration** dialog box in the Azure portal.

6. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section, select the **Download** link next to **Certificate (Base64)**, per your requirements, and save the certificate on your computer.

	![Certificate download link](common/certificatebase64.png)

7. In the **Set up Promapp** section, copy the appropriate URLs, based on your requirements:

	![Copy the configuration URLs](common/copy-configuration-urls.png)

	1. **Login URL**.

	1. **Azure AD Identifier**.

	1. **Logout URL**.

### Configure Promapp single sign-on

1. Sign in to your Promapp company site as an admin.

2. In the menu at the top of the window, select **Admin**:
   
    ![Select Admin][12]

3. Select **Configure**:
   
    ![Select Configure][13]

4. In the **Security** dialog box, take the following steps.
   
    ![Security dialog box][14]
	
	1. Paste the **Login URL** that you copied from the Azure portal into the **SSO-Login URL** box.
	
	1. In the **SSO - Single Sign-on Mode** list, select **Optional**. Select **Save**.

	   > [!NOTE]
	   > Optional mode is for testing only. After you're happy with the configuration, select **Required** in the **SSO - Single Sign-on Mode** list to force all users to authenticate with Azure AD.

	1. In Notepad, open the  certificate that you downloaded in the previous section. Copy the contents of the certificate without the first line (**-----BEGIN CERTIFICATE-----**) or the last line (**-----END CERTIFICATE-----**). Paste the certificate content into the **SSO-x.509 Certificate** box, and then select **Save**.

### Create an Azure AD test user

In this section, you'll create a test user named Britta Simon in the Azure portal.

1. In the Azure portal, select **Azure Active Directory** in the left pane,, select **Users**, and then select **All users**.

    ![The "Users and groups" and "All users" links](common/users.png)

2. Select **New user** at the top of the screen.

    ![New user Button](common/new-user.png)

3. In the User properties, perform the following steps.

    ![The User dialog box](common/user-properties.png)

    a. In the **Name** field enter **BrittaSimon**.
  
    b. In the **User name** field type brittasimon@yourcompanydomain.extension. For example, BrittaSimon@contoso.com

    c. Select **Show password** check box, and then write down the value that's displayed in the Password box.

    d. Click **Create**.

### Assign the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Promapp.

1. In the Azure portal, select **Enterprise Applications**, select **All applications**, then select **Promapp**.

	![Enterprise applications blade](common/enterprise-applications.png)

2. In the applications list, select **Promapp**.

	![The Promapp link in the Applications list](common/all-applications.png)

3. In the menu on the left, select **Users and groups**.

    ![The "Users and groups" link](common/users-groups-blade.png)

4. Click the **Add user** button, then select **Users and groups** in the **Add Assignment** dialog.

    ![The Add Assignment pane](common/add-assign-user.png)

5. In the **Users and groups** dialog select **Britta Simon** in the Users list, then click the **Select** button at the bottom of the screen.

6. If you are expecting any role value in the SAML assertion then in the **Select Role** dialog select the appropriate role for the user from the list, then click the **Select** button at the bottom of the screen.

7. In the **Add Assignment** dialog click the **Assign** button.

### Create Promapp test user

In this section, a user called Britta Simon is created in Promapp. Promapp supports just-in-time user provisioning, which is enabled by default. There is no action item for you in this section. If a user doesn't already exist in Promapp, a new one is created after authentication.

### Test single sign-on 

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Promapp tile in the Access Panel, you should be automatically signed in to the Promapp for which you set up SSO. For more information about the Access Panel, see [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## Additional Resources

- [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [What is conditional access in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[12]: ./media/promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/promapp-tutorial/tutorial_promapp_07.png
