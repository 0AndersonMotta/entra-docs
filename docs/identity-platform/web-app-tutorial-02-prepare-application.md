---
title: "Tutorial: Prepare a web application for authentication"
description: Prepare an ASP.NET Core application for authentication using Visual Studio.
author: cilwerner
ms.author: cwerner
manager: CelesteDG
ms.service: active-directory
ms.topic: tutorial
ms.date: 10/18/2022
#Customer intent: As an application developer, I want to use an IDE to set up an ASP.NET Core project, set up and upload a self signed certificate to the Azure portal and configure the application for authentication.
#TBD
---

# Tutorial: Prepare an application for authentication

This tutorial demonstrates how to create an ASP.NET Core project in a supported integrated development environment (IDE). You'll create and upload a self-signed certificate and configure the application for authentication.

In this tutorial:

> [!div class="checklist"]
> * Create an ASP.NET Core project
> * Create a self-signed certificate
> * Configure the settings for the application
> * Define platform settings
> * Define the URLs needed for the application

## Prerequisites

* Completion of the prerequisites and steps in [Tutorial: Register an application with the Microsoft identity platform](web-app-tutorial-01-register-application.md).
* Although any IDE that supports .NET applications can be used, the following IDEs are used for this tutorial. They can be downloaded from the [Downloads](https://visualstudio.microsoft.com/downloads) page.
    - Visual Studio 2022
    - Visual Studio Code
    - Visual Studio 2022 for Mac
* Some steps in this tutorial use the .NET CLI. For more information about this tool, see [dotnet command](/dotnet/core/tools/dotnet).
* A minimum requirement of [.NET Core 6.0 SDK](https://dotnet.microsoft.com/download/dotnet)

## Create an ASP.NET Core project

After registering an application on the Azure portal, a .NET web application needs to be created using an IDE. Use the tabs below to create an ASP.NET Core project with your IDE.

### [Visual Studio](#tab/visual-studio)

1. Open Visual Studio, and then select **Create a new project**.
1. Search for and choose the **ASP.NET Core Web App** template, and then select **Next**.
1. Enter a name for the project, such as *NewWebAppLocal*.
1. Choose a location for the project or accept the default option, and then select **Next**.
1. Accept the default for the **Framework**, **Authentication type**, and **Configure for HTTPS**. The **Authentication type** could be changed to Microsoft identity platform, which would add the authentication configuration to the application.
1. Select **Create**.

### [Visual Studio Code](#tab/visual-studio-code)

1. In Visual Studio Code, select **File** > **Open Folder...**. Navigate to and select the location in which to create your project.
1. Create a new folder using the **New Folder...** icon in the **Explorer** pane. Provide a name similar to the one registered previously, for example, *NewWebAppLocal*.
1. Open a new terminal by selecting **Terminal** > **New Terminal**.
1. Run the following commands in the terminal to change into the folder directory and create the project:

```powershell
cd NewWebAppLocal
dotnet new webapp
```

### [Visual Studio for Mac](#tab/visual-studio-for-mac)

1. Open Visual Studio, and then select **New**.
1. Under **Web and Console** in the left navigation bar, select **App**.
1. Under **ASP.NET Core**, select **Web Application** and ensure **C#** is selected in the drop down menu, then select **Continue**.
1. Accept the default for the **Target Framework and Advanced** > **Continue**.
1. Enter a name for **Project name**, this will be reflected in **Solution Name**. Provide a similar name to the one registered on the Azure portal, such as *NewWebAppLocal*.
1. Accept the default location for the project or choose a different location, and then select **Create**.

---

## Create and upload a self-signed certificate

The use of certificates is suggested for securing the application. When using certificates, make sure to manage and monitor them appropriately. This tutorial uses a self-signed certificate for development purposes, certificates issued by a certificate authority should be used for production.

### [Visual Studio](#tab/visual-studio)

1. Select **Tools > Command Line > Developer Command Prompt**.

1. Enter the following command to create a new self-signed certificate:

    ```powershell
    dotnet dev-certs https -ep ./certificate.crt --trust
    ```

### [Visual Studio Code](#tab/visual-studio-code)

1. In the **Terminal**, enter the following command to create a new self-signed certificate:

    ```powershell
    dotnet dev-certs https -ep ./certificate.crt --trust
    ```

### [Visual Studio for Mac](#tab/visual-studio-for-mac)

1. Select **Tools > Command Line > Developer Command Prompt**.

1. Enter the following command to create a new self-signed certificate:

    ```powershell
    dotnet dev-certs https -ep ./certificate.crt --trust
    ```

---

### Upload certificate to the portal

To make the certificate available to the application, it must be uploaded into the tenant.

1. Starting from the **Overview** page of the app created earlier, under **Manage**, select **Certificates & secrets** and select the **Certificates (0)** tab.
1. Select **Upload certificate**.

:::image type="content" source="./media/web-app-tutorial-02-prepare-application/upload-certificate-inline.png" alt-text="Screenshot of uploading a certificate into an Azure Active Directory tenant." lightbox="./media/web-app-tutorial-02-prepare-application/upload-certificate-expanded.png":::

1. Browse for and select the certificate that was previously created.
1. Enter a description for the certificate.
1. Select **Add**.
1. Double-click the thumbprint, select the vertical ellipsis, and then select **Copy** to record the thumbprint for the certificate to be used later.

    :::image type="content" source="./media/web-app-tutorial-02-prepare-application/copy-certificate-thumbprint.png" alt-text="Screenshot showing copying the certificate thumbprint.":::

## Configure the application for authentication and API reference

The values recorded earlier will be used to configure the application for authentication. As it will also be calling in to a web API, the app settings must contain a reference to this as well.

1. Open *appsettings.json* and replace the file contents with the following snippet:
  
    ``` json
    {
      "AzureAd": {
        "Instance": "https://login.microsoftonline.com/",
        "TenantId": "Enter the tenant ID here",
        "ClientId": "Enter the client ID here",
        "ClientCertificates": [
          {
            "SourceType": "StoreWithThumbprint",
            "CertificateStorePath": "CurrentUser/My",
            "CertificateThumbprint": "Enter the certificate thumbprint here"
          }    
        ],
        "CallbackPath": "/signin-oidc"
      },
      "DownstreamApi": {
        "BaseUrl": "https://graph.microsoft.com/v1.0/me",
        "Scopes": "user.read"
          },
      "Logging": {
        "LogLevel": {
          "Default": "Information",
          "Microsoft.AspNetCore": "Warning"
        }
      },
      "AllowedHosts": "*"
    }
    ```

    * `Instance` - The endpoint of the cloud provider. Check with the different available endpoints in [National clouds](authentication-national-cloud.md#azure-ad-authentication-endpoints).
    * `TenantId` - The identifier of the tenant where the application is registered. Replace the text in quotes with the **Directory (tenant) ID** value that was recorded earlier from the overview page of the registered application.
    * `ClientId` - The identifier of the application, also referred to as the client. Replace the text in quotes with the **Application (client) ID** value that was recorded earlier from the overview page of the registered application.
    * `ClientCertificates` - A self-signed certificate is used for authentication in the application. Replace the text of the `CertificateThumbprint` with the thumbprint of the certificate that was previously recorded.
    * `CallbackPath` - Is an identifier to help the server redirect a response to the appropriate application. 
    * `DownstreamApi` - Is an identifier that allows the API to be called. The `BaseUrl` and `Scopes` can remain unchanged.
1. Save changes to the file.
1. In the **Properties** folder, open the *launchSettings.json* file.
1. Within the `https` bracket of the JSON, record the https URL listed for `applicationURL`, for example `https://localhost:{port}`. This URL will be used when defining the **Redirect URI**.


## Define the platform and URLs

1. In the Azure portal, under **Manage**, select **App registrations**, and then select the application that was previously created.
1. In the left menu, under **Manage**, select **Authentication**.
1. In **Platform configurations**, select **Add a platform**, and then select **Web**.

    :::image type="content" source="./media/web-app-tutorial-02-prepare-application/select-platform-inline.png" alt-text="Screenshot on how to select the platform for the application." lightbox="./media/web-app-tutorial-02-prepare-application/select-platform-expanded.png":::

1. Under **Redirect URIs**, enter the `applicationURL` and the `CallbackPath`, `/signin-oidc`, in the form of `https://localhost:{port}/signin-oidc`.
1. Under **Front-channel logout URL**, enter the following URL for signing out, `https://localhost:{port}/signout-oidc`.

## Next steps

> [!div class="nextstepaction"]
> [Tutorial: Add sign-in to an application](web-app-tutorial-03-sign-in-users.md)