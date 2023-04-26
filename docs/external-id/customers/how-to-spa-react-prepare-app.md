---
title: Prepare a React Single Page App (SPA) for authentication
description: Learn how to prepare a React Single Page App (SPA) for authentication
services: active-directory
author: godonnell
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.subservice: ciam
ms.topic: how-to
ms.date: 04/12/2023
ms.author: godonnell
ms.custom: it-pro

#Customer intent: As a dev, devops, or IT admin, enable authentication in my own React SPA
---
# Prepare a Single-page application for authentication

After registration is complete, a React project can be created using an integrated development environment (IDE). This tutorial demonstrates how to create a React Single-page application using npm and create files needed for authentication and authorization.

In this article, you learn how to:

> [!div class="checklist"]
> * Create a new React project
> * Configure the settings for the application
> * Install identity and bootstrap packages
> * Add authentication code to the application

## Prerequisites
* Completion of the prerequisites and steps in [Prepare your customer tenant for building a React Single Page App (SPA)](./how-to-spa-react-prepare-tenant.md))
* Although any IDE that supports React applications can be used, Visual Studio Code is used for this guide. This can be downloaded from the [Downloads](https://visualstudio.microsoft.com/downloads/) page.
* [Node.js](https://nodejs.org/en/download/)

## Create a new React project

Use the following tabs to create a React project within the IDE.

1. Open Visual Studio Code, select **File** > **Open Folder...**. Navigate to and select the location in which to create your project.
1. Open a new terminal by selecting **Terminal** > **New Terminal**.
1. Run the following commands to create a new React project with the name *reactspalocal*, change to the new directory and start the React project. A web browser will open with the address `http://localhost:3000/` by default. The browser remains open and re-renders for every saved change.

    ```powershell
    npx create-react-app reactspalocal
    cd reactspalocal
    npm start
    ```

## Install identity and bootstrap packages

Identity related **npm** packages must be installed in the project to enable user  authentication. For project styling, **Bootstrap** will be used.

1. In the **Terminal** bar, select the **+** icon to create a new terminal. A separate terminal window will open with the previous node terminal continuing to run in the background.
1. Ensure that the correct directory is selected (*reactspalocal*) then enter the following into the terminal to install the relevant `msal` and `bootstrap` packages.

    ```powershell
    npm install @azure/msal-browser @azure/msal-react
    npm install react-bootstrap bootstrap
    ```

## Creating the authentication configuration file

1. In the *src* folder, create a new file called *authConfig.js*.
1. Open *authConfig.js* and add the following code snippet:

   :::code language="javascript" source="~/ms-identity-docs-code-javascript/react-spa/src/authConfig.js" :::

1. Replace the following values with the values from the Azure portal.
    - `clientId` - The identifier of the application, also referred to as the client. Replace `Enter_the_Application_Id_Here` with the **Application (client) ID** value that was recorded earlier from the overview page of the registered application.
    - `authority` - This is composed of two parts:
        - The *Instance* is endpoint of the cloud provider. Check with the different available endpoints in [National clouds](../../develop/authentication-national-cloud.md)
        - The *Tenant ID* is the identifier of the tenant where the application is registered. Replace the `_Enter_the_Tenant_Info_Here` with the **Directory (tenant) ID** value that was recorded earlier from the overview page of the registered application.


## Modify index.js to include the authentication provider

All parts of the app that require authentication must be wrapped in the [`MsalProvider`](/javascript/api/@azure/msal-react/#@azure-msal-react-msalprovider) component. You instantiate a [PublicClientApplication](/javascript/api/@azure/msal-browser/publicclientapplication) then pass it to `MsalProvider`.

1. In the *src* folder, open *index.js* and replace the contents of the file with the following code snippet to use the `msal` packages and bootstrap styling:

    :::code language="javascript" source="~/ms-identity-docs-code-javascript/react-spa/src/index.js" :::


## Next steps

> [!div class="nextstepaction"]
> [Add Sign-in and Sign-out functionality to your app.](./how-to-spa-react-sign-in-out.md)