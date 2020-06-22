---
title: Migrate to Microsoft Authentication Library (MSAL)
titleSuffix: Microsoft identity platform
description: Learn about the differences between Microsoft Authentication Library (MSAL) and Azure AD Authentication Library (ADAL) and how to migrate to MSAL.
services: active-directory
author: jmprieur
manager: CelesteDG

ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 06/16/2020
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
# Customer intent: As an application developer, I want to learn about the differences between the ADAL and MSAL libraries so I can migrate my applications to MSAL.
---
# Migrate applications to Microsoft Authentication Library (MSAL)

Many developers have built and deployed applications using the Active Active Directory Authentication Library (ADAL). We now recommend using the Microsoft Authentication Library (MSAL) for authentication and authorization of Azure AD entities.

By using MSAL instead of ADAL:

- You can authenticate a broader set of identities:
  - Azure AD identities
  - Microsoft accounts
  - Social and local accounts by using Azure AD B2C
- Your users will get the best single-sign-on experience.
- Your application can enable incremental consent.
- Supporting Conditional Access is easier.
- You benefit from innovation. Because all Microsoft development efforts are now focused on MSAL, no new features will be implemented in ADAL.

**MSAL is now the recommended authentication library for use with the Microsoft identity platform**.

## Migration steps

The following articles can help you migrate to MSAL:

- [Migrate to MSAL.Android](migrate-android-adal-msal.md)
- [Migrate to MSAL.iOS / macOS](migrate-objc-adal-msal.md)
- [Migrate to MSAL Java](migrate-adal-msal-java.md)
- [Migrate to MSAL.js](msal-compare-msal-js-and-adal-js.md)
- [Migrate to MSAL.NET](msal-net-migration.md)
- [Migrate to MSAL Python](migrate-python-adal-msal.md)
- [Migrate Xamarin apps using brokers to MSAL.NET](msal-net-migration-ios-broker.md)

## Frequently asked questions (FAQ)

__Q: What do you mean by “deprecating ADAL”?__  
A: Starting June 30th, 2020, we will no longer add any new features to ADAL. We will continue adding critical security patches to ADAL until June 30th, 2022. We have archived or added notes on deprecation to ADAL docs, samples, and repos. 

__Q: How do I know which of my apps are using ADAL?__  
A: If you have the source code, you can reference the above migration guides to determine which client library you’re using and how to update to MSAL. If you don’t have the source code, you can reach out to support and they can provide you with a list of apps and which client library they are using.

__Q: What about my existing ADAL apps?__  
A: Your existing apps will continue to work without any changes. If you are planning on keeping them beyond June 30th, 2022, you could consider updating them to MSAL to keep them secure, but it is not required to maintain existing functionality.

__Q: Why should I invest in moving to MSAL?__  
A: MSAL contains new features that are not in ADAL, such as incremental consent, single sign-on, and token cache management. MSAL will also continue to receive security patches beyond June 30th, 2022. [Learn more.](https://docs.microsoft.com/azure/active-directory/develop/msal-overview)

__Q: Will you release a tool that helps me move my apps from ADAL to MSAL?__  
A: No, there are some differences between the two that make the cost of such a tool expensive and we would rather spend that time on new feature investments in MSAL. However, we do provide you a set of migration guides above to make the changes needed.

__Q: How does MSAL work with AD FS?__  
A: MSAL.NET supports certain scenarios to authenticate against AD FS 2019. If your app needs to acquire tokens directly from earlier version of AD FS, you should remain on ADAL. [Learn more.](https://docs.microsoft.com/azure/active-directory/develop/msal-net-adfs-support)

__Q: How do I get help migrating my application?__  
A: Each platform (e.g. MSAL.NET) has its own migration guides (see above). If after reading those guides you have further questions, you can post on Stack Overflow with the tag ‘adal-deprecation’ or open an issue against the platform’s Github repo. 

## Next steps

- [Update your applications to use Microsoft Authentication Library and Microsoft Graph API](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/update-your-applications-to-use-microsoft-authentication-library/ba-p/1257363)
- [Learn more about Microsoft identity platform (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/v2-overview)
- [Review our MSAL code samples](https://docs.microsoft.com/azure/active-directory/develop/sample-v2-code)
