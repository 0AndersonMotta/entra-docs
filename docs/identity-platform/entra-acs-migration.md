---
title: Migrating from Azure Access Control Service (ACS)
description: Options for moving apps & services off of Azure Access Control Service | Microsoft Azure
services: active-directory
documentationcenter: dev-center-name
author: dastrock
manager: mbaldwin
editor: ''

ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2017
ms.author: dstrockis
---


# Migrating from Azure Access Control Service (ACS)

Microsoft Azure Active Directory Access Control (also known as Access Control Service or ACS) is being retired on TODO: insert date here.  Applications & services currently using ACS will need to fully migrate to a new cloud authentication service before this date. This document describes our recommendations for current customers as they plan to deprecate their use of ACS. It will be updated over time as we work with customers individually and discover effective migration patterns. If you are not currently using ACS, you do not need to take any action.


## Brief ACS Overview

ACS is a cloud authentication service that provides an easy way of authenticating and authorizing users to gain access to your web applications and services while allowing many features of authentication and authorization to be factored out of your code. It is used primarily by developers & architects of .NET clients, ASP.NET web applications & WCF web services.

The use cases for ACS can be broken down into three main categories:

- Authenticating to certain Microsoft cloud services, including Azure Service Bus, Dynamics CRM, and others. Client applications could obtain tokens from ACS that can be used to authenticate to these services to perform various actions.
- Adding authentication to web applications, both of the custom built variety & of the pre-packaged variety (such as Sharepoint). Using ACS "passive" authentication, web applications could support login with accounts from Google, Facebook, Yahoo, Microsoft account (Live ID), Azure Active Directory, and ADFS.
- Securing custom built web services with tokens issued by ACS. Using "active" authentication, web services could ensure that they only allow access from known clients that have authenticated with ACS.

Each of these use cases and their recommended migration strategies will be discussed in the sections below. In the vast majority of cases, significant code changes will be required to migrate existing apps & services to newer technologies. It is recommended that you begin planning & executing your migration immediately to avoid any potential for outages or downtime.

Architecturally, ACS is entirely comprised of the following components:

- A secure token service (STS), which recieves authentication requests & issues security tokens in return.
- Two management portals:
    - The classic Azure portal, which is used for creating, managing, and deleting ACS namespaces.
    - A dedicated ACS management portal, which is used for customizing & configuring the behavior of ACS.
- A management service, which can be used to automate the functions of the above portals.
- A token tranformation rule engine, which can be used to define complex logic for manipulating the output format of tokens issued by ACS.

To use these components, you must create one or more ACS **namespaces**. A namespace is a dedicated instance of ACS that your applications & services communicate with; it is isolated from all other ACS customers, who use their own namespaces. A namespace in ACS will have a dedicated URL, like:

```HTTP
https://mynamespace.accesscontrol.windows.net
```

All communication with the STS and management operations are done at this URL, with different paths for different purposes. For additional details on ACS, please refer to [this archived documentation on MSDN](https://msdn.microsoft.com/en-us/library/hh147631.aspx).

## Retirement Schedule

As of October 2017, all ACS components are fully supported & operational. The only restriction is that [new ACS namespaces cannot be created via the classic Azure Portal](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

The timeline for deprecation of these components will follow this rough estimate:

- **November 2017**:  The Azure AD admin experience in the classic Azure portal [will be retired](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). At this point, namespace management for ACS will be made available at a new, dedicated URL. This is to allow you to view your existing namespaces, enable/disable them, and delete them entirely if you wish. The exact URL will be provided here in November.
- **April 2018**: ACS namespace management will no longer be available at this dedicated URL. At this point in time, you will not be able to disable/enable, delete, or enumerate your ACS namespaces. The ACS management portal, however, will be fully functional and located at `https://{namespace}.accesscontrol.windows.net`. All other components of ACS will continue to operate normally as well.
- **October 2018**: All ACS components will be shut down permanently. This includes the ACS management portal, the management service, STS, and token tranformation rule engine. At this point, any requests sent to ACS (located at `*.accesscontrol.windows.net`) will fail. You should have migrated all existing apps & services to other technologies well before this time period.


## Migration Strategies

The following sections will describe high level reccommendations for migrating off of ACS to other Microsoft technologies.

### Clients of Microsoft cloud services

Each of the Microsoft cloud services that accept tokens issued by ACS now supports at least one alternate form of authentication. The correct authentication mechanism will vary for each service, so we reccommend referring to the specific documentation of each service for official guidance. For convenience, we have collected the following information:

- Azure Service Bus: [Migrate to Shared Access Signatures](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas)
- Azure Relay: [Migrate to Shared Access Signatures](https://docs.microsoft.com/en-us/azure/service-bus-relay/relay-migrate-acs-sas)
- Azure Cache: Retired? TODO
- Azure Data Market: Retired? TODO
- BizTalk: To Retire? TODO
- Dynamics CRM: Migrate to SAS? TODO
- Azure Media Services: TODO
- Azure Backup: TODO
- Others: TODO

### Web applications using passive authentication

For web applications using ACS for user authentication, ACS provided the following features & capabilities to developers & architects of web applciations:

- Deep integration with Windows Identity Foundation (WIF).
- Federation with Google, Facebook, Yahoo, Microsoft account (Live ID), Azure Active Directory, and ADFS.
- Support for the following authentication protocols: OAuth 2.0 draft 13, Ws-Trust, and Ws-Federation.
- Support for the following token formats: JSON web token (JWT), SAML 1.1, SAML 2.0, and Simple web token (SWT).
- A home-realm discovery experience, integrated into WIF, that allowed users to pick the type of account they use to sign in. This experience was hosted by the web application and fully customizable.
- Token tranformation that allowed rich customization of the claims received by the web application from ACS, including:
    - passing through claims from identity providers
    - adding additional custom claims
    - simple if-then logic to issue claims under certain conditions

Unfortunately, there is not one service that provides all of these equivalent capabilities.  Our recommendation is to evaluate which capabilities of ACS you need, and then choose between using [**Azure Active Directory**](https://azure.microsoft.com/develop/identity/signin/) (Azure AD) or [**Azure AD B2C**](https://azure.microsoft.com/services/active-directory-b2c/).

#### Azure Active Directory

One path to consider is integrating your apps & services directly with Azure Active Directory. Azure AD is the cloud based identity provider for Microsoft work & school accounts - it is the identity provider for Office 365, Azure, and much more. It provides similar federated authentication capabilities to ACS, but does not support all ACS features. The primary example is federation with social identity providers, such as Facebook, Google, and Yahoo. If your users sign in with these types of credentials, Azure AD is not for you. It also does not necessarily support the exact same authentication protocols as ACS - while both ACS & Azure AD support OAuth, for instance, there will be subtle differences between each implementation that will require you to modify code as part of migration.

Azure AD does however provide several potential advantages to customers of ACS. It natively supports Microsoft work & school accounts hosted in the cloud, which are commonly used by ACS customers.  An Azure AD tenant can also be federated to one or more instances of on-prem Active Directory via ADFS, allowing your app to authenticate both cloud-based users and users hosted on premises.  It also supports the Ws-Federation protocol, which makes it relatively straightforward to integrate with a web application using Windows Identity Foundation (WIF).

The following table compares the features of ACS relevant to web applications with those available in Azure AD. At an extremely high level, **Azure Active Directory is probably the proper choice for your migration if you only let users sign-in with their Microsoft work & school accounts** (also known as "Windows Azure AD" accounts in ACS).

| Capability | ACS Support | Azure AD Support |
| ---------- | ----------- | ---------------- |
| **Types of Accounts** | | |
| Microsoft work & school accounts | Supported | Supported |
| Accounts from Windows Server Active Directory & ADFS | Supported via federation with an Azure AD tenant <br /> Supported via direct federation with ADFS | Only supported via federation with an Azure AD tenant | 
| Accounts from other enterprise identity managment systems | Possible via federation with an Azure AD tenant <br />Supported via direct federation | Possible via federation with an Azure AD tenant |
| Microsoft accounts for personal use (Windows Live ID) | Supported | Supported via Azure AD's v2.0 OAuth protocol, but not over any other protocols. | 
| Facebook, Google, Yahoo accounts | Supported | Not supported whatsoever. |
| **Protocols & SDK Compatibility** | | |
| Windows Identity Foundation (WIF) | Supported | Supported, but limited instructions available. |
| Ws-Federation | Supported | Supported |
| OAuth 2.0 | Support for Draft 13 | Support for RFC 6749, the most modern specification. |
| Ws-Trust | Supported | Not supported |
| **Token Formats** | | |
| Json Web Tokens (JWTs) | Supported In Beta | Supported |
| SAML 1.1 tokens | Supported | Supported |
| SAML 2.0 tokens | Supported | Supported |
| Simple Web Tokens (SWT) | Supported | Not Supported |
| **Customizations** | | |
| Customizeable home realm discovery/account picking UI | Downloadable code that can be incorporated into apps | Not supported |
| Upload custom token signing certificates | Supported | Supported |
| Customize claims in tokens | Passthrough input claims from identity providers<br />Get access token from identity provider as a claim<br />Issue output claims based on values of input claims<br />Issue output claims with constant values | Cannot passthrough claims from federated identity providers<br />Cannot get access token from identity provider as a claim<br />Cannot issue output claims based on values of input claims<br />Can issue output claims with constant values<br />Can issue output claims based on properties of users sync'd to Azure AD |
| **Automation** | | |
| Automate configuration/management tasks | Supported via ACS Management Service | Supported via Microsoft Graph & Azure AD Graph API. |

#### Azure AD B2C

The other migration path to consider is Azure AD B2C.  B2C is a cloud authentication service that, similar to ACS, allows developers to outsource their authentication and identity management logic to a cloud service.  It is a paid service (with free & premium tiers) designed for consumer facing applications that can have up to millions of active users.

Like ACS, one of the most attractive features of B2C is that is supports many different types of accounts. With B2C, you can sign-in users with their Facebook, Google, Microsoft, LinkedIn, GitHub, Yahoo accounts, and more. B2C additionally supports "local accounts", or username & passwords that users create specifically for your application. Azure AD B2C also provides rich extensibilty that you can use to customize your login flows. It does not, however, support the breadth of authentication protocols & token formats that ACS customers may require. It also cannot be used to get tokens & query additional information about the user from the identity provider, Microsoft or otherwise.

The following table compares the features of ACS relevant to web applications with those available in Azure AD B2C. At an extremely high level, **Azure AD B2C is probably the right choice for your migration if your application is consumer facing, or if it supports many different types of accounts.**

| Capability | ACS Support | Azure AD Support |
| ---------- | ----------- | ---------------- |
| **Types of Accounts** | | |
| Microsoft work & school accounts | Supported | Supported via advanced policies  |
| Accounts from Windows Server Active Directory & ADFS | Supported via direct federation with ADFS | Supported via advanced policies |
| Accounts from other enterprise identity managment systems | Supported via direct federation | Supported via advanced policies |
| Microsoft accounts for personal use (Windows Live ID) | Supported | Supported | 
| Facebook, Google, Yahoo accounts | Supported | Supported |
| **Protocols & SDK Compatibility** | | |
| Windows Identity Foundation (WIF) | Supported | Not supported. |
| Ws-Federation | Supported | Not supported. |
| OAuth 2.0 | Support for Draft 13 | Support for RFC 6749, the most modern specification. |
| Ws-Trust | Supported | Not supported. |
| **Token Formats** | | |
| Json Web Tokens (JWTs) | Supported In Beta | Supported |
| SAML 1.1 tokens | Supported | Not supported. |
| SAML 2.0 tokens | Supported | Not supported. |
| Simple Web Tokens (SWT) | Supported | Not supported. |
| **Customizations** | | |
| Customizeable home realm discovery/account picking UI | Downloadable code that can be incorporated into apps | Fully customizable UI via custom CSS. |
| Upload custom token signing certificates | Supported | Supported via advanced policies. |
| Customize claims in tokens | Passthrough input claims from identity providers<br />Get access token from identity provider as a claim<br />Issue output claims based on values of input claims<br />Issue output claims with constant values | Can passthrough claims from identity providers<br />Cannot get access token from identity provider as a claim<br />Can issue output claims based on values of input claims via advanced policies<br />Can issue output claims with constant values via advanced policies |
| **Automation** | | |
| Automate configuration/management tasks | Supported via ACS Management Service | Supported via Microsoft Graph & Azure AD Graph API. |

TODO:
change advanced policies to custom policies
column changes to B2C
change IDPs to call out OIDC & SAML
yahoo only through custom, try to make sure
custom token signing keys, but not certs
custom policies allow any claim from idps
can't create tenants & apps & policies programatically, can create users


TODO: Tips for migrating to Azure AD from ACS:
- If using WIF, watch out for key rolling. There's some plugin you can use which rolls them automatically I think.
- What to do about user identifiers.
- What to do about AuthZ
- You'll want to use custom apps, not app registrations for Ws-Fed. That way, the token customization page will show up.
- TODO: other stuff


Possible nameIdentifiers from ACS (via AAD or ADFS):
- ADFS - Whatever ADFS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Let's look at Kusto to determine what IDs are being sent to ACS
    - 510 applications in last week hitting ESTS from ACS
    - Can't tell, that shit's redacted

Possible nameIdentifiers from AAD:
- objectID: Need as nameIdentifier from Dilesh
- subject: Need as nameIdentifier from Dilesh (no policy written for nameID)
- UPN: already available via custom apps
- mail: already available via custom apps
- other stuff also available


TODO: links & resources 
TODO: Table comparing AAD & AAD B2C

### Web services using active authentication

For web services secured with tokens issued by ACS, ACS provided the following features & capabilities:

- Ability to register one or more **service identities** in your ACS namespace that can be used to request tokens.
- Support for the OAuth WRAP protocol for requesting tokens, using the following types of credentials:
    - A simple password created for the service identity
    - A signed SWT using a symmetric key or X509 certificate
    - A SAML token issued by a trusted identity provider (typically an ADFS instance)
- Support for the following token formats: JSON web token (JWT), SAML 1.1, SAML 2.0, and Simple web token (SWT).
- Simple token transformation rules

Service identities in ACS are typically used for implementing server-to-server (S2S) like authentication.  Our recommendation for this type of authentication flow is to migrate to [**Azure Active Directory**](https://azure.microsoft.com/develop/identity/signin/) (Azure AD). Azure AD is the cloud based identity provider for Microsoft work & school accounts - it is the identity provider for Office 365, Azure, and much more. But it can also be used to server to server authentication using Azure AD's implementation of the OAuth client credentials grant.  The following table compares the capabilities of ACS in server to server authentication with those available in Azure AD.

| Capability | ACS Support | Azure AD Support |
| ---------- | ----------- | ---------------- |
| How to register a web service | Create a relying party in the ACS managment portal. | Create an Azure AD web application in the Azure Portal. |
| How to register a client | Create a service identity in ACS management portal. | Create another Azure AD web application in the Azure Portal. |
| Protocol used | OAuth WRAP protocol. | OAuth client credentials grant. |
| Client authentication methods | Simple password<br />Signed SWT<br />SAML token from federated IDP | Simple password<br />Signed JWT |
| Token formats | JWT<br />SAML 1.1<br />SAML 2.0<br />SWT<br /> | JWT only |
| Token transformation | Add custom claims<br />Simple if-then claim issuance logic | Add custom claims | 
| Automate configuration/management tasks | Supported via ACS Management Service | Supported via Microsoft Graph & Azure AD Graph API. |

TODO: links to next steps (code samples, resources, etc.)