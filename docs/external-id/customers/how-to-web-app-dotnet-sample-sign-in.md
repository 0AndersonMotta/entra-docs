---
title: Sign in users in a sample ASP.NET web application by using Microsoft Entra
description: Learn how to configure a sample web app to sign in and sign out users by using Microsoft Entra.
services: active-directory
author: cilwerner
manager: celestedg

ms.author: cwerner
ms.service: active-directory
ms.workload: identity
ms.subservice: ciam
ms.topic: how-to
ms.date: 04/30/2023
ms.custom: developer

#Customer intent: As a dev, devops, I want to learn about how to configure a sample Node.js web app to sign in and sign out users with my Azure Active Directory (Azure AD) for customers tenant
---

# Sign in users in a sample ASP.NET web application by using Microsoft Entra

This how-to guide uses a sample ASP.NET web application to show the fundamentals of modern authentication with Azure AD Consumer Identity and Access Management (CIAM), using the [Microsoft Authentication Library for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) [Microsoft Identity Web](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for Node to handle authentication.

In this article:

> [!div class="checklist"]
> * Register a web application in the Microsoft Entra admin center
> * Create a sign in and sign out user flow in Microsoft Entra admin center.
> * Associate your web application with the user flow. 
> * Update a sample ASP.NET web application using your own Azure Active Directory (Azure AD) for customers tenant details.
> * Run and test the sample web application.

# Prerequisites

- A minimum requirement of [.NET Core 6.0 SDK](https://dotnet.microsoft.com/download/dotnet).

- [Visual Studio Code](https://code.visualstudio.com/download) or another code editor.

- Azure AD for customers tenant. If you don't already have one, [sign up for a free trial](https://developer.microsoft.com/identity/customers). 

## Register the web app

[!INCLUDE [active-directory-b2c-register-app](./includes/register-app/register-client-app-common.md)]
[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/register-app/add-platform-redirect-url-node.md)]  

## Add app client secret 

[!INCLUDE [active-directory-b2c-add-client-secret](./includes/register-app/add-app-client-secret.md)] 

## Grant API permissions

[!INCLUDE [active-directory-b2c-grant-delegated-permissions](./includes/register-app/grant-api-permission-sign-in.md)] 

## Create a user flow 

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/configure-user-flow/create-sign-in-sign-out-user-flow.md)] 

## Associate the web application with the user flow

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/configure-user-flow/add-app-user-flow.md)]

## Clone or download sample web application

To get the web app sample code, you can do either of the following tasks:

- [Download the .zip file](https://github.com/Azure-Samples/ms-identity-ciam-dotnet-tutorial/archive/refs/heads/main.zip). Extract the sample app file to a folder where the total length of the path is 260 or fewer characters
- Clone the sample web application from GitHub by running the following command:

    ```powershell
    git clone https://github.com/Azure-Samples/ms-identity-ciam-dotnet-tutorial.git
    ``` 

Once downloaded, change to the directory that contains the ASP.NET sample app:

    ```powershell
    cd 1-Authentication\1-sign-in-aspnet-core-mvc
    ```

## Register the web app

[!INCLUDE [active-directory-b2c-register-app](./includes/register-app/register-client-app-common.md)]

## Configure the application

1. Navigate to and open the *appsettings.json* file.
1. Find the key `Enter_the_Application_Id_Here` and replace the existing value with the application ID (clientId) of `ciam-aspnet-webapp` app copied from the Microsoft Entra admin center.
1. Find the key `Enter_the_Tenant_Id_Here` and replace the existing value with your Azure AD tenant/directory ID.

## Run the code sample

1. From your shell or command line, execute the following commands:

    ```powershell
    dotnet run
    ```
1. Open your web browser and navigate to `https://localhost:7274`.
1. Sign-in with an account registered to the CIAM tenant.
1. Once signed in the display name is shown next to the **Sign out** button as shown in the following screenshot.

    :::image type="content" source="media/how-to-web-app-dotnet-sign-in-sign-in-out/display-aspnet-welcome.png" alt-text="Screenshot of sign in into a node web app.":::

1. To sign-out from the application, select the **Sign out button**

## Next steps

Learn how to: 

- [Enable password reset](how-to-enable-password-reset-customers.md)

- [Customize the default branding](how-to-customize-branding-customers.md)

- [Configure sign-in with Google](how-to-google-federation-customers.md)

- [Sign in users in your own ASP.NET web application by using Microsoft Entra](how-to-web-app-dotnet-sign-in-overview.md)