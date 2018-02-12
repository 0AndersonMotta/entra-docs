---
title: Azure AD Domain Services - Check the health of your managed domain | Microsoft Docs
description: Check the health of your managed domain using the health page in the Azure portal.
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand

ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: ergreenl

---
# Azure AD Domain Services - Check the health of your managed domain

## Your domain's health

Using the health page on your Azure portal, you are able to keep up-to-date on what is happening on your managed domain. This article walks through the elements of the health page.

### View your health page

1. Navigate to the Azure AD Domain Services page in the [Azure portal](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices)
2. Click on the domain you want to view the health of.
3. In the left-hand navigation, click "Health".

The following picture is an example health page.

![Example health page](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Your managed domain's health is evaluated every hour. After making changes to your managed domain, wait until the next evaluation cycle to view your domain's updated health. The "Last evaluated" timestamp in the top right corner shows when the health of your managed domain was last evaluated.
>

### Status of your managed domain
The status in the top right of your health page indicates the overall health of your domain. The status factors in all of the existing alerts on your domain. You can also view the status of your domain on the overview page of Azure AD Domain Services.

| Status | Icon | Explanation |
| --- | :----: | --- |
| Running | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | Your managed domain is running smoothly and does not have any critical or warning alerts. This domain may have informational alerts. |
| Needs attention (Warning) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | There are no critical alerts on your managed domain, but there are one or more warning alerts that need to be addressed. |
| Needs attention (Critical) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | There are one or more critical alerts on your managed domain. You may also have warning and/or informational alerts. |
| Deploying | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | Your domain is in the process of being deployed. |

## Monitors
Monitors are aspects of your managed domain that Azure AD Domain Services monitors on a regular basis. The best way to keep your monitors in a healthy state is to resolve any alerts on your managed domain.

The monitors currently available are:
 - Backup
 - Synchronization with AAD

### Backup
This monitors whether regular backups of your managed domain are being performed. The following table explains what to expect in the details column of the backup monitor:

| Detail value | Explanation |
| --- | --- |
|"Never backed up" | This state is normal for a newly created managed domain. The first backup is usually created after 24 hours. If your managed domain is not newly created or you see this state for an abnormal amount of time, [contact support](active-directory-ds-contact-us.md). |
| Last backup was taken 1 to 14 days ago | In general, this is the expected value of the backup monitor. |
| Last backup was taken more than 14 days ago. | Any time longer than two weeks is an unusually long time since your last backup. Active critical alerts may prevent your managed domain from being backed up on a regular basis. First, resolve any active alerts for your managed domain, and then if the issue still persists, [contact support](active-directory-ds-contact-us.md). |


### Synchronization with Azure AD
Microsoft monitors how often your managed domain is synchronized with Azure Active Directory. The number of objects (users & groups) in your managed domain as well as the amount of changes made since the last sync can both affect how long a synchronization period can take. In general, if it has been longer than three days since your managed domain was last synchronized, [contact support](active-directory-ds-contact-us.md).

## Alerts
Alerts are generated for issues on your managed domain that need to be addressed in order for Azure AD Domain Services to run. Each alert explains the issue and provides a resolution URL that outlines specific steps to resolve the issue. To view all alerts and their resolutions, visit the [Troubleshoot alerts](active-directory-ds-troubleshoot-alerts.md) article.

### Severity
Alerts are categorized into three different levels of severity: critical, warning, and informational.

 * **Critical alerts** are issues that severely impact your managed domain. These alerts should be addressed immediately, as Microsoft cannot monitor, manage, patch, and synchronize your managed domain.
 * **Warning alerts** can be issues that may impact your domain in the future, but are not necessarily "breaking" your service. These alerts outline best practices and give suggestions to protect your managed domain.
 * **Informational alerts** are notifications that are not negatively impacting your domain. Informational alerts are designed to keep you knowledgeable about what is happening in your domain and Azure AD Domain Services.

## Next steps
- [Resolve alerts on your managed domain](active-directory-ds-troubleshoot-alerts.md)
- [Read more about Azure AD Domain Services](active-directory-ds-features.md)
- [Contact Us](active-directory-ds-contact-us.md)
