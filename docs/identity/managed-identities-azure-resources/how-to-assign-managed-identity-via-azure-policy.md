---
title: [Preview] Use Azure Policy to assign managed identities - Azure Active Directory
description: Documentation for the Azure Policy that can be used to assign managed identities to Azure resources.
services: active-directory
documentationcenter: 
author: vakarand
manager: skwan
editor: 
ms.service: active-directory
ms.subservice: msi
ms.devlang: 
ms.topic: how-to
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 5/16/2022
ms.author: vakarand
ms.collection: M365-identity-device-management
ms.custom: 
---
# [Preview] Use Azure Policy to assign managed identities
## Overview
[Azure Policy](../../governance/policy/overview.md) helps to enforce organizational standards and to assess compliance at-scale. Through its compliance dashboard, it provides an aggregated view to evaluate the overall state of the environment, with the ability to drill down to the per-resource, per-policy granularity. It also helps to bring your resources to compliance through bulk remediation for existing resources and automatic remediation for new resources. Common use cases for Azure Policy include implementing governance for resource consistency, regulatory compliance, security, cost, and management. Policy definitions for these common use cases are already available in your Azure environment as built-ins to help you get started. 

Azure Monitoring Agents require a [managed identity](overview.md) to be available on the Azure Virtual Machines (VMs) that is being monitored. This document describes the behavior of a built-in Azure Policy provided by Microsoft to ensure a managed identity needed for these scenarios is assigned to VMs at scale. 

While using system-assigned managed identity is possible, when used at scale (for example, for all VMs in a subscription) it results in substantial number of identities created (and deleted) in Azure AD (Azure Active Directory). To avoid this churn of identities, it is recommended to use user-assigned managed identities, which can be created once and shared across multiple VMs. 
> We recommend using a user-assigned managed identity per Azure subscription per Azure region.

The policy is designed to implement this recommendation. 

You can view the policy definition over here. 

When executed, the policy takes the following actions: 
1.	Create, if not exist, a new built-in user-assigned managed identity in the subscription and each Azure region based on the VMs that are in scope of the policy. 
2.	Once created, put a lock on the user-assigned managed identity so that it will not be accidentally deleted. 
3.	Assign the built-in user-assigned managed identity to Virtual Machines from the subscription and region based on the VMs that are in scope of the policy. 
> If the Virtual Machine has exactly 1 user-assigned managed identity already assigned, then the policy skips this VM to assign the built-in identity. This is to make sure assignment of the policy does not break applications that take a dependency on the default behavior of the token endpoint on IMDS.

There are two scenarios to use the policy: 
1.	Let the policy create and use a “built-in” user-assigned managed identity.
2.	Bring your own user-assigned managed identity.

The policy takes three input parameters: 
1.	Bring-Your-Own-UAMI? - Should the policy create, if not exist, a new user-assigned managed identity? 
2.	If set to true, then you must specify
- Name of the managed identity
- Resource group in which the managed identity should be created. 
3.	If set to false, then no additional input is needed.
- The policy will create the required user-assigned managed identity called “built-in-identity” in a resource group calle “built-in-identity-rg".

## Using the policy
### Creating the policy assignment

The policy definition can be assigned to different scopes in Azure – at the management group subscription or a specific resource group. As policies need to be enforced all the time, the assignment operation is performed using a managed identity associated with the policy-assignment object. The policy assignment object supports both system-assigned and user-assigned managed identity. 
For example, Joe can create a user-assigned managed identity called PolicyAssignmentMI. The built-in policy creates a user-assigned managed identity in each subscription and in each region with resources that are in scope of the policy assignment. The user-assigned managed identities created by the policy has the following resourceId format: 

> /subscriptions/<subscription-id>/resourceGroups/built-in-identity-rg/providers/Microsoft.ManagedIdentity/userAssignedIdentities/built-in-identity-<location>

For example: 
> /subscriptions/<subscription-id>/resourceGroups/built-in-identity-rg/providers/Microsoft.ManagedIdentity/userAssignedIdentities/built-in-identity-eastus

### Required authorization 

For PolicyAssignmentMI managed identity to be able to assign the built-in policy across the specified scope, it needs the following permissions, expressed as an Azure RBAC (Azure role-based access control) Role Assignment:

| Principal| Role / Action | Scope | Purpose |
|----|----|----------------|----|
|PolicyAssigmentMI |Managed Identity Operator | /subscription/<subscription-id>/resourceGroups/built-in-identity <br> OR <br>Bring-your-own-User-assinged-Managed identity |Required to assign the built-in identity to VMs.|
|PolicyAssigmentMI |Contributor | /subscription/<subscription-id>/ |Required to create the resource-group that holds the built-in managed identity in the subscription. |
|PolicyAssigmentMI |Managed Identity Contributor | /subscription/<subscription-id>/resourceGroups/built-in-identity |Required to create a new user-assigned managed identity.|
|PolicyAssigmentMI |User Access Adminstrator | /subscription/<subscription-id>/resourceGroups/built-in-identity <br> OR <br>Bring-your-own-User-assinged-Managed identity |Required to set a lock on the user-assigned managed identity created by the policy.|

As the policy assignment object must have this permission ahead of time, PolicyAssignmentMI cannot be a system-assigned managed identity for this scenario. The user performing the policy assignment task must pre-authorize PolicyAssignmentMI ahe3ad of time with the above role assignments.
As you can see the resultant least privilege role required is “contributor” at the subscription scope.



## Known issues

1. Possible race condition with another deployment that changes the identities assigned to a VM can result in unexpected results.

If there are two or more parallel deployments updating the same virtual machine and they all change the identity configuration of the virtual machine, then it is possible, under specific race conditions, that all expected identities will NOT be assigned to the machines. 
For example, if you the policy in this document is updating the managed identities of a VM and at the same time another process is also making changes to the managed identities section, then it is not guranteed that all the expected identities are proerly assigned to the VM. 