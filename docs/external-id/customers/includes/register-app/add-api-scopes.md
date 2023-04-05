---
author: kengaderdus
ms.service: active-directory
ms.subservice: ciam
ms.topic: include
ms.date: 03/30/2023
ms.author: kengaderdus
---

1. From the **App registrations** page, select the API application that you created (`ciam-ToDoList-api`) to open its **Overview** page.

1. Under **Manage**, select **Expose an API**.

1. At the top of the page, next to **Application ID URI**, select the **Set** link to generate a URI that is unique for this app.
 
1. Accept the proposed Application ID URI such as `https://{tenantName}.ciamlogin.com/{clientId}`, and then select **Save**. When your web application requests an access token for the web API, it should add this URI as the prefix for each scope that you define for the API.
 
1. Under **Scopes defined by this API**, select **Add a scope**, and on the pane that appears, create a scope that defines read access to the API:
    
    1. For **Scope name**, enter `ToDoList.Read`.
    
    1. For **Admin consent display name**, enter **Read users ToDo list using the 'TodoListApi'**.
    
    1. For **Admin consent description**, enter **Allow the app to read the user's ToDo list using the 'TodoListApi'**.
    
    1. Keep **State** as **Enabled** and select **Add scope**.
    
    1. Select the **Add scope** button at the bottom of the pane to save the scope.

1. Select **Add a scope** again, and then add a scope that defines write and write access to the API:

    1. For **Scope name**, enter `ToDoList.ReadWrite`.
    
    1. For **Admin consent display name**, enter **Read and write users ToDo list using the 'TodoListApi'**.
    
    1. For **Admin consent description**, enter **Allow the app to read and write the user's ToDo list using the 'TodoListApi'**.
    
    1. Keep **State** as **Enabled**.
    
    1. Select the **Add scope** button at the bottom of the pane to save the scope.

1. Under **Manage**, select **Manifest** to open the API manifest editor.

1. Set `accessTokenAcceptedVersion` property to **2**.

1. Select **Save**.

Learn more about [the principle of least privilege when publishing permissions](https://learn.microsoft.com/security/zero-trust/develop/protected-api-example) for a web API. 