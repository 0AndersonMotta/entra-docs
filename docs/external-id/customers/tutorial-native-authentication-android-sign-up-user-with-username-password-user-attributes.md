---
title: Sign up user with username, password, and user attributes
description: Learn how to Sign up user with username, password, and user attributes.

author: henrymbuguakiarie
manager: mwongerapk

ms.author: henrymbugua
ms.service: active-directory

ms.subservice: ciam
ms.topic: how-to
ms.date: 02/23/2024
ms.custom: developer
#Customer intent: As a dev, devops, I want to learn how to Sign up user with username, password and user attributes.
---

# Tutorial: Sign up user with username, password, and user attributes  
 
This tutorial aims to demonstrate how to sign up a user with a username, password, and user attributes (**City and Country/Region**), and use one-time passcode for validation of the user's email address.  
 
In this tutorial, you learn how to:  
 
- Sign up using username, password, and user attributes.  
- Handle errors.  
 
## Prerequisites  
 
- An Android project. 
- [How to run the Android sample app](how-to-run-native-authentication-sample-android-app.md). Ensure that when creating the user flow, you select **Email with password** in the **Identity providers** section, and choose **Country/Region** and **City** under **User attributes** section.
- [Tutorial: Add sign in and sign out with email one-time passcode](tutorial-native-authentication-android-sign-in-sign-out.md). 
 
## Sign up user using username, password, and user attributes  
 
To sign up user using username (email address), password, and user attributes, we need to verify the email through email one-time passcode.  
 
The MSAL Android SDK provides a utility class `UserAttribute.Builder` to create user attributes.  
 
1. To include a user attribute, use the `UserAttribute.Builder` utility class as shown in the following code snippet:  
 
    ```kotlin 
    val userAttributes = UserAttributes.Builder 
        .country(country) 
        .city(city) 
        .build() 
     
    CoroutineScope(Dispatchers.Main).launch { 
        val actionResult = authClient.signUp( 
            username = emailAddress, 
            password = password, 
            attributes = userAttributes 
        ) 
    } 
    ``` 
 
1. To continue with the flow, the `signUp()` method returns `SignUpResult.CodeRequired`, which will provide access to `submitCode()` and `resendCode()`. To submit the code that the user supplied us with, use the following code snippet:  
 
    ```kotlin 
    CoroutineScope(Dispatchers.Main).launch { 
        val actionResult = authClient.signUp( 
            username = emailAddress, 
            password = password, 
            attributes = userAttributes 
        ) 
        if (actionResult is SignUpResult.CodeRequired) { 
            val nextState = actionResult.nextState 
            val submitCodeActionResult = nextState.submitCode( 
                code = code 
            ) 
            if (submitCodeActionResult is SignUpResult.Complete) { 
                // Handle sign up success 
            } 
        } 
    } 
    ``` 
 
## Handle errors  
 
The `signUp()` action return results denoted by a dedicated results class `SignUpResult`. These can be of type: 
- `SignUpResult.Complete`
- `SignUpResult.CodeRequired`
- `SignUpResult.AttributesRequired`
- `SignUpError`

In the case of `SignUpError`, the SDK provides utility methods  for further analyzing the specific type of error returned: 
- `isUserAlreadyExists()`
- `isInvalidAttributes()`
- `isInvalidUsername()`
- `isBrowserRequired()`
- `isAuthNotSupported()`

Errors such as these indicate that the previous operation was unsuccessful, and because of that they don't include a reference to a new state. 
 
The utility method `isInvalidAttributes()` returns true when one or more attributes that were sent failed input validation. It contains an `invalidAttributes` parameter, which is a list of all attributes that were sent by the developer that failed input validation.  
 
The result `SignUpResult.AttributesRequired` indicates that the server requires one or more attributes to be sent, before the user account can be created. This happens when one or more attributes is set as mandatory in the tenant configuration. This result contains a `requiredAttributes` parameter, which is a list of `RequiredUserAttribute` objects, which outline details about the user attributes that the API requires.  
 
To handle `isInvalidAttributes` and `AttributesRequired`, use the following code snippet:  
 
```kotlin 
val actionResult = authClient.signUp( 
    username = email, 
    attributes = attributes 
) 
 
if (actionResult is SignUpError && actionResult.isInvalidAttributes()) {
    val invalidAttributes = actionResult.invalidAttributes 
    // Handle "invalid attributes" error 
    authClient.signUp( 
        username = email, 
        attributes = resubmittedAttributes 
    ) 
} else if (actionResult is SignUpResult.AttributesRequired) { 
    val requiredAttributes = actionResult.requiredAttributes 
    // Handle "attributes required" result 
    val nextState = actionResult.nextState 
    nextState.submitAttributes( 
        attributes = moreAttributes 
    ) 
} 
``` 
  
## Next steps  
  
[Tutorial: Sign up user with username and password](tutorial-native-authentication-android-sign-up-user-with-username-password.md) 
