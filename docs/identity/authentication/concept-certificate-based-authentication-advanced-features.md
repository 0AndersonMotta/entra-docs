---
title:  Advanced features for Azure Active Directory certificate-based authentication
description: Learn about advanced features for Azure Active Directory certificate-based authentication

services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 09/27/2022


ms.author: justinha
author: vimrang
manager: amycolannino
ms.reviewer: vimrang

ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
---

# Advanced features for Azure AD certificate-based authentication (CBA)


## Using one certificate for multiple accounts 

certificateUserIds attribute is a unique constraint multi valued attribute. An admin can use multiple bindings and add appropriate values into the multi values and achieve authenticating into multiple accounts using one certificate.

The Azure AD user object lookup happens with the Azure AD UPN the user enters ( on in case of Windows login the Azure AD UPN that windows send) and the username bindings is used to validate the certificate to successfully authenticate the user. Also, if the admin has configured multiple bindings, Azure AD will evaluate all the bindings until a successful authentication, or all the bindings are evaluated. This helps the admin use specific configurations to achieve one certificate to multiple accounts.

By default, Azure AD CBA has a single user binding configured. The Principal Name attribute in the Subject Alternative Name of a certificate presented to Azure AD. Some administrators require the ability for Azure AD to be able to map a single certificate to multiple Azure AD accounts. We refer to this as 1:M mapping. Azure AD CBA supports this implementation via administrators adding additional mapping methods to the policy. 

An example of this would be a developer use case. In this example Bob has a regular productivity account that is used to accomplish his everyday tasks and a developer account to use when he is doing task related to his developer job roles. The organization issues a single high assurance certificate to Bob and wishes for him to be able to use this same certificate for both his productivity and developer accounts. 

This 1:M implementation could be implemented in Azure AD CBA by configuring the policy as follows. 

|Certificate Information | Principal Name in SAN = Bob.Smith@contoso.com, Certificate's Subject Key Identifier (SKI) = 89b0f468c1abea65ec22f0a882b8fda6fdd6750p |
|Bobs Productivity Account| AAD User Principal Name = Bob.Smith@contoso.com, certificateUserIDs = Empty|
|Bobs Developer Account| AAD UserPrincipalName = Bob.Smith-dev@contoso.com, certificateUserIds = x509:\<SKI\>89b0f468c1abea65ec22f0a882b8fda6fdd6750p |
|Tenant User Binding Policy | Priority 1 Principal Name in SAN -> Azure AD UPN , Priority 2 Certificate SKI -> certificateUserIds |


**Certificate Information**

Principal Name in SAN = Bob.Smith@contoso.com
Certificate Subject Key Identifier (SKI) = 89b0f468c1abea65ec22f0a882b8fda6fdd6750p

**Bobs Productivity Account**

AAD User Principal Name = Bob.Smith@contoso.com
certificateUserIDs = Empty

**Bobs Developer Account**

AAD UserPrincipalName = Bob.Smith-dev@contoso.com
certificateUserIds = x509:\<SKI\>89b0f468c1abea65ec22f0a882b8fda6fdd6750p

**Tenant User Binding Policy**
 
Priority 1 Principal Name in SAN -> Azure AD UPN 
Priority 2 Certificate SKI -> certificateUserIds

The above configuration would allow the same certificate to be used by Bob for both his productivity and developer account.

**How can I scope 1:M for only specific group of users?**
 
If the tenant Admin wishes for that certificate to ONLY be used for Bob productivity account and block the use of the certificate on other accounts, they would configure Bob's productivity account to hold all of the values available in the username mapping policy. 

In this example to lock Bobs certificate to only Bob's productivity account as certificateUserIds attribute has unique constraint and no other user account can have the same values.
 
**Bobs Productivity Account**
 
AAD User Principal Name = Bob.Smith@Contoso.com 
certificateUserIDs = [ x509:\<PN\>Bob.Smith@Contoso.com , x509:\<SKI\>89b0f468c1abea65ec22f0a882b8fda6fdd6750p]

## External identities support

Today a B2B user cannot meet MFA with AAD CBA in the resource tenant. The only way to accomplish MFA in resource tenant with CBA is for user to perform CBA in home tenant and resource tenant should set up cross tenant settings to trust MFA from home tenant.

Enable "Trust multi-factor authentication from Azure AD tenants". Please visit [Configure B2B collaboration cross-tenant access](https://learn.microsoft.com/en-us/azure/active-directory/external-identities/cross-tenant-access-settings-b2b-collaboration#to-change-inbound-trust-settings-for-mfa-and-device-claims) for more information.

## Next steps

- [Overview of Azure AD CBA](concept-certificate-based-authentication.md)
- [Technical deep dive for Azure AD CBA](concept-certificate-based-authentication-technical-deep-dive.md)
- [Limitations with CBA](concept-certificate-based-authentication-limitations.md)
- [How to configure CBA](how-to-certificate-based-authentication.md)
- [Windows SmartCard logon using Azure AD CBA](concept-certificate-based-authentication-smartcard.md)
- [Azure AD CBA on mobile devices (Android and iOS)](concept-certificate-based-authentication-mobile.md)
- [Sync rules for certificateUserIds](concept-certificate-based-authentication-certificateuserids.md)
- [How to migrate federated users](concept-certificate-based-authentication-migration.md)
- [FAQ](certificate-based-authentication-faq.yml)
