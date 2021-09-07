---
title: Azure AD feature availability in Azure Government
description: Learn which Azure AD features are availabile in Azure Government. 

services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/25/2021

ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
---

# Cloud feature availability

This topic covers Azure AD feature availabilityin Azure Government.

## Feature availability


Authentication, single sign-on and multifactor authentication (MFA)

| Feature | Availability |
|---------|:------------:|
| **Authentication, single sign-on and multifactor authentication (MFA)** |   | 
| Cloud authentication (Pass-through authentication, password hash synchronization) | ● |
| Federated authentication (Active Directory Federation Services or federation with other identity providers) | ● |
| Single sign-on (SSO) unlimited | ● | 
| Multifactor authentication (MFA) | AGC: SMS/Voice/Push notification aren't available due to lack of internet connectivity and third party provider are not available in air gapped. | 
| Passwordless (Windows Hello for Business, Microsoft Authenticator, FIDO2 security key integrations) | Gov (ARL): Authenticator app will only shows GUID and not UPN for compliance reason.<br>AGC: Microsoft Authenticator app, which relies on phones and third party push notification services, is not available.  | 
| Service-level agreement | ● | 
| **Applications Access** |   |
| SaaS apps with modern authentication (Azure AD application gallery apps, SAML, and OAUTH 2.0) | ● | 
| Group assignment to applications | ● | 
| Cloud app discovery (Microsoft Cloud App Security) | ● | 
| Application Proxy for on-premises, header-based, and Integrated Windows Authentication | ● | 
| Secure hybrid access partnerships (Kerberos, NTLM, LDAP, RDP, and SSH authentication) | ● | 
| **Authorization and Conditional Access)** |   |
| Role-based access control (RBAC) |    ● | 
| Conditional Access  | ● | 
| SharePoint limited access | ● | 
| Session lifetime management | ● | 
| Identity Protection (Risky sign-ins, risky users, risk-based conditional access) |  ● | 
| **Administration and hybrid identity** |   |
| User and group management | ● | 
| Advanced group management (Dynamic groups, naming policies, expiration, default classification) |  ● | 
| Directory synchronization—Azure AD Connect (sync and cloud sync) | ● | 
| Azure AD Connect Health reporting | ● | 
| Delegated administration—built-in roles  | ● | 
| Global password protection and management – cloud-only users |  ● | 
| Global password protection and management – custom banned passwords, users synchronized from on-premises Active Directory | ● | 
| Microsoft Identity Manager user client access license (CAL) |  ● | 
| **End-user self-service** |  |
| Application launch portal (My Apps) | ● | 
| User application collections in My Apps | ● |
| Self-service account management portal (My Account) | ● |
| Self-service password change for cloud users |  ● |
| Self-service password reset/change/unlock with on-premises write-back | ● |
| Self-service sign-in activity search and reporting |  ● |
| Self-service group management (My Groups) |  ● |
| Self-service entitlement management (My Access) | ● |
| **Identity Governance** |  |
| Automated user provisioning to apps |  ● |
| Automated group provisioning to apps | ● |
| HR-driven provisioning |  ● |
| Terms of use attestation | ● |
| Access certifications and reviews |  ● |
| Entitlements management |  ● |
| Privileged Identity Management (PIM), just-in-time access |  ● |
| **Event logging and reporting**|   |
| Basic security and usage reports |  ● |
| Advanced security and usage reports | ● |
| Identity Protection: vulnerabilities and risky accounts |  ● |
| Identity Protection: risk events investigation, SIEM connectivity |  ● |
| **Frontline workers** |  |
| SMS sign-in | AGC: SMS/Voice/Push notification aren't available |
| Shared device sign-out |  ● |
| Delegated user management portal (My Staff) |  ● |







