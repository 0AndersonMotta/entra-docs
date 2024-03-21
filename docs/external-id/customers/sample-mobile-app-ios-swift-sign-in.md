---
title: Sign in users in sample iOS (Swift) mobile app 
description: Learn how to Authenticate users in a Microsoft Entra ID for customers tenant using a sample iOS (Swift) application.

author: henrymbuguakiarie
manager: mwongerapk

ms.author: henrymbugua
ms.service: entra-external-id

ms.subservice: customers
ms.topic: sample
ms.date: 03/20/2024
ms.custom: developer
#Customer intent: As a developer, I want to authenticate users and call a protected web API from a sample iOS mobile app so that I can experience how Microsoft Entra ID for customers work.
---

# Sign in users and call web API in sample iOS (Swift) mobile app

This guide demonstrates how to configure a sample iOS mobile application to sign in users, and call an ASP.NET Core web API.

In this article, you do the following tasks: 

- Register an application in the Microsoft Entra admin center.
- Add a platform redirect URL.
- Enable public client flows.   
- Update the iOS configuration code sample file to use your own Microsoft Entra External ID for customer tenant details.  
- Run and test the sample iOS mobile application.  

## Prerequisites  

- <a href="https://developer.apple.com/xcode/resources/" target="_blank">Xcode</a> 
- Microsoft Entra External ID for customers tenant. If you don't already have one, <a href="https://aka.ms/ciam-free-trial?wt.mc_id=ciamcustomertenantfreetrial_linkclick_content_cnl" target="_blank">sign up for a free trial</a>. 
- An API registration that exposes at least one scope (delegated permissions) and one app role (application permission) such as *ToDoList.Read*. If you haven't already, follow the instructions for [call an API in a sample iOS mobile app](sample-native-authentication-ios-sample-app-call-web-api.md) to have a functional protected ASP.NET Core web API. Make sure you complete the following steps:

    - Register a web API application
    - Configure API scopes
    - Configure app roles
    - Configure optional claims
    - Clone or download sample web API
    - Configure and run sample web API

## Register an application

[!INCLUDE [register client app](../customers/includes/register-app/register-client-app-common.md)]

## Add a platform redirect URL

[!INCLUDE [Enable public client](../customers/includes/register-app/add-platform-redirect-url-ios.md)]

## Enable public client flow

[!INCLUDE [Enable public client](../customers/includes/register-app/enable-public-client-flow.md)]

## Grant API permissions

[!INCLUDE [Grant API permissions](../customers/includes/register-app/grant-native-authentication-api-permission.md)]

## Grant web API permissions to the iOS sample app

Once you've registered both your client app and web API and you've exposed the API by creating scopes, you can configure the client's permissions to the API by following these steps:

[!INCLUDE [grant-api-permission-call-api-common](../customers/includes/register-app/grant-api-permission-call-api-common.md)]

## Clone sample iOS mobile application

1. Open Terminal and navigate to a directory where you want to keep the code.  
1. Clone the application from GitHub by running the following command:  

   ```bash 
   git clone https://github.com/Azure-Samples/ms-identity-ciam-browser-delegated-ios-sample.git
   ```

## Configure the sample iOS mobile application

1. In Xcode, open the project that you cloned.
1. Open */MSALiOS/Configuration.swift* file.
1. Find the placeholder:

    - `Enter_the_Application_Id_Here` and replace it with the **Application (client) ID** of the app you registered earlier.
    - `Enter_the_Redirect_URI_Here` and replace it with the value of *kRedirectUri* in the Microsoft Authentication Library (MSAL) configuration file you downloaded earlier when you added the platform redirect URL.
    - `Enter_the_Protected_API_Full_URL_Here` and replace it the URL to your web API.
    - `Enter_the_Protected_API_Scopes_Here` and replace it with the scopes recorded in [Grant web API permissions to the Android sample app](#grant-web-api-permissions-to-the-ios-sample-app).
    - `Enter_the_Tenant_Subdomain_Here` and and replace it with the Directory (tenant) subdomain. For example, if your tenant primary domain is `contoso.onmicrosoft.com`, use `contoso`. If you don't know your tenant subdomain, learn how to [read your tenant details](how-to-create-customer-tenant-portal.md#get-the-customer-tenant-details).

You've configured the app and it's ready to run.

## Run iOS sample app and call web API 
 
To build and run your app, follow these steps:
 
1. To build and run your code, select **Run** from the **Product** menu in Xcode. After a successful build, Xcode will launch the sample app in the Simulator. 
1. Select the API tab to test the API call. A successful call to the web API returns HTTP `200`, while HTTP `403` signifies unauthorized access.

## Related content

- [Sign in users in sample iOS (Swift) mobile app by using native authentication](how-to-run-native-authentication-sample-ios-app.md).
