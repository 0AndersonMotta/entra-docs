---
title: Bulk upload to add or create members of a group - Azure Active Directory | Microsoft Docs
description: Add group members in bulk in the Azure Active Directory admin center. 
services: active-directory 
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 04/27/2020
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
---

# Bulk import group members in Azure Active Directory

Using Azure Active Directory (Azure AD) portal, you can add a large number of members to a group by using a comma-separated values (CSV) file to bulk import group members.

## Understand the bulk upload spreadsheet

Use the bulk upload spreadsheet as the template to fill in to successfully add Azure AD group members in bulk. Your spreadsheet might look like this example:

![Spreadsheet for upload and call-outs explaining the purpose and values for each row and column](./media/groups-bulk-import-members/template-with-callouts.png)

The first two rows of the upload template must not be removed or modified, or the upload can't be processed. We have included one row of examples of acceptable values. You must remove the examples row and replace it with your entries. Any additional columns you add are ignored and not processed. The required columns are listed first.

> [!Note]
> Older versions of the template might not have the examples row. We recommend that you download the latest version of the template as often as possible.

The format of the column headings is &lt;*Item name*&gt; [PropertyName] &lt;*Required or blank*&gt;. For example, `Name [displayName] Required`. Some older versions of the template might have something like `Name (example: Chris Green) [displayName] *`.

For groups, format is the same but you have the option to decide which identifier you use. An example of one such column heading is `Member object ID or user principal name [memberObjectIdOrUpn] Required`.

## To bulk import group members

1. Sign in to [the Azure portal](https://portal.azure.com) with a User administrator account in the organization. Group owners can also bulk import members of groups they own.
1. In Azure AD, select **Groups** > **All groups**.
1. Open the group to which you're adding members and then select **Members**.
1. On the **Members** page, select **Import members**.
1. On the **Bulk import group members** page, select **Download** to get the CSV file template with required group member properties.

    ![The Import Members command is on the profile page for the group](./media/groups-bulk-import-members/import-panel.png)

1. Open the CSV file and add a line for each group member you want to import into the group (required values are either **Member object ID** or **User principal name**). Then save the file.

   ![The CSV file contains names and IDs for the members to import](./media/groups-bulk-import-members/csv-file.png)

1. On the **Bulk import group members** page, under **Upload your csv file**, browse to the file. When you select the file, validation of the CSV file starts.
1. When the file contents are validated, the bulk import page displays **File uploaded successfully**. If there are errors, you must fix them before you can submit the job.
1. When your file passes validation, select **Submit** to start the Azure bulk operation that imports the group members to the group.
1. When the import operation completes, you'll see a notification that the bulk operation succeeded.

## Check import status

You can see the status of all of your pending bulk requests in the **Bulk operation results** page.

[![](media/groups-bulk-import-members/bulk-center.png "Check status in the Bulk Operations Results page")](media/groups-bulk-import-members/bulk-center.png#lightbox)

For details about each line item within the bulk operation, select the values under the **# Success**, **# Failure**, or **Total Requests** columns. If failures occurred, the reasons for failure will be listed.

## Bulk import service limits

Each bulk activity to import a list of group members can run for up to one hour. This enables importation of a list of at least 40,000 members.

## Next steps

- [Bulk remove group members](groups-bulk-remove-members.md)
- [Download members of a group](groups-bulk-download-members.md)
- [Download a list of all groups](groups-bulk-download.md)
