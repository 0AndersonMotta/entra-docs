---
title: Archive & report with azure monitor - Azure AD entitlement management
description: Learn how to archive logs and create reports with Azure Monitor in Azure Active Directory entitlement management.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 01/22/2020
ms.author: ajburnle
ms.reviewer: 
ms.collection: M365-identity-device-management


#Customer intent: As an administrator, I want to extend data retention in entitlement management past the default period by using Azure Monitor.

---
# Archiving logs and reporting on Azure AD entitlement management in Azure Monitor

Azure AD stores audit events for up to 30 days in the audit log. However, you can keep the audit data for longer than the default retention period, outlined in [How long does Azure AD store reporting data?](../reports-monitoring/reference-reports-data-retention.md), by routing it to an Azure Storage account or using Azure Monitor. You can then use workbooks and custom queries and reports on this data.


## Configuring Azure AD to use Azure Monitor
Before using the Azure Monitor workbooks, you must configure Azure AD to send a copy of its audit logs to Azure Monitor.

Archiving Azure AD audit logs requires you to have Azure Monitor in an Azure subscription. You can read more about the prerequisites and estimated costs of using Azure Monitor for this scenario in [Azure AD activity logs in Azure Monitor](../reports-monitoring/concept-activity-logs-azure-monitor.md).

**Prerequisite role**: Global admin

1. Sign in to the Azure portal as a user who is a global administrator. Make sure you have access to the resource group containing the Azure Monitor workspace.
 
1. Select **Azure Active Directory** then click **Diagnostic settings** under Monitoring in the left navigation menu. Check if there's already a setting to send the audit logs to that workspace.

1. If there isn't already a setting, click **Add diagnostic setting**. Use the instructions in the article [Integrate Azure AD logs with Azure Monitor logs](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md#send-logs-to-azure-monitor)
to send the Azure AD audit log to the Azure Monitor workspace.

1. After the log is sent to Azure monitor, select **Log Analytics workspaces**, and select the workspace that contains the Azure AD audit logs.

1. Select **Usage and estimated costs** and click **Data Retention**. Change the slider to the number of days you want to keep the data to meet your auditing requirements.

## Creating custom Azure Monitor queries using the Azure Portal
You can create your own queries on Azure AD audit events, including entitlement management events.  

1. In the Azure AD extension in the Azure portal, click on **Logs** under the Monitoring section in the left navigation menu to create a new query page.

1. Your workspace should be shown in the upper left of the query page. If you have multiple Azure monitor workspaces, and the workspace you're using to store Azure AD audit events isn't shown, click **Select Scope** and select the correct subscription and workspace.

1. Next, in the query text area, delete the string “search *” and replace it with the following cmdlet:

    ```sql
    AuditLogs | where Category == "EntitlementManagement"
    ```

1. Then click **Run**. 

The table will show the Audit log events for entitlement management from the last hour by default. You can change the “Time range” setting to view older events. However, changing this setting will only show events that occurred after Azure AD was configured to send events to Azure Monitor.

If you would like to know the oldest and newest audit events held in Azure monitor, use the following query:

```sql
AuditLogs | where TimeGenerated > ago(3653d) | summarize OldestAuditEvent=min(TimeGenerated), NewestAuditEvent=max(TimeGenerated) by Type
```

For more information on the columns that are stored for audit events in Azure monitor, see [Interpret the Azure AD audit logs schema in Azure Monitor](../reports-monitoring/reference-azure-monitor-audit-log-schema.md).

## Creating custom Azure Monitor queries using Azure PowerShell

Once you've configured Azure AD to send logs to Azure Monitor, you can access those logs through PowerShell. You can send queries from scripts or the PowerShell command line, without needing to be a Global Admin in the tenant. 

You'll want to ensure you, the user or service principal authenticating to Azure AD, are in the appropriate Azure role in the Log Analytics workspace. The role options are either Log Analytics Reader or the Log Analytics Contributor.

To set the role assignment and create a query, do the following steps:
1. In the Azure Portal, locate the [Log Analytics workspace](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.OperationalInsights%2Fworkspaces
)

1. Select **Access Control (IAM)**

1. Then click **Add** to add a role assignment

1. Once you have the appropriate role assignment, launch PowerShell, and [install the Azure PowerShell module](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-3.3.0) (if you haven’t already), by typing:

```powershell
install-module -Name az -allowClobber -Scope CurrentUser
```
    
Now you're ready to authenticate to Azure AD, and retrieve the id of the Log Analytics workspace you’re querying.

### Retrieve Log Analytics ID with one Azure subscription
If you have only a single Azure subscription, and a single Log Analytics workspace, then authenticate to Azure AD, connecting to that subscription and retrieving that workspace, by typing:
 
```powershell
Connect-AzAccount
$wks = Get-AzOperationalInsightsWorkspace
```
 
### Retrieve Log Analytics ID with multiple Azure subscriptions

 `Get-AzOperationalInsightsWorkspace` operates in one subscription at a time. So, if you have multiple Azure subscriptions, you'll want to make sure you connect to the one that has the Log Analytics workspace with the Azure AD logs. 
 
 The following cmdlets display a list of subscriptions, and find the id of the subscription that has the Log Analytics workspace:
 
```azurepowershell
Connect-AzAccount
$subs = Get-AzSubscription
$subs | ft
```
 
You can reauthenticate and associate your PowerShell session to that subscription using a command such as `Connect-AzAccount –Subscription $subs[0].id`. To learn more about how to authenticate to Azure from PowerShell, including non-interactively, see [Sign in with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/authenticate-azureps?view=azps-3.3.0&viewFallbackFrom=azps-2.5.0
).

If you have multiple Log Analytics workspaces in that subscription, then the cmdlet `Get-AzOperationalInsightsWorkspace` returns the list of workspaces, so you can find the one that has the Azure AD logs. The CustomerId field returned by this cmdlet is the same as the value of the "Workspace id" displayed in the Azure Portal in the Log Analytics workspace overview.
 
```powershell
$wks = Get-AzOperationalInsightsWorkspace
$wks | ft CustomerId, Name
```

### Send the query to the Log Analytics workspace
Finally, once you have a workspace identified, you can use [Invoke-AzOperationalInsightsQuery](https://docs.microsoft.com/en-us/powershell/module/az.operationalinsights/Invoke-AzOperationalInsightsQuery?view=azps-3.3.0
) to send a Kusto query to that workspace. These queries are written in [Kusto query language](https://docs.microsoft.com/en-us/azure/kusto/query/).
 
For example, you can retrieve the date range of the audit event records from the Log Analytics workspace, with PowerShell cmdlets to send a query like:
 
```powershell
$aQuery = "AuditLogs | where TimeGenerated > ago(3653d) | summarize OldestAuditEvent=min(TimeGenerated), NewestAuditEvent=max(TimeGenerated) by Type"
$aResponse = Invoke-AzOperationalInsightsQuery -WorkspaceId $wks[0].CustomerId -Query $aQuery
$aResponse.Results |ft
```

You can also retrieve entitlement management events using a query like:

```powershell
$bQuery = = 'AuditLogs | where Category == "EntitlementManagement"'
$bResponse = Invoke-AzOperationalInsightsQuery -WorkspaceId $wks[0].CustomerId -Query $Query
$bResponse.Results |ft 
```

Next steps:
- [Create interactive reports with Azure Monitor workbooks](../../azure-monitor/app/usage-workbooks.md) 

