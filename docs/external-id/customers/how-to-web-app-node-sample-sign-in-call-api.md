---
title: Sign in users and call an API in sample Node.js web application by using Microsoft Entra
description: Learn how to configure a sample web app to sign in users and call an API by using Microsoft Entra.
services: active-directory
author: kengaderdus
manager: mwongerapk

ms.author: kengaderdus
ms.service: active-directory
ms.workload: identity
ms.subservice: ciam
ms.topic: how-to
ms.date: 05/05/2023
ms.custom: developer

#Customer intent: As a dev, devops, I want to learn about how to configure a sample web app to sign in and sign out users with my CIAM tenant
---

# Sign in users and call an API in sample Node.js web application by using Microsoft Entra

This how-to guide uses a sample Node.js web application to show how to add authentication and authorization by using Microsoft Entra. The sample application sign in users to a Node.js web app, which then calls a .NET Core API. You enable authentication and authorization by using your Azure Active Directory (Azure AD) for customers tenant details. The sample web application uses [Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) for Node to handle authentication.

In this article, you complete the following tasks:

- Register and configure a web API in the Microsoft Entra admin center.

- Register and configure a client web application in the Microsoft Entra admin center. 

- Create a sign-up and sign-in user flow in the Microsoft Entra admin center, and then associate a client web app with it.

- Update a sample Node web application and ASP.NET Core web API to use your Azure AD for customers tenant details.

- Run and test the sample web application and API.

## Prerequisites

- [Node.js](https://nodejs.org).

- [.NET 7.0](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install) or later. 

- [Visual Studio Code](https://code.visualstudio.com/download) or another code editor.

- Azure AD for customers tenant. If you don't already have one, [sign up for free trial](https://aka.ms/ciam-hub-free-trial).

## Register a web application and a web API

In this step, you create the web and the web API application registrations, and you specify the scopes of your web API.

### Register a web API application

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/register-app/register-api-app.md)]

### Configure API scopes

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/register-app/add-api-scopes.md)]

### Configure app roles

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/register-app/add-app-role.md)]

### Configure optional claims

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/register-app/add-optional-claims-access.md)]

### Register the web app

[!INCLUDE [active-directory-b2c-register-app](./includes/register-app/register-client-app-common.md)]
[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/register-app/add-platform-redirect-url-node.md)]  

### Create a client secret

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/register-app/add-app-client-secret.md)]

### Grant API permissions to the web app

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/register-app/grant-api-permission-call-api.md)]

## Create a user flow 

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/configure-user-flow/create-sign-in-sign-out-user-flow.md)] 

##  Associate web application with the user flow

[!INCLUDE [active-directory-b2c-app-integration-add-user-flow](./includes/configure-user-flow/add-app-user-flow.md)]

##  Clone or download sample web application and web API

To get the web app and web API sample code, [download the .zip file](https://github.com/Azure-Samples/ms-identity-ciam-javascript-tutorial/archive/refs/heads/main.zip) or clone the sample web application from GitHub by running the following command:

```powershell
git clone https://github.com/Azure-Samples/ms-identity-ciam-javascript-tutorial.git
```

If you choose to download the `.zip` file, extract the sample app file to a folder where the total length of the path is 260 or fewer characters.

##  Install project dependencies 

1. Open a console window, and change to the directory that contains the Node.js/Express sample app:

    ```powershell
    cd 2-Authorization\4-call-api-express\App
    ```
1. Run the following commands to install web app dependencies:

    ```powershell
    npm install && npm update
    ```

## Configure the sample web app and API

To use your app registration in the client web app sample:

1. In your code editor, open `App\authConfig.js` file.

1. Find the placeholder:

    1. `Enter_the_Application_Id_Here` and replace it with the Application (client) ID of the app you registered earlier.
    
    1. `Enter_the_Tenant_Info_Here` and replace it with the Directory (tenant) ID you copied earlier.
    
    1. `Enter_the_Client_Secret_Here` and replace it with the app secret value you copied earlier.
    
    1. `Enter_the_Web_Api_Application_Id_Here` and replace it with the Application (client) ID of the web API you copied earlier.

To use your app registration in the web API sample: 

1. In your code editor, open `API\TodoListAPI\appsettings` file.

1. Find the placeholder:
    
    1. `Enter the Client ID (aka 'Application ID')` and replace it with the Application (client) ID of the web API you copied. 
    
    1. `Enter the tenant ID` and replace it with the Directory (tenant) ID you copied earlier.


##  Run and test sample web app and API 

1. Open a console window, then run the web API by using the following commands:

    ```powershell
    cd 2-Authorization\4-call-api-express\API\TodoListAPI
    dotnet run
    ``` 

1. Run the web app client by using the following commands:

```powershell
    cd 2-Authorization\4-call-api-express\App
    npm start
```

1. Open your browser, then go to http://localhost:3000. 

1. Select the **Sign In** button. You're prompted to sign in.

    :::image type="content" source="media/how-to-web-app-node-sample-sign-in-call-api/web-app-node-sign-in.png" alt-text="Screenshot of sign in into a node web app.":::

1. On the sign-in page, type your **Email address**, select **Next**, type your **Password**, then select **Sign in**. If you don't have an account, select **No account? Create one** link, which starts the sign-up flow.

1. If you choose the sign-up option, after filling in your email, one-time passcode, new password and more account details, you complete the whole sign-up flow. You see a page similar to the following screenshot. You see a similar page if you choose the sign-in option.

    :::image type="content" source="media/how-to-web-app-node-sample-sign-in-call-api/sign-in-call-api-view-todo.png" alt-text="Screenshot of sign in into a node web app and call an A P I.":::

### Call API

1. To call the API, select the **View your todolist** link. You see a page similar to the following screenshot.
    
    :::image type="content" source="media/how-to-web-app-node-sample-sign-in-call-api/sign-in-call-api-manipulate-todo.png" alt-text="Screenshot of manipulate A P I to do list.":::

1. Manipulate the to-do list by creating and removing items.

You trigger an API call each time you view, add or remove a task. Each time you trigger an API call, the client web app acquires an access token with the required permissions (scopes) to call an API endpoint. For example, to read a task, the client web app must acquire an access token with `Todolist.Read` permission/scope.

On the web API side, the endpoint must validate that the permissions/scopes present in the access token that the client app presents are valid. If the access token is valid, the endpoint responds to the HTTP request, otherwise, it responds with a `401 Unauthorized` HTTP error. 

If you want to build a web app and web API similar to the sample you've run, complete the steps in [Sign in users and call an API in your own Node.js web application by using Microsoft Entra](how-to-web-app-node-sign-in-call-api-overview.md) article. 

## Next steps

Learn how to: 

- [Sign in users and call an API in your own Node.js web application by using Microsoft Entra](how-to-web-app-node-sign-in-call-api-overview.md).

- [Enable password reset](how-to-enable-password-reset-customers.md).

- [Customize the default branding](how-to-customize-branding-customers.md).

- [Configure sign-in with Google](how-to-google-federation-customers.md).
