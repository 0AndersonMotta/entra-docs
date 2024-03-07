---
title: Sign in users and call an API in sample iOS mobile app by using native authentication
description: Learn how sign in users and call an API in sample iOS mobile app by using native authentication

author: henrymbuguakiarie
manager: mwongerapk

ms.author: henrymbugua
ms.service: entra-external-id

ms.subservice: customers
ms.topic: how-to
ms.date: 03/06/2024
ms.custom: developer
#Customer intent: As a developer, I aim to learn registering a web API, configuring API scopes, roles, optional claims, and calling a web API in an iOS sample app.
---

# Sign in users and call an API in sample iOS mobile app by using native authentication

This sample demonstrates how to configure iOS sample application to call an ASP.NET Core web API.

## Prerequisites

- [Sign in users in sample iOS (Swift) mobile app by using native authentication](how-to-run-native-authentication-sample-ios-app.md).

### Register a web API application

[!INCLUDE [register-api-app](./includes/register-app/register-api-app.md)]

### Configure API scopes

[!INCLUDE [add-api-scopes](./includes/register-app/add-api-scopes.md)]

### Configure app roles

[!INCLUDE [add-app-role](./includes/register-app/add-app-role.md)]

### Configure optional claims

[!INCLUDE [add-optional-claims-access](./includes/register-app/add-optional-claims-access.md)]

## Grant API permissions to the iOS sample app

Once you've registered both your client app and web API and you've exposed the API by creating scopes, you can configure the client's permissions to the API by following these steps:

[!INCLUDE [grant-api-permission-call-api-common](./includes/register-app/grant-api-permission-call-api-common.md)]

## Clone or download sample web API

To get the web API sample code, [download the .zip file](https://github.com/Azure-Samples/ms-identity-ciam-dotnet-tutorial/archive/refs/heads/main.zip) or clone the sample web application from GitHub by running the following command:

```bash
git clone https://github.com/Azure-Samples/ms-identity-ciam-dotnet-tutorial.git
```

If you choose to download the .zip file, extract the sample app file to a folder where the total length of the path is 260 or fewer characters.

## Configure and run sample web API

1. In your code editor, open `2-Authorization/1-call-own-api-aspnet-core-mvc/ToDoListAPI/appsettings.json` file.
1. Find the placeholder:

    - `Enter_the_Application_Id_Here` and replace it with the **Application (client) ID** of the web API you copied earlier. 
    - `Enter_the_Tenant_Id_Here` and replace it with the **Directory (tenant) ID** you copied earlier.
    - `Enter_the_Tenant_Subdomain_Here` and replace it with the Directory (tenant) subdomain. For example, if your tenant primary domain is `contoso.onmicrosoft.com`, use `contoso`. If you don't have your tenant name, learn how to [read your tenant details](how-to-create-customer-tenant-portal.md#get-the-customer-tenant-details).

1. Open a console window, then run the web API by using the following command:

    ```bash
    cd /2-Authorization/1-call-own-api-aspnet-core-mvc/ToDoListAPI

    dotnet run
    ```

## Configure sample iOS mobile app to call web API

1. In your Xcode, open `/NativeAuthSampleApp/ProtectedAPIViewController.swift` file.
1. Find `Enter_the_Protected_API_Full_URL_Here` and replace this value with your web API URL.

    ```swift
    let protectedAPIUrl = "Enter_the_Protected_API_Full_URL_Here" // Developers should set the respective URL of their web API here
    ```
    
1. Find `Enter_the_Protected_API_Scopes_Here` and set the scopes recorded in [Grant API permissions to the iOS sample app](#grant-api-permissions-to-the-ios-sample-app).

    ```swift
    let protectedAPIScopes = ["Enter_the_Protected_API_Scopes_Here"] // Developers should set the respective scopes of their web API here.For example, let protectedAPIScopes = ["api://{clientId}/{ToDoList.Read}","api://{clientId}/{ToDoList.ReadWrite}"]
    ```
    
## Run iOS sample app and call web API 
 
To build and run your app, follow these steps:
 
1. To build and run your code, select **Run** from the **Product** menu in Xcode. After a successful build, Xcode will launch the sample app in the Simulator. 
1. Select the API tab to test the API call. A successful call to the web API returns HTTP `200`, while HTTP `403` signifies unauthorized access.

## Next steps

- [Tutorial: Self-service password reset](tutorial-native-authentication-ios-self-service-password-reset.md) 
