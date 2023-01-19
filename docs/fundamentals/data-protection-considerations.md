---
title: Data protection considerations
description: Learn how services store and retrieve Azure AD object data through an RBAC authorization layer.
services: active-directory
author: janicericketts
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 01/19/2023
ms.author: jricketts
ms.reviewer: jricketts
ms.custom: "it-pro"
ms.collection:
---

# Data protection considerations

As shown in the following diagram, services store and retrieve Azure Active Directory (Azure AD) object data through a role-based access control (RBAC) authorization layer that calls the internal directory data access layer. This ensures the user's data request is permitted: 

   ![Diagram of services storing and retrieving Azure AD object data.](./media/data-protection-considerations/tenant-isolation.PNG)

**Azure AD Internal Interfaces Access**: Service-to-service communication with other Microsoft services, such as Microsoft 365 use Azure AD interfaces, which authorize the service's callers using client certificates.  

**Azure AD External Interfaces Access**: Azure AD external interface helps prevent data leakage by using RBAC. When a security principal, such as a user, makes an access request to read information through Azure AD interfaces, a security token must accompany the request. The token contains claims about the principal making the request. 

The security tokens are issued by the Azure AD Authentication Services. Information about the user’s existence, enabled state, and role is used by the authorization system to decide whether the requested access to the target tenant is authorized for this user in this session.  

**Application Access**: Because applications can access the Application Programming Interfaces (APIs) without user context, the access check includes information about the user’s application and the scope of access requested, for example read only, read/write, etc. Many applications use OpenID Connect or OAuth to obtain tokens to access the directory on behalf of the user. These applications must be explicitly granted access to the directory or they won't receive a token from Azure AD Authentication Service, and they access data from the granted scope. 

**Auditing**: Access is audited. For example, authorized actions such as create user and password reset create an audit trail that can be used by a tenant administrator to manage compliance efforts or investigations. Tenant administrators can generate audit reports by using the Azure AD audit API.  

Learn more: [Azure Active Directory audit report events](../active-directory/active-directory-reporting-audit-events.md)

**Tenant Isolation**: Enforcement of security in Azure AD multi-tenant environment helps achieve two primary goals:  

* Prevent data leakage and access across tenants: Data belonging to Tenant 1 cannot be obtained by users in Tenant 2 without explicit authorization by Tenant 1.  
* Resource access isolation across tenants: Operations performed by Tenant 1 cannot affect access to resources for Tenant 2.  

## Tenant isolation

The following information outlines tenant isolation.

* The service secures tenants using RBAC policy to ensure data isolation.n.  
* To enable access to a tenant, a principal, for example a user or application, needs to be able to authenticate against Azure AD to obtain context and has explicit permissions defined in the tenant. If a principal is not authorized in the tenant, the resulting token will not carry permissions, and the RBAC system rejects requests in this context.  
* RBAC ensures access to a tenant is performed by a security principal authorized in the tenant. Access across tenants is possible when a tenant administrator creates a security principal representation in the same tenant (for example, provisioning a guest user account using B2B collaboration), or when a tenant administrator creates a policy to enable a trust relationship with another tenant. For example, a cross-tenant access policy to enable B2B Direct Connect. This is because each tenant is an isolation boundary; existence in one tenant does not equate existence in another tenant unless the administrator allows it.  
* Azure AD data for multiple tenants is stored in the same physical server and drive for a given partition. Isolation is ensured because access to the data is protected by the RBAC authorization system. 
* A customer application cannot access Azure AD without needed authentication. The request is rejected if not accompanied by credentials as part of the initial connection negotiation process. This prevents unauthorized access to a tenant by neighboring tenants. Only user credential’s token, or Security Assertion Markup Language (SAML) token, is brokered with a federated trust. Therefore it is validated by Azure AD based on the shared keys configured by the Global Administrator of the Azure AD tenant.  
* Because there is no application component that can execute from the Core Store, it is not possible for one tenant to forcibly breach the integrity of a neighboring tenant.  

## Data security

**Encryption in Transit**: To assure data security, directory data in Azure AD is signed and encrypted while in transit between data centers in a scale unit. The data is encrypted and unencrypted by the Azure AD Core Store tier, which resides in secured server hosting areas of the associated Microsoft data centers.  

Customer-facing web services are secured with the Transport Layer Security (TLS) protocol.  

**Secret Storage**: Azure AD Service back-end uses encryption to store sensitive material for service use, such as certificates, keys, credentials, and hashes using Microsoft proprietary technology. The store used depends on the service, the operation, the scope of the secret (user-wide or tenant-wide), and other requirements.  

These stores are operated by a security-focused group via established automation and workflows, including certificate request, renewal, revocation, and destruction. 

There is activity auditing related to these stores/workflows/processes, and there is no standing access. Access is request- and approval-based, and for a limited amount of time.  

For more information about Secret encryption at rest, see the following table. 

**Algorithms**: The following table lists the minimum cryptography algorithms used by Azure AD components. As a cloud service, Microsoft reassesses and improves the cryptography, based on security research findings, internal security reviews, key strength against hardware evolution, etc. 

|Data/scenario|Cryptography algorithm|
|---|---|
|Password hash sync</br>Cloud account passwords|Hash: Password Key Derivation Function 2 (PBKDF2), using HMAC-SHA256 @ 1000 iterations |
|Directory in transit between data centers|AES-256-CTS-HMAC-SHA1-96</br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 |
|Pass-through authentication user credential flow|RSA 2048-Public/Private key pair </br> Learn more: [Azure Active Directory Pass-through Authentication security deep dive](../hybrid/how-to-connect-pta-security-deep-dive.md)|
|Self-service password reset password writeback with Azure AD Connect: Cloud to on-premises communication |RSA 2048 Private/Public key pair</br>AES_GCM (256-bits key, 96-bits IV size)|
|Self-service password reset: Answers to security questions|SHA256|
|SSL certificates for Azure AD application</br>Proxy published applications |AES-GCM 256-bit |
|Disk-level encryption|XTS-AES 128|
|[Seamless single sign-on (SSO)](../../active-directory/hybrid/how-to-connect-sso-how-it-works.md) service acount password</br>SaaS application provisioning credentials|AES-CBC 128-bit |
|Azure AD Managed Identities|AES-GCM 256-bit|
|Microsoft Authenticator app: Passwordless sign-in to Azure AD |Asymmetric RSA Key 2048-bit|
|Microsoft Authenticator app: Backup and restore of enterprise account metadata |AES-256  |

## Resources
* [Azure AD and data residency](azure-ad-data-residency.md)
* [Microsoft Service Trust Documents](https://servicetrust.microsoft.com/Documents/TrustDocuments)
* [Microsoft Azure Trust Center](https://azure.microsoft.com/overview/trusted-cloud/)
* [Where is my data? - Office 365 documentation](http://o365datacentermap.azurewebsites.net/)
* [Recover from deletions in Azure Active Directory](recover-from-deletions.md)
