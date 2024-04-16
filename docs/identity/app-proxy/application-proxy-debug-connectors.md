---
title: Debug private network connectors
description: Debug issues with Microsoft Entra private network connectors.
author: kenwith
manager: amycolannino
ms.service: entra-id
ms.subservice: app-proxy
ms.topic: troubleshooting
ms.date: 02/26/2024
ms.author: kenwith
ms.reviewer: ashishj
---

# Debug private network connector issues 

This article helps you troubleshoot issues with Microsoft Entra private network connectors. If you're using the application proxy service for remote access to an on-premises web application, but you're having trouble connecting to the application, use this flowchart to debug connector issues. 

## Flowchart for connector issues

This flowchart walks you through the steps for debugging some of the more common connector issues. For details about each step, see the table following the flowchart.

![Flowchart showing steps for debugging a connector](media/application-proxy-debug-connectors/application-proxy-connector-debugging-flowchart.png)

| Step | Action | Description |
|---------|---------|---------|
|1 | Find the connector group assigned to the app | You probably have a connector installed on multiple servers, in which case the connectors should be assigned to a connector group. To learn more about connector groups, see [Understand Microsoft Entra private network connector groups](../../global-secure-access/concept-connector-groups.md).|
|2 | Install the connector and assign a group | If you don't have a connector installed, see [Install and register a connector](application-proxy-add-on-premises-application.md#install-and-register-a-connector).<br></br> If you're having issues installing the connector, see [Problem installing the connector](application-proxy-connector-installation-problem.md).<br></br> If the connector isn't assigned to a group, see [Assign the connector to a group](application-proxy-connector-groups.md#create-connector-groups).<br></br>If the application isn't assigned to a connector group, see [Assign the application to a connector group](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups).|
|3 | Run a port test on the connector server | On the connector server, run a port test by using [telnet](/windows-server/administration/windows-commands/telnet) or other port testing tool to check if ports [443 and 80 are open](application-proxy-add-on-premises-application.md#open-ports).|
|4 | Configure the domains and ports | [Confirm that domains and ports are configured correctly](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment) for the connector. Certain ports must be open and URLs that your server must be able to access. For more information, see [Tutorial: Add an on-premises application for remote access through application proxy in Microsoft Entra ID](application-proxy-add-on-premises-application.md#open-ports). |
|5 | Check if a back-end proxy is in use | Check to see if the connectors are using back-end proxy servers or bypassing them. For details, see [Troubleshoot connector proxy problems and service connectivity issues](application-proxy-configure-connectors-with-proxy-servers.md#troubleshoot-connector-proxy-problems-and-service-connectivity-issues). |
|6 | Update the connector and updater settings with the back-end proxy information | If a back-end proxy is in use, make sure the connector is using the same proxy. For details about troubleshooting and configuring connectors to work with proxy servers, see [Work with existing on-premises proxy servers](application-proxy-configure-connectors-with-proxy-servers.md). |
|7 | Load the app's internal URL on the connector server | On the connector server, load the app's internal URL. |
|8 | Check internal network connectivity | There's a connectivity issue in your internal network that this debugging flow is unable to diagnose. The application must be accessible internally for the connectors to work. You can enable and view connector event logs as described in [private network connectors](application-proxy-connectors.md#under-the-hood). |
|9 | Lengthen the time-out value on the back end | In the **Additional Settings** for your application, change the **Backend Application Timeout** setting to **Long**. See [Add an on-premises app to Microsoft Entra ID](application-proxy-add-on-premises-application.md). |
|10 | If issues persist, debug applications. | [Debug application proxy application issues](application-proxy-debug-apps.md). |

## Next steps

- [Debug application proxy application issues](application-proxy-debug-apps.md)
- [Work with existing on-premises proxy servers](application-proxy-configure-connectors-with-proxy-servers.md)
- [Understand private network connectors](application-proxy-connectors.md)
- [Install and register a connector](application-proxy-add-on-premises-application.md)
- [Troubleshoot application proxy problems and error messages](application-proxy-troubleshoot.md)
