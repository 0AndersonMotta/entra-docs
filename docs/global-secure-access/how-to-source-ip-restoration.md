---
title: Enable source IP restoration with the Global Secure Access preview
description: Learn how to enable source IP restoration to ensure source IP match in downstream resources.

ms.service: network-access
ms.subservice: 
ms.topic: how-to
ms.date: 05/23/2023

ms.author: joflore
author: MicrosoftGuyJFlo
manager: amycolannino
ms.reviewer: mamkumar
---
# Source IP restoration

With a cloud based network proxy between users and their resources, the IP address that the resources see doesn't match the actual source IP address. In place of the end-users’ source IP, the resource endpoints see the cloud proxy as the source IP address. Customers with these cloud proxy solutions can't use this source IP information. 

Microsoft’s existing solutions such as Conditional Access and continuous access evaluation (CAE) enforcement for Microsoft 365 apps rely on source IP information. With the Global Secure Access preview and source IP restoration, organizations can continue using IP location-based Conditional Access policies, including CAE enforcement.

Source IP restoration allows services to see the real source IP address, these services include: [Conditional Access](/azure/active-directory/conditional-access/overview), [continuous access evaluation](/azure/active-directory/conditional-access/concept-continuous-access-evaluation), [Identity Protection risk detections](/azure/active-directory/identity-protection/concept-identity-protection-risks), [sign-in logs](/azure/active-directory/reports-monitoring/concept-sign-ins), and [endpoint detection & response (EDR)](/microsoft-365/security/defender-endpoint/overview-endpoint-detection-response).

[!INCLUDE [Public preview important note](./includes/public-preview-important-note.md)]

## Prerequisites

* Administrators who interact with **Global Secure Access preview** features must have both of the following role assignments depending on the tasks they're performing.
   * A **Global Secure Access Administrator** role to manage the Global Secure Access preview features
   * [Conditional Access Administrator](/azure/active-directory/roles/permissions-reference#conditional-access-administrator) or [Security Administrator](/azure/active-directory/roles/permissions-reference#security-administrator) to create and interact with Conditional Access policies and named locations.
* A Windows client machine with the [Global Secure Access client installed](how-to-install-windows-client.md) and running or a [remote network configured](how-to-manage-remote-networks.md).

## Enable Global Secure Access signaling for Conditional Access

To enable the required setting to allow source IP restoration, an administrator must take the following steps.

1. Sign in to the **Microsoft Entra admin center** as a Global Secure Access Administrator.
1. Browse to **NEED THE PATH** > **Security** > **Adaptive Access**.
1. Select the toggle to **Enable Network Access signaling in Conditional Access**.

This functionality allows services like Microsoft Graph, Microsoft Entra ID, SharePoint Online, and Exchange Online to see the actual source IP address.

:::image type="content" source="media/how-to-source-ip-restoration/toggle-enable-signaling-in-conditional-access.png" alt-text="Screenshot showing the toggle to enable signaling in Conditional Access.":::

> [!CAUTION]
> If your organization has active Conditional Access policies based on IP location checks, and you disable Global Secure Access signaling in Conditional Access, you may unintentionally block targeted end-users from being able to access the resources. If you must disable this feature, first delete any corresponding Conditional Access policies. 

## Sign-in log behavior

To see source IP restoration in action, administrators can take the following steps.

1. Sign in to the **Microsoft Entra admin center** as a [Security Reader](/azure/active-directory/roles/permissions-reference#security-reader).
1. Browse to **Azure Active Directory** > **Users** > select one of your test users > **Sign-in logs**.
1. With source IP restoration enabled, you see IP addresses that include their actual IP address. 
   1. If source IP restoration is disabled, you won't see their actual IP address.

Sign-in log data may take some time to appear, this delay is normal as there's some processing that must take place.

:::image type="content" source="media/how-to-source-ip-restoration/sign-in-logs-enabled-disabled.png" alt-text="Screenshot of the sign-in logs showing events with source IP restoration on, then off, then on again." lightbox="media/how-to-source-ip-restoration/sign-in-logs-enabled-disabled.png":::

- [Set up tenant restrictions V2 (Preview)](../active-directory/external-identities/tenant-restrictions-v2.md)
- [Enable compliant network check with Conditional Access](how-to-compliant-network.md)