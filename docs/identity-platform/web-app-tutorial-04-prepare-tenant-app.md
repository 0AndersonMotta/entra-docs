---
title: "Tutorial: Prepare an application for authentication in the Azure Active Directory tenant"
description: Prepare an application for authentication in the Azure Active Directory tenant.
author: davidmu1
ms.author: davidmu
manager: CelesteDG
ms.service: active-directory
ms.topic: tutorial
ms.date: 10/18/2022
#Customer intent: As an application developer, I want to add a platform to my web application
---

# Tutorial: Prepare an application for authentication in an Azure Active Directory tenant

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Define platform settings
> * Define the URLs needed for the application

## Prerequisites

* Completion of the prerequisites and steps in [Tutorial: Add sign-in to an application](web-app-tutorial-03-sign-in-users.md).

## Define the platform

1. In the Azure portal, under **Manage**, select **App registrations**, and then select the application that was previously created.
1. In the left menu, under **Manage**, select **Authentication**.
1. In **Platform configurations**, select **Add a platform**, and then select **Web**.

    :::image type="content" source="./media/web-app-tutorial-04-prepare-tenant-app/select-platform-inline.png" alt-text="Screenshot on how to select the platform for the application." lightbox="./media/web-app-tutorial-04-prepare-tenant-app/select-platform-expanded.png":::

## Define URLs

1. Under **Redirect URIs**, enter the **applicationURL** and the **CallbackPath** that was recorded when configuring the application. For example, `https://localhost:7141/signin-oidc`.
1. Under **Front-channel logout URL**, enter the application URL and a callback path for signing out. For example, `https://localhost:7141/signout-oidc`.
1. Under **Implicit grant and hybrid flows**, select **ID tokens**.

<!-- See also articles needed -->

## Next steps

You can choose either of the following options depending on your workflow. 

To call Microsoft Graph; 

> [!div class="nextstepaction"]
> [Tutorial: Call an API and display results](web-app-tutorial-05-call-web-api.md)

Alternatively, to set up a web API from scratch, refer to the web API tutorial series. 

> [!div class="nextstepaction"]
> [Tutorial: Register an API with the Microsoft identity platform](web-app-tutorial-05-call-web-api.md)
<!-- Link to web API tutorial series recommended here -->
