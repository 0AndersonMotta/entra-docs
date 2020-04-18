---
title: Review and clean up collaboration partners from external organizations using Azure Active Directory Access Reviews| 
description: Use Access Reviews to extend of remove access from members of partner organizations
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/18/2020
ms.author: barclayn
---


# Use Access Reviews to review and clean up collaboration partners from external organizations

The cloud makes it easier than ever to collaborate with colleagues and friends within your organization, or beyond organizational boundaries. Embracing Office 365, organizations start to see a proliferation of external identities (“Guests”), as users use capabilities to jointly work on data, documents or digital workspaces such as Teams. Organizations need to balance between user-initiated collaboration for greatest possible flexibility and processes and guardrails, to allow security and governance requirements to be fulfilled. Part of these efforts should be evaluating and cleaning out no longer needed external partners and identities from resources and the Azure AD tenant.

>[!Note]
>A valid Azure AD Premium P2, Enterprise Mobility + Security E5 paid, or trial license is required to use Azure AD access reviews. For more information, see [Azure Active Directory editions](../fundamentals/active-directory-whatis.md).

## Why should you review partners from external organizations that collaborate with you in your tenant?

While the process of inviting business partners and vendors for collaboration is user-initiated, organizations need to provide resource owners and end users a way to regularly evaluate and attest the partners they invited. For many projects, onboarding of new collaboration partners is planned and accounted for, however offboarding or the removal of users is often neglected. 
Also, for identity lifecycle management reasons, IT organizations may want to keep Azure AD clean, and remove users that no longer have and need access to the organization’s resources. Keeping only the relevant identity references for partners and vendors in the directory also helps reduce the risk of your users inadvertently selecting and granting permission to a partner that’s still there.

This article describes methods and functionality that allow for finding and reviewing external identities, and – should they no longer be needed – remove them from Azure AD. 

## Use Powershell to find partners from a domain

```powershell
Get-AzureADUser -Filter "usertype eq 'Guest'" -All $true | ?{ $_.mail -like
"@microsoft.com" }

$users.Count
```

The Azure AD Governance team has a sample script published on GitHub, that allows for searching, categorizing and building a simple inventory of where external identities are used in Azure AD. You can find the script here:

## Create a dynamic group that has external partners as members

You can use Azure AD dynamic groups to build groups that structure external identities based on their home domain. The dynamic group will be kept up-to-date and, should new partners and  vendors from the same domain, reflect changes in the membership of the group. These groups can later be used to perform Access Reviews on them, or run a script that performs actions on members of these groups.

Administrators create a new dynamic group that contains external identities in Azure AD with the following steps. In this example, all external users from the “microsoft.com” domain are used:

1. Open the Azure AD Portal – select “Azure Active Directory” \> Groups
2. Select “+ New Group” in the top menu.
3. In the “New Group” blade, select “Security” as the Group Type, choose a group name, (in this example “EXTERNALS_FROM_MICROSOFT”), select “Security” and Membership type of “Dynamic User”
4. In “Owners”, choose the rightful owner for this dynamic group. This can be someone who owns the business relationship with the partner company. 
5. Click “Add dynamic query”.
6. In dynamic membership rules”, use the following query to scope the group to contain all external identities in your tenant from the “microsoft.com” domain. Click the “Edit” link on the Rule syntax” box to enter the following filter:

```
  (user.userPrincipalName -contains "#EXT#") and (user.mail -contains "@microsoft.com") and (user.accountEnabled -eq true)
```

 For other domains, replace “microsoft.com” in this sample with the target domains that you look for. ![A screenshot of a video game Description automatically generated](media\access-reviews-external-domains\461d614073eaed08001d1ddd68941411.png)
7. Click Save. Click Create.

The query above is looking for external identities that were invited through Azure AD or Office 365, that are still enabled and can work in your tenant.

### Example: Access Review external partners from a specific domain

1. Navigate to the Azure Active Directory admin center and navigate to “Azure Active Directory”>“Identity Governance”>“Access Reviews”
2. Select “+ New Access Review” from the top menu.

In the “Create an access review” setup page, you define the settings of a new Access Review. For this scenario, asking a Teams team owner to review the membership list every three months. Upon  declining members, the system will remove users at the end of each review cycle.

1. Provide a “Review name”, a “Start date” for the review cycle.
2. Select “Quarterly” in the “Frequency” drop down list.
3. Select “Never” for the setting to “End” the Access Review cycles.
4. In the “Users” section of the setup page, select “Members of a group” for “Users to review” and select “Guest users only”, so that reviewers get a chance to review all external users from the dynamic group.
5. In “Group”, select the dynamic group that you created earlier, containing all external partners from a specific domain.
6. Under “Reviewers”, select one of the following options:
      - **Group owners** – if you have assigned an owner to the dynamic group, and that owner is the person who manages the relationship with the external partner.
      - **Selected users** – if you want to specify specific users in your organization that should review the partners from that external company.
      - **Members (self)** – if you want to allow all external partners to self-attest whether they need continued access to your company’s resources.

7.  In “Upon completion settings”, specify that you do not want to take any actions once the review is completed. This will allow to review the results and take manual actions.
8.  Click “Start” to create the Access Review. The review will automatically start at the specified start date – and notify reviewers.

![A screenshot of a cell phone Description automatically generated](media\access-reviews-external-domains\41af935e14610985740dc4c2852b6625.png)

Once the review is completed, the “Results” page of the Access Review will contain an overview of how Reviewers decided on every external partner – and whether continued access through these external partners is desired.
External partners that are no longer needed, as attested by the reviewers, can be removed through the Azure AD Portal or a Powershell script.

## Example: Powershell: Disable and delete external partners from the results CSV file, following an Access Review

Administrators or resource owners can take action on the results from Access Reviews. The result of all access reviews is available both in the access reviews section of the Azure AD Portal,  as well as a download. All results are kept as comma separated value (CSV) files, that can be used for manual processing and reporting or programmatic action-taking, through Powershell.

The results in a CSV format is available on an access review, in the “Results” menu. Using “Download”, the browser will download the CSV file.

The following script parses the results:

```powershell

$csv = Import-CSV "C:\Users\johndoe\Downloads\data.csv" #the CSV downloaded from Azure AD Portal

$deniedUsers = @()

$allowedUsers = @()

#Read the CSV file. For every line, do...

$csv | foreach {

# Read three columns/values for every line and save them:

$user = $_.UserID

$user_displayName = $_."User Display Name"

$result = $_.Outcome

# Depending on the result, put the user in one of two arrays (approved/denied):

switch($result) {

'Approve' { $allowedUsers += $user}

'Deny' { $deniedUsers += $user }

default { Write-Host "Unexpected result, $($result)"}

}

}

foreach($u in $deniedUsers)

{

Write-Host "Remove-AzureADGroupMember -ObjectID <objectID of reviewed group>
-MemberId $u"

}
```

## Example: Powershell: Disable and delete external partners using Graph, following an Access Review


Florian commented:

Complete the script and have Chief Inspector Wahl review. Then upload to GitHub and reference here.

..

## Example: Disable and delete external partners from a specific domain using Access Reviews



