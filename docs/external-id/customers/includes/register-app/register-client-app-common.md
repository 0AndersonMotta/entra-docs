---
author: kengaderdus
ms.service: active-directory
ms.subservice: ciam
ms.topic: include
ms.date: 03/30/2023
ms.author: kengaderdus
---
To enable your application to sign in with Microsoft Entra, Azure Active Directory (Azure AD) for customers must be made aware of the application you create. The app registration establishes a trust relationship between the app and Microsoft Entra.

During registration, you'll specify a **Redirect URI** which redirects the user after authentication with Microsoft Entra. The app registration process also generates a unique identifier known as an **Application (client) ID**. Once registered, Microsoft Entra uses both values to create authentication requests.

The following steps show you how to register your app in the Microsoft Entra admin center:

1.  Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com/).

1. If you have access to multiple tenants, make sure you use the directory that contains your Azure AD for customers tenant:
    
    1. Select the **Directories + subscriptions** icon in the portal toolbar. 
    
    1. On the **Portal settings | Directories + subscriptions** page, find your Azure AD for customers directory in the **Directory name** list, and then select **Switch**. 

1. On the sidebar menu, select **Azure Active Directory**.

1. Select **Applications**, then select **App Registrations**.

1. Select **+ New registration**.

1. In the **Register an application page** that appears, enter your application's registration information:
    
    1. In the **Name** section, enter a meaningful application name that will be displayed to users of the app, for example *ciam-client-app*.
    
    1. Under **Supported account types**, select **Accounts in this organizational directory only**.

1. Select **Register** to create the application.

1. Record the **Application (client) ID** and **Directory (tenant) ID** for later use, when you configure the sample application.