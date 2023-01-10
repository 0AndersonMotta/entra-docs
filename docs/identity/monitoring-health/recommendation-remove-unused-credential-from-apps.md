---
title: Azure Active Directory recommendation - Remove unused credentials from apps | Microsoft Docs
description: Learn why you should remove unused credentials from apps.
services: active-directory
author: shlipsey3
manager: amycolannino
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.subservice: report-monitor
ms.date: 01/09/2023
ms.author: sarahlipsey
ms.reviewer: hafowler
ms.collection: M365-identity-device-management
---
# Azure AD recommendation: Remove unused credentials from apps
[Azure AD recommendations](overview-recommendations.md) is a feature that provides you with personalized insights and actionable guidance to align your tenant with recommended best practices.

This article covers the recommendation to remove unused credentials from apps.

## Description

Application credentials can include certificates and other types of secrets that need to be registered with that application. These credentials are used to prove the identity of the application. Only credentials actively in use by an application should remain registered with the application.

## Logic 

This recommendation shows up if your tenant has application credentials that haven't been used in more than 30 days. 

## Value 

An application credential is used to get a token that grants access to a resource or another service. If an application credential is compromised, it could be used to access sensitive resources or allow a bad actor to move latterly, depending on the access granted to the application.

Removing credentials not actively used by applications improves security posture and promotes app hygiene. It reduces the risk of application compromise and improves the security posture of the application by reducing the attack surface for credential misuse by discovery.

## Action plan

1. For application resources, navigate to **Azure AD** > **App registration** and locate the application with unused credentials.
1. Navigate to the **Certificates & Secrets** section of the app registration.
1. Locate the unused credential and remove it.
1. To remove a credential from a service principal resource, use the MS Graph Service Principal API service action `removePassword`.
 
## Next steps

- [What is Azure Active Directory recommendations](overview-recommendations.md)
- [Learn about app and service principal objects in Azure AD](../develop/app-objects-and-service-principals.md)