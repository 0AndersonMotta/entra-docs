---
title: Road to the cloud - Moving identity and access management from AD to Azure AD
description: Learn to plan your migration if IAM from AD to Azure AD.
documentationCenter: ''
author: janicericketts
manager: martinco
ms.service: active-directory
ms.topic: how-to
ms.subservice: none
ms.date: 06/03/2022
ms.author: jricketts
ms.custom: references_regions
---

# Establish an Azure AD footprint

If you're using Microsoft Office 365, Exchange Online, or Teams then you are already using Azure AD. If you do, your next step is to establish more Azure AD capabilities.

* Establish hybrid identity synchronization between AD and Azure AD using [Azure AD Connect](../hybrid/whatis-azure-ad-connect.md) or [Azure AD Connect Cloud Sync](../cloud-sync/what-is-cloud-sync.md).

* [Select authentication methods](../hybrid/choose-ad-authn.md). We strongly recommend password hash synchronization (PHS).

* Secure your hybrid identity infrastructure by following [Secure your Azure AD identity infrastructure - Azure Active Directory](../../security/fundamentals/steps-secure-identity.md)

## Optional tasks

The following aren't specific nor mandatory to transforming from AD to Azure AD but are recommended functions to incorporate into your environment. These are also items recommended in the [Zero Trust](security/zero-trust/) guidance.

### Deploy Passwordless authentication

In addition to the security benefits of [passwordless credentials](../authentication/concept-authentication-passwordless.md), this simplifies your environment because the management and registration experience is already native to the cloud. Azure AD provides different passwordless credentials that align with different use cases. Use the information in this document to plan your deployment: [Plan a passwordless authentication deployment in Azure Active Directory](../authentication/howto-authentication-passwordless-deployment.md)

Once you roll out passwordless credentials to your users, consider reducing the usage of password credentials. Approaches include:

* Using the [reporting and Insights dashboard](../authentication/howto-authentication-methods-activity.md) to continue to drive usage of passwordless credentials and reduce usage of passwords in Azure AD

>[!IMPORTANT]
>During your application discovery, you might find applications that have a dependency or assumptions around passwords. Users of these applications need to have access to their passwords until those applications are updated or migrated.

### Configure hybrid Azure AD join for existing Windows clients

You can configure hybrid Azure AD join for existing AD joined Windows clients to benefit from cloud-based security features such as [co-management](/mem/configmgr/comanage/overview), conditional access, and Windows Hello for Business. New devices should be Azure AD joined and not hybrid Azure AD joined.

To learn more, check: [Plan your hybrid Azure Active Directory join deployment](../devices/hybrid-azuread-join-plan.md)

## Next steps

[Introduction](road-to-the-cloud-introduction.md)

[Cloud transformation posture](road-to-the-cloud-posture.md)

[Implement a cloud-first approach](road-to-the-cloud-implement.md)

[Transition to the cloud](road-to-the-cloud-migrate.md)
