---
title: "Tutorial: Get user profile from a web API"
titleSuffix: Microsoft identity platform
description: In this tutorial, you will use a React single-page application to call a web API and get user data
services: active-directory
author: Dickson-Mwendia
manager: CelesteDG

ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 09/27/2021
ms.author: dmwendia
ms.reviewer: marsma, dhruvmu
ms.custom: include
---

## Acquire a token

Before calling an API, such as Microsoft Graph, you'll need to acquire an access token. Add the following code to index.js

:::code language="JavaScript" source="~/src/index.js" id="ms_docref_get_graph_token":::


## Call Microsoft Graph API

:::code language="JavaScript" source="~/src/index.js" id="ms_docref_make_graph_call":::

## Next steps

In this tutorial, you acquired an access token and called the Microsoft Graph API using a React single-page application.

Now that you have an app that can sign in users and call a web API, $NEXT_STEP_DESCRIPTION_HERE.

> [!div class="nextstepaction"]
> [$NEXT_STEP_HERE](../../authorization-basics.md)
