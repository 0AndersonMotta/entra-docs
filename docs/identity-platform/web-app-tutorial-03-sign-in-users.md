---
title: "Tutorial: Add sign in to an application"
description: Add sign in to an ASP.NET Core application using Visual Studio.
author: cilwerner
ms.author: cwerner
manager: CelesteDG
ms.service: active-directory
ms.topic: tutorial
ms.date: 02/09/2023
#Customer intent: As an application developer, I want to install the NuGet packages necessary for authentication in my IDE, and implement authentication in my web app.
---

# Tutorial: Add sign in to an application

In the [previous tutorial](web-app-tutorial-02-prepare-application.md), an ASP.NET Core project was created and configured for authentication. This tutorial will install the required packages and add code that implements authentication to the sign in and sign out experience.

In this tutorial:

> [!div class="checklist"]
> * Identify and install the NuGet packages that are needed for authentication
> * Implement authentication in the code
> * Add the sign in and sign out experiences

## Prerequisites

* Completion of the prerequisites and steps in [Tutorial: Prepare an application for authentication](web-app-tutorial-02-prepare-application.md).

## Install identity packages

Identity related **NuGet packages** must be installed in the project for authentication of users to be enabled.

### [Visual Studio](#tab/visual-studio)

1. Open the project that was previously created in Visual Studio. In the top menu of Visual Studio, select **Tools > NuGet Package Manager > Manage NuGet Packages for Solution**.
1. With the **Browse** tab selected, search for and select **Microsoft.Identity.Web**. Select the **Project** checkbox, and then select **Install**.
1. Repeat the previous step for the **Microsoft.Identity.Web.UI** package.

### [Visual Studio Code](#tab/visual-studio-code)

1. In Visual Studio Code, select **Terminal** then **New Terminal.**
1. Ensure that the correct directory is selected (*NewWebAppLocal*), then enter the following into the terminal to install the relevant NuGet packages:

    ```powershell
    dotnet add package Microsoft.Identity.Web
    dotnet add package Microsoft.Identity.Web.UI
    dotnet add package Microsoft.Identity.Web.Diagnostics
    ``` 

### [Visual Studio for Mac](#tab/visual-studio-for-mac)

1. In the top menu, select **Tools** > **Manage NuGet Packages**.
1. Search for **Microsoft.Identity.Web**, select the `Microsoft.Identity.Web` package, select **Project**, and then select **Add Package**. 
1. Modify your search to read **Microsoft.Identity.Web.UI** and select **Add Packages**.
1. In the pop-up, ensure the correct project is selected, then select **Ok**.
1. Select **Accept** if additional **License Acceptance** windows appear.
---

## Implement authentication and acquire tokens

1. Open *Program.cs* and replace the entire file contents with the following snippet:

    ```csharp
    // Imports packages
    using Microsoft.AspNetCore.Authorization;
    using Microsoft.AspNetCore.Mvc.Authorization;
    using Microsoft.Identity.Web;
    using Microsoft.Identity.Web.UI;

    var builder = WebApplication.CreateBuilder(args);
    // Retrieve the defined scopes from the configuration in appsettings.json
    IEnumerable<string>? initialScopes = builder.Configuration["DownstreamApi:Scopes"]?.Split(' ');

    // Adds the AzureAd service configuration in appsettings.json, and configures the service to acquire a token
    builder.Services.AddMicrosoftIdentityWebAppAuthentication(builder.Configuration, "AzureAd")
        .EnableTokenAcquisitionToCallDownstreamApi(initialScopes)
        .AddDownstreamWebApi("DownstreamApi", builder.Configuration.GetSection("DownstreamApi"))
        .AddInMemoryTokenCaches();

    // Adds authentication services to the container.
    builder.Services.AddRazorPages().AddMvcOptions(options =>
    {
      var policy = new AuthorizationPolicyBuilder()
        .RequireAuthenticatedUser()
        .Build();
      options.Filters.Add(new AuthorizeFilter(policy));
    }).AddMicrosoftIdentityUI();

    var app = builder.Build();

    // Configure the HTTP request pipeline.
    if (!app.Environment.IsDevelopment())
    {
        app.UseExceptionHandler("/Error");
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    // Includes the MVC options and authentication
    app.UseAuthentication();
    app.MapControllers();

    app.UseRouting();
    app.UseAuthorization();
    app.MapRazorPages();
    app.Run();
    ```

## Add the sign in and sign out experience

After installing the NuGet packages and adding necessary code for authentication, add the sign in and sign out experiences.

### Create the *_LoginPartial.cshtml* file

### [Visual Studio](#tab/visual-studio)

1. Expand **Pages**, right-click **Shared**, and then select **Add > Razor page**.
1. Select **Razor Page - Empty**, and then select **Add**.
1. Enter *_LoginPartial.cshtml* for the name, and then select **Add**.

### [Visual Studio Code](#tab/visual-studio-code)

1. In the Explorer bar, select **Pages**, right-click **Shared**, and select **New File**. Give it the name *_LoginPartial.cshtml*.

### [Visual Studio for Mac](#tab/visual-studio-for-mac)

1. Expand **Pages**, right-click **Shared**, and then select **Add > Razor page**.
1. Select **Razor Page - Empty**, and then select **Add**.
1. Enter *_LoginPartial.cshtml* for the name, and then select **Add**.
---

### Edit the *_LoginPartial.cshtml* file

1. Open *_LoginPartial.cshtml* and add the following code for adding the sign in and sign out experience:

    ```csharp
    @using System.Security.Principal

    <ul class="navbar-nav">
      @if (User.Identity?.IsAuthenticated == true)
      {
        <li class="nav-item">
          <span class="navbar-text text-dark">Hello @User.Identity?.Name!</span>
        </li>
        <li class="nav-item">
          <a class="nav-link text-dark" asp-area="MicrosoftIdentity" asp-controller="Account" asp-action="SignOut">Sign out</a>
        </li>
      }
      else
      {
        <li class="nav-item">
          <a class="nav-link text-dark" asp-area="MicrosoftIdentity" asp-controller="Account" asp-action="SignIn">Sign in</a>
        </li>
      }
    </ul>
    ```

1. Open *_Layout.cshtml* and add a reference to `_LoginPartial` created in the previous step. This single line should be placed between `</ul>` and `</div>`:

    ```csharp
      </ul>
        <partial name="_LoginPartial" />
    </div>
    ```

## Next steps

> [!div class="nextstepaction"]
> [Tutorial: Call an API and display results](web-app-tutorial-04-call-web-api.md)
