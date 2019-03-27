---
title: Advanced certificate signing options in the SAML token for pre-integrated apps in Azure Active Directory | Microsoft Docs
description: Learn how to use advanced certificate signing options in the SAML token for pre-integrated apps in Azure Active Directory
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman

ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: celested
ms.reviewer: jeedes
ms.custom: aaddev

ms.collection: M365-identity-device-management
---
# Advanced certificate signing options in the SAML token for gallery apps in Azure Active Directory

Today Azure Active Directory (Azure AD) supports thousands of pre-integrated applications in the Azure Active Directory App Gallery. Over 500 of the applications support single sign-on by using the SAML 2.0 protocol, such as the [NetSuite application](https://azuremarketplace.microsoft.com/marketplace/apps/aad.netsuite). When a user authenticates to an application through Azure AD by using SAML, Azure AD sends a token to the application (via an HTTP POST). The application then validates and uses the token to sign in the user instead of prompting for a username and password. These SAML tokens are signed with the unique certificate that's generated in Azure AD and by specific standard algorithms.

Azure AD uses some of the default settings for the gallery applications. The default values are set up based on the application's requirements.

In Azure AD, you can set up certificate signing options and the certificate signing algorithm.

## Certificate signing options

Azure AD supports three certificate signing options:

* **Sign SAML assertion**. This default option is set for most of the gallery applications. If this option is selected, Azure AD as an IdP signs the SAML assertion and certificate with the X509 certificate of the application. Also, it uses the signing algorithm, which is selected in the **Signing Algorithm** drop-down list.

* **Sign SAML response**. If this option is selected, Azure AD as an IdP signs the SAML response with the X509 certificate of the application. Also, it uses the signing algorithm, which is selected in the **Signing Algorithm** drop-down list.

* **Sign SAML response and assertion**. If this option is selected, Azure AD as an IdP signs the entire SAML token with the X509 certificate of the application. Also, it uses the signing algorithm, which is selected in the **Signing Algorithm** drop-down list.

## Certificate signing algorithms

Azure AD supports two signing algorithms to sign the SAML response:

* **SHA-256**. Azure AD uses this default algorithm to sign the SAML response. It's the newest algorithm and is treated as more secure than SHA-1. Most of the applications support the SHA-256 algorithm. If an application supports only SHA-1 as the signing algorithm, you can change it. Otherwise, we recommend that you use the SHA-256 algorithm for signing the SAML response.

* **SHA-1**. This algorithm is older, and it's treated as less secure than SH-256. If an application supports only this signing algorithm, you can select this option in the **Signing Algorithm** drop-down list. Azure AD then signs the SAML response with the SHA-1 algorithm.

## Change the certificate signing options and certificate signing algorithm

To change an application's SAML certificate signing options and the certificate signing algorithm, select the application in question:

1. Go to the [Azure portal](https://portal.azure.com/).
1. Sign in to your Azure account.
1. In the Microsoft Azure blade, select **Azure Active Directory**. An overview page for your Azure AD account appears.
1. In the account overview blade, select **Enterprise applications**. The applications in your account appear.
1. In the account overview page, select an application. An overview page for the application appears.

   ![Application overview page](./media/certificate-signing-options/application-overview-page.png)

Next, change the certificate signing options in the SAML token for that application:

1. In the application overview blade, select **Single sign-on**.

2. If the **Set up Single Sign-On with SAML - Preview** page appears, go to step 5.

3. If the **Select a single sign-on method** page doesn't appear, select **Change single sign-on modes** to display that page.

4. In the **Select a single sign-on method** page, select **SAML** if available. (If **SAML** isn't available, the application doesn't support SAML, and you can ignore the rest of this procedure and article.)

5. In the **Set up Single Sign-On with SAML - Preview** page, select the **Edit** icon (a pencil) in the **SAML Signing Certificate** heading. The **SAML Signing Certificate** page appears.

   ![SAML signing page](./media/certificate-signing-options/saml-signing-page.png)

6. In the **Signing Option** dropdown list, choose **Sign SAML response**, **Sign SAML assertion**, or **Sign SAML response and assertion**. (These options are described in the [Certificate signing options](#certificate-signing-options) section earlier.)

7. In the **Signing Algorithm** dropdown list, choose **SHA-1** or **SHA-256**, which are described in the [Certificate signing algorithms](#certificate-signing-algorithms) section earlier.

8. If you're satisfied with your choices, select **Save** to apply the new SAML signing certificate settings. Otherwise, select the **X** to discard the changes.

## Next steps

* [Configure single sign-on to applications that are not in the Azure Active Directory App Gallery](configure-federated-single-sign-on-non-gallery-applications.md)
* [Troubleshoot SAML-based single sign-on](../develop/howto-v1-debug-saml-sso-issues.md)
