---
title: 'Azure Active Directory Terms of Use. | Microsoft Docs'
description: Azure AD Terms of Use will allow you and your company the ability to provide terms of use to users of Azure AD servcies.
services: active-directory
documentationcenter: ''
author: billmath
manager: femila
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2017
ms.author: billmath

---

# Azure Active Directory Terms of Use (Preview)
Azure AD Terms of Use provides a simple method for an organization to ensure that end users see relevant disclaimers or other information needed for legal or compliance requirements. 

Azure AD Terms of Use uses the pdf format to allow organizations to present content to end-users.   This can be any content, such as existing contract documents, allowing you to collect end-user agreements during user sign-in.  Futhermore, they can use the terms of use for applications, groups of users, or if you have multiple terms of use for different purposes.

Azure AD Terms of Use uses the pdf format to present this to your end-users.  

This remainder of this document describes how to get going with Azure AD Terms of Use.  

## Why use Azure AD Terms of Use
Finding it difficult to get employee’s or guests to agree to your terms of use before getting access? Need help figuring out who has or hasn’t agreed to your company terms of use?  With Azure AD Terms of use you now have a simple method that your organization can use to ensure that end users see relevant disclaimers or other information needed for legal or compliance requirements prior to getting access. 

Azure AD Terms of Use can be used in the following scenarios:
-	General terms of use for all users in your organization
-	Specific terms of use based on a user attributes (ex. doctors vs nurses or domestic vs international employees) (using dynamic groups)
-	Specific terms of use based on accessing high business impact apps, like Salesforce


## Azure AD Terms of Use Prerequisites
Use the following steps to configure Azure AD Terms of Use:

1. Sign in to Azure AD as a global administrator in the directory where you want to setup Azure AD Terms of Use.
2. Ensure that the directory has an Azure AD Premium P1, P2, EMS E3 or EMS E5 subscription.  If you do not [Get Azure AD Premium](active-directory-get-started-premium.md) or [start a trial](https://azure.microsoft.com/trial/get-started-active-directory/).
3. View the Azure AD Terms of User dashboard at [https://aka.ms/aadconnecthealth](https://aka.ms/aadtou).

## About Azure AD Terms of Use
When you setup Azure AD Terms of Use you start in the Azure AD Terms of user screen.  

![Terms of Use](media/active-directory-tou/tou1.png)


On the screen you will notice buttons for Adding, Publishing and Unpublishing terms of use. Azure Terms of Use have 3 distinct states.  These states describe the status of the individual Terms of Use that you are using.  

You can have multiple terms of use that target different user groups.  Currently Azure AD Terms of Use supports X number of individual terms of use.  The following table describes the states in more detail.

|Status|Description|
|-----|-----|
|Published|A terms of use that is currently live and users need to actively accept it.|
|Not yet Published|A terms of use that has never been published.|
|Unpublished|A terms of use that was once published but is no longer published.|

## Adding a Terms of Use
Once you have finalized your Terms of Use and you are ready to use it with Azure services you can use the following procedure to add it

### To add a terms of use
1. Navigate to the dashboard at [https://aka.ms/catou](https://aka.ms/catou)
2. Click Add.</br>
![Add TOU](media/active-directory-tou/tou2.png)
3. Enter the **Name** for the Terms of Use
4. Enter **Display Name**.  This is the header that users will see when they sign in.
5. **Browse** to your finalized terms of use pdf and select it.
6. You can **Enforce** the terms of use using one of the either a template or custom conditional access policy
7. Click **Create**.
8. If you selected a custom conditional access template than a new screen will appear which allows you to customize the CA policy.
7. You should now see your new Terms of Use.</br>

![Add TOU](media/active-directory-tou/tou3.png)

## Publishing a Terms of Use
When you are ready to publish the terms of use so that your users will start seeing it and accepting it use the following procedure.

1. Select your terms of use and click publish.</br>
![Publish TOU](media/active-directory-tou/tou4.png)
2. Click **Yes** to confirm.
3. You should now see the status set to publish.

![Publish TOU](media/active-directory-tou/tou5.png)


## Unpublishing a Terms of Use
When you are ready to unpublish the terms of use so that update it or change it, use the following procedure.

1. Select your terms of use and click unpublish.</br>
![Unpublish TOU](media/active-directory-tou/tou7.png)
2. Click **Yes** to confirm.
3. You should now see the status set to unpublish.</br>
![Unpublish TOU](media/active-directory-tou/tou6.png)

## Auditing a Terms of Use
Azure AD Terms of Use provides easy to use auditing so that you can see who has accepted and when they accepted your terms of use.  To get started with auditing use the following procedure:

### To audit terms of use
1. Navigate to the dashboard at [https://aka.ms/catou](https://aka.ms/catou)
2. Click Audit Event.</br>
![Audit Event](media/active-directory-tou/tou8.png)
3.  On the Azure AD audit logs screen, you can filter the information using the provided drops downs to target specific audit log information.
![Audit Event](media/active-directory-tou/tou9.png)
4.  You can also download the information in a .csv file for use locally.

## End user Preview
Once a terms of use is created an enforced this is what an end user will see if they are in scope.
-	Best practice is to have the font within the PDF at size 24.
![Audit Event](media/active-directory-tou/tou10.png)
-	Will be shown on mobiles as well
![Audit Event](media/active-directory-tou/tou11.png)

## Troubleshooting
The following is some general information that you need to be aware of and can assist with troubleshooting any terms of use issues.

-	Global administrator, security administrator, or conditional access administrator need read/write access.
-	After creating a TOU using the “access to cloud apps” template the admin will start to see “sad clouds” when access other areas of the portal. If they refresh the browser they will start to see “AAD token issues”
    - This behavior is expected and the reason for this is that the user created a new conditional access policy that they are in scope of. The new policy is not satisfied so they are unable to access any cloud apps until that policy in satisfied.
    - Resolution: in order to resolve the user must sign out and sign back in. This will allow them to satisfy any remaining controls that they may now fall in scope of.
-	If a tenant already has a CA policy enforced with a TOU, the next time a TOU is created the admin (and all active sessions) will start to see “sad clouds” when access other areas of the portal. If they refresh the browser they will start to see “AAD token issues.”
    - This behavior is expected and due to limitation in the CA extensibility framework.
    - Resolution: in order to resolve the user must sign out and sign back in. This will allow them to get a fresh token. 
-	If  TOU is enforced using a custom CA policy, and the admin wants to delete a TOU, they need to make sure that TOU is not enforced with any policies.