---
title: Microsoft CloudKnox Permissions Management reports - Overview
description: An overview of Microsoft CloudKnox Permissions Management reports.
services: active-directory
author: Yvonne-deQ
manager: karenh444
ms.service: active-directory
ms.subservice: ciem
ms.workload: identity
ms.topic: overview
ms.date: 12/27/2021
ms.author: v-ydequadros
---

# Microsoft CloudKnox Permissions Management reports - overview

Microsoft CloudKnox Permissions Management has various types of reports available that capture specific sets of data. These reports allow management, auditors, and administrators to:

- Make timely decisions.
- Analyze trends and system/user performance.
- Identify trends in data and high risk areas so that management can address issues more quickly and improve their efficiency.



| Report name                | Type of the report                | File format              | Description               | Availability                | Collated report?                 |
|----------------------------|-----------------------------------|--------------------------|---------------------------| ----------------------------|----------------------------------|
| Access Key Entitlement and Usage Report                | Summary </p>Detailed                | CSV                 | This report displays: </p> - Access key age, last rotation date, and last usage date availability in the summary report. Use this report to decide when to rotate access keys. </p> - Granted task and PCI score. This report provides supporting information when you want to take the action on the keys.                | AWS</p>Azure                 | Yes      |
| All Permissions for Identity                 | Detailed                 | CSV                | This report lists all the assigned permissions for the selected identities.                | AWS </p>Azure </p>GCP                | N/A      |
| CIS Benchmarks       | Detailed </p>Summary </p>Dashboard	| CSV </p>PDF	| **Dashboard**: This report tracks the overall progress of the CIS benchmark with this report. This report lists the percentage passing, overall pass or fail of test control along with the breakup of L1/L2 per Auth system. </p> **Summary**: For each authorized system, this report lists the test control pass or fail per authorized system. The number of resources evaluated for each test control. </p> **Detailed**: This report will help auditors and administrators to track the resource level pass or fail per test control.      | AWS </p>Azure </p>GCP                 | Yes      |
| Group Entitlements and Usage       | Summary                | CSV                | This report tracks all group level entitlements and the permission assignment, PCI. The number of members is also listed as part of this report.                 | AWS </p>Azure </p>GCP                 | Yes      |
| Identity Permissions                 | Summary                | CSV                 | This report tracks any, or specific, task usage per **User**, **Group**, **Role**, or **App**.                 | AWS </p>Azure </p>GCP                 | No      |
| Identity Privilege Activity Report                | Summary                | PDF                | This report helps monitor the **Identity Privilege** related activity across the authorized systems. It captures any Identity permission change. </p>This report has the following main sections: **User Summary**, **Group Summary**, **Role Summary & Delete Task Summary**. </p>The **User Summary** lists the current granted permissions along with high-risk permissions and resources accessed in 1-day, 7-day, or 30-days durations. There are subsections for newly added or deleted users, users with PCI change, high-risk active/inactive users. </p>The **Group Summary** lists the administrator level groups with the current granted permissions along with high-risk permissions and resources accessed in 1-day, 7-day, or 30-day durations. There are subsections for newly added or deleted groups, groups with PCI change, High-risk active/inactive groups. </p>The **Role Summary** and the **Group Summary** list similar details. </p>The **Delete Task** summary section lists the number of times the **Delete Task** has been executed in the given period.                | AWS </p>Azure </p>GCP                 | No      |
| NIST 800-53                 | Detailed </p>Summary </p>Dashboard                | CSV </p>PDF                | **Dashboard**: This report helps track the overall progress of the NIST 800-53 benchmark. It lists the percentage passing, overall pass or fail of test control along with the breakup of L1/L2 per Auth system. </p>**Summary**: For each authorized system, this report lists the test control pass or fail per authorized system and the number of resources evaluated for each test control. </p>**Detailed**: This report helps auditors and administrators to track the resource level pass or fail per test control.                | AWS </p>Azure </p>GCP                | Yes      |
| PCI DSS                | Detailed </p>Summary </p>Dashboard                 | CSV                 | **Dashboard**: This report helps track the overall progress of the PCI-DSS benchmark. It lists the percentage passing, overall pass or fail of test control along with the breakup of L1/L2 per Auth system. </p>**Summary**: For each authorized system, this report lists the test control pass or fail per authorized system and the number of resources evaluated for each test control. </p>**Detailed**: This report helps auditors and administrators to track the resource level pass or fail per test control.                | AWS </p>Azure </p>GCP                | Yes      |
| PCI History                 | Summary                 | CSV                 | This report helps track **Monthly PCI History** for each authorized system. It can be used to plot the trend of the PCI.                 | AWS </p>Azure </p>GCP                 | Yes      |
| Permissions Analytics Report (PAR)  |     Detailed                 | CSV                 | This report lists the different key findings in the selected authorized systems. The key findings include **Super identities**, **Inactive identities**, **Over-provisioned active identities**, **Storage bucket hygiene**, **Access key age (AWS)**, and so on. </p>This report helps administrators to visualize the findings across the organization and make decisions.                	| AWS </p>Azure </p>GCP                | Yes      |
| Role/Policy Details                 | Summary                 | CSV                | This report captures **Assigned/Unassigned** and **Custom/system policy with used/unused condition** for specific or all AWS accounts. </p>Similar data can be captured for Azure and GCP for assigned and unassigned roles.                | AWS </p>Azure </p>GCP                 | No      |
| User Entitlements and Usage     | Detailed <p>Summary                 | CSV                 | This report provides a summary and details of **User entitlements and usage**. </p>**Data displayed on Usage Analytics** screen is downloaded as part of the **Summary** report. </p>**Detailed permissions usage per User** is listed in the Detailed report.                 | AWS </p>Azure </p>GCP                | Yes      |
| Well-Architected Framework                 | Detailed </p>Summary </p>Dashboard                 | CSV </p>PDF                 | **Dashboard**: This report tracks the overall progress of the AWS- Well-Architected Framework. It lists the percentage passing, overall pass or fail of test control and the breakup of L1/L2 per Auth system. </p>**Summary**: For each authorized system, this report lists the test control pass or fail per authorized system. The number of resources evaluated for each test control. </p>**Detailed**: This report helps auditors and administrators track the resource level pass or fail per test control.                | AWS only                | Yes      |




<!---## Next steps--->