---
title: Configure single sign-on - Azure Active Directory | Microsoft Docs
description: This tutorial uses the Azure portal to configure SAML-based single sign-on for an application with Azure Active Directory (Azure AD). 
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 04/08/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
---

# How to configure SAML-based single sign-on

This article describes how to configure SAML-based single sign-on for an app after you've added the app to your Azure Active Directory (Azure AD) Enterprise Applications.  

> [!IMPORTANT]
> If the app was added from the Azure AD gallery, there is a setup tutorial available for the app. See the [list of tutorials for integrating SaaS apps with Azure Active Directory](../saas-apps/tutorial-list.md) for step-by-step guidance tailored to your app.

## Before you begin

- If the application hasn't been added to your Azure AD tenant, see [Add a gallery app](add-gallery-app.md) or [Add a non-gallery app](add-non-gallery-app.md).
- Contact your application vendor to get the correct information for the following settings:

    | Basic SAML Configuration setting | SP-Initiated | idP-Initiated | Description |
    |:--|:--|:--|:--|
    | Identifier (Entity ID) | Required for some apps | Required for some apps | Uniquely identifies the application for which single sign-on is being configured. Azure AD sends the identifier to the application as the Audience parameter of the SAML token. The application is expected to validate it. This value also appears as the Entity ID in any SAML metadata provided by the application. *You can find this value as the **Issuer** element in the **AuthnRequest** (SAML request) sent by the application.* |
    | Reply URL | Optional | Required | Specifies where the application expects to receive the SAML token. The reply URL is also referred to as the Assertion Consumer Service (ACS) URL. |
    | Sign-on URL | Required | Don't specify | When a user opens this URL, the service provider redirects to Azure AD to authenticate and sign on the user. Azure AD uses the URL to start the application from Office 365 or the Azure AD Access Panel. When blank, Azure AD relies on the identity provider to start single sign-on when a user launches the application.|
    | Relay State | Optional | Optional | Specifies to the application where to redirect the user after authentication is completed. Typically the value is a valid URL for the application. However, some applications use this field differently. For more information, ask the application vendor.
    | Logout URL | Optional | Optional | Used to send the SAML Logout responses back to the application.

## Step 1. Edit the Basic SAML Configuration

1. Sign in to the [Azure portal](https://portal.azure.com) as a cloud application admin, or an application admin for your Azure AD tenant.

1. Navigate to **Azure Active Directory** > **Enterprise applications** and select the application from the list. 
   
   - To search for the application, in the **Application Type** menu, select **All applications**, and then select **Apply**. Enter the name of the application in the search box, and then select the application from the results.

1. Under the **Manage** section, select **Single sign-on**. 

1. Select **SAML**. The **Set up Single Sign-On with SAML - Preview** page appears.

1. To edit the basic SAML configuration options, select the **Edit** icon (a pencil) in the upper-right corner of the **Basic SAML Configuration** section.

     ![Configure certificates](media/configure-single-sign-on-portal/basic-saml-configuration-edit-icon.png)

1. In the appropriate fields, enter the information described in the [Before you begin](#before-you-begin) section.

1. At the top of the page, select **Save**.

## Step 2. Configure User attributes and claims 

To specify the information Azure AD sends to the application in the SAML token when a user signs in, you can configure user attributes and claims. For example, you can configure Azure AD to send the user's name, email, and employee ID to the application. Depending on the application, certain user attributes and claims might be required to make single sign-on work properly. For example, the application might require a different set of claim URIs or claim values. Or, **Name** might need to be something other than the username stored in Microsoft identity platform. For more information, see the [application-specific tutorial](../saas-apps/tutorial-list.md), or ask the application vendor. Below are general steps for configuring user attributes and claims. For details about customizing claims, refer to the following articles: 
   - [How to configure the role claim issued in the SAML token for enterprise applications](../develop/active-directory-enterprise-app-role-management.md) 
   - [How to customize claims issued in the SAML token for enterprise applications](../develop/active-directory-saml-claims-customization.md) 
   - [Customize claims - PowerShell](../develop/active-directory-claims-mapping.md)
   - [How to provide optional claims to your Azure AD app](../develop/active-directory-optional-claims.md)
   - [Configure group claims for applications with Azure Active Directory (Public Preview)](../hybrid/how-to-connect-fed-group-claims.md)
   - [Configurable token lifetimes in Azure Active Directory (Preview)](../develop/active-directory-configurable-token-lifetimes.md)

To edit user attributes and claims, follow these steps.
1. select the **Edit** icon (a pencil) in the upper-right corner of the **User Attributes and Claims** section.

   The **Name Identifier Value** is set with the default value of *user.principalname*. The user identifier uniquely identifies each user within the application. For example, if the email address is both the username and the unique identifier, set the value to *user.mail*.

1. To modify the **Name Identifier Value**, select the **Edit** icon (a pencil) for the **Name Identifier Value** field. Make the appropriate changes to the identifier format and source, as needed. For details, see [Editing NameId](https://docs.microsoft.com/en-us/azure/active-directory//develop/active-directory-saml-claims-customization#editing-nameid). Save the changes when you're done. 
 
1. To configure group claims, select the **Edit** icon for the **Groups returned in claim** field.

3. To add a claim, select **Add new claim** at the top of the page. Enter the **Name** and select the appropriate source. If you select the **Attribute** source, you'll need to choose the **Source attribute** you want to use. If you select the **Translation** source, you'll need to choose the **Transformation** and **Parameter 1** you want to use. For details, see the following articles:

4. Select **Save**. The new claim appears in the table.
 
## Step 3. Generate a SAML signing certificate

Azure AD uses a certificate to sign the SAML tokens that it sends to the application. For gallery applications, details on the certificate format are available in the application’s SAML documentation. You can download the active certificate in Base64 or Raw format directly from the **Set up Single Sign-On with SAML** page. Or, you can get the active certificate by downloading the application metadata XML file or by using the App federation metadata URL.

1. To generate a new certificate, select the **Edit** icon (a pencil) in the upper-right corner of the **SAML Signing Certificate** section.

2. In the **SAML Signing Certificate** section, select **New Certificate**.

3. In the new certificate row that appears, set the **Expiration Date**. 

4. To configure advanced certificate signing options, use the following options. For descriptions of these options, see the [Advanced certificate signing options](certificate-signing-options.md) article.

   - In the **Signing Option** drop-down list, choose **Sign SAML response**, **Sign SAML assertion**, or **Sign SAML response and assertion**.

   - In the **Signing Algorithm** drop-down list, choose **SHA-1** or **SHA-256**.

1. Enter the **Notification email addresses**. When the active certificate is near the expiration date, Azure AD sends a notification to this email address.

1. Select **Save** at the top of the **SAML Signing Certificate** section. 

## Step 4. Set up the application to use Azure AD

The **Set up \<applicationName>** section lists the values that need to be configured in the application so it will use Azure AD as a SAML identity provider. The required values vary according to the application. For details, see the application's SAML documentation.

1. Scroll down to the **Set up \<applicationName>** section. 
2. Copy the value from each row in this section as needed and follow the application-specific instructions for adding the value to the application. For gallery apps, you can view the documentation by selecting **View step-by-step instructions**. 
   - The **Login URL** and **Logout URL** values both resolve to the same endpoint, which is the SAML request-handling endpoint for your instance of Azure AD. 
   - The **Azure AD Identifier** is the value of the **Issuer** in the SAML token issued to the application.
1. When you've pasted all the values into the appropriate fields, select **Save**.

## Step 5. Validate single sign-on

You're ready to test the settings to see if single sign-on works for you, the admin.  

1. Open the single sign-on settings for your application. 
2. Scroll to the **Validate single sign-on with <applicationName>** section. For this tutorial, this section is called **Set up GitHub-test**.
3. Select **Test**. The testing options appear.
4. Select **Sign in as current user**. 

If sign-on is successful, you're ready to assign users and groups to your SAML application.
If an error message appears. Complete the following steps:

1. Copy and paste the specifics into the **What does the error look like?** box.

    ![Get resolution guidance](media/configure-single-sign-on-portal/error-guidance.png)

2. Select **Get resolution guidance**. The root cause and resolution guidance appear.  In this example, the user wasn't assigned to the application.

3. Read the resolution guidance and then, if possible, fix the issue.

4. Run the test again until it completes successfully.

## Next steps

- [Assign users or groups to the application](methods-for-assigning-users-and-groups)
- [Configure automatic user account provisioning](configure-automatic-user-provisioning-portal.md)
