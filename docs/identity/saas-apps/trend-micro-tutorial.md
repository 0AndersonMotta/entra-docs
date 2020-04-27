---
title: 'Tutorial: Azure AD SSO integration with Trend Micro Web Security (TMWS)'
description: Learn how to configure single sign-on between Azure Active Directory and Trend Micro Web Security (TMWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess

ms.assetid: 827285d3-8e65-43cd-8453-baeda32ef174
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/21/2020
ms.author: jeedes

ms.collection: M365-identity-device-management
---

# Tutorial: Azure Active Directory single sign-on (SSO) integration with Trend Micro Web Security (TMWS)

In this tutorial, you'll learn how to integrate Trend Micro Web Security (TMWS) with Azure Active Directory (Azure AD). When you integrate Trend Micro Web Security with Azure AD, you can:

* Control in Azure AD who has access to Trend Micro Web Security.
* Enable your users to be automatically signed in to Trend Micro Web Security with their Azure AD accounts.
* Manage your accounts in one central location: the Azure portal.

To learn more about SaaS app integration with Azure AD, see [Single sign-on to applications in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## Prerequisites

To get started, you need:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* A Trend Micro Web Security subscription that's enabled for SSO.

## Scenario description

In this tutorial, you'll configure and test Azure AD SSO in a test environment.

* Trend Micro Web Security supports SP-initiated SSO.
* After you configure Trend Micro Web Security, you can enforce session control, which protects exfiltration and infiltration of your organization's sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control by using Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## Add Trend Micro Web Security from the gallery

To configure the integration of Trend Micro Web Security into Azure AD, you need to add Trend Micro Web Security from the gallery to your list of managed SaaS apps.

1. Sign in to the [Azure portal](https://portal.azure.com) with either a work or school account or a personal Microsoft account.
1. In the left pane, select the **Azure Active Directory** service.
1. Select **Enterprise applications** and then select **All applications**.
1. To add a new application, select **New application**.
1. In the **Add from the gallery** section, enter **Trend Micro Web Security (TMWS)** in the search box.
1. Select **Trend Micro Web Security (TMWS)** in the search results and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Trend Micro Web Security

You'll configure and test Azure AD SSO with Trend Micro Web Security by using a test user called B.Simon. For SSO to work, you need to establish a link between an Azure AD user and the related user in Trend Micro Web Security.

You'll complete these basic steps to configure and test Azure AD SSO with Trend Micro Web Security:

1. [Configure Azure AD SSO](#configure-azure-ad-sso) to enable the feature for your users.
    1. [Create an Azure AD user](#create-an-azure-ad-test-user) to test Azure AD single sign-on.
    1. [Assign the Azure AD test user](#assign-the-azure-ad-test-user) to enable the user for Azure AD single sign-on.
    1. [Configure user and group synchronization settings in Azure AD](#configure-user-and-group-synchronization-settings-in-azure-ad).
1. [Configure Trend Micro Web Security SSO](#configure-trend-micro-web-security-sso) on the application side.
1. [Test SSO](#test-sso) to verify the configuration.

## Configure Azure AD SSO

Complete these steps to enable Azure AD SSO in the Azure portal.

1. In the [Azure portal](https://portal.azure.com/), on the **Trend Micro Web Security (TMWS)** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up Single Sign-On with SAML** page, select the pen button for **Basic SAML Configuration** to edit the settings:

   ![Edit the Basic SAML Configuration settings](common/edit-urls.png)

1. In the **Basic SAML Configuration** section, enter values for the following fields:

    a. In the **Identifier (Entity ID)** box, enter a URL in the following pattern:

    `https://auth.iws-hybrid.trendmicro.com/([0-9a-f]{16})`

    b. In the **Reply URL** box, enter this URL:

    `https://auth.iws-hybrid.trendmicro.com/simplesaml/module.php/saml/sp/saml2-acs.php/ics-sp`

    > [!NOTE]
    > The identifier value in the previous step isn't the value that you should enter. You need to use the actual identifier. You can get this value in the **Service Provider Settings for the Azure Admin Portal** section on the **Authentication Method** page for Azure AD from **Administration > Directory Services**.

1. Trend Micro Web Security expects the SAML assertions in a specific format, so you need to add custom attribute mappings to your SAML token attributes configuration. This screenshot shows the default attributes:

    ![Default attributes](common/default-attributes.png)

1. In addition to the attributes in the preceding screenshot, Trend Micro Web Security expects two more attributes to be passed back in the SAML response. These attributes are shown in the following table. The attributes are pre-populated, but you can change them to meet your requirements.
    
    | Name | Source attribute|
    | --------------- | --------- |
    | sAMAccountName | user.onpremisessamaccountname |
    | uPN | user.userprincipalname |

1. On the **Set up single sign-on with SAML** page, in the **SAML Signing Certificate** section, find **Certificate (Base64)**. Select the **Download** link next to this certificate name to download the certificate and save it on your computer:

    ![Certificate download link](common/certificatebase64.png)

1. In the **Set up Trend Micro Web Security (TMWS)** section, copy the appropriate URL or URLs, based on your requirements:

    ![Copy the configuration URLs](common/copy-configuration-urls.png)

### Create an Azure AD test user

In this section, you'll create a test user in the Azure portal called B.Simon.

1. From the left pane in the Azure portal, select **Azure Active Directory**, select **Users**, and then select **All users**.
1. Select **New user** at the top of the screen.
1. In the **User** properties, follow these steps:
   1. In the **Name** field, enter `B.Simon`.  
   1. In the **User name** field, enter the username@companydomain.extension. For example, `B.Simon@contoso.com`.
   1. Select the **Show password** check box, and then write down the value that's displayed in the **Password** box.
   1. Click **Create**.

### Assign the Azure AD test user

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Trend Micro Web Security(TMWS).

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Trend Micro Web Security(TMWS)**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.

   ![The "Users and groups" link](common/users-groups-blade.png)

1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.

    ![The Add User link](common/add-assign-user.png)

1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you're expecting any role value in the SAML assertion, in the **Select Role** dialog, select the appropriate role for the user from the list and then click the **Select** button at the bottom of the screen.
1. In the **Add Assignment** dialog, click the **Assign** button.

### Configure user and group synchronization settings in Azure AD

1. From the left navigation, click **Azure Active Directory**.

1. Under **Manage**, click **App registrations** and then click your new enterprise application under the **All applications** area.

1. Under **Manage**, click **Certificates & secrets**.

1. Under the Client secrets area that appears, click **New client secret**.

1. On the Add a client secret screen that appears, optionally add a description and select an expiration period for this client secret, and then click **Add**. The newly added client secret appears under the Client secrets area.

1. Record the value. Later, you will type the information into TMWS.

1. Under **Manage**, click **API permissions**. 

1. On the API permissions screen that appears, click **Add a permission**.

1. On the Microsoft APIs tab of the Request API permissions screen that appears, click **Microsoft Graph** and then **Application permissions**.

1. Locate and add the following permissions: 

    * Group.Read.All
    * User.Read.All

1. Click **Add permissions**. A message appears to confirm that your settings were saved successfully. The newly added permissions appear on the API permissions screen.

1. Under the Grant consent area, click **Grant admin consent for < your administrator account > (Default Directory)** and then **Yes**. A message appears to confirm that the admin consent for the requested permissions was successfully granted.

1. Click **Overview**. 

1. In the right pane that appears, record the Application (client) ID and Directory (tenant) ID. Later, you will type the information into TMWS. You can also click **Custom domain names** under Azure **Active Directory > Manage** and record the domain name in the right pane.

## Configure Trend Micro Web Security SSO

Complete these steps to configure Trend Micro Web Security SSO on the application side.

1. Sign into the TMWS management console, and go to **Administration** > **USERS & AUTHENTICATION** > **Directory Services**.

1. Click here on the upper area of the screen.

1. On the Authentication Method screen that appears, click **Azure AD**.

1. Click **On** or **Off** to decide whether to allow the AD users of your organization to visit websites through TMWS if their data is not synchronized to TMWS.

    > [!NOTE]
    > Users not synchronized from Azure AD can be authenticated only through known TMWS gateways or the dedicated port for your organization.

1. On the **Identity Provider Settings** section, perform the following steps:

    a. In the **Service URL** field, paste the **Login URL** value, which you have copied from Azure portal

    b. In the **Logon name attribute** field, paste the User claim name with the **user.onpremisessamaccountname** source attribute from the Azure portal.

    c. In the **Public SSL certificate** field, use the downloaded **Certificate (Base64)** from the Azure portal.

1. On the **Synchronization Settings** section, perform the following steps:

    a. In the **Tenant** field, use **Directory (tenant) ID** or **Custom domain name** value from the Azure portal.

    b. In the **Application ID** field, **Application (client) ID** value from the Azure portal.

    c. In the **Client secret** field, use **Client secret** from the Azure portal.

    d. In the **Synchronization schedule** field, Select to synchronize with Azure AD manually or according to a schedule. If you choose Manually, whenever there are changes to Active Directory user information, remember to go back to the Directory Services screen and perform manual synchronization so that information in TMWS remains current.

    e. Click **Test Connection** to check whether the Azure AD service can be connected successfully. 
    
    f. Click **Save**.
 
 > [!NOTE]
 > For more information on how to configure Trend Micro Web Security with Azure AD, please refer [this](https://docs.trendmicro.com/en-us/enterprise/trend-micro-web-security-online-help/administration_001/directory-services/azure-active-directo/configuring-azure-ad.aspx) document.

## Test SSO 

Once you successfully configured the Azure AD service and specified Azure AD as the user authentication method, you can log on to the TMWS proxy server to verify your setup. After the Azure AD logon verifies your account, you can visit the Internet.

> [!NOTE]
> TMWS does not support testing single sign-on from the Azure AD portal, under Overview > Single sign-on > Set up Single Sign-on with SAML > Test of your new enterprise application.

1. Clear the browser of all cookies and then restart the browser. 

1. Point your browser to the TMWS proxy server. 
For details, see [Traffic Forwarding Using PAC Files](https://docs.trendmicro.com/en-us/enterprise/trend-micro-web-security-online-help/administration_001/pac-files/traffic-forwarding-u.aspx#GUID-A4A83827-7A29-4596-B866-01ACCEDCC36B).

1. Visit any Internet website. TMWS will direct you to the TMWS captive portal.

1. Specify an Active Directory account (format: domain\sAMAccountName or sAMAccountName@domain), or email address, or UPN, and then click **Log On**. TMWS sends you to the Azure AD logon.

1. On the Azure AD logon, type your AD account credentials. You should successfully log on to TMWS.

## Additional resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [What is conditional access in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Try Trend Micro Web Security(TMWS) with Azure AD](https://aad.portal.azure.com/)

- [What is session control in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [How to protect Trend Micro Web Security(TMWS) with advanced visibility and controls](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

