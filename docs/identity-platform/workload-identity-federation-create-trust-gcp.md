---
title: Access Azure resources from Google Cloud without credentials
titleSuffix: Microsoft identity platform
description: Access Azure AD protected resources from a service running in Google Cloud without using secrets or certificates.  Use workload identity federation to set up a trust relationship between an app in Azure AD and an identity in Google Cloud.  
services: active-directory
author: rwike77
manager: CelesteDG

ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 12/16/2021
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: udayh
#Customer intent: As an application developer, I want to create a trust relationship with Google Cloud so my service can access Azure AD protected resources without managing secrets.
---

# Access Azure AD protected resources from an app in Google Cloud (preview)

Software workloads running in Google Cloud need an Azure Active Directory (Azure AD) application to authenticate and access Azure AD protected resources. A common practice is to configure that application with credentials (a secret or certificate). The credentials are used by a Google Cloud workload to request an access token from Microsoft identity platform. These credentials pose a security risk and have to be stored securely and rotated regularly. You also run the risk of service downtime if the credentials expire.

[Workload identity federation](workload-identity-federation.md) allows you to access Azure AD protected resources from services running in Google Cloud without needing to manage secrets. Instead, you can configure your Azure AD application to trust a token issued by Google and exchange it for an access token from Microsoft identity platform.

## Create an app registration in Azure AD

[Create an app registration](quickstart-register-app.md) in Azure AD. Grant your app the permissions necessary to access the Azure AD protected resources targeted by your software workload running in Google Cloud.

Take note of the *object ID* of the app (not the application (client) ID) which you need in the following steps.  Go to the [list of registered applications](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) in the Azure portal, select your app registration, and find the **Object ID** in **Overview**->**Essentials**.

## Set up an identity in Google Cloud

You need an identity in Google Cloud that can be associated with your Azure AD application. A [service account](https://cloud.google.com/iam/docs/service-accounts), for example, used by an application or compute workload.  You can either use the default service account of your Cloud project or create a dedicated service account.

Each service account has a unique ID. When you visit the **IAM & Admin** page in the Google Cloud console, click on **Service Accounts**. Select the service account you plan to use, and copy its **Unique ID**.

:::image type="content" source="media/workload-identity-federation-create-trust-gcp/service-account-details.png" alt-text="Shows a screen shot of the Service Accounts page" border="false":::

Tokens issued by Google to the service account will have this unique ID as the *subject* claim.

The *issuer* claim in the tokens will be `https://accounts.google.com`.

You need these claim values to configure a trust relationship with an Azure AD application, which allows your application to trust tokens issued by Google to your service account.

## Configure an Azure AD app to trust Google Cloud

Configure a federated identity credential on your Azure AD application to set up the trust relationship. You can add up to 20 of these trusts to each Azure AD application.  See this article for steps to [Create a federated identity credential](workload-identity-federation-create-trust.md#create-a-federated-identity-credential),

The most important fields for creating the federated identity credential are:

- *object ID*: the object ID of the app (not the application (client) ID) you previously registered in Azure AD.
- *subject*: must match the `sub` claim in the token issued by another identity provider, such as Google. This is the Unique ID of the service account you plan to use.
- *issuer*: must match the `iss` claim in the token issued by the identity provider. A URL that complies with the [OIDC Discovery spec](https://openid.net/specs/openid-connect-discovery-1_0.html). Azure AD uses this issuer URL to fetch the keys that are necessary to validate the token. In the case of Google Cloud, the issuer is `https://accounts.google.com`.
- *audiences*: must match the `aud` claim in the token. For security reasons, you should pick a value that is unique for tokens meant for Azure AD. The Microsoft recommended value is “api://AzureADTokenExchange”.

## Exchange a Google token for an access token

Now that you have configured the Azure AD application to trust the Google service account, you are ready to get a token from Google and exchange it for an access token from Microsoft identity platform.

### Get an ID token for your Google service account

As mentioned earlier, Google cloud resources such as App Engine automatically use the default service account of your Cloud project. You can also configure the app to use a different service account when you deploy your service. Your service can [request an ID token](https://cloud.google.com/compute/docs/instances/verifying-instance-identity#request_signature) for that service account from the metadata server that handles such requests. With this approach, you don't need any keys for your service account: these are all managed by Google.

Here’s an example in Node.js of how to request an ID token from the Google metadata server:

```nodejs
async function googleIDToken() {
    const headers = new Headers();
    const endpoint="http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/identity?audience=api://AzureADTokenExchange";
    return fetch(endpoint, {method: "GET", headers: headers});
}
```

> [!IMPORTANT]
> The *audience* here needs to match the *audiences* value you configured on your Azure AD application when [creating the federated identity credential](#configure-an-Azure-AD-app-to-trust-google-cloud).

### Exchange the identity token for an Azure AD access token

Now that you have an identity token from Google, you can exchange it for an access token from Microsoft identity platform. Use the [Microsoft Authentication Library (MSAL)](msal-overview.md) to pass the Google token as a client assertion. The following MSAL versions support client assertions:
- [MSAL Go (Preview)](https://github.com/AzureAD/microsoft-authentication-library-for-go)
- [MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node)
- [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)
- [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)
- [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java)

Using MSAL, you write a token class (implementing the `TokenCredential` interface) exchange the ID token.  The token class is used to with different client libraries to access Azure AD protected resources. 

The following Node.js sample code snippet implements the `TokenCredential` interface, gets an ID token from Google (using the `googleIDToken` method previously defined), and exchanges the ID token for an access token.

```nodejs
const msal = require("@azure/msal-node");
import {TokenCredential, GetTokenOptions, AccessToken} from "@azure/core-auth"

class ClientAssertionCredential implements TokenCredential {

    constructor(clientID:string, tenantID:string, aadAuthority:string) {
        this.clientID = clientID;
        this.tenantID = tenantID;
        this.aadAuthority = aadAuthority;  // https://login.microsoftonline.com/
    }
    
    async getToken(scope: string | string[], _options?: GetTokenOptions):Promise<AccessToken> {

        var scopes:string[] = [];           
        // write code here to update the scopes array, based on scope parameter

        // Get the ID token from Google.
        return googleIDToken() // calling this directly just for clarity, 
                               // this should be a callback
        // pass this as a client assertion to the confidential client app
        .then((clientAssertion:any)=> {
            let msalApp = new msal.ConfidentialClientApplication({
                auth: {
                    clientId: this.clientID,
                    authority: this.aadAuthority + this.tenantID,
                    clientAssertion: clientAssertion,
                }
            });
            return msalApp.acquireTokenByClientCredential({ scopes })
        })
        .then(function(aadToken) {
            // return in form expected by TokenCredential.getToken
            return({ 
                token: aadToken.accessToken,
                expiresOnTimestamp: aadToken.expiresOn.getTime()
            });
        })
        .catch(function(error) {
            // error stuff
        });
    }
}
```

Here’s an example of how you can access Azure Blob storage using `ClientAssertionCredential` token class and the Azure Blob Storage client library. When you make requests to the `BlobServiceClient` to access storage, the `BlobServiceClient` calls the `getToken` method on the `ClientAssertionCredential` object get a fresh ID token and exchange it for an access token.  

The following Node.js example initializes a new `ClientAssertionCredential` object and then creates a new `BlobServiceClient` object.

```nodejs
const { BlobServiceClient } = require("@azure/storage-blob");

const tokenCredential =  new ClientAssertionCredential(clientID,
                                                tenantID,
                                                aadAuthority);
                                             
const blobClient = new BlobServiceClient(blobUrl, tokenCredential);
```

## Next steps
