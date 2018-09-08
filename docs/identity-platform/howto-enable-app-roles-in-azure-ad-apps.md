---
title: How to add app roles in your application and recieve them in the token | Microsoft Docs 
description: Learn how to add app roles in an application registered in Azure Active Directory, assign users and groups to these roles and receive them in the `roles` claim in the token.
services: active-directory
documentationcenter: ''
author: kkrishna
manager: mtillman
editor: ''

ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: ''
ms.custom: aaddev

---
# How to: Add app roles in your application and recieve them in the token

## Introduction

Role-based access control (RBAC) is a popular mechanism to enforce authorization in applications. When using RBAC, an administrator grants permissions to roles, and not to individual users or groups. The administrator can then assign roles to different users and groups to control who has access to what content and functionality.

Using RBAC with Application Roles and Role Claims, developers can securely enforce authorization in their apps with little effort on their part.

Another approach is to use Azure AD Groups and Group Claims, as shown in [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet). Azure AD Groups and Application Roles are by no means mutually exclusive; they can be used in tandem to provide even finer grained access control.

## Declare roles for an application

These application roles are defined in the [Azure portal](https://portal.azure.com) in the application's registration manifest.  When a user signs into the application, Azure AD emits a `roles` claim for each role that the user has been granted individually to the user and from their group membership.  Assignment of users and groups to roles can be done through the portal's UI, or programmatically using the [Microsoft Graph](https://graph.microsoft.com).

### Declare app roles using Azure Portal

1. Sign in to the [Azure portal](https://portal.azure.com).
1. On the top bar, click on your account, and then on **Switch Directory**.
1. Once the *Directory + subscription* pane opens, choose the Active Directory tenant where you wish to register your application, from the *Favorites* or *All Directories* list.
1. Click on **All services** in the left-hand nav, and choose **Azure Active Directory**.
1. In the  **Azure Active Directory** pane, click on **App registrations**, to view a list of all your applications.

     If you do not see the application you want show up here, use the various filters at the top of the **App registrations** list to restrict the list or scroll down the list to locate your application.

1. Select the application you want to defines app roles in.
1. In the blade for your application, click **Manifest**.
1. Edit the manifest by locating the `appRoles` setting and adding all your Application Roles.  
    >Each role definition in this manifest must have a different valid **Guid** for the "Id" property.
    >The `"value"` property of each role should exactly match the strings are used in the code in the application.
1. Save the manifest.

### Examples

An example of `appRoles` that can be assigned to `users` is provided below.

>The `id` should be a unique guid.

An example of `appRoles` that can be assigned to `users` is provided below.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-98f8-45fd-aa4a-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

> App roles can be defined to target `users`, `applications` or both. When available to `applications`, they appear as application permissions in the **Required Permissions** blade. An example of a app role targeted towards an `Application` is provided below.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "Consumer Apps",
      "id": "47fbb575-859a-4941-89c9-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

### Assign users and groups to roles

1. In the **Azure Active Directory** pane, select **Enterprise Applications** from the **Azure Active Directory** left-hand navigation menu.
1. Select **All Applications** to view a list of all your applications.

     If you do not see the application you want show up here, use the various filters at the top of the **All applications** list to restrict the list or scroll down the list to locate your application.

1. Select the application in which you want to assign users or security group to roles.
1. 1. Select the **Users and groups** pane in the application’s left-hand navigation menu.
1. At the top of the **Users and groups** list, select the **Add user** button to open the **Add Assignment** pane.
1. Select the **Users and groups** selector from the **Add Assignment** pane.

     A list of users and security groups will be shown along with a textbox to search and locate a certain user or group. This screen allows you to select multiple users and groups in one go.

1. Once you are done selecting the users and groups, press the **Select** button on bottom to move to the next part.
1. Click on the **Select Role** selector from the **Add Assignment** pane.
1. All the roles declared earlier in the app manifest will show up.
1. Click on a role and press the **Select** button.
1. Press the **Assign** button on the bottom to finish the assignments of users and groups to the app.
1. Confirm that the users and groups you added are showing up in the updated **Users and groups** list.

## More information

- [Authorization in a web app using Azure AD application roles &amp; role claims (Sample)](https://azure.microsoft.com/en-us/resources/samples/active-directory-dotnet-webapp-roleclaims/)
- [Using Security Groups and Application Roles in your apps (Video)](https://www.youtube.com/watch?v=V8VUPixLSiM)
- [Azure Active Directory, now with Group Claims and Application Roles](https://cloudblogs.microsoft.com/enterprisemobility/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles)
- [Azure Active Directory app manifest](https://docs.microsoft.com/en-us/azure/active-directory/develop/reference-app-manifest)
- [Azure AD token reference](https://docs.microsoft.com/en-us/azure/active-directory/develop/v1-id-and-access-tokens)