---
title: Migrate apps to MSAL.ObjectiveC | Microsoft identity platform
description: Learn about the differences between Microsoft Authentication Library for ObjectiveC (MSAL for iOS and macOS) and Azure AD Authentication Library for ObjectiveC (ADAL.ObjC) and how to migrate to MSAL for iOS and macOS.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''

ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/19/2019
ms.author: twhitney
ms.reviewer: oldalton
ms.custom: aaddev
#Customer intent: As an application developer, I want to learn about the differences between the Objective-C ADAL and MSAL for iOS and macOS libraries so I can migrate my applications to MSAL for iOS and macOS.
ms.collection: M365-identity-device-management
---

# Migrate applications to MSAL for iOS and macOS

The Azure Active Directory Authentication Library ([ADAL Objective-C](https://github.com/AzureAD/azure-activedirectory-library-for-objc)) was created to work with  Azure Active Directory accounts via the v1.0 endpoint.

The Microsoft Authentication Library for iOS and macOS (MSAL) is built to work with all Microsoft identities such as Azure Active Directory (Azure AD) accounts, personal Microsoft accounts, and Azure AD B2C accounts. It does this via the Microsoft identity platform (formally the Azure AD v2.0 endpoint).

The Microsoft identity platform has a few key differences with Azure Active Directory v1.0. This article highlights these differences and provides guidance to migrate an app from ADAL to MSAL.

##  ADAL and MSAL app capability differences

### Who can sign in

* ADAL only supports work and school accounts--also known as Azure AD accounts.
* MSAL supports personal Microsoft accounts (MSA accounts) such as Hotmail.com, Outlook.com, and Live.com.
* MSAL supports work and school accounts, and Azure AD B2C accounts.

### Standards compliance

* The Microsoft identity Platform endpoint follows OAuth 2.0 and OpenId Connect standards.

### Incremental and dynamic consent

* The Azure Active Directory v1.0 endpoint requires that all permissions be declared in advance during application registration. This means those permissions are static.
* The Microsoft identity platform allows you to request permissions dynamically. Apps can ask for permissions only as needed and request more as the app needs them.

For more about differences between Azure Active Directory v1.0 and the Microsoft identity platform, see [Why update to Microsoft identity platform (v2.0)?](https://docs.microsoft.com/azure/active-directory/develop/azure-ad-endpoint-comparison).

## ADAL and MSAL library differences

The MSAL public API reflects a few key differences between Azure AD v1.0 and the Microsoft identity platform.

### ADAuthenticationContext instead of MSALPublicClientApplication

`ADAuthenticationContext` is the first object an ADAL app creates. It represents an instantiation of ADAL. Apps create a new instance of `ADAuthenticationContext` for each Azure Active Directory cloud and tenant (authority) combination. The same `ADAuthenticationContext` can be used to get tokens for multiple public client applications.

In MSAL, the main interaction is through a`MSALPublicClientApplication` object, which is modeled after [OAuth 2.0 Public Client](https://tools.ietf.org/html/rfc6749#section-2.1). One instance of `MSALPublicClientApplication` can be used to interact with multiple AAD clouds, and tenants, without needing to create a new instance for each authority. For most apps, one `MSALPublicClientApplication` instance is sufficient.

### Scopes instead of resources

In ADAL, an app had to provide a *resource* identifier like `https://graph.microsoft.com`to acquire tokens from the Azure Active Directory v1.0 endpoint. A resource can define a number of scopes, or oAuth2Permissions in the app manifest, that it understands. This allowed client apps to request tokens from that resource for a certain set of scopes pre-defined during app registration.

In MSAL, instead of a single resource identifier, apps provide a set of scopes per request. A scope is a resource identifier followed by a permission name in the form resource/permission. For example, `https://graph.microsoft.com/user.read`

There are two ways to provide scopes in MSAL:

* Provide a list of all the permissions your apps needs. For example: 

    `@[@"https://graph.microsot.com/directory.read", @"https://graph.microsoft.com/directory.write"]`

    In this case, the app requests the `directory.read` and `directory.write` permissions. The user will be asked to consent for those permissions if they haven't consented to them before for this app. The application might also receive additional permissions that the user has already consented to for the application. The user will only be prompted to consent for new permissions, or permissions that haven't been granted.

* The `/.default` scope.

This is the built-in scope for every application. It refers to the static list of permissions configured when the application was registered. Its behavior is similar to that of `resource`. This can be useful when migrating to ensure that a similar set of scopes and user experience is maintained.

To use the `/.default` scope, append `/.default` to the resource identifier. For example: `https://graph.microsoft.com/.default`. If your resource ends with a slash (`/`), you should still append `/.default`, including the leading forward slash, resulting in a scope that has a double forward slash (`//`) in it.

You can read more information about using the "/.default" scope [here](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#the-default-scope)

### Supporting different WebView types & browsers

ADAL only supports UIWebView/WKWebView for iOS, and WebView for macOS. MSAL for iOS supports more options for displaying web content when requesting an authorization code; which can improve the user experience.

By default, MSAL uses [ASWebAuthenticationSession](https://developer.apple.com/documentation/authenticationservices/aswebauthenticationsession?language=objc), which is the web component Apple recommends for authentication on iOS 12+ devices. It provides Single Sign-On (SSO) benefits through cookie sharing between apps and web content.

You can choose to use a different web component depending on app requirements and the end-user experience you want. See [supported web view types](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Customizing-Browsers-and-WebViews) for more options.

When migrating from ADAL to MSAL, WKWebView provides the user experience most similar to ADAL on iOS. You're encouraged to migrate to ASWebAuthenticationSession if possible.

### Account management API differences

When you call the ADAL methods `acquireToken()` or `acquireTokenSilent()`, you receive an `ADUserInformation` object containing a list of claims from the `id_token` that represents the account being authenticated. Additionally, `ADUserInformation` returns a `userId` based on the `upn` claim. After initial interactive token acquisition, ADAL expects developer to provide `userId` in all silent calls.

ADAL didn't provide an API to retrieve known user identities. It relies on the app to save and manage those accounts.

MSAL provides a set of APIs that list all accounts known to MSAL without having doing token acquisition.

Like ADAL, MSAL returns account information that holds a list of claim from the `id_token` in its result object `MSALResult`.

MSAL provides a set of APIs to remove accounts, making the removed accounts inaccessible to the app. After the account is removed, later token acquisition calls will prompt the user to do interactive token acquisition. Account removal only applies to the client application that initiated it, and doesn't remove the account from the other apps running on the device or from the system browser. This ensures that the user continues to have a SSO experience on the device even after signing out of an individual app.

Additionally, MSAL also returns an account identifier that can be used to request a token silently later. However, the account identifier `homeAccountId` isn't displayable and you can't assume what format it is in nor should you try to interpret or parse it.

### Migrating the account cache

When migrating from ADAL, apps normally store ADAL's `userId`, which doesn't have the `homeAccountId` required by MSAL. As a onetime migration step, an app can query an MSAL account using ADAL's userId with the following API:

`- (nullable MSALAccount *)accountForUsername:(nonnull NSString *)username error:(NSError * _Nullable __autoreleasing * _Nullable)error;`

This API reads both MSAL's and ADAL's cache to find the account by ADAL userId (UPN).

If the account is found, the developer should use the account to do silent token acquisition. The first silent token acquisition will effectively upgrade the account, and the developer will get a MSAL compatible account identifier in the MSAL result (`homeAccountId`). After that, only `homeAccountId` should be used for  account lookups by using the following API:

`- (nullable MSALAccount *)accountForHomeAccountId:(nonnull NSString *)homeAccountId error:(NSError * _Nullable __autoreleasing * _Nullable)error;`

Although it's possible to continue using ADAL's `userId` for all operations in MSAL, since `userId` is based on UPN, it's subject to multiple limitations that result in a bad user experience. For example, if the UPN changes, the user has to sign in again. We recommend all apps use the non-displayable `homeAccountId` for all operations.

Read more about [cache state migration](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/SSO-between-ADAL-and-MSAL-based-apps).

### Token acquisition changes

MSAL introduces some token acquisition call changes:

* Like ADAL, `acquireTokenSilent` always results in a silent request.
* Unlike ADAL, `acquireToken` always results in user actionable UI either through the web view or the Microsoft Authenticator app. Depending on the SSO state inside webview/Microsoft Authenticator, the user may be prompted to enter their credentials.
* In ADAL, `acquireToken` with `AD_PROMPT_AUTO` first tries silent token acquisition, and only shows UI if the silent request fails. In MSAL, this logic can be achieved by first calling `acquireTokenSilent` and only calling `acquireToken` if silent acquisition fails. This allows developers to customize user experience before starting interactive token acquisition.

### Error handling differences

MSAL provides more clarity between errors that can be handled by your app and those that require intervention by the user. There are a limited number of errors developer should handle:

* `MSALErrorInteractionRequired`: The user must do an interactive request. This can be caused for various reasons such as an expired authentication session, conditional access policy has changed, a refresh token expired or was revoked, there are no valid tokens in the cache, and so on.
* `MSALErrorServerDeclinedScopes`: The request wasn't fully completed and some scopes weren't granted access. This can be caused by a user declining consent to one or more scopes.

See [Handling exceptions and errors using MSAL](msal-handling-exceptions.md) for more about MSAL error handling.

### Broker support

MSAL, starting with version 0.3.0, provides support for brokered authentication using the Microsoft Authenticator app. Microsoft Authenticator also enables support for conditional access scenarios. Examples of conditional access scenarios include device compliance policies that require the user to enroll the device through Intune or register with AAD to get a token. And Mobile Application Management (MAM) conditional access policies, which require proof of compliance before your app can get a token.

To enable broker for your application:

1. Register a broker compatible redirect URI format for the application. The broker compatible redirect URI format is `msauth.<app.bundle.id>://auth`. Replace `<app.bundle.id>` with your application's bundle ID. If you're migrating from ADAL and your application was already broker capable, there's nothing extra you need to do. Your previous redirect URI is fully compatible with MSAL, so you can skip to step 3.
2. Add your application's redirect URI scheme to your info.plist file. For the default MSAL redirect URI, the format is `msauth.<app.bundle.id>`. For example:

    ```xml
    <key>CFBundleURLSchemes</key>
    <array>
        <string>msauth.<app.bundle.id></string>
    </array>
    ```

3. Add following schemes to your app's Info.plist under LSApplicationQueriesSchemes:

    ```xml
    <key>LSApplicationQueriesSchemes</key>
    <array>
         <string>msauth</string>
         <string>msauthv2</string>
    </array>
    ```

4. Add the following to your AppDelegate.m file to handle callbacks:

    ```objc
    - (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options`
    {
        return [MSALPublicClientApplication handleMSALResponse:url sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
    }
    ```

For more information about AAD authentication brokers and MSAL, see [Microsoft Authentication brokers and Conditional Access](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Microsoft-Authentication-brokers-and-Conditional-Access).

### Business to business (B2B)

In ADAL, you create separate instances of `ADAuthenticationContext` for each tenant that the app requests tokens for. This is no longer a requirement in MSAL. In MSAL, you can create a single instance of `MSALPublicClientApplication` and use it for any AAD cloud and organization by specifying a different authority for acquireToken and acquireTokenSilent calls.

## SSO in partnership with other SDKs

MSAL for iOS and macOS can achieve SSO via a unified cache with following SDKs:

- ADAL Objective-C 2.7.x+
- MSAL.NET for Xamarin 2.4.x+
- ADAL.NET for Xamarin 4.4.x+

SSO is achieved via iOS keychain sharing and is only available between apps published from the same Apple Developer account.

SSO through iOS keychain sharing is the only silent SSO type.

MSAL for iOS and macOS also supports two other types of SSO:

- SSO through the web browser. MSAL supports `ASWebAuthenticationSession`, which provides SSO through cookies shared between other apps on the device and specifically the Safari browser.
- SSO through an Authentication broker. On an iOS device, Microsoft Authenticator acts as the Authentication broker. It can follow conditional access policies such as requiring a compliant device, and provides SSO for registered devices. MSAL SDKs starting with version 0.3.0 support a broker by default.

## Intune MAM SDK

The [Intune MAM SDK](https://docs.microsoft.com/intune/app-sdk-get-started) doesn't yet support MSAL for iOS and macOS. When it does, you'll need to update the version of the Intune MAM SDK to support MSAL.

## MSAL and ADAL in the same app

ADAL version 2.7.0, and above, can't coexist with MSAL in the same application. The main reason is because of the shared submodule common code. Because Objective-C doesn't support namespaces, if you add both ADAL and MSAL frameworks to your application, there will be two instances of the same class. And guarantee for which one gets picked at runtime. If both SDKs are using same version of the conflicting class, your app may still work. However, if it's a different version, your app might experience unexpected crashes that are difficult to diagnose.

Running ADAL and MSAL in the same production application isn't supported. However, if you're just testing and migrating your users from ADAL Objective-C to MSAL for iOS and macOS, you can continue using [ADAL Objective-C 2.6.10](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases/tag/2.6.10). It's the only version that works with MSAL in the same application. There will be no new feature updates for this ADAL version, so it should be only used for migration and testing purposes. Your app shouldn't rely on ADAL and MSAL coexistence long term. ADAL and MSAL coexistence isn't supported.

## Practical migration steps

### App registration migration

You don't need to change your existing AAD application to switch to MSAL and enable AAD accounts. However, if your ADAL-based application doesn't support brokered authentication, you'll need to register a new redirect URI for the application before you can switch to MSAL.

The redirect URI should be in this format: `msauth.<app.bundle.id>://auth`. Replace `<app.bundle.id>` with your application's bundle ID. Specify the redirect URI in the [Azure portal](https://aka.ms/MobileAppReg).

To support cert-based authentication, an additional redirect URI needs to be registered in your application and the Azure portal in the following format: `msauth://code/<broker-redirect-uri-in-url-encoded-form>`. For example, `msauth://code/msauth.com.microsoft.mybundleId%3A%2F%2Fauth`

We recommend all apps register both redirect URIs.

If you wish to add support for incremental consent, select the APIs and permissions your app is configured to request access to in your app registration under the **API permissions** tab.

If you're migrating from ADAL and want to support both AAD and MSA accounts, your existing application needs to be updated to support both. We don't recommend you update your existing production app to support both AAD and MSA right away. Instead, create another client ID that supports both AAD and MSA for testing, and after you've verified that all scenarios work, update the existing app.

### Add MSAL to your app

You can add MSAL SDK to your app using your preferred package management tool. See [detailed instructions here](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Installation).

### Update your app's Info.plist file

Add your application's redirect URI scheme to your info.plist file. For ADAL broker compatible apps, it should be there already. The default MSAL redirect URI will be in the format: `msauth.<app.bundle.id>`.  

```xml
<key>CFBundleURLSchemes</key>
<array>
    <string>msauth.<app.bundle.id></string>
</array>
```

Add following schemes to your app's Info.plist under `LSApplicationQueriesSchemes`. If you already have `msauth` declared, only add `msauthv2`:

```xml
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
     <string>msauthv2</string>
</array>
```

### Update your AppDelegate code

Add the following to your AppDelegate.m file:

```objc
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options`
{
    return [MSALPublicClientApplication handleMSALResponse:url sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
}
```

This allows MSAL to handle responses from the broker and web component.
This wasn't necessary in ADAL since it "swizzled" app delegate methods automatically. Adding it manually is less error prone and gives the application more control.

### Create MSALPublicClientApplication and switch to its acquireToken and acquireTokeSilent calls

You can create `MSALPublicClientApplication` using following code:

```objc
    NSError *error = nil;
    MSALPublicClientApplicationConfig *configuration = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];
    
    MSALPublicClientApplication *application =
    [[MSALPublicClientApplication alloc] initWithConfiguration:configuration
                                                         error:&error];
```

Then call the account management API to see if there are any accounts in the cache:

```objc
NSError *error = nil;
MSALAccount *account = [application accountForHomeAccountId:accountIdentifier error:&error];
```

or read all of the accounts:

```objc
NSError *error = nil;
NSArray<MSALAccount *> *accounts = [application allAccounts:&error];
```

If an account is found, call the MSAL `acquireTokenSilent` API:

```objc
    MSALSilentTokenParameters *silentParameters = [[MSALSilentTokenParameters alloc] initWithScopes:@[@"<your-resource-here>/.default"] account:account];
    
    [application acquireTokenSilentWithParameters:silentParameters
                                  completionBlock:^(MSALResult *result, NSError *error)
    {
        if (!error)
        {
            NSString *accessToken = result.accessToken;
            // Use your token
        }
        else
        {
            // Check the error
            if ([error.domain isEqual:MSALErrorDomain] && error.code == MSALErrorInteractionRequired)
            {
                // Interactive auth will be required
            }
            
            // Other errors may require trying again later, or reporting authentication problems to the user
        }
    }];
```

## Next steps

Learn more about [Authentication flows and application scenarios](authentication-flows-app-scenarios.md)
