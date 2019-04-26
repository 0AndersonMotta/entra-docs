---
title: Client applications (Microsoft Authentication Library) | Azure
description: Learn about public client and confidential client applications in the Microsoft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''

ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
#Customer intent: As an application developer, I want to learn about the types of client application so I can decide if this platform meets my application development needs and requirements.
ms.collection: M365-identity-device-management
---

# Authentication flows

Authentication flows can be used in different scenarios.

| Flow | Description | Diagram |
| ---- | ----------- | ------- |
| [On-behalf-of](#on-behalf-of) | An application invokes a service/web API, which in turn needs to call another service/web API. The idea is to propagate the delegated user identity and permissions through the request chain. |![On-behalf-of flow][media/msal-authentication-flows/on-behalf-of.png] |


## Implicit grant

## Authorization code
When users login to Web applications (web sites) using Open Id connect, the web application receives an authorization code which it can redeem to acquire a token to call Web APIs. In ASP.NET / ASP.NET core web apps, the only goal of `AcquireTokenByAuthorizationCode` is to add a token to the token cache, so that it can then be used by the application (usually in the controllers) which just get a token for an API using `AcquireTokenSilent`.

## On-behalf-of

MSAL supports the [OAuth 2 on-behalf-of authentication flow](v2-oauth2-on-behalf-of-flow).  This flow is used when an application invokes a service/web API, which in turn needs to call another service/web API. The idea is to propagate the delegated user identity and permissions through the request chain. For the middle-tier service to make authenticated requests to the downstream service, it needs to secure an access token from the Microsoft identity platform, on behalf of the user.

![On-behalf-of flow][media/msal-authentication-flows/on-behalf-of.png]

- A client (Web, desktop, mobile, Single-page application) calls a protected Web API, providing a JWT bearer token in its "Authorization" Http Header.
- The protected Web API validates the token, requests a token on-behalf-of, to Azure AD, another token so that it can, itself, call a second Web API (named the downstream Web API) on behalf of the user.
- The protected Web API uses this token to call a downstream API, it can also later call `AcquireTokenSilent` to request tokens for other downstream APIs (but still on behalf of the same user). `AcquireTokenSilent` refreshes the token when needed.

## Username/password 
[Resource owner password credentials](v2-oauth-ropc.md)

In your desktop application, you can use the Username/Password flow to acquire a token silently. No UI is required when using the application.

> [!WARNING]
> This flow is **not recommended** because your application asking a user for their password is not secure. For more information about this problem, see [this article](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

The preferred flow for acquiring a token silently on Windows domain joined machines is [Integrated Windows Authentication](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Integrated-Windows-Authentication). Otherwise you can also use [Device code flow](https://aka.ms/msal-net-device-code-flow)

Although this is useful in some cases (DevOps scenarios), if you want to use Username/password in interactive scenarios where you provide your onw UI, you should really think about how to move away from it. By using username/password you are giving-up a number of things:
- core tenants of modern identity: password gets fished, replayed. Because we have this concept of a share secret that can be intercepted.
This is incompatible with passwordless.
- users who need to do MFA won't be able to sign-in (as there is no interaction)
- Users won't be able to do single sign-on

### Constraints

Apart from the [Integrated Windows Authentication constraints](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Integrated-Windows-Authentication#constraints), the following constraints also apply:

- Available starting with MSAL 2.1.0
- The Username/Password flow is not compatible with conditional access and multi-factor authentication: As a consequence, if your app runs in an Azure AD tenant where the tenant admin requires multi-factor authentication, you cannot use this flow. Many organizations do that.
- It works only for Work and school accounts (not MSA)
- The flow is available on .net desktop and .net core, but not on UWP

### Azure AD B2C specifics

[More information on using ROPC with Azure AD B2C](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics#resource-owner-password-credentials-ropc-with-b2c).

## Confidential client

MSAL.NET supports three types of client credentials:

- Application secrets
- Certificates
- Optimized client assertions

These client credentials need to be:

- registered with Azure AD
- Passed in the constructors of ``ConfidentialClientApplication`` in your code

Then you can call ``AcquireTokenForClient``.

## Device code
Interactive authentication with Azure AD requires a web browser (for details see [Usage of web browsers](MSAL.NET-uses-web-browser)). However, in the case of devices and operating systems that do not provide a Web browser, Device code flow lets the user use another device (for instance another computer or a mobile phone) to sign-in interactively. By using the device code flow, the application obtains tokens through a two-step process especially designed for these devices/OS. Examples of such applications are applications running on iOT, or Command-Line tools (CLI). The idea is that:

1. Whenever user authentication is required, the app provides a code and asks the user to use another device (such as an internet-connected smartphone) to navigate to a URL (for instance, http://microsoft.com/devicelogin), where the user will be prompted to enter the code. That done, the web page will lead the user through a normal authentication experience, including consent prompts and multi-factor authentication if necessary.

2. Upon successful authentication, the command-line app will receive the required tokens through a back channel and will use it to perform the web API calls it needs.

## Constraints

- Device Code Flow is only available on public client applications
- The authority passed in the `PublicClientApplicationBuilder` needs to be:
  - tenanted (of the form `https://login.microsoftonline.com/{tenant}/` where `tenant` is either the guid representing the tenant ID or a domain associated with the tenant.
  - or any work and school accounts (`https://login.microsoftonline.com/organizations/`)

  > Microsoft personal accounts are not yet supported by the Azure AD v2.0 endpoint (you cannot use /common or /consumers tenants)


## Integrated Windows Authentication
If your desktop or mobile application runs on Windows, and on a machine connected to a Windows domain - AD or AAD joined - it is possible to use the Integrated Windows Authentication (IWA) to acquire a token silently. No UI is required when using the application.

### Constraints

- **Federated** users only, i.e. those created in an Active Directory and backed by Azure Active Directory. Users created directly in AAD, without AD backing - **managed** users - cannot use this auth flow. This limitation does not affect the Username/Password flow.
- IWA is for apps written for .NET Framework, .NET Core and UWP platforms
- IWA does NOT bypass MFA (multi factor authentication). If MFA is configured, IWA might fail if an MFA challenge is required, because MFA requires user interaction. 
  > This one is tricky. IWA is non-interactive, but 2FA requires user interactivity. You do not control when the identity provider requests 2FA to be performed, the tenant admin does. From our observations, 2FA is required when you login from a different country, when not connected via VPN to a corporate network, and sometimes even when connected via VPN. Don’t expect a deterministic set of rules, Azure Active Directory uses AI to continuously learn if 2FA is required. You should fallback to a user prompt (https://aka.ms/msal-net-interactive) if IWA fails

- The authority passed in the `PublicClientApplicationBuilder` needs to be:
  - tenanted (of the form `https://login.microsoftonline.com/{tenant}/` where `tenant` is either the guid representing the tenant ID or a domain associated with the tenant.
  - for any work and school accounts (`https://login.microsoftonline.com/organizations/`)

  > Microsoft personal accounts are not supported (you cannot use /common or /consumers tenants)

- Because Integrated Windows Authentication is a silent flow:
  - the user of your application must have previously consented to use the application 
  - or the tenant admin must have previously consented to all users in the tenant to use the application.
  - This means that:
     - either you as a developer have pressed the **Grant** button on the Azure portal for yourself, 
     - or a tenant admin has pressed the **Grant/revoke admin consent for {tenant domain}** button in the **API permissions** tab of the registration for the application (See [Add permissions to access web APIs](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-permissions-to-access-web-apis))
     - or you have provided a way for users to consent to the application (See [Requesting individual user consent](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-permissions-and-consent#requesting-individual-user-consent))
     - or you have provided a way for the tenant admin to consent for the application (See [admin consent](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant))

- This flow is enabled for .net desktop, .net core and Windows Universal Apps. On .net core only the overload taking the username is available as the .NET Core platform cannot ask the username to the OS.
  
For more details on consent see [v2.0 permissions and consent](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-permissions-and-consent)