---
title: Provisioning logs in the Azure Active Directory portal | Microsoft Docs
description: Introduction to sign-in activity reports in the Azure Active Directory portal 
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''

ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/10/2019
ms.author: markvi
ms.reviewer: dhanyahk

ms.collection: M365-identity-device-management
---
# Provisioning reports in the Azure Active Directory portal

The reporting architecture in Azure Active Directory (Azure AD) consists of the following components:

- **Activity** 
    - **Sign-ins** – Information about the usage of managed applications and user sign-in activities.
    - **Audit logs** - [Audit logs](concept-audit-logs.md) provide system activity information about users and group management, managed applications and directory activities.
    - **Provisioning logs** - Provide system activity about user, groups, and roles that are provisioned by the Azure AD provisioning service. 

- **Security** 
    - **Risky sign-ins** - A [risky sign-in](concept-risky-sign-ins.md) is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.
    - **Users flagged for risk** - A [risky user](concept-user-at-risk.md) is an indicator for a user account that might have been compromised.

This topic gives you an overview of the provisioning report.

## Prerequisites

### Who can access the data?
* Users in the Security Administrator, Security Reader, Report Reader, Application Administrator, and Cloud Application Adminstrator roles
* Global Administrators


### What Azure AD license do you need to access sign-in activity?
* Your tenant must have an Azure AD Premium license associated with it to see the all up sign-in activity report. See [Getting started with Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) to upgrade your Azure Active Directory edition. 

## Provisioning logs

The provisioning logs provide answers to the following questions:

* What groups were successfully created in ServiceNow?
* How roles were imported from Amazon Web Services?
* What users were unsuccessfully created in DropBox?

You can access the provisioning logs by selecting **Provisioning Logs** in the **Activity** section of the **Azure Active Directory** blade in the [Azure portal](https://portal.azure.com). Note that it may take upto two hours for some sign-in records to show up in the portal.

![Sign-in activity](./media/concept-sign-ins/61.png "Sign-in activity")

> [!IMPORTANT]
Lore Ipsum

A sign-ins log has a default list view that shows:

- The sign-in date
- The related user
- The application the user has signed-in to
- The sign-in status
- The status of the risk detection
- The status of the multi-factor authentication (MFA) requirement

![Sign-in activity](./media/concept-sign-ins/01.png "Sign-in activity")

You can customize the list view by clicking **Columns** in the toolbar.

![Sign-in activity](./media/concept-sign-ins/19.png "Sign-in activity")

This enables you to display additional fields or remove fields that are already displayed.

![Sign-in activity](./media/concept-sign-ins/02.png "Sign-in activity")

Select an item in the list view to get more detailed information.

![Sign-in activity](./media/concept-sign-ins/03.png "Sign-in activity")


## Filter sign-in activities

To narrow down the reported data to a level that works for you, you can filter the sign-ins data using the following default fields. Note that the values in the filters are dynamically populated based on your tenant. If, for example, you don't have any create events in your tenant, there won't be a filter option for create.

- Action
- Application
- Identity
- Source System
- Target System
- Status
- Date

![Sign-in activity](./media/concept-sign-ins/04.png "Sign-in activity")

The **Identity** filter enables you to specify the name or the identity that you care about. This identity could be a user, group, role, or other object. You can search by the name or id of the object. The id varies by scenario. For example, when provisioning a user from Azure AD to SalesForce, the Source ID is the object ID of the user in Azure AD while the TargetID is the ID of the user in Salesforce. When provisioning from Workday to Active Directory, the Source ID is the Workday worker employee ID. Note that the Name of the user may not always be present in the Identity column. There will always be one ID. 

The **Source System** filter enables you to specify where the identity is getting provisioned from. For example, when provisioning a user from Azure AD to ServiceNow, the Source system is Azure AD. 

The **Target System** filter enables you to specify where the identity is getting provisioned to. For example, when provisioning a user from Azure AD to ServiceNow, the Target System is ServiceNow. 

The **Status** filter enables you to select:

- All
- Success
- Failure
- Skipped

The **Action** filter enables you to filter the:

- Create 
- Update
- Delete
- Disable
- Other

The **Date** filter enables to you to define a timeframe for the returned data.  
Possible values are:

- 1 month
- 7 days
- 24 hours
- Custom time interval

When you select a custom timeframe, you can configure a start time and an  

## Drawer tabs (need to reword)

The **Steps** tab outlines the fours steps taken to provision a user. Note that not all 4 steps all 4 steps will be present in every case. If we skip a user for example, step 3 / 4 won't be present. 

The 4 steps are import user, determine if user is in scope, match user between source and target, provision user (take action - this could be a create, update, delete, or disable)

The **troubleshoot and recomendations** tab provides the error code and reason. This is only present if there is a failure. 

The **modified properties** shows the old value and new value. In cases where there is no old value the old value column will be blank. 

The **summary** tab provides an overview of what happened and identifiers for the object in the source and target system. 

## What you should know
- data stored for 30 days
- Change ID - unique ID that can be used to identify the event and contact support
-Download  - Not yet supported but coming soon
-Log Analytics - Nt yet supported but coming soon


## Next steps

* [Sign-in activity report error codes](reference-sign-ins-error-codes.md)
* [Azure AD data retention policies](reference-reports-data-retention.md)
* [Azure AD report latencies](reference-reports-latencies.md)

