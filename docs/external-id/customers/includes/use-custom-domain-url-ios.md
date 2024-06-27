---
author: henrymbuguakiarie
ms.service: entra-external-id
ms.subservice: customers
ms.topic: include
ms.date: 06/27/2024
ms.author: henrymbuguakiarie
ms.manager: mwongerapk
---

### Use custom domain URL (Optional)

Use a custom domain to fully brand the authentication URL. From a user perspective, users remain on your domain during the authentication process, rather than being redirected to *ciamlogin.com* domain name.

Use the following steps to use a custom domain:

1. Use the steps in [Enable custom URL domains for apps in external tenants](../how-to-custom-url-domain.md) to enable custom domain URL for your external tenant.

1. Open *Configuration.swift* file:
    1. Update the value of the `kAuthority` property to *https://Enter_the_Custom_Domain_Here/Enter_the_Tenant_ID_Here*. Replace `Enter_the_Custom_Domain_Here` with your custom domain URL and `Enter_the_Tenant_ID_Here` with your tenant ID. If you don't have your tenant ID, learn how to [read your tenant details](../how-to-create-external-tenant-portal.md#get-the-external-tenant-details). 
    
After you make the changes to your *Configuration.swift* file, if your custom domain URL is *login.contoso.com*, and your tenant ID is *aaaabbbb-0000-cccc-1111-dddd2222eeee*, then your file should look similar to the following snippet:

```swift
    import Foundation

    @objcMembers
    class Configuration {
        static let kTenantSubdomain = "Enter_the_Tenant_Subdomain_Here"
        
        // Update the below to your client ID you received in the portal.
        static let kClientID = "Enter_the_Application_Id_Here"
        static let kRedirectUri = "Enter_the_Redirect_URI_Here"
        static let kProtectedAPIEndpoint = "Enter_the_Protected_API_Full_URL_Here"
        static let kScopes = ["Enter_the_Protected_API_Scopes_Here"]
        
        static let kAuthority = "https://login.contoso.com/aaaabbbb-0000-cccc-1111-dddd2222eeee"
    
    }
```