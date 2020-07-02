---
title: Replica sets concepts for Azure AD Domain Services | Microsoft Docs
description: Learn what replica sets are in Azure Active Directory Domain Services and how they provide redundancy to applications that require identity services.
services: active-directory-ds
author: iainfoulds
manager: daveba

ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: iainfou
---

# Replica sets concepts and features for Azure Active Directory Domain Services

When you first create an Azure Active Directory Domain Services (Azure AD DS) managed domain, you create a unique namespace, the domain name, with two domain controllers (DCs). This deployment of two DCs is known as a replica set. In its current form, Azure AD DS only allows one unique namespace per Azure AD tenant and only one replica set per Azure AD tenant.

Replica sets expand Azure AD DS by allowing the existing namespace to have more than one replica set per tenant. This ability provides geographical disaster recover for a managed domain. You can add a replica set to any peered virtual network in any Azure region that supports Azure AD DS. Additional replica sets for a managed domain in different Azure regions provide geographical disaster recovery for legacy applications if an Azure region goes offline.You can add a replica set to a virtual network that already has an existing replica set provided the new replica set resides in a different virtual subnet to offer additional high availability in a given region.

> [!NOTE]
> Replica sets don't let you deploy multiple unique managed domains in a single Azure tenant.

This conceptual article explains what replica sets are, and how they provide redundancy for applications that require identity services. Replica sets are currently in preview.

## How replica sets work

The first replica set for a domain name anchors the domain configuration. Additional replica sets share the same domain namespace and configuration.

You create each replica set in a virtual network. Each virtual network is peered to every other virtual network that hosts an Azure AD Domain Services replica set. This creates a fully-meshed network topology that supports directory replication.

Users, groups, group memberships, and password hashes are replicated using normal intrastate replication to provide the quickest convergence possible.

The following diagram shows a managed domain with two replica sets. The first replica set created with the domain namespace and a second replica set:

![Diagram of example managed domain with two replica sets](./media/concepts-replica-sets/two-replica-set-example.png)

> [!NOTE]
> Replica sets ensure availability of authentication services in regions where a replica set is configured. For an application to have geographical redundancy if there's a regional outage, the application platform that relies on the managed domain must also reside in the other region.
>
> Resiliency of other services required for the application to function, such as Azure VMs or Azure
App Services, isn't provided by replica sets. Availability design of other
application components needs to consider resiliency features for services that make up the application.

The following example shows a managed domain with three replica sets to further provide resiliency and ensure availability of authentication services:

![Diagram of example managed domain with three replica sets](./media/concepts-replica-sets/three-replica-set-example.png)

## Deployment considerations

The default SKU for a managed domain is the *Enterprise* SKU, which supports multiple replica sets. If you change the SKU to *Standard*, [upgrade the managed domain](change-sku.md) to *Enterprise* or *Premium* to create additional replica sets. The maximum number of replica sets supported during preview is four. This includes the first replica created when you created the managed domain.

If you use an existing Azure AD tenant that has secure LDAP enabled, check when you TLS certificate expires before you add a replica set. If the certificate expires in the next six months, we
encourage you to renew it and upload it now using the Azure portal.

Azure bills each replica set based on the domain configuration SKU. For example, if you have a managed domain that uses the *Enterprise* SKU and you have three replica sets, Azure bills your
subscription per hour for each of the three replica sets

## Frequently asked questions

### Can I use my production managed domain with this preview?

We strongly encourage you to use a test tenant while replica sets are in preview. There's nothing to prevent you from using a production environment, though refer to the [Azure Active Directory Preview SLA](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).

### Can I create a replica set in subscription different from my managed domain?

No. Replica sets must be in the same subscription as the managed domain.

### How many replica sets can I create?

The preview is limited to a maximum of 4 replica sets - three replicas, plus the initial replica set.

### How does user and group information get synchronized to my replica sets?

All replica sets are connected to each other using a mesh virtual network peering. One replica set receives user and group updates from Azure AD. Those changes are then replicated to the other replica sets using normal Active Directory replication over the peered network. Just like with on-premises Active Directory Domain Services, an extended disconnected state can cause disruption in replication. As peered virtual networks aren't transitive, the design requirements for replica sets requires a fully meshed network topology.

### How do I make changes in my managed domain after I have replica sets?

Changes within the managed domain work just like they previously did. You use a management VM with the RSAT tools that is.

## Next steps

To get started with creating an Azure AD DS managed domain with replica sets, see [Create and configure an Azure AD DS managed domain][tutorial-create-advanced].

<!-- LINKS - INTERNAL -->
[tutorial-create-advanced]: tutorial-create-instance-advanced.md
