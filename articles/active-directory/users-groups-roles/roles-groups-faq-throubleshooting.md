---
title: Assign a role to a group frequently asked questions - Azure Active Directory | Microsoft Docs
description: Assign an Azure AD role to a role-eligible group in the Azure portal, PowerShell, or Graph API.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 05/13/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro

ms.collection: M365-identity-device-management
---

# Assigning roles to groups FAQ and troubleshooting in Azure ACtive Directory

These are some common questions and troubleshooting tips for assigning roles to groups in Azure Active Directory (Azure AD).

**Q:** I'm a Groups Administrator but I can't see 'Eligible for role assignment' toggle.
**A:** Only Privileged role administrator or Global Administrator can create a group that is eligible for role assignment. Only in these two roles can see this control.

**Q:** Who can modify the membership of groups that is assigned to Azure AD role?
**A:** By default, only  Privileged Role Administrator and Global Administrator can manage the membership of a role-eligible group, but you can delegate the management of role-assignable groups by adding group owners.

**Q:** I can't update password of a user. He does not have any higher privileged role assigned. Why is it happening?
**A:** It might be because the user could be an owner of a role-eligible group. We protect owners of role-eligible groups to avoid elevation of privilege. For example, suppose there is a group Contoso_Security_Admins which is assigned to Security Administrator built-in role. Bob is owner of this group. Alice is Password Administrator in the tenant. If this protection is not present, Alice can reset Bob's credentials and take over his identity. After that, Alice can add herself or anyone she likes to the group Contoso_Security_Admins and become Security Administrator in the tenant. In this scenario, get the list of owned objects of that user and see if any of the group has isAssignableToRole set to true or not. If yes, then that user is protected and the behavior is by design. Refer to these documentations for getting owned objects:

- [Get-AzureADUserOwnedObject](https://docs.microsoft.com/powershell/module/azuread/get-azureaduserownedobject?view=azureadps-2.0)  
- [List ownedObjects](https://docs.microsoft.com/graph/api/user-list-ownedobjects?view=graph-rest-1.0&tabs=http)

**Q:** Can I create an access review on groups that can be assigned to Azure AD roles (i.e. those with isAssignableToRole property set to true)?  
**A:** Yes, you can. If you are on newest version of Access Review (your reviewers are directed to My Access by default) , then only Global Administrator can create access review on role-assignable groups. However, if you are on older version of Access Review (your reviewers are directed to the Access Panel by default), then both Global Administrator and User Administrator can create access review on role-assignable groups. The new experience will be rolled out to all customers on July 28th, 2020 but if you’d like to upgrade sooner, make a request to [Azure AD Access Reviews - Updated reviewer experience in My Access Signup](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR5dv-S62099HtxdeKIcgO-NUOFJaRDFDWUpHRk8zQ1BWVU1MMTcyQ1FFUi4u).

**Q:** Can I create an access package and put groups that can be assigned to Azure AD roles (i.e. those with isAssignableToRole property set to true) in it? 
**A:** Yes, you can. Earlier Global Administrator and User Administrator had unfettered power to put any group in an access package. Nothing changes for Global Administrator but there is a slight change in User Administrator behavior. To put a role-assignable group into an access package you must be an User Administrator plus owner of that role-assignable group. Here's the full table showing who can create access package in Enterprise License Management  

Azure AD directory role | Entitlement management role | Can add security group\* | Can add Office 365 Group\* | Can add app | Can add SharePoint Online site
----------------------- | --------------------------- | ----------------------- | ------------------------- | ----------- |  -----------------------------
Global administrator | n/a | ✔️ | ✔️ | ✔️  | ✔️
User administrator  | n/a  | ✔️  | ✔️  | ✔️
Intune administrator | Catalog owner | ✔️  | ✔️  | &nbsp;  | &nbsp;
Exchange administrator  | Catalog owner  | &nbsp; | ✔️  | &nbsp;  | &nbsp;
Teams service administrator | Catalog owner  | &nbsp; | ✔️  | &nbsp;  | &nbsp;
SharePoint administrator | Catalog owner | &nbsp; | ✔️  | &nbsp;  | ✔️ 
Application administrator | Catalog owner  | &nbsp;  | &nbsp; | ✔️  | &nbsp;
Cloud application administrator | Catalog owner  | &nbsp;  | &nbsp; | ✔️  | &nbsp;
User | Catalog owner | Only if group owner | Only if group owner | Only if app owner  | &nbsp;

\*Group is not role-eligible; that is, isAssignableToRole = false. If group is role-eligible, then the person creating the access package must also be owner of that role-eligible group.

**Q:** I cannot find "Remove assignment" option in "Assigned Roles". How do I delete role assignment to a user? 
**A:** Applicable only to P1 tenants. Navigate to Azure Portal --> Azure Active Directory --> Users --> {user} --> Assigned roles. Tap on one of the roles entries, it brings up a drawer from the bottom of the screen. This drawer has the information of if it has been assigned directly or indirectly. There is a "Remove" button besides direct assignment. To remove indirect role assignment, remove the user from the group that has been assigned the role.

**Q:** How do I see all groups that are role-eligible; that is ones for which "Eligible for role assignment" is switched on?
**A:** Navigate to Azure Portal --> Azure Active Directory --> Groups --> All Groups --> Add filters --> Role assignable.

**Q:** How do I know which role are assigned to a principal directly and indirectly? 
**A:**

- In Azure AD P1 license tenants: Navigate to Azure Portal --> Azure Active Directory --> Users --> {user} --> Assigned roles --> Click on 'gear' icon. A bottom drawer will come up that can give this information. 
- In Azure AD P2 license tenants: Navigate to Azure Portal --> Azure Active Directory --> Users --> {user} --> Assigned roles. You will find direct vs. inherited information under 'Membership' column. 

**Q:** Why do we enforce creating a new cloud group for assigning it to role?  
**A:** If you assign an existing group to a role, the existing group owner can add other members to this group without realizing that they will have the role. Moreover, since these groups are special, we are putting lots of restrictions to protect such groups. Suddenly, there could be lots of changes to this group that would be surprising to the person managing the group.

## Next steps

- [Create a role-eligible group](roles-groups-create-eligible.md)
- [Assign a role to a group](roles-groups-assign-role.md)
- [View a group's role assignments](roles-groups-view-assignments.md)
- [Remove a group role assignment](roles-groups-remove-assignment.md)
