---
title: 'Azure AD Connect cloud provisioning on-demand provisioning'
description: This article describes the on-demand provisioning feature.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/14/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
---

# Azure AD Connect cloud provisioning on-demand provisioning

Azure AD Connect cloud provisioning has introduced a new feature, that will allow you to test configuration changes, by applying these changes to a single user or group.  You can use this to validate and verify that the changes made to the configuration were applied properly and are being correctly synchronized to Azure AD.  

## Using on-demand provisiong
To use the new feature, follow the steps below.


1.  In the Azure portal, select **Azure Active Directory**.
2.  Select **Azure AD Connect**.
3.  Select **Manage provisioning**.

    ![Manage provisioning](media/how-to-configure/manage1.png)
4. Under **Configuration**, select your configuration.
5. Under **Validate** click the **Provision a user** button.  This will bring up the 

 ![Provision a user](media/how-to-on-demand-provision/on-demand2.png)

6. This will bring up the on demand provisioning screen.  Enter the **distinguished name** of a user or group and click the **Provision** button.  
 
 ![Provisioning on demand](media/how-to-on-demand-provision/on-demand3.png)
7. This will kick off the process.  Once it completes you should see a success screen and 4 green check boxes indicating it was successfully provisioned.  If you receive and error, you will see that on the left of the screen.

  ![Provisioning on demand](media/how-to-on-demand-provision/on-demand4.png)

Now you can look at the user or group and determine if the changes you made in the configuration have been applied.  The remainder of this document will describe the individual sections that are displayed in the details of a successfully synchronized user or group.

## Import User details
This section provides information on the user or group that was imported.  This is what the user or group looks like before it is provisioned into Azure AD.  Click the **View detais** link to display this information.

![Import user](media/how-to-on-demand-provision/on-demand5.png)

Using this information, you can see the various attributes, and their values, that were imported.  If you have created a custom attribute mapping, you will be able to see the value here.
![Import user details](media/how-to-on-demand-provision/on-demand6.png)

## Determine if user is in scope details
This section provides information on whether the user or group that was imported to Azure AD is in scope.  Click the **View detais** link to display this information.

![User scope](media/how-to-on-demand-provision/on-demand7.png)

Using this information, you can see additional information about the scope of your users or groups.

![User scope details](media/how-to-on-demand-provision/on-demand10.png)

## Match user between source and target system details
This section provides information on whether the user or group already exists in Azure AD and should a join occur instead of provisioning a new user or group.  Click the **View detais** link to display this information.
![User scope](media/how-to-on-demand-provision/on-demand8.png)

Using this information, you can see whether a match was found or if a new user or group is going to be created.

![User scope details](media/how-to-on-demand-provision/on-demand11.png)
## Perform action details
This section provides information on the user or group that was provisioned or exported into Azure AD after the configuration is applied.  This is what the user or group looks like once it is provisioned into Azure AD.  Click the **View detais** link to display this information.
![User scope](media/how-to-on-demand-provision/on-demand9.png)

Using this information, you can see the values of the attributes after the configuration is applied.  Do they look similar to what was imported or are the different?  Does the configuration apply successful?  

This will allow you to trace the attribute transformation as it moves through the cloud and into your Azure AD tenant.

![User scope details](media/how-to-on-demand-provision/on-demand12.png)

## Next steps 

- [What is Azure AD Connect cloud provisioning?](what-is-cloud-provisioning.md)
- [How to install Azure AD Connect cloud provisioning](how-to-install.md)
 