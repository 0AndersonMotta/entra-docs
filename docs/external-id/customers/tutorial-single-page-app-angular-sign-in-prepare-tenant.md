---
title: Tutorial - Prepare your customer tenant to authenticate users in an Angular single-page app (SPA)
description: Learn how to configure your Microsoft Entra External ID for customers tenant for authentication with an Angular single-page app (SPA).
services: active-directory
author: garrodonnell
manager: celestedg
ms.service: active-directory
ms.subservice: ciam
ms.topic: tutorial
ms.date: 10/27/2023
ms.author: godonnell

#Customer intent: As a dev I want to prepare my customer tenant for building a single-page app (SPA) with Angular
---

# Tutorial: Prepare your customer tenant to authenticate users in an Angular single-page app (SPA)

This tutorial series demonstrates how to build an Angular single-page application (SPA) and prepare it for authentication using the Microsoft Entra admin center. You'll use the [Microsoft Authentication Library for JavaScript](/javascript/api/overview/msal-overview) library to authenticate your app with your Microsoft Entra External ID for customers tenant. 

In this tutorial:

> [!div class="checklist"]
> * Register a SPA in the Microsoft Entra admin center, and record its identifiers
> * Define the platform and URLs
> * Grant permissions to the web application to access the Microsoft Graph API
> * Create a sign-in and sign-out user flow in the Microsoft Entra admin center
> * Associate your SPA with the user flow

## Prerequisites

- An Azure subscription. If you don't have one, [create a free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.
- This Azure account must have permissions to manage applications. Any of the following Azure roles include the required permissions:

    * Application Administrator
    * Application Developer
    * Cloud Application Administrator

- A Microsoft Entra External ID for customers tenant. If you haven't already got one, [create one now](https://aka.ms/ciam-free-trial?wt.mc_id=ciamcustomertenantfreetrial_linkclick_content_cnl). You can use an existing customer tenant if you have one.

## Register the SPA and record identifiers

[!INCLUDE [register-client-app-common](./includes/register-app/register-client-app-common.md)]

## Add a platform redirect URL

[!INCLUDE [add-platform-redirect-url-react](./includes/register-app/add-platform-redirect-url-react.md)]

## Grant sign-in permissions

[!INCLUDE [grant-api-permission-sign-in](./includes/register-app/grant-api-permission-sign-in.md)]

## Create a sign-in and sign-up user flow

[!INCLUDE [register-client-app-common](./includes/configure-user-flow/create-sign-in-sign-out-user-flow.md)]

## Associate the application with your user flow

[!INCLUDE [add-app-user-flow](./includes/configure-user-flow/add-app-user-flow.md)]

## Next steps

> [!div class="nextstepaction"]
> [Prepare Angular SPA](./tutorial-single-page-app-angular-sign-in-prepare-app.md)