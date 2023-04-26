---
title: Prepare your customer tenant for building a React Single Page App (SPA) in Microsoft Entra
description: Learn how to prepare my customer tenant for building a React Single Page App (SPA) in Microsoft Entra
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

#Customer intent: As a dev I want to prepare my customer tenant for building a Single Page App with React
---
# Prepare your customer tenant for building a Single Page App (SPA) in Microsoft Entra

Before your applications can interact with Microsoft Identity Platform they must be registered in a Microsoft Entra for Customers tenant that you manage and must be associated with a user flow.

In this article, you learn how to:

> [!div class="checklist"]
> * Register your application and record identifiers.
> * Create a user flow to allow sign-up and sign-in.
> * Associate the user flow with your application.
> * Create a client secret for your application.

## Prerequisites

An Azure subscription. If you don't have one, [create a free account](https://aka.ms/ciam-hub-free-trial) before you begin.

This Azure account must have permissions to manage applications. Any of the following Azure AD roles include the required permissions:
* Application administrator
* Application developer
* Cloud application administrator

If you haven't already created your own Microsoft Entra for Customers Tenant, [create one now](https://aka.ms/ciam-hub-free-trial). You can use an existing customer tenant if you have one.

## Register the application and record identifiers
[!INCLUDE [register-client-app-common](./includes/register-app/register-client-app-common.md)]

## Create a sign-in and sign-up user flow
[!INCLUDE [register-client-app-common](./includes/configure-user-flow/create-sign-in-sign-out-user-flow.md)]

## Associate the application with your user flow
[!INCLUDE [add-app-user-flow](./includes/configure-user-flow/add-app-user-flow.md)]

## Creating a client secret
[!INCLUDE [add-app-client-secret](./includes/register-app/add-app-client-secret.md)]


## Next steps

> [!div class="nextstepaction"]
> [Start building your React Single Page Application](./how-to-spa-react-prepare-app.md)
