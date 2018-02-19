---
title: Add a multitenant application to the Azure AD Application Gallery | Microsoft Docs
description:  Explains how you can list your custom-developed multitenant application in the Azure AD Application Gallery.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila

ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: jeedes

---

# Add a multitenant application to the Azure AD Application Gallery

## What is the Azure AD Application Gallery?

Azure Active Directory (Azure AD) is a cloud-based identity service. The [Azure AD Application Gallery](https://azure.microsoft.com/marketplace/active-directory/all/) is in the Azure Marketplace app store, where all application connectors are published for single sign-on and user provisioning. Customers who use Azure AD as an identity provider find the different SaaS application connectors published here. IT administrators add connectors from the app gallery, and then configure and use the connectors for single sign-on and provisioning. Azure AD supports all major federation protocols, including SAML 2.0, OpenID Connect, OAuth, and WS-Fed for single sign-on. 

## If your application supports SAML or OpenIDConnect
If you have a multitenant application you'd like listed in the Azure AD Application Gallery, you must first make sure that your application supports one of the following single sign-on technologies:

- **OpenID Connect**: To have your app listed, create the multitenant application in Azure AD and implement the [Azure AD consent framework](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) for your application. Send the login request to a common endpoint so that any customer can provide consent to the application. You can control a user's access based on the tenant ID and the user's UPN received in the token. Submit the application by using the process in [Listing your application in the Azure Active Directory application gallery](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

- **SAML**: If your application supports SAML 2.0, Microsoft can list your application in the gallery. Follow the instructions in [Listing your application in the Azure Active Directory application gallery](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

## If your application does not support SAML or OpenIDConnect
If your application does not support SAML or OpenIDConnect, Microsoft can still integrate it into our gallery by using Password Single Sign-on technology.

- **Password SSO**: Password-based SSO, also referred to as password vaulting, enables you to manage user access and passwords to web applications that don't support identity federation. It is also useful for scenarios in which several users need to share a single account, such as to your organization's social media app accounts. 

If you'd like to list your application with this technology:
1. Create a web application that has an HTML sign-in page to configure [password-based single sign-on](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis). 
2. Submit the request as described in [Listing your application in the Azure Active Directory application gallery](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

## Escalations

For any escalations, drop an email to [Azure AD SSO Integration Team](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) and we get back to you ASAP.

## Next steps
Learn how to [List your application in the Azure Active Directory application gallery](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
