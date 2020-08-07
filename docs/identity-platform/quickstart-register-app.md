---
title: "Quickstart: Register an app in the Microsoft identity platform | Azure"
description: In this quickstart, you learn how to register an application with the Microsoft identity platform.
services: active-directory
author: rwike77
manager: CelesteDG

ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 03/12/2020
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40
ms.reviewer: aragra, lenalepa, sureshja
# Customer intent: As an enterprise developer or software-as-a-service (SaaS) provider, I want to
# know how to register my application with the Microsoft identity platform so that the security
# token service can issue ID and/or access tokens to clients that want to access it.
---

# Quickstart: Register an application with the Microsoft identity platform

In this quickstart, you register an app in the Azure portal so the Microsoft identity platform can provide authentication and authorization services for your application and its users.

Each application you want the Microsoft identity platform to perform identity and access management (IAM) for needs to be registered. Whether it's a client application like a web or mobile app, or it's a web API that backs a client app, registering it establishes a *trust relationship* between your application and the identity provider, the Microsoft identity platform.

## Prerequisites

* An Azure account with an active subscription - [create an account for free](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* Completion of [Quickstart: Set up a tenant](quickstart-create-new-tenant.md)

## Register an application

Registering your application establishes a trust relationship between your app and the Microsoft identity platform. The trust is *unidirectional*: your app trusts the the Microsoft identity platform, and not the other way around.

Follow these steps to create the app registration:

1. Sign in to the [Azure portal](https://portal.azure.com).
1. If you have access to multiple tenants, use the **Directory + subscription** filter :::image type="icon" source="./media/quickstart-register-app/portal-01-directory-subscription-filter.png" border="false"::: in the top menu to select the tenant in which you want to register an application.
1. Search for and select **Azure Active Directory**.
1. Under **Manage**, select **App registrations**, then **New registration**.
1. Enter a **Name** for your application. Users of your app might see this name, and you can change it later.
1. Specify who can use the application, sometimes referred to as the *sign-in audience*:

    | Supported account types | Description |
    |-------------------------|-------------|
    | **Accounts in this organizational directory only** | Select this option if you're building an application for use only by users (or guests) in *your* tenant.<br><br>Often called a *line-of-business* (LOB) application, this is a **single-tenant** application in the Microsoft identity platform. |
    | **Accounts in any organizational directory** | Select this option if you'd like users in *any* Azure AD tenant to be able to use your application. This option is appropriate if, for example, you're building a software-as-a-service (SaaS) application that you intend to provide to multiple organizations.<br><br>This is known as a **multi-tenant** application in the Microsoft identity platform. |
    | **Accounts in any organizational directory and personal Microsoft accounts** | Select this option to target the widest set of customers.<br><br>By selecting this option, you're registering a **multi-tenant** application that can also support users with personal **Microsoft accounts** (MSA). |

1. Don't enter anything for **Redirect URI (optional)**, you'll configure one in the next section.
1. Select **Register** to complete the initial app registration.

    :::image type="content" source="media/quickstart-register-app/portal-02-app-reg-01.png" alt-text="Screenshot of the Azure portal in a web browser showing the Register an application pane.":::

When registration completes, the Azure portal displays the app registration's **Overview** pane, which includes its **Application (client) ID**. Also referred to simply as the *client ID*, this value uniquely identifies your application in the Microsoft identity platform.

Your application's code, or more typically an authentication library used in your application, also uses the client ID as one aspect in validating the security tokens it receives from the identity platform.

:::image type="content" source="media/quickstart-register-app/portal-03-app-reg-02.png" alt-text="Screenshot of the Azure portal in a web browser showing an app registration's Overview pane.":::

## Add a redirect URI

A redirect URI is the location to which the Microsoft identity platform redirects a user's client and sends security tokens after authentication.

### Configure platform settings

Each application type (platform) is configured differently in **Platform configurations** in the Azure portal. Some platforms, like **Web** and **Single-page application** require a manually configured redirect URI. Redirect URIs for other platforms like mobile and desktop can created automatically when you configure those platforms' other options.

To configure application settings based on the platform or device you're targeting:

1. Under **Manage**, select **Authentication**.
1. Under **Platform configurations**, select **Add a platform**.
1. Configure settings appropriate for your application type or platform.

    :::image type="content" source="media/quickstart-register-app/portal-04-app-reg-03-platform-config.png" alt-text="Screenshot of the Platform configuration pane in the Azure portal" border="false":::

    | Platform | Configuration settings |
    | -------- | ---------------------- |
    | **Web** | Enter the **Redirect URI** for your application.<br/><br/>A redirect URI is the location to which the Microsoft identity platform redirects a user's client and sends security tokens after authentication. |
    | **Single-page application** | Enter the **Redirect URI** for your application.<br/><br/>A redirect URI is the location to which the Microsoft identity platform redirects a user's client and sends security tokens after authentication. |
    | **iOS / macOS** | Enter the app **Bundle ID**, which you can find in XCode in *Info.plist* or Build Settings. Adding the bundle ID automatically creates a redirect URI for the application. |
    | **Android** | Provide the app **Package name**, which you can find in the *AndroidManifest.xml* file.<br/>Generate and enter the **Signature hash**. Adding the signature hash automatically creates a redirect URI for the application. |
    | **Mobile and desktop applications** | (Optional) Select one of the **Suggested redirect URIs** if you're building apps for desktop and devices.<br/><br/>(Optional) Enter a **Custom redirect URI**, the location to which the Microsoft identity platform redirects a user's client and sends security tokens after authentication. |

### Redirect URI restrictions

* Active Directory Federation Services (AD FS) and Azure AD B2C redirect URIs must include a port number. For example, `http://localhost:1234`.
* Redirect URIs for mobile applications that aren't using the latest Microsoft Authentication Library (MSAL) or are not using a broker must be configured in **Mobile and desktop applications**.

For a full list of restrictions and limitations, see [Redirect URI/reply URL restrictions and limitations](reply-url.md).

## Add credentials

To add a credential to a web application, add a certificate or create a client secret. To add a certificate:

1. From the app **Overview** page, select the **Certificates & secrets** section.
1. Select **Upload certificate**.
1. Select the file you'd like to upload. It must be one of the following file types: .cer, .pem, .crt.
1. Select **Add**.

To add a client secret:

1. From the app **Overview** page, select the **Certificates & secrets** section.
1. Select **New client secret**.
1. Add a description for your client secret.
1. Select a duration.
1. Select **Add**.

    **Be sure to copy the secret value** for use in your client application code as it's not accessible once you leave this page.

## Next steps

Client applications typically need to access resources in a web API. In addition to protecting your client application with the Microsoft identity platform, you can use it for authorizing scoped, permissions-based access to your web API.

Move on to the next quickstart in the series to create another app registration for your web API and expose its scopes.

> [!div class="nextstepaction"]
> [Configure an application to expose a web API](quickstart-configure-app-expose-web-apis.md)
