---
author: kengaderdus
ms.service: entra-external-id
ms.subservice: customers
ms.topic: include
ms.date: 03/28/2024
ms.author: kengaderdus
ms.manager: mwongerapk
---
Once your app acquires an ID token, you can retrieve the claims associated with the current account. To do so, use the following code snippet.

```kotlin
val preferredUsername = accountState.getClaims()?.get("preferred_username")
val city = accountState.getClaims()?.get("City")
val givenName = accountState.getClaims()?.get("given_name")
//custom attribute
val loyaltyNumber = accountState.getClaims()?.get("loyaltyNumber")
```

The key you use to access the claim value is the name that you specify when yo add the user attribute as a token claim. 

To learn how to specify which built-in or custom attributes to include as claims in the token [Add user attributes to token claims](../../how-to-add-attributes-to-token.md) article.