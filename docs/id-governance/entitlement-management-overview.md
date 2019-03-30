---
title: What is Azure AD entitlement management? (Preview)
description: #Required; article description that is displayed in search results.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/26/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management


#Customer intent: As a < type of user >, I want < what? > so that < why? >.

---
# What is Azure AD entitlement management? (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) entitlement management is currently in public preview.
> This preview version is provided without a service level agreement, and it's not recommended for production workloads. Certain features might not be supported or might have constrained capabilities.
> For more information, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Managing access to all the resources a user needs to be productive is challenging. In most cases, there is no organized list of all the resources a user needs for a project. The project manager has a good understanding of all the resources needed, the scope of the project, the individuals involved in the project, and how long the project will last. However, the project manager typically does not have permissions to approve or grant access to others. This scenario gets more complicated when you try to work with individuals or companies outside your organization.

Azure Active Directory (Azure AD) entitlement management enables organizations to manage access to packages of groups, applications, and SharePoint Online sites for internal users and users from external partners.

## Why use entitlement management?

Enterprise organizations often face challenges when managing access for users across resources:

- Users may not know what access rights they should have
- Users may have difficulty locating the right individuals or right resources
- Once users find and receive access to a resource, they may hold on to access rights longer than is required for business purposes

These problems are compounded for users who need access from another directory, such as external users that are from supply chain organizations or other business partners. For example:

- Organizations may not know all of the specific individuals in other directories to be able to invite them
- Even if organizations were able to invite these users, organizations may not remember to manage all of the user's access rights consistently

Azure AD entitlement management can help address these challenges.

## What can I do with entitlement management?

Here are some of capabilities of entitlement management:

- Create packages of related resources that users need
- Define rules for how to request resources and when access expires
- Govern the lifecycle of access for both internal and external users
- Delegate management of resources
- Designate approvers to approve requests
- Create reports to track history

For an overview of identity governance and entitlement management, watch the following video from the Ignite 2018 conference:

>[!VIDEO https://www.youtube.com/embed/aY7A0Br8u5M]

## What resources can you manage?

Here are the types of resources you can manage access to with entitlement management:

- Azure AD security groups
- Office 365 groups
- Azure AD enterprise applications
- SaaS applications
- Custom integrated applications
- SharePoint Online site collections
- SharePoint Online sites

## Prerequisites

To use Azure AD entitlement management (Preview), you must have one of the following licenses:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5 license

For more information, see [Sign up for Azure Active Directory Premium editions](../fundamentals/active-directory-get-started-premium.md) or [Enterprise Mobility + Security E5 Trial](https://aka.ms/emse5trial).

## What are access packages and policies?

Entitlement management introduces the concept of an *access package*. An access package is a bundle of all the resources a user needs to work on a project or perform their job. The resources include access to groups, applications, and sites. Access packages are used to govern access for your internal employees, and also from users from outside your organization. Access packages are defined in containers called *catalogs*.

Access packages also include one or more *policies*. A policy defines the rules or guardrails to access an access package. Enabling a policy enforces that only the right users are granted access, to the right resources, and for the right amount of time.

With an access package and its policies, the access package manager defines:

- Resources
- Roles the users need for the resources
- Users and external users that are eligible to request access
- Approval process and the users that can approve or deny access
- Duration of user's access

![Entitlement management overview](./media/entitlement-management-overview/elm-overview.png)

## Terminology

To better understand entitlement management and its documentation, you should review the following terms.

| Term or concept | Description |
| --- | --- |
| entitlement management | A service that assigns, revokes, and administers access packages. |
| catalog | A container of related resources and access packages. A catalog can be configured to be visible outside the current directory. |
| Default catalog | A built-in catalog that is always available. To add resources to the Default catalog requires certain permissions. |
| access package | A collection of permissions and policies to resources that users can request. An access package is always contained in a catalog. |
| access request | A request to access an access package. A request typically goes through a workflow. |
| policy | A set of rules that defines the access lifecycle, such as how users get access, who can approve, and how long users have access. Example policies include employee access and external access. |
| resource | An asset or service (such as a group, application, or site) that a user can be granted permissions to. |
| resource type | The type of resource, which includes groups, applications, and SharePoint Online sites. |
| resource role | A collection of permissions associated with a resource. |
| resource directory | A directory that has one or more resources to share. |
| assigned users | An assignment of an access package to a user or group. |
| enable | The process of making an access package visible for users to request. |

## Roles and permissions

Entitlement management has different roles based on job function.

| Role | Description |
| --- | --- |
| [User administrator](../users-groups-roles/directory-assign-admin-roles.md#user-administrator) | Manage all aspects of entitlement management.<br/>Create users and groups. |
| Catalog creator | Create and manage catalogs. Typically an IT administrator or resource owner. The person that creates a catalog automatically becomes the catalog's first Catalog owner. |
| Catalog owner | Edit and manage existing catalogs. Typically an IT administrator or resource owner. |
| Access package manager | Edit and manage all existing access packages within a catalog. |
| Approver | Approve requests to access packages. |
| Requestor | Request access packages. |

The following table lists the permissions for each of these roles.

| Task | User admin | Catalog creator | Catalog owner | Access package manager | Approver | Requestor |
| --- | :---: | :---: | :---: | :---: | :---: | :---: |
| Create a catalog | :heavy_check_mark: | :heavy_check_mark: |  |  |  |  |
| Edit/delete a catalog | :heavy_check_mark: |  | :heavy_check_mark: |  |  |  |
| Add additional catalog owners | :heavy_check_mark: |  | :heavy_check_mark: |  |  |  |
| Add/remove resources to/from the **Default** catalog | :heavy_check_mark: |  |  |  |  |  |
| Add/remove resources to/from a catalog | :heavy_check_mark: |  | :heavy_check_mark: |  |  |  |
| Create an access package in the **Default** catalog | :heavy_check_mark: |  :heavy_check_mark: |  |  |  |  |
| Create an access package in a catalog | :heavy_check_mark: |   | :heavy_check_mark: |  |  |  |
| Delete an access package | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |  |
| Add/remove access packages to/from a catalog | :heavy_check_mark: |  |  | :heavy_check_mark: |  |  |
| Add/remove resource roles to/from an access package | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |  |
| Directly assign a user to an access package | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |  |
| Specify who can request an access package | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |  |
| View who has an assignment to an access package | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |  |
| View an access package's requests | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |  |
| View a request's fulfillment errors | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |  |
| Cancel a pending request | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |  |
| Hide an access package | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |  |
| Approve an access request |  |  |  |  | :heavy_check_mark: |  |
| Request an access package |  |  |  |  |  | :heavy_check_mark: |

## Next steps

- [Tutorial: Create your first access package](entitlement-management-first-access-package.md)
- [Common scenarios](entitlement-management-scenarios.md)
