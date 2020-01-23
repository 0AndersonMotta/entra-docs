---
title: Users and groups in Conditional Access policy - Azure Active Directory
description: What are conditions in an Azure AD Conditional Access policy

services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 01/10/2020

ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb

ms.collection: M365-identity-device-management
---
# Conditional Access: Users and groups

A Conditional Access policy must include a user assignment as one of the signals in the decision process. 

![User as a signal in the decisions made by Conditional Access](./media/concept-conditional-access-users-groups/conditional-access-overview-how-it-works.png)

Users can be included or excluded from Conditional Access policies. 

## Include users

This list of users typically includes all of the users an organization is targeting in a Conditional Access policy. 

- None
- All users
- Select users and groups
   - All guest and external users (preview)
      - This selection includes any B2B guests and external users including any user with the attibute set to guest. This selection also applies to any external user signed-in from a different organization like a Cloud Solution Provider (CSP). 
   - Directory roles (preview)
      - Allows administrators to select specific Azure AD directory roles used to determine assignment. For example organizations may create a more restrictive policy on users assigned the global administrator role.
   - Users and groups
      - Allows targeting of specific sets of users. For example, organizations can select a group that contains all members of the HR department when an HR app is selected as the cloud app. A group can be any type of group in Azure AD, including dynamic or assigned security and distribution groups.

## Exclude users

Exclusions are commonly used for emergency access or break-glass accounts. More information about emergency access accounts and why they are important can be found in the following articles: 

* [Manage emergency access accounts in Azure AD](../users-groups-roles/directory-emergency-access.md)
* [Create a resilient access control management strategy with Azure Active Directory](../authentication/concept-resilient-controls.md)



## Next steps

- [Conditional Access: Cloud apps or actions](concept-conditional-access-cloud-apps.md)