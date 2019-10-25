---
title: What is Conditional Access report-only mode? - Azure Active Directory
description: How can report-only mode help with Conditional Access policy deployment

services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/25/2019

ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: dawoo

ms.collection: M365-identity-device-management
---
# What is Conditional Access report-only mode?

Conditional Access is widely used by our customers to stay secure by applying the right access controls in the right circumstances. However one of the challenges with deploying a Conditional Access policy in your organization is determining the impact to end users. It can be difficult to anticipate the number and names of users disrupted by common deployment initiatives such as blocking legacy authentication, requiring multi-factor authentication for a population of users, or implementing sign-in risk policies. 

Report-only mode is a new Conditional Access policy state that allows administrators to evaluate the impact of Conditional Access policies before enabling them in their environment.  With the release of Report-only mode:

- Conditional Access policies can be enabled in Report-only mode.
- During sign-in, policies in Report-only mode are evaluated but not enforced. Results are logged in the **Conditional Access** and **Report-only (Preview)** tabs of the Sign-in log details.
- Customers with an Azure Monitor subscription can monitor the impact of their Conditional Access policies using the Conditional Access insights workbook.

![Report-only tab in Azure AD sign-in log](./media/concept-conditional-access-report-only/report-only-detail-in-sign-in-log.png)

## Policy results

When a policy in report-only mode is evaluated for a given sign-in, there are four new possible result values:

| Result | Description |
| --- | --- |
| Report-only: Success | All configured policy conditions and required grant controls were satisfied (including non-interactive grant controls such as device checks for Hybrid Azure AD join or compliant device). |
| Report-only: Failure | All configured policy conditions were satisfied but not all the required grant controls were satisfied (including non-interactive grant controls such as device checks for Hybrid Azure AD join or compliant device). |
| Report-only: User action required | All configured policy conditions were satisfied and user action would be required to satisfy the required grant controls (i.e. multi-factor authentication, accepting Terms of Use). With Report-only mode, the user is not prompted to satisfy the required grant control. |
| Report-only: Not applied | Not all configured policy conditions were satisfied. For example, the user is excluded from the policy or the policy only applies to certain trusted named locations. |

## Conditional Access Insights workbook

Administrators have the capability to create multiple policies in Report-only mode, so it is necessary to understand both the individual impact of each policy and the combined impact of multiple policies evaluated together. The new Conditional Access Insights workbook enables administrators to visualize Conditional Access queries and monitor the impact of a policy for a given time range, set of applications, and users. 
 
## Next steps

[Configure report-only mode on a Conditional Access policy](howto-conditional-access-report-only.md)
