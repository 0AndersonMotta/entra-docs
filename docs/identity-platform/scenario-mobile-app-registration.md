---
title: mobile app that calls Web APIs - app's code configuration | Azure
description: Learn how to build a mobile app that calls Web APIs (app's code configuration)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''

ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2019
ms.author: dadobali
ms.custom: aaddev 
#Customer intent: As an application developer, I want to know how to write a mobile app that calls Web APIs using the Microsoft identity platform for developers.
ms.collection: M365-identity-device-management
---

# Mobile app that calls Web APIs - app registration

This article contains the app registration instructions for creating a mobile application.

## Supported account types

The account types supported in mobile applications depend on the experience you want to enable and users your app is targeting. 

## Platform Configuration & Redirect URIs  

When building a mobile app, the most critical registration step is the redirect URI. This can be set through the [platform configuration in the Authentication blade](https://aka.ms/MobileAppReg). 

This experience will enable your app to get SSO through the Microsoft Authenticator (and Intune Company Portal on Android) as well as support device management policies. 

If you prefer to manually configure the redirect URI, you can do so through the Application Manifest. The recommmended format is the following:

- ***iOS***: `msauth.<BUNDLE_ID>://auth`
- ***Android***: `msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
    - The Android signature hash can be generated using the release or debug keys through the KeyTool commmand.

## API Permissions 

Mobile applications call APIs on behalf of the signed-in user. Your app needs to request delegated permissions, also referred to as scopes. Depending on the desired experience, this can be done statically through the Azure portal or dynamically at run-time. Statically registering permissions allows admins to easily approve your app and is recommended. 

## Next steps

> [!div class="nextstepaction"]
> [Acquiring a token](scenario-mobile-acquire-token.md)
