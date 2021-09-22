---
title: Troubleshoot custom security attributes in Azure AD (Preview) - Azure Active Directory
description: Learn how to troubleshoot custom security attributes in Azure Active Directory.
services: active-directory
author: rolyon
ms.author: rolyon
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
ms.date: 09/15/2021
ms.collection: M365-identity-device-management
---

# Troubleshoot custom security attributes in Azure AD (Preview)

> [!IMPORTANT]
> Custom security attributes are currently in PREVIEW.
> See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

## Symptom - Custom security attributes page is disabled

When signed in to the Azure portal as Global Administrator and you try to access the **Custom security attributes** page, it is disabled.

![Custom security attributes page disabled in Azure portal.](./media/custom-security-attributes-troubleshoot/attributes-disabled.png)

**Cause**

Custom security attributes requires an Azure AD Premium P1 or P2 license.

**Solution**

Open **Azure Active Directory** > **Overview** and check the license for your tenant.

## Symptom - Add attribute set is disabled

When signed in to the Azure portal as Global Administrator and you try to click the **Custom security attributes** > **Add attribute set** button, it is disabled.

**Cause**

You don't have permissions to add an attribute set. To add an attributes set and custom security attributes, you must be assigned the [Attribute Definition Administrator](../roles/permissions-reference.md#attribute-definition-administrator) role.

> [!IMPORTANT]
> By default, [Global Administrator](../roles/permissions-reference.md#global-administrator), [Global Reader](../roles/permissions-reference.md#global-reader), [Privileged Role Administrator](../roles/permissions-reference.md#privileged-role-administrator), and [User Administrator](../roles/permissions-reference.md#user-administrator) do not have permissions to read, filter, define, manage, or assign custom security attributes.

**Solution**

Open **Azure Active Directory** > **Users** > *user name* > **Assigned roles** and check if the Attribute Definition Administrator role is assigned to you. If not, ask your Azure AD administrator to you assign you this role. For more information, see [Assign Azure AD roles to users](../roles/manage-roles-portal.md).

## Symptom - Error when you try to save a custom security attribute assignment

**Cause**

You don't have permissions to assign custom security attributes. By default, [Global Administrator](../roles/permissions-reference.md#global-administrator), [Privileged Role Administrator](../roles/permissions-reference.md#privileged-role-administrator), and [User Administrator](../roles/permissions-reference.md#user-administrator) do not have permissions to read, filter, define, manage, or assign custom security attributes.

**Solution**

Make sure that you are assigned the following Azure AD built-in role:

- [Attribute Assignment Administrator](../roles/permissions-reference.md#attribute-assignment-administrator)

## Symptom - Cannot filter custom security attributes for users or applications

**Cause 1**

To read and filter custom security attributes for users or enterprise applications, you must be assigned the [Attribute Assignment Reader](../roles/permissions-reference.md#attribute-assignment-reader) or [Attribute Assignment Administrator](../roles/permissions-reference.md#attribute-assignment-administrator) role. By default, [Global Administrator](../roles/permissions-reference.md#global-administrator), [Global Reader](../roles/permissions-reference.md#global-reader), [Privileged Role Administrator](../roles/permissions-reference.md#privileged-role-administrator), and [User Administrator](../roles/permissions-reference.md#user-administrator) roles do not have permissions to read, filter, define, manage, or assign custom security attributes.

**Solution 1**

Make sure that you are assigned one of the following Azure AD built-in roles:

- [Attribute Assignment Administrator](../roles/permissions-reference.md#attribute-assignment-administrator)
- [Attribute Assignment Reader](../roles/permissions-reference.md#attribute-assignment-reader)
    
Make sure you have been assigned access to an attribute set at either the tenant level or attribute set level. For more information, see [Manage access to custom security attributes in Azure AD](custom-security-attributes-manage.md).

**Cause 2**

You are assigned the Attribute Assignment Reader or Attribute Assignment Administrator role, but you have not been assigned access to an attribute set.

**Solution 2**

You can delegate the management of custom security attributes at the tenant level or at the attribute set level. Make sure you have been assigned access to an attribute set at either the tenant level or attribute set level. For more information, see [Manage access to custom security attributes in Azure AD](custom-security-attributes-manage.md).

**Cause 3**

There are no custom security attributes defined and assigned yet for your tenant.

**Solution 3**

Add and assign custom security attributes to users or enterprise applications. For more information, see [Add or deactivate custom security attributes in Azure AD](custom-security-attributes-add.md), [Assign or remove custom security attributes for a user](../enterprise-users/users-custom-security-attributes.md), or [Assign or remove custom security attributes for an application](../manage-apps/custom-security-attributes-apps.md).

## Symptom - Custom security attributes cannot be deleted

**Cause**

Currently, you can only activate and deactivate custom security attribute definitions. Deletion of custom security attributes is not supported. Deactivated definitions do not count towards the tenant wide 500 definition limit.

**Solution**

Deactivate the custom security attributes you no longer need. For more information, see [Add or deactivate custom security attributes in Azure AD](custom-security-attributes-add.md).

## Symptom - Cannot add a role assignment at an attribute set scope using PIM

When you try to add an eligible Azure AD role assignment using [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md), you cannot set the scope to an attribute set.

**Cause**

PIM currently does not support adding an eligible Azure AD role assignment at an attribute set scope.


## Next steps

- [Manage access to custom security attributes in Azure AD](custom-security-attributes-manage.md)
- [Troubleshoot Azure role assignment conditions](../../role-based-access-control/conditions-troubleshoot.md)
