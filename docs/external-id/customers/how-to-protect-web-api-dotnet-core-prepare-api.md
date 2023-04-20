---
title: Configure web API for protection using Microsoft Entra
description: Learn how to configure web API settings so as to protect it using Microsoft Entra.
services: active-directory
author: SHERMANOUKO
manager: mwongerapk

ms.author: shermanouko
ms.service: active-directory
ms.workload: identity
ms.subservice: ciam
ms.topic: how-to
ms.date: 04/17/2023
ms.custom: developer

#Customer intent: As a dev, I want to configure my web API settings so as to protect it using Microsoft Entra.
---

# Secure an ASP.NET web API by using Microsoft Entra - configure your web API

In this how to article, we go through the steps you take to configure your web API before securing it's endpoints. When using the Microsoft identity platform to secure your web API, you first need to have it registered before configuring your API.

## Prerequisites

Go through the [overview of creating a protected web API](how-to-protect-web-api-dotnet-core-overview.md) before proceeding further with this tutorial.

## Create an ASP.NET Core web API

In this how to guide, we use Visual Studio Code and .NET 7.0. If you're using Visual Studio to create the API, see the [create a Create a web API with ASP.NET Core](/aspnet/core/tutorials/first-web-api).

1. Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
1. Change directories (`cd`) to the folder that contains the project folder.
1. Run the following commands:

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   ```

1. When a dialog box asks if you want to add required assets to the project, select **Yes**.

## Add packages

Install the following packages:

- *Microsoft.EntityFrameworkCore.InMemory* that allows Entity Framework Core to be used with an in-memory database. It's not designed for production use.
- `Microsoft.Identity.Web` simplifies adding authentication and authorization support to web apps and web APIs integrating with the Microsoft identity platform.

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.InMemory
  dotnet add package Microsoft.Identity.Web
  ```

## Configure app registration details

To protect your web API, you need to have the Application (Client) ID, Directory / Tenant ID and the secret value that you generated during registration on the Microsoft Entra admin center. If you haven't registered your web API yet, kindly follow the [web API registration instructions]() before proceeding.

Open the *appsettings.json* file in your app folder and add in the app registration details.

```json
{
    "AzureAd": {
        "Instance": "https://login.microsoftonline.com/",
        "ClientId": "your-client-id",
        "TenantId": "your tenant id"
    },
    "Logging": {...},
  "AllowedHosts": "*"
}
```

## Add app role and scope

All APIs must publish a minimum of one scope, also called [Delegated Permission](/azure/active-directory/develop/permissions-consent-overview#types-of-permissions), for the client apps to obtain an access token for a user successfully. In a similar sense, APIs should also publish a minimum of one app role for applications, also called [Application Permission](/azure/active-directory/develop/permissions-consent-overview#types-of-permissions), for the client apps to obtain an access token as themselves, that is, when they aren't signing-in a user.

We register these permissions in the *appsettings.json* file. These permissions are configured via the Microsoft Entra admin center. For the purposes of this tutorial, we have configured three permissions. *ToDoList.ReadWrite* and *ToDoList.Read* as the delegated permissions and *ToDoList.ReadWrite.All* as the application permission.

```json
{
  "AzureAd": {...},
  "RequiredTodoAccessPermissions": {
    "RequiredDelegatedTodoReadClaims": "ToDoList.Read", //Add your permissions here and label the keys as you wish
    "RequiredDelegatedTodoWriteClaims": "ToDoList.ReadWrite",
    "RequiredApplicationTodoReadWriteClaims": "ToDoList.ReadWrite.All"
  },
  "Logging": {...},
  "AllowedHosts": "*"
}
```

We then set these keys as constants to make them accessible to the `RequiredScopeOrAppPermission` attribute. Create a folder called *Options* in the project root directory. Navigate to this directory and add a new file called *RequiredToDoAccessPermissionsOptions.cs* with the following content.

```csharp
namespace TodoApi.Options;

public class RequiredTodoAccessPermissionsOptions
{
    public const string RequiredTodoAccessPermissions = "RequiredTodoAccessPermissions";

    // Set these keys as constants to make them accessible to the 'RequiredScopeOrAppPermission' attribute. You can add
    // multiple spaces separated entries for each string in the 'appsettings.json' file and they will be used by the
    // 'RequiredScopeOrAppPermission' attribute.
    public const string RequiredDelegatedTodoReadClaimsKey =
        $"{RequiredTodoAccessPermissions}:RequiredDelegatedTodoReadClaims";

    public const string RequiredDelegatedTodoWriteClaimsKey =
        $"{RequiredTodoAccessPermissions}:RequiredDelegatedTodoWriteClaims";

    public const string RequiredApplicationTodoReadWriteClaimsKey =
        $"{RequiredTodoAccessPermissions}:RequiredApplicationTodoReadWriteClaims";
}
```

## Add authentication scheme

An [authentication scheme](/aspnet/core/security/authorization/limitingidentitybyscheme) is named when the authentication service is configured during authentication. In this article, we use the JWT bearer authentication scheme.

Add the following code in the *Programs.cs* file to add authentication scheme.

```csharp
// Add the following to your imports
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.Identity.Web;

// Add authentication scheme
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApi(builder.Configuration);
```

## Create your models

Create a folder called *Models* in the root folder of your project. Navigate to the folder and create a file called *TodoItem.cs* and add the following code. This code creates a model called *TodoItem*.

```csharp
namespace TodoApi.Models;

public class TodoItem
{
    public int ID { get; set; }
    public Guid UserId { get; set; }
    public string Message { get; set; } = string.Empty;
}
```

##  Add a database context

The database context is the main class that coordinates Entity Framework functionality for a data model. This class is created by deriving from the [*Microsoft.EntityFrameworkCore.DbContext*](/dotnet/api/microsoft.entityframeworkcore.dbcontext) class. In this article, we use an in-memory database for testing purposes.

Create a folder called *DbContext* in the root folder of the project. Navigate into that folder and create a file called *TodoContext.cs*. Add the following contents to that file:

```csharp
using Microsoft.EntityFrameworkCore;
using TodoApi.Models;

namespace TodoApi;

public class TodoContext : DbContext
{
    public TodoContext(DbContextOptions<TodoContext> options)
        : base(options)
    {
    }

    public DbSet<TodoItem> TodoItems { get; set; } = null!;
}
```

Add the following code in the *Program.cs* file.

```csharp
// Add the following to your imports
using TodoApi;
using Microsoft.EntityFrameworkCore;

builder.Services.AddDbContext<TodoContext>(opt =>
    opt.UseInMemoryDatabase("TodoItems"));
```

## Next steps

> [!div class="nextstepaction"]
> [Protect your web API endpoints >](how-to-protect-web-api-dotnet-core-protect-endpoints.md)
