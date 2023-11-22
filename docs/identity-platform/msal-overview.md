---
title: Overview of the Microsoft Authentication Library (MSAL)
description: The Microsoft Authentication Library (MSAL) enables application developers to acquire tokens in order to call secured web APIs. These web APIs can be the Microsoft Graph, other Microsoft APIs, third-party web APIs, or your own web API. MSAL supports multiple application architectures and platforms.

author: cilwerner
manager: CelesteDG

ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.date: 11/17/2023
ms.author: cwerner
ms.reviewer: saeeda
ms.custom:  has-adal-ref

#Customer intent: As an application developer, I want to learn about the Microsoft Authentication Library so I can decide if this platform meets my application development needs and requirements.
---

# Overview of the Microsoft Authentication Library (MSAL)

The Microsoft Authentication Library (MSAL) enables developers to acquire [security tokens](developer-glossary.md#security-token) from the Microsoft identity platform to authenticate users and access secured web APIs. It can be used to provide secure access to Microsoft Graph, other Microsoft APIs, third-party web APIs, or your own web API. MSAL supports many different application architectures and platforms including .NET, JavaScript, Java, Python, Android, and iOS.

MSAL provides multiple ways to get security tokens, with a consistent API for many platforms. Using MSAL provides the following benefits:

* There is no need to directly use the OAuth libraries or code against the protocol in your application.
* Can acquire tokens on behalf of a user or application (when applicable to the platform).
* Maintains a token cache for you and handles token refreshes when they're close to expiring.
* Helps you specify which audience you want your application to sign in. The sign in audience can include personal Microsoft accounts, social identities with Azure AD B2C organizations, work, school, or users in sovereign and national clouds.
* Helps you set up your application from configuration files.
* Helps you troubleshoot your app by exposing actionable exceptions, logging, and telemetry.

> [!VIDEO https://www.youtube.com/embed/zufQ0QRUHUk]

## Application types and scenarios

Using MSAL, a token can be acquired for the following application types: 

- Web applications
- Web APIs
- Single-page apps (JavaScript)
- Mobile and native applications
- Daemons and server-side applications


MSAL can be used in several application scenarios, such as the following:

* [Single page applications (JavaScript)](scenario-spa-overview.md)
* [Web app signing in users](scenario-web-app-sign-user-overview.md)
* [Web application signing in a user and calling a web API on behalf of the user](scenario-web-app-call-api-overview.md)
* [Protecting a web API so only authenticated users can access it](scenario-protected-web-api-overview.md)
* [Web API calling another downstream web API on behalf of the signed-in user](scenario-web-api-call-api-overview.md)
* [Desktop application calling a web API on behalf of the signed-in user](scenario-desktop-overview.md)
* [Mobile application calling a web API on behalf of the user who's signed-in interactively](scenario-mobile-overview.md).
* [Desktop/service daemon application calling web API on behalf of itself](scenario-daemon-overview.md)

## MSAL documentation

You can refer to the following documentation to learn more about the different MSAL libraries.

[MSAL.NET](/entra/msal/dotnet/)
[MSAL for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android/tree/dev/docs)
[MSAL Angular](/javascript/api/@azure/msal-angular/)
[MSAL for iOS and macOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc/tree/dev/docs)
[MSAL Java](/entra/msal/java/)
[MSAL.js](/javascript/api/overview/msal-overview)
[MSAL Node](/javascript/api/%40azure/msal-node/)
[MSAL Python](/entra/msal/python/)
[MSAL React](/javascript/api/%40azure/msal-react/)

## Languages and frameworks

| Library | Supported platforms and frameworks|
| --- | --- |
| [MSAL for Android](https://github.com/AzureAD/microsoft-authentication-library-for-android)|Android|
| [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular)| Single-page apps with Angular and Angular.js frameworks|
| [MSAL for iOS and macOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|iOS and macOS|
| [MSAL Go (Preview)](https://github.com/AzureAD/microsoft-authentication-library-for-go)|Windows, macOS, Linux|
| [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java)|Windows, macOS, Linux|
| [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)| JavaScript/TypeScript frameworks such as Vue.js, Ember.js, or Durandal.js|
| [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)| .NET Framework, .NET Core, Xamarin Android, Xamarin iOS, Universal Windows Platform|
| [MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node)|Web apps with Express, desktop apps with Electron, Cross-platform console apps|
| [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)|Windows, macOS, Linux|
| [MSAL React](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-react)| Single-page apps with React and React-based libraries (Next.js, Gatsby.js)|

## Migrate apps that use ADAL to MSAL

Active Directory Authentication Library (ADAL) has ended support. We recommend that customers ensure their applications are migrated to MSAL. MSAL integrates with the Microsoft identity platform (v2.0) endpoint, which is the unification of Microsoft personal accounts and work accounts into a single authentication system. ADAL integrates with a v1.0 endpoint which doesn't support personal accounts.

For more information about how to migrate to MSAL, see [Migrate applications to the Microsoft Authentication Library (MSAL)](msal-migration.md).
