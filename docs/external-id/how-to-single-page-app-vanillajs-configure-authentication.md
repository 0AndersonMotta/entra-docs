---
title: Sign in users to a Vanilla JS Single-page application using Microsoft Entra - Configure application for authentication
description: Learn how to configure vanilla JavaScript single-page app (SPA) for authentication and authorization with Azure AD CIAM.

services: active-directory
author: OwenRichards1
manager: CelesteDG

ms.author: owenrichards
ms.service: active-directory
ms.workload: identity
ms.subservice: ciam
ms.topic: how-to
ms.date: 04/26/2023
ms.custom: developer

#Customer intent: As a developer, I want to learn how to configure vanilla JavaScript single-page app (SPA) to sign in and sign out users with my CIAM tenant.
---

# Create components for authentication and authorization

In the previous article., you created a vanilla JavaScript (JS) single-page application (SPA) and a server to host it. In this article, you'll configure the application to authenticate and authorize users to access protected resources. Authentication and authorization are handled by the [Microsoft Authentication Library for JavaScript (MSAL.js)](/javascript/api/overview/). The library is used to authenticate users and acquire access tokens from Azure AD CIAM.

In this article:
> [!div class="checklist"]
>
> * Create a configuration file for authentication
> * Create a sign-in and sign-out button
> * Create a sign-in and sign-out function
> * Create a function to get an access token and call a protected API

## Prerequisites

* Completion of the prerequisites and steps in [Prepare a Single-page application for authentication](how-to-single-page-app-vanillajs-prepare-app.md).

## Creating the authentication configuration file

The application uses the [Implicit Grant Flow](../develop\v2-oauth2-implicit-grant-flow.md) to authenticate users. The Implicit Grant Flow is a browser-based flow that does not require a back-end server. The flow redirects the user to the Azure AD CIAM sign-in page, where the user signs in and consents to the permissions that are being requested by the application. The purpose of *authConfig.js* is to configure the authentication flow.

1. In your IDE, create a new folder and name it **public**
1. In the *public* folder, create a new file and name it *authConfig.js*.
1. Open *authConfig.js* and add the following code snippet:

    ```javascript
    /**
     * Configuration object to be passed to MSAL instance on creation. 
     * For a full list of MSAL.js configuration parameters, visit:
     * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/configuration.md 
     */
    const msalConfig = {
        auth: {
            clientId: 'Enter_the_Application_Id_Here', // This is the ONLY mandatory field that you need to supply.
            authority: 'https://Enter_the_Tenant_Name_Here.ciamlogin.com/', // Replace "Enter_the_Tenant_Name_Here" with your tenant name
            redirectUri: '/', // You must register this URI on Azure Portal/App Registration. Defaults to window.location.href e.g. http://localhost:3000/
            navigateToLoginRequestUrl: true, // If "true", will navigate back to the original request location before processing the auth code response.
        },
        cache: {
            cacheLocation: 'sessionStorage', // Configures cache location. "sessionStorage" is more secure, but "localStorage" gives you SSO.
            storeAuthStateInCookie: false, // set this to true if you have to support IE
        },
        system: {
            loggerOptions: {
                loggerCallback: (level, message, containsPii) => {
                    if (containsPii) {
                        return;
                    }
                    switch (level) {
                        case msal.LogLevel.Error:
                            console.error(message);
                            return;
                        case msal.LogLevel.Info:
                            console.info(message);
                            return;
                        case msal.LogLevel.Verbose:
                            console.debug(message);
                            return;
                        case msal.LogLevel.Warning:
                            console.warn(message);
                            return;
                    }
                },
            },
        },
    };
    
    /**
     * Scopes you add here will be prompted for user consent during sign-in.
     * By default, MSAL.js will add OIDC scopes (openid, profile, email) to any login request.
     * For more information about OIDC scopes, visit: 
     * https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-permissions-and-consent#openid-connect-scopes
     */
    const loginRequest = {
        scopes: [],
        extraQueryParameters: {
            dc: "ESTS-PUB-EUS-AZ1-FD000-TEST1"
        }
    };
    
    /**
     * An optional silentRequest object can be used to achieve silent SSO
     * between applications by providing a "login_hint" property.
     */
    
    // const silentRequest = {
    //   scopes: ["openid", "profile"],
    //   loginHint: "example@domain.net"
    // };
    
    // exporting config object for jest
    if (typeof exports !== 'undefined') {
        module.exports = {
            msalConfig: msalConfig,
            loginRequest: loginRequest,
        };
}
     ```

1. Replace the following values with the values from the Admin center.
    * `clientId` - The identifier of the application, also referred to as the client. Replace `Enter_the_Application_Id_Here` with the **Application (client) ID** value that was recorded earlier from the overview page of the registered application.
    * `authority` - This is composed of two parts:
        * The *Instance* is endpoint of the cloud provider. Check with the different available endpoints in [National clouds](../develop\authentication-national-cloud.md#azure-ad-authentication-endpoints).
        * The *Tenant ID* is the identifier of the tenant where the application is registered. Replace the `_Enter_the_Tenant_Info_Here` with the **Directory (tenant) ID** value that was recorded earlier from the overview page of the registered application.
1. Save the file.

## Creating the redirection file

A redirection file is required to handle the response from the Azure AD CIAM sign-in page. The redirection file is used to extract the access token from the URL fragment and use it to call the protected API.

1. Right-click the *public* folder and select **New File**. Name the file *authRedirect.js*.
1. Open *authRedirect.js* and add the following code snippet:

    ```javascript
    // Create the main myMSALObj instance
    // configuration parameters are located at authConfig.js
    const myMSALObj = new msal.PublicClientApplication(msalConfig);
    
    let username = "";
    
    /**
     * A promise handler needs to be registered for handling the
     * response returned from redirect flow. For more information, visit:
     * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/initialization.md#redirect-apis
     */
    myMSALObj.handleRedirectPromise()
        .then(handleResponse)
        .catch((error) => {
            console.error(error);
        });
    
    function selectAccount() {
    
        /**
         * See here for more info on account retrieval: 
         * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-common/docs/Accounts.md
         */
    
        const currentAccounts = myMSALObj.getAllAccounts();
    
        if (!currentAccounts) {
            return;
        } else if (currentAccounts.length > 1) {
            // Add your account choosing logic here
            console.warn("Multiple accounts detected.");
        } else if (currentAccounts.length === 1) {
            welcomeUser(currentAccounts[0].username);
            updateTable(currentAccounts[0]);
        }
    }
    
    function handleResponse(response) {
    
        /**
         * To see the full list of response object properties, visit:
         * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/request-response-object.md#response
         */
    
        if (response !== null) {
            welcomeUser(response.account.username);
            updateTable(response.account);
        } else {
            selectAccount();
    
            /**
             * If you already have a session that exists with the authentication server, you can use the ssoSilent() API
             * to make request for tokens without interaction, by providing a "login_hint" property. To try this, comment the 
             * line above and uncomment the section below.
             */
    
            // myMSALObj.ssoSilent(silentRequest).
            //     then((response) => {
            //         welcomeUser(response.account.username);
            //         updateTable(response.account);
            //     }).catch(error => {
            //         console.error("Silent Error: " + error);
            //         if (error instanceof msal.InteractionRequiredAuthError) {
            //             signIn();
            //         }
            //     });
        }
    }
    
    function signIn() {
    
        /**
         * You can pass a custom request object below. This will override the initial configuration. For more information, visit:
         * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/request-response-object.md#request
         */
    
        myMSALObj.loginRedirect(loginRequest);
    }
    
    function signOut() {
    
        /**
         * You can pass a custom request object below. This will override the initial configuration. For more information, visit:
         * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/request-response-object.md#request
         */
    
        // Choose which account to logout from by passing a username.
        const logoutRequest = {
            account: myMSALObj.getAccountByUsername(username),
            postLogoutRedirectUri: '/signout', // remove this line if you would like navigate to index page after logout.
    
        };
    
        myMSALObj.logoutRedirect(logoutRequest);
    }
    ```

1. Save the file.

## Creating the authPopup.js file

The application uses *authPopup.js* to  handle the authentication flow when the user signs in using the pop-up window. The pop-up window is used when the user is already signed in to Azure AD CIAM and the application needs to get an access token for a different resource. 

1. Right-click the **public** folder and select **New File**. Name the file *authPopup.js*.
1. Open *authPopup.js* and add the following code snippet:

   ```javascript
    // Create the main myMSALObj instance
    // configuration parameters are located at authConfig.js
    const myMSALObj = new msal.PublicClientApplication(msalConfig);
    
    let username = "";
    
    /**
     * A promise handler needs to be registered for handling the
     * response returned from redirect flow. For more information, visit:
     * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/initialization.md#redirect-apis
     */
    myMSALObj.handleRedirectPromise()
        .then(handleResponse)
        .catch((error) => {
            console.error(error);
        });
    
    function selectAccount() {
    
        /**
         * See here for more info on account retrieval: 
         * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-common/docs/Accounts.md
         */
    
        const currentAccounts = myMSALObj.getAllAccounts();
    
        if (!currentAccounts) {
            return;
        } else if (currentAccounts.length > 1) {
            // Add your account choosing logic here
            console.warn("Multiple accounts detected.");
        } else if (currentAccounts.length === 1) {
            username = currentAccounts[0].username;
            welcomeUser(username);
            updateTable();
        }
    }
    
    function handleResponse(response) {
    
        /**
         * To see the full list of response object properties, visit:
         * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/request-response-object.md#response
         */
    
        if (response !== null) {
            username = response.account.username;
            welcomeUser(username);
            updateTable();
        } else {
            selectAccount();
    
            /**
             * If you already have a session that exists with the authentication server, you can use the ssoSilent() API
             * to make request for tokens without interaction, by providing a "login_hint" property. To try this, comment the 
             * line above and uncomment the section below.
             */
    
            // myMSALObj.ssoSilent(silentRequest).
            //     then(() => {
            //         const currentAccounts = myMSALObj.getAllAccounts();
            //         username = currentAccounts[0].username;
            //         welcomeUser(username);
            //         updateTable();
            //     }).catch(error => {
            //         console.error("Silent Error: " + error);
            //         if (error instanceof msal.InteractionRequiredAuthError) {
            //             signIn();
            //         }
            //     });
        }
    }
    
    function signIn() {
    
        /**
         * You can pass a custom request object below. This will override the initial configuration. For more information, visit:
         * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/request-response-object.md#request
         */
    
        myMSALObj.loginRedirect(loginRequest);
    }
    
    function signOut() {
    
        /**
         * You can pass a custom request object below. This will override the initial configuration. For more information, visit:
         * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/request-response-object.md#request
         */
    
        // Choose which account to logout from by passing a username.
        const logoutRequest = {
            account: myMSALObj.getAccountByUsername(username),
            postLogoutRedirectUri: '/signout', // remove this line if you would like navigate to index page after logout.
    
        };
    
        myMSALObj.logoutRedirect(logoutRequest);
    }
    ```

1. Save the file.

## Next steps

> [!div class="nextstepaction"]
> [Configure a Single-page application User Interface and Sign-In](how-to-single-page-app-vanillajs-sign-in-sign-out.md)
