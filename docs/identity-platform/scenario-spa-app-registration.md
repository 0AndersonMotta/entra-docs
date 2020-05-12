---
title: Register single-page apps - Microsoft identity platform | Azure
description: Learn how to build a single-page application (app registration)
services: active-directory
author: hahamil
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 5/11/2020
ms.author: hahamil
ms.custom: aaddev
#Customer intent: As an application developer, I want to know how to write a single-page application by using the Microsoft identity platform for developers.
---

# Single-page application: App registration

This page explains the app registration specifics for a single-page application (SPA).

## Preliminary steps 

Follow the steps to [register a new application with the Microsoft identity platform](quickstart-register-app.md), and select the supported accounts for your application. The SPA scenario can support authentication with accounts in your organization or any organization and personal Microsoft accounts.

Next, learn the specific aspects of application registration that apply to single-page applications. Note that there are slight differences in registering redirect URIs for use with MSAL.js 1.0 and MSAL.js 2.0. It's important to register a redirect URI where your application can receive tokens. Ensure that the redirect URI exactly matches the URI for your application.

## MSAL.js 2.0  

The following steps apply for registering redirect URIs for apps using MSAL.js 2.0. The latest version on MSAL supports the Authorization Code Flow with PKCE and CORS support [link to docs TODO]. 

### New Application or redirect URI 
 
#### Add MSAL to your application

The following applies for apps using MSAL.js for the first time. Further bellow you will find steps to migrate from MSAL 1.0 to 2.0.

Install MSAL via the following npm command: 
```bash 
npm install @azure/msal-browser
```

Follow the [tutorial](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-javascript-auth-code) or [quickstart](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v2-javascript-auth-code) for information on configuring your application and using MSAL.  

#### Register a **new** redirect URI 

1. Go to your **application registration** and select the **Authentication** pane. 

2. Select the **"Add a platform"** button at the top of the page. (If you do not see this, click "switch to new experience" at the top of the page.)
![add platform in registration pane](media/scenario-spa-app-registration/add_platform.png)

3. Select the **Single-Page Application** platform tile.
![add SPA platform](media/scenario-spa-app-registration/configure-platform.png)

4.Enter your [redirect URI](https://docs.microsoft.com/azure/active-directory/develop/reply-url). If you are using MSAL 2.0, you should not have to select any implicit grant settings. Click configure. 

![configure SPA](media/scenario-spa-app-registration/configure-spa.png)

5.You have now registered your SPA redirect URI. 

### Existing Application Migrating from implicit flow to authorization code flow 

If you already have an application using MSAL.js 1.0 and/or the implicit flow in production and would like to update to MSAL.js 2.0 and/or the authorization code flow and use the same redirect URIs, carefully follow the bellow steps. You can always also register new redirect URIs instead and delete old URIs, as instucted above. 

#### Add MSAL 2.0 to your application

#### Update existing redirect URIs to new "SPA" platform 
1.Navigate to the Authentication pane of your app registration

2.In the "Web App" platform tile, you will notice a warning banner. Select "Migrate URIs"
![warning to migrate from 'web' to 'spa'](media/scenario-spa-app-registration/web-warning.png)

3.In the pane that pops up on the side, select **only the redirect URIs used with MSAL.js**
![screenshot of pane to migrate from web to spa](media/scenario-spa-app-registration/migration-pane.png)

4.These redirect URIs will now appear in the new "SPA" platform tile. This enables CORS support with the authorization code flow and PKCE for these URIs. 


#### Uncheck implicit settings 
Once all of your apps using this client ID in production have succesfully integrated MSAL 2.0/the authorization code flow, you should uncheck the implicit settings. Note that these settings apply to the entire application registration. Ensure that this app registration is not using the [implicit flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-implicit-grant-flow) in production before unchecking these settings to avoid breaking this flow.

## MSAL.js 1.0 and the Implicit Flow 

#### Add MSAL.js 1.0 to your app

Follow the [tutorial](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-javascript-spa) or [quickstart](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v2-javascript) to add MSAL.js 1.0 to your app

### Register a new redirect URI 

1. Go to your app registration and select the **Authentication** blade. 

2. Select the "Add a platform" button at the top of the page. (If you do not see this, click "switch to new experience" at the top of the page.

3. Select the "Single Page Application" platform tile.

4.Enter your redirect URI [Docs on what is a redirect URI?].If your application is only signing in users and getting ID tokens, it's enough to select the **ID tokens** check box.

If your application also needs to get access tokens to call APIs, make sure to select the **Access tokens** check box as well. For more information, see [ID tokens](./id-tokens.md) and [Access tokens](./access-tokens.md).

![configure SPA with implicit settings](media/scenario-spa-app-registration/configure-spa-implicit.png)

5.You have now registered your SPA redirect URI. 


### Note about authorization flows 

New applications created in the SPA platofrm will by default be enabled for the authorization code flow. To actually take advantage of this flow, you will have to update your application to be using MSAL 2.0. Once none of your in production applications are using MSAL.js 1.0 and implicit flow, you can uncheck the implicit settings in the Authentication Blade of the Azure portal. So long as the implicit settings are still checked, your application can use MSAL 1.0 and the implicit flow. 

Applications using MSAL.js 1.0 will still be using the implicit flow. The latest [OAuth Best Current Practices](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow) now recommends using the authorization code flow rather than the implicit flow for SPAs. Having limited lifetime refresh tokens will also help your application adapt to [modern browser cookie privacy limitations](https://docs.microsoft.com/azure/active-directory/develop/reference-third-party-cookies-spas) such as Safari ITP. 

## API permissions

Single-page applications can call APIs on behalf of the signed-in user. They need to request delegated permissions. For details, see [Add permissions to access web APIs](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## Next steps

> [!div class="nextstepaction"]
> [App's code configuration](scenario-spa-app-configuration.md)
