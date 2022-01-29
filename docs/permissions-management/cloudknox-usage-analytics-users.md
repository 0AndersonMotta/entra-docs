---
title: View analytic information about users in Microsoft CloudKnox Permissions Management
description: How to view analytic information about users in Microsoft CloudKnox Permissions Management.
services: active-directory
author: Yvonne-deQ
manager: karenh444
ms.service: active-directory
ms.subservice: ciem
ms.workload: identity
ms.topic: how-to
ms.date: 01/28/2022
ms.author: v-ydequadros
---

# View analytic information about  users

The **Usage Analytics** dashboard in Microsoft CloudKnox Permissions Management (CloudKnox) provides details about identities, resources, and tasks that you can use make informed decisions about granting permissions, and reducing risk on unused permissions.

- **Users**: Tracks assigned permissions and usage of various identities.
- **Groups**: Tracks assigned permissions and usage of the group and the group members.
- **Active resources**: Tracks active resources (used in the last 90 days).
- **Active tasks**: Tracks active tasks (performed in the last 90 days).
- **Access keys**: Tracks the permission usage of access keys for a given user.
- **Serverless functions**: Tracks assigned permissions and usage of the serverless functions.

The **Usage Analytics** dashboard allows system administrators to collect, analyze, report on, and visualize data about all identity types.

This article describes how to view usage analytics about active users.

## Create a query to view active users

When you select **Users**, the **Usage Analytics** dashboard provides a high-level overview of tasks used by various identities. 

- On the main **Usage Analytics** dashboard, select **Users** from the  drop-down list at the top of the screen. 

The following components make up the **Users** dashboard:

- **Authorization system type** - Select the authorization you want to use: Amazon Web Services (AWS), Microsoft Azure, or Google Cloud Platform (GCP).
- **Authorization system** - Select from a **List** of accounts and **Folders***.
- **Tasks type** - Select **All** tasks, **High-risk tasks** or, for a list of tasks where users have deleted data, select **Delete tasks**.
- **Search** - Enter criteria to find specific tasks.
- **Apply** - Select to display the criteria you've selected.
- **Reset filter** - Select to discard your changes.


## View the results of your query

The **Identities** table displays the the results of your query.

- **Name**: Provides the name of the group. 
    - To view details about the group, select the down arrow. 
- The **Domain/Account** name.
- The **Permission creep index (PCI)** - Provides the following information:
    - **Index** - A numeric value assigned to the PCI.
    - **Since** - How many days the PCI value has been at the displayed level.
- **Tasks** Displays the number of **Granted** and **Executed** tasks.
- **Resources** - The number of resources used.
- **User groups** - The number of users who accessed the group.
- **Last activity on** - The date the function was last accessed.
- Select the ellipses **(...)** and select **Tags** to add a tag or **Auto remediate** to remediate your results automatically.

## Add a tag to a user

1. Select the ellipses **(...)** and select **Tags**.
1. From the **Select a tag** dropdown, select a tag.
1. To create a custom tag select **New custom tag**, add a tag name, and then select **Create**.
1. In the **Value (Optional)** box, enter a value.
1. Select the ellipses **(...)** to select **Advanced save** options, and then select **Save**.
1. To add the tag to the serverless function, select **Add tag**.

## Set the auto remediate option

- Select the ellipses **(...)** and select **Auto remediate**.


## Apply filters to your query  

There are many filter options within the **Users** screen, including filters by **Authorization system**, **Identity type**, and **Identity state**. 
Filters can be applied in one, two, or all three categories depending on the type of information you're looking for. 

### Apply filters by authorization system type

1. From the **Authorization system type** dropdown, select the authorization system you want to use: **AWS**, **Azure**, or **GCP**.
1. Select **Apply** to run your query and display the information you selected.

    Select **Reset filter** to discard your changes. 


### Apply filters by authorization system

1. From the **Authorization system type** dropdown, select the authorization system you want to use: **AWS**, **Azure**, or **GCP**. 
1. From the **Authorization system** dropdown, select accounts from a **List** of accounts and **Folders**.
1. Select **Apply** to run your query and display the information you selected.

    Select **Reset filter** to discard your changes. 

### Apply filters by identity type

1. From the **Authorization system type** dropdown, select the authorization system you want to use: **AWS**, **Azure**, or **GCP**.
1. From the **Authorization system** dropdown, select from a **List** of accounts and **Folders**.
1. From the **Identity type**, select the type of user: **All**, **User**, **Role/App/Service a/c**, or **Resource**.
1. Select **Apply** to run your query and display the information you selected.

    Select **Reset filter** to discard your changes.

### Apply filters by identity subtype

1. From the **Authorization system type** dropdown, select the authorization system you want to use: **AWS**, **Azure**, or **GCP**.
1. From the **Authorization system** dropdown, select from a **List** of accounts and **Folders**.
1. From the **Identity subtype**, select the type of user: **All**, **ED**, **Local**, or **Cross-account**.
1. Select **Apply** to run your query and display the information you selected.

    Select **Reset filter** to discard your changes.

### Apply filters by identity state

1. From the **Authorization system type** dropdown, select the authorization system you want to use: **AWS**, **Azure**, or **GCP**.
1. From the **Authorization system** dropdown, select from a **List** of accounts and **Folders**.
1. From the **Identity state**, select the type of user: **All**, **Active**, or **Inactive**.
1. Select **Apply** to run your query and display the information you selected.

    Select **Reset filter** to discard your changes.


### Apply filters by identity filters

1. From the **Authorization system type** dropdown, select the authorization system you want to use: **AWS**, **Azure**, or **GCP**.
1. From the **Authorization system** dropdown, select from a **List** of accounts and **Folders**.
1. From the **Identity type**, select: **Risky** or **Inc. in PCI calculation only**.
1. Select **Apply** to run your query and display the information you selected.

    Select **Reset filter** to discard your changes.


### Apply filters by task type

You can filter user details by type of user, user role, app, or service used, or by resource.

1. From the **Authorization system type** dropdown, select the authorization system you want to use: **AWS**, **Azure**, or **GCP**.
1. From the **Authorization system** dropdown, select from a **List** of accounts and **Folders**.
1. From the **Task type**, select the type of user: **All** or **High-risk tasks**.
1. Select **Apply** to run your query and display the information you selected.

    Select **Reset filter** to discard your changes.


## Export the results of your query

- To view a report of the results of your query as a comma-separated values (CSV) file, select **Export**, and then select **CSV**. 



## Next steps

- To view active tasks, see [View analytic information about active tasks](cloudknox-usage-analytics-active-tasks.md)
- To view assigned permissions and usage of the group and the group members, see [View analytic information about groups](cloudknox-usage-analytics-groups.md).
- To view active resources, see [View analytic information about active resources](cloudknox-usage-analytics-active-resources.md).
- To view the permission usage of access keys for a given user, see [View analytic information about access keys](cloudknox-usage-analytics-access-keys.md).
- To view assigned permissions and usage of the serverless functions, see [View analytic information about serverless functions](cloudknox-usage-analytics-serverless-functions.md).