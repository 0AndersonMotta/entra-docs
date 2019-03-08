---
title: Deploy password protection for Azure Active Directory preview
description: Deploy the password protection for Azure Active Directory preview to ban bad passwords on-premises

services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019

ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
---

# Preview: Deploy password protection for Azure Active Directory

|     |
| --- |
| Password protection for Azure Active Directory is a public preview feature of Azure Active Directory. For more information about previews, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Now that you have an understanding of [how to enforce password protection for Azure Active Directory for Windows Server Active Directory](concept-password-ban-bad-on-premises.md), the next step is to plan and execute the deployment.

## Deployment strategy

We recommend that you start deployments in audit mode. Audit mode is the default initial setting, where passwords can continue to be set. Passwords that would be blocked are recorded in the event log. After proxy servers and DC Agents are fully deployed in audit mode, you should monitor the impacts that the password policy will have on users and the environment if the policy is enforced.

During the audit stage, many organizations determine that:

* They need to improve existing operational processes to use more-secure passwords.
* Users often use unsecure passwords.
* They need to inform users about the upcoming change in security enforcement, possible the impact on them, and to choose more-secure passwords.

After the feature has been running in audit mode for a reasonable period,you can switch the configuration from **Audit** to **Enforce**,  require use of more-secure passwords. Focused monitoring during this time is important.

## Deployment requirements

* All domain controllers that get the password protection for Azure Active Directory DC Agent service installed must be running Windows Server 2012 or later.
* All machines that get the password protection for Azure Active Directory proxy service installed must be running Windows Server 2012 R2 or later.
* All machines that get password protection for Azure Active Directory components installed, including domain controllers, must have the Universal C Runtime installed. You can get the runtime by making sure all Windows Updates are installed. Or you get it via an operating-specific update package. For more information, see [Update for Universal C Runtime in Windows](https://support.microsoft.com/help/2999226/update-for-uniersal-c-runtime-in-windows).
* Network connectivity must exist between at least one domain controller in each domain and at least one server that hosts the password protection proxy service. This connectivity must allow the domain controller to access the RPC endpoint mapper port (135) and the RPC server port on the proxy service. By default, the RPC server port is a dynamic RPC port but can be configured to [use a static port](#static).
* All machines that host the password protection proxy service must have network access to the following endpoints:

    |Endpoint |Purpose|
    | --- | --- |
    |`https://login.microsoftonline.com`|Authentication requests|
    |`https://enterpriseregistration.windows.net`|Azure AD Password Protection functionality|

* All machines that host the password protection proxy service must be configured to allow outbound TLS 1.2 HTTP traffic.
* A global administrator account to register the password protection proxy service and forest with Azure AD.
* An account with Active Directory domain administrator privileges in the forest root domain to register the Windows Server Active Directory forest with Azure AD.
* Any Active Directory domain that runs the DC Agent service software must use Distributed File System Replication (DFSR) for sysvol replication.
* The Microsoft Key Distribution Service must be enabled on all Windows Server 2012 and later domain controllers in the domain. By default, this service is enabled via manual trigger start.

## Single forest deployment

The following diagram shows how the basic components of password protection for Azure Active Directory work together in an on-premises Active Directory environment.

![How password protection for Azure Active Directory components works together](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

It's a good idea to review how the software works before you deploy it. See [Conceptual overview of Azure AD password protection](concept-password-ban-bad-on-premises.md).

### Download the software

There are two required installers for password protection for Azure Active Directory. They are available from the [Microsoft download center](https://www.microsoft.com/download/details.aspx?id=57071)

### Install and configure the password protection proxy service

1. Choose one or more servers to host the password protection for Azure Active Directory proxy service.
   * Each such service can only provide password policies for a single forest. The host machine must be domain-joined to a domain in that forest. Root and child domains are both supported. There must be network connectivity between at least one DC in each domain of the forest and the password protection for Azure Active Directory machine.
   * You can run the Azure AD Password Protection Proxy service on a domain controller for testing. But that domain controller then requires internet connectivity, which can be a security concern. We recommend this configuration for testing only.
   * We recommend at least two proxy servers for redundancy. See [High availability](howto-password-ban-bad-on-premises-deploy.md#high-availability).

2. Install the password protection proxy service by using the AzureADPasswordProtectionProxySetup.msi MSI package.
   * The software installation doesn't require a reboot. You can automate it by using standard MSI procedures. For example:

     `msiexec.exe /i AzureADPasswordProtectionProxySetup.msi /quiet /qn`

      > [!NOTE]
      > The Windows Firewall service must be running before you install the AzureADPasswordProtectionProxySetup.msi MSI package to avoid an installation error. If Windows Firewall is configured to not run, the workaround is to temporarily enable and run the Firewall service during the installation. The proxy software has no specific dependency on the Windows Firewall software after installation. If you're using a third-party firewall, it must still be configured to satisfy the deployment requirements. These include allowing inbound access to port 135 and the proxy RPC server port. See [Deployment requirements](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements).

3. Open a PowerShell window as an Administrator.
   * The passsword protection proxy software includes a new PowerShell module, *AzureADPasswordProtection.* The following steps are based on running various cmdlets from this PowerShell module. Import the new module as follows:

      ```PowerShell
      Import-Module AzureADPasswordProtection
      ```

   * To check that the service is running:

      `Get-Service AzureADPasswordProtectionProxy | fl`.

     The result should show a **Status** of  "Running."

4. Register the proxy.
   * After step 3 is completed, the password protection proxy service is running on the machine. But it doesn't yet have the necessary credentials to communicate with Azure AD. Registration with Azure AD is required:

     `Register-AzureADPasswordProtectionProxy`

     This cmdlet requires global administrator credentials for your Azure tenant. You must also have on-premises Active Directory domain administrator privileges in the forest root domain. After this command succeeds once for a given proxy service, additional invocations of it will succeed but are unnecessary.

      The Register-AzureADPasswordProtectionProxy cmdlet supports the following three authentication modes.

      * Interactive authentication mode:

         ```PowerShell
         Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
         ```
         > [!NOTE]
         > This mode doesn't work on Server Core operating systems. Instead, use one of the following authentication modes.

         > [!NOTE]
         > This mode might fail if Internet Explorer Enhanced Security Configuration is enabled. The workaround is to disable that Configuration, register the proxy, and then re-enable it.

      * Device-code authentication mode:

         ```PowerShell
         Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
       
         ```

         You can then complete the authentication by following the displayed instructions on a different device.

      * Silent (password-based) authentication mode:

         ```PowerShell
         $globalAdminCredentials = Get-Credential
         Register-AzureADPasswordProtectionProxy -AzureCredential $globalAdminCredentials
         ```

         > [!NOTE]
         > This mode fails if Azure Multi-Factor Authentication is required. In that case, use use one of the previous two authentication modes.

      You don't currently have to specify the *-ForestCredential* parameter, which is reserved for future functionality.

   > [!NOTE]
   > Registration of the password protection proxy service is necessary only once in the lifetime of the service. After that, the proxy service will automatically perform any other necessary maintenance.

   > [!TIP]
   > There may be a noticeable delay before completion the first time that this cmdlet is run for a given Azure tenant. Unless a failure is reported, don't worry about this delay.

5. Register the forest.
   * You must initialize the on-premises Active Directory forest by the necessary credentials to communicate with Azure by using the `Register-AzureADPasswordProtectionForest` PowerShell cmdlet. The cmdlet requires global administrator credentials for your Azure tenant. It also requires on-premises Active Directory domain administrator privileges in the forest root domain. This step is run once per forest.

      The Register-AzureADPasswordProtectionForest cmdlet supports the following three authentication modes.

      * Interactive authentication mode:

         ```PowerShell
         Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
         ```
         > [!NOTE]
         > This mode won't work on Server Core operating systems. Instead use one of the following two authentication modes.

         > [!NOTE]
         > This mode might fail if Internet Explorer Enhanced Security Configuration is enabled. The workaround is to disable that Configuration, register the proxy, and then re-enable it.  

      * Device-code authentication mode:

         ```PowerShell
         Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
         To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
         ```

         You can then complete the authentication by following the displayed instructions on a different device.

      * Silent (password-based) authentication mode:
         ```PowerShell
         $globalAdminCredentials = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
         ```

         > [!NOTE]
         > This mode fails if Azure Multi-Factor Authentication is required. In that case, use use one of the previous two authentication modes.

      These examples only succeed if the currently signed-in user is also an Active Directory domain administrator for the root domain. If this isn't the case, you can supply alternative domain credentials via the *-ForestCredential* parameter.

   > [!NOTE]
   > If multiple proxy servers are installed in your environment, it doesn't matter which proxy server is used to register the forest.

   > [!TIP]
   > There may be a noticeable delay before completion the first time that this cmdlet is run for a given Azure tenant. Unless a failure is reported, don't worry about this delay.

   > [!NOTE]
   > Registration of the Active Directory forest is necessary only once in the lifetime of the forest. After that, the domain controller agents in the forest will automatically perform any other necessary maintenance. After `Register-AzureADPasswordProtectionForest` runs successfully for a given forest, additional invocations of the cmdlet succeed but are unnecessary.

   > [!NOTE]
   > For `Register-AzureADPasswordProtectionForest` to succeed, at least one Windows Server 2012 or later domain controller must be available in the proxy server's domain. But the DC Agent software doesn't have to be installed on any domain controllers before this step.

6. Configure the password protection for Azure Active Directory proxy service to communicate through an HTTP proxy.

   If your environment requires the use of a specific HTTP proxy to communicate with Azure, follow this example:

   Create a file named `proxyservice.exe.config` file in the `%ProgramFiles%\Azure AD Password Protection Proxy\Service` folder that has the following content:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   If your HTTP proxy requires authentication, add the useDefaultCredentials tag as follows:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true" useDefaultCredentials="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   In both cases replace `http://yourhttpproxy.com:8080` with the address and port of your specific HTTP proxy server.

   If your HTTP proxy is configured with an authorization policy, access must be granted to the Active Directory computer account of the machine that hosts the password protection for Azure Active Directory proxy service.

   We recommend that you stop and restart the password protection proxy service after you create or update the `proxyservice.exe.config` file.

   The password protection proxy service doesn't support the use of specific credentials for connecting to an HTTP proxy.

7. Optional: Configure the password protection proxy service to listen on a specific port.
   * RPC over TCP is used by the password protection DC Agent software on the domain controllers to communicate with the password protection proxy service. By default, the password protection proxy service listens on any available dynamic RPC endpoint. But the service can be configured to listen on a specific TCP port, if this is necessary because of your networking topology or firewall requirements in your environment.
      * <a id="static" /></a>To configure the service to run under a static port, use the `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet.
         ```PowerShell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > You must stop and restart the service for these changes to take effect.

      * To configure the service to run under a dynamic port, use the same procedure but set *StaticPort* back to zero:
         ```PowerShell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > You must stop and restart the service for these changes to take effect.

   > [!NOTE]
   > The password protection proxy service requires a manual restart after any change in port configuration. But you don't have to restart the DC Agent service software on domain controllers after making these configuration changes.

   * To query for the current configuration of the service, use the `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet:

      ```PowerShell
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0
      ```

### Install the password protection DC Agent service

   Install the password protection for Azure Active Directory DC Agent service software by using the `AzureADPasswordProtectionDCAgent.msi` MSI package. 

   The software installation, or uninstallation, require a reboot because of the operating system requirement that password filter DLLs are only loaded or unloaded by a reboot.

   You can install the DC Agent service on a machine that's not yet a domain-controller. In this case, the service will start and run but remain inactive until the machine is promoted to be a domain controller.

   You can automate the software installation by using standard MSI procedures. For for example:

   `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > The example msiexec command here causes an immediate reboot. To avoid that, use the `/norestart` flag.

After the password protection DC Agent software is installed on a domain controller and that computer is rebooted, the software installation is complete. No other configuration is required or possible.

## Multiple forest deployments

There are no additional requirements if you deploy password protection for Azure Active Directory across multiple forests. Each forest is independently configured, as described in the "Single forest deployment" section. Each password protection proxy can only support domain controllers from the forest that it's joined to. The password protection software in a given forest is unaware of password protection software that's deployed in other forests, regardless of Active Directory trust configurations.

## Read-only domain controllers

Password changes/sets are not processed and persisted on read-only domain controllers (RODCs). They are forwarded to writable domain controllers. So, you don't have to install the DC agent software on RODCs.

## High availability

The main concern in ensuring high availability of password protection is the availability of proxy servers when the domain controllers in a forest try to download new policies or other data from Azure. Each DC Agent uses a simple round-robin-style algorithm when deciding which proxy server to call. The Agent skips  proxy servers that aren't responding. For most fully connected Active Directory deployments that have healthy replication of both directory and sysvol state, two proxy servers is enough to ensure availability. This results in timely download of new policies and other data. But you can deploy additional proxy servers.

The usual problems that are associated with high availability are mitigated by the design of the DC Agent software. The DC Agent maintains a local cache of the most recently downloaded password policy. Even if all registered proxy servers become unavailable, the DC Agents continue to enforce their cached password policy. A reasonable update frequency for password policies in a large deployment is usually measured in *days*, not hours or less. Therefore, brief outages of the proxy servers don't significant impact password protection for Azure Active Directory.

## Next steps

Now that you've installed the services required for password protection for Azure Active Directory on your on-premises servers, perform [post-install configuration and gather reporting information](howto-password-ban-bad-on-premises-operations.md) to complete your deployment.

[Conceptual overview of Azure AD Password Protection](concept-password-ban-bad-on-premises.md)
