---
title: "Tutorial: Call an API and display the results"
description: Call an API and display the results.
author: cilwerner
ms.author: cwerner
manager: CelesteDG
ms.service: active-directory
ms.topic: tutorial
ms.date: 02/09/2023
#Customer intent: As an application developer, I want to use my app to call a web API, in this case Microsoft Graph. I need to know how to modify my code so the API can be called successfully.

# Review Stage 3: PM Review - (ADO-54212).
---

# Tutorial: Call an API and display the results 

The application can now be configured to call an API. For the purposes of this tutorial, the Microsoft Graph API will be called to display the profile information of the logged-in user.  

In this tutorial: 

> [!div class="checklist"] 
> * Call the API and display the results 
> * Test the application 

## Prerequisites

* Completion of the prerequisites and steps in [Tutorial: Add sign in to an application](web-app-tutorial-03-sign-in-users.md). 

## Call the API and display the results

1. Under **Pages**, open the *Index.cshtml.cs* file and replace the entire contents of the file with the following snippet. Check that the project `namespace` matches your project name.  

    ```csharp
    using System.Text.Json;
    using Microsoft.Identity.Web;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.AspNetCore.Mvc.RazorPages;
    
    namespace NewWebAppLocal.Pages;
    
    // The `AuthorizeForScopes` attribute is provided by `Microsoft.Identity.Web`. It makes sure that the user is asked for consent if needed, and incrementally.
    [AuthorizeForScopes(ScopeKeySection = "DownstreamApi:Scopes")]
    public class IndexModel : PageModel
    {
        // Define API object and gets added to the logger
        private readonly IDownstreamWebApi _downstreamWebApi;
        private readonly ILogger<IndexModel> _logger;    
        public IndexModel(ILogger<IndexModel> logger, IDownstreamWebApi downstreamWebApi)
        {
          _logger = logger;
          _downstreamWebApi = downstreamWebApi;
        }

        // Calls the API and displays the result
        public async Task OnGet()
        {
          using var response = await _downstreamWebApi.CallWebApiForUserAsync("DownstreamApi").ConfigureAwait(false);
          if (response.StatusCode == System.Net.HttpStatusCode.OK)
          {
            var apiResult = await response.Content.ReadFromJsonAsync<JsonDocument>().ConfigureAwait(false);
            ViewData["ApiResult"] = JsonSerializer.Serialize(apiResult, new JsonSerializerOptions { WriteIndented = true });
          }
          else
          {
            var error = await response.Content.ReadAsStringAsync().ConfigureAwait(false);
            throw new HttpRequestException($"Invalid status code in the HttpResponseMessage: {response.StatusCode}: {error}");
          }
        }
    }
    ``` 
  
1. Open the *Index.cshtml* file, which handles how the API is displayed to the user. Add the following code at the bottom of the file: 

	```html
    <p>Before rendering the page, the Page Model was able to make a call to Microsoft Graph's <code>/me</code> API for your user and received the following:</p>

    <p><pre><code class="language-js">@ViewData["ApiResult"]</code></pre></p>

    <p>Refreshing this page will continue to use the cached access token acquired for Microsoft Graph, which is valid for future page views will attempt to refresh this token as it nears its expiration.</p>
    ``` 

## Test the application 

### [Visual Studio](#tab/visual-studio)
1. Start the application by selecting **Start without debugging**. 

### [Visual Studio Code](#tab/visual-studio-code)
1. Start the application by typing the following in the terminal:

    ```powershell
    dotnet run
    ``` 

### [Visual Studio for Mac](#tab/visual-studio-for-mac)
1. Start the application by selecting the **Play** icon.  

---

2. Depending on your IDE, you may need to enter the application URI into the browser, for example `https://localhost:7100`. After the sign in window appears, select the account with which to sign in. 

    :::image type="content" source="./media/web-app-tutorial-04-call-web-api/pick-account.png" alt-text="Screenshot depicting account options to sign in."::: 
 
1. Upon selecting the account, a second window appears indicating that a code will be sent to your email address. Select **Send code**, and check your email inbox. 

    :::image type="content" source="./media/web-app-tutorial-04-call-web-api/sign-in-send-code.png" alt-text="Screenshot depicting a screen to send a code to the user's email."::: 
 
1. Open the email from the sender **Microsoft account team**, and enter the 7-digit *single-use code*. Once entered, select **Sign in**. 

    :::image type="content" source="./media/web-app-tutorial-04-call-web-api/enter-code.png" alt-text="Screenshot depicting the single-use code sign in procedure."::: 

1. For **Stay signed in**, you can select either **No** or **Yes**. 

    :::image type="content" source="./media/web-app-tutorial-04-call-web-api/stay-signed-in.png" alt-text="Screenshot depicting the option on whether to stay signed in."::: 

1. The app will ask for permission to sign in and access data. Select **Accept** to continue. 

    :::image type="content" source="./media/web-app-tutorial-04-call-web-api/permissions-requested.png" alt-text="Screenshot depicting the permission requests."::: 

1. The web app now displays profile data acquired from the Microsoft Graph API. 

    :::image type="content" source="./media/web-app-tutorial-04-call-web-api/display-api-call-results.png" alt-text="Screenshot depicting the results of the API call."::: 

## Next steps

Learn how to use the Microsoft identity platform by trying out the following tutorial series on how to build a web API. 

> [!div class="nextstepaction"] 
> [Tutorial: Register a web API with the Microsoft identity platform](web-app-tutorial-01-register-application.md) 