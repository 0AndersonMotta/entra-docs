---
title: Desktop app that calls Web APIs - calling a Web API | Azure
description: Learn how to build a Desktop app that calls Web APIs (calling a Web API |)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''

ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev 
#Customer intent: As an application developer, I want to know how to write a Desktop app that calls Web APIs using the Microsoft identity platform for developers.
ms.collection: M365-identity-device-management
---

# Calling a Web API from a desktop app

Now that you have a token, you can call a protected Web API.

## Calling a Web API from .NET

[!INCLUDE [Call Web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## Calling several APIs - Incremental consent and conditional access

If you need to call several APIs for the same user, once you got a token for the first API, you can just call `AcquireTokenSilent`, and you'll get a token for the other APIs silently most of the time.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

The cases where interaction is required is when:

- The user consented for the first API, but now needs to consent for more scopes (incremental consent)
- The first API didn't require multiple-factor authentication, but the next one does.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## Next steps

> [!div class="nextstepaction"]
> [Move to production](scenario-desktop-production.md)
