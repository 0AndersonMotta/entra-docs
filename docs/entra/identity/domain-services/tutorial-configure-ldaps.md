---
title: Tutorial - Configure LDAPS for Azure Active Directory Domain Services | Microsoft Docs
description: In this tutorial, you learn how to configure secure lightweight directory access protocol (LDAPS) for an Azure Active Directory Domain Services managed domain.
author: iainfoulds
manager: daveba

ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: iainfou

#Customer intent: As an identity administrator, I want to secure access to an Azure Active Directory Domain Services managed domain using secure lightweight directory access protocol (LDAPS)
---

# Tutorial: Configure secure LDAP for an Azure Active Directory Domain Services managed domain

To communicate with your Azure Active Directory Domain Services (Azure AD DS) managed domain, the Lightweight Directory Access Protocol (LDAP) is used. By default, the LDAP traffic isn't encrypted, which is a security concern for many environments. With Azure AD DS, you can configure the managed domain to use secure Lightweight Directory Access Protocol (LDAPS). When you use secure LDAP, the traffic is encrypted. Secure LDAP is also known as LDAP over Secure Sockets Layer (SSL) / Transport Layer Security (TLS).

This tutorial shows you how to configure LDAPS for an Azure AD DS managed domain.

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Create a digital certificate for use with Azure AD DS
> * Enable secure LDAP for Azure AD DS
> * Configure secure LDAP for use over the public internet
> * Bind and test secure LDAP for an Azure AD DS managed domain

If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

## Prerequisites

To complete this tutorial, you need the following resources and privileges:

* An active Azure subscription.
    * If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* An Azure Active Directory tenant associated with your subscription, either synchronized with an on-premises directory or a cloud-only directory.
    * If needed, [create an Azure Active Directory tenant][create-azure-ad-tenant] or [associate an Azure subscription with your account][associate-azure-ad-tenant].
* The *LDP.exe* tool installed on your computer.
    * If needed, [install the Remote Server Administration Tools (RSAT)][rsat] for *Active Directory Domain Services and LDAP*.

## Sign in to the Azure portal

In this tutorial, you configure secure LDAP for the Azure AD DS managed domain using the Azure portal. To get started, first sign in to the [Azure portal](https://portal.azure.com).

## Create a certificate for secure LDAP

To use secure LDAP, a digital certificate is used to encrypt the communication. This digital certificate is applied to your Azure AD DS managed domain, and lets tools like *LDP.exe* use secure encrypted communication when querying data. There are two ways to create a certificate for secure LDAP access to the managed domain:

* A certificate from a public certificate authority (CA) or an enterprise CA.
    * If your organization gets certificates from a public CA, get the secure LDAP certificate from that public CA. If you use an enterprise CA in your organization, get the secure LDAP certificate from the enterprise CA.
    * A public CA only works when you use a custom DNS name with your Azure AD DS managed domain. If the DNS domain name of your managed domain ends in *.onmicrosoft.com*, you can't create a digital certificate to secure the connection with this default domain. Microsoft owns the *.onmicrosoft.com* domain, so a public Certificate Authority (CA) won't issue a certificate. In this scenario, create a self-signed certificate and use that to configure secure LDAP.
* A self-signed certificate that you create yourself.
    * This approach is good for testing purposes, and is what this tutorial shows.

The certificate you request or create must meet the following requirements. Your managed domain encounters problems if you enable secure LDAP with an invalid certificate:

* **Trusted issuer** - The certificate must be issued by an authority trusted by computers connecting to the managed domain using secure LDAP. This authority may be a public CA or an Enterprise CA trusted by these computers.
* **Lifetime** - The certificate must be valid for at least the next 3-6 months. Secure LDAP access to your managed domain is disrupted when the certificate expires.
* **Subject name** - The subject name on the certificate must be your managed domain. For instance, if your domain is named *contoso100.com*, the certificate's subject name must be *contoso100.com*. Set the DNS name (subject alternate name) to a wildcard name for your managed domain.
* **Key usage** - The certificate must be configured for *digital signatures* and *key encipherment*.
* **Certificate purpose** - The certificate must be valid for SSL server authentication.

In this tutorial, let's create a self-signed certificate for secure LDAP using PowerShell. Open a PowerShell window as **Administrator** and run the following commands. Replace the *$dnsName* variable with the DNS name used by your own managed domain, such as *contoso100.com*:

```powershell
# Define your own DNS name used by your Azure AD DS managed domain
$dnsName="contoso100.com"

# Get the current date to set a one-year expiration
$lifetime=Get-Date

# Create a self-signed certificate for use with Azure AD DS
New-SelfSignedCertificate -Subject $dnsName `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.$dnsName, $dnsName.com
```

The following example output shows that the certificate was successfully generated and is stored in the local certificate store (*LocalMachine\MY*):

```output
PS C:\WINDOWS\system32> New-SelfSignedCertificate -Subject $dnsName `
>>   -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
>>   -Type SSLServerAuthentication -DnsName *.$dnsName, $dnsName.com

   PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\MY

Thumbprint                                Subject
----------                                -------
959BD1531A1E674EB09E13BD8534B2C76A45B3E6  CN=contoso100.com
```

## Export the certificate to a .PFX file

Before you can use the digital certificate with your Azure AD DS managed domain, export the certificate created in the previous step to a *.PFX* file. The certificate must be correctly exported for Azure AD DS to secure LDAP.

1. To open the *Run* dialog, select the **Windows** and **R** keys.
1. Open the Microsoft Management Console (MMC) by entering **mmc** in the *Run* dialog, then select **OK**.
1. On the **User Account Control** prompt, click **Yes** to launch MMC as administrator.
1. From the **File** menu, click **Add/Remove Snap-in...**
1. In the **Certificates snap-in** wizard, choose **Computer account**, then select **Next**.
1. On the **Select Computer** page, choose **Local computer: (the computer this console is running on)**, then select **Finish**.
1. In the **Add or Remove Snap-ins** dialog, click **OK** to add the certificates snap-in to MMC.
1. In the MMC window, expand **Console Root**. Select **Certificates (Local Computer)**, then expand the **Personal** node, followed by the **Certificates** node.

    ![Open the personal certificates store in the Microsoft Management Console](./media/tutorial-configure-ldaps/open-personal-store.png)

1. The self-signed certificate created in the previous step is shown, such as *contoso100.com*. Right-click this certificate, then choose **All Tasks > Export...**

    ![Export certificate in the Microsoft Management Console](./media/tutorial-configure-ldaps/export-cert.png)

1. In the **Certificate Export Wizard**, select **Next**.
1. The private key for with the certificate must exported. If the *.PFX* file that doesn't contain the private key for the certificate, the action to enable secure LDAP for your managed domain fails.

    On the **Export Private Key** page, choose **Yes, export the private key**, then select **Next**.
1. Azure AD DS managed domains only support the *.PFX* file format. Don't export the certificate as *.CER* file format.
    
    On the **Export File Format** page, select **Personal Information Exchange - PKCS #12 (.PFX)** as the file format for the exported certificate. Check the box for *Include all certificates in the certification path if possible*:

    ![Choose the option to export the certificate in the PKCS 12 (.PFX) file format](./media/tutorial-configure-ldaps/export-cert-to-pfx.png)

1. On the **Security** page, choose the option for **Password** to protect the *.PFX* file. Enter and confirm a password, then select **Next**. This password is used in the next section to enable secure LDAP for your Azure AD DS managed domain.
1. On the **File to Export** page, specify the file name and location where you'd like to export the certificate, such as *C:\Users\\<accountname>\azure-ad-ds*.
1. On the review page, select **Finish** to export the certificate to a *.PFX* file. A confirmation dialog is displayed when the certificate has been successfully exported.
1. Leave the MMC open for use in one of the following sections.

## Enable secure LDAP for Azure AD DS

With a digital certificate created and exported to the correct format, now enable secure LDAP on your Azure AD DS managed domain. To enable secure LDAP, perform the following configuration steps:

1. In the [Azure portal](https://portal.azure.com), search for *domain services* in the **Search resources** box. Select **Azure AD Domain Services** from the search result.

    ![Search for and select your Azure AD DS managed domain in the Azure portal](./media/tutorial-configure-ldaps/search-for-domain-services.png)

1. Choose your managed domain, such as *contoso100.com*.
1. On the left-hand side of the Azure AD DS window, choose **Secure LDAP**.
1. By default, secure LDAP access to your managed domain is disabled. Toggle **Secure LDAP** to **Enable**.
1. Secure LDAP access to your managed domain over the internet is disabled by default. When you enable public secure LDAP access, your domain is susceptible to password brute force attacks over the internet. In the next step, a network security group is configured to lock down access to only the required source IP address ranges.

    Toggle **Allow secure LDAP access over the internet** to **Enable**.

1. Select the folder icon next to **.PFX file with secure LDAP certificate**. Browse to the path of the *.PFX* file, then select the certificate created in the previous step.
1. Enter the **Password to decrypt .PFX file** set in the previous step when the certificate was exported to a *.PFX* file.
1. Select **Save** to enable secure LDAP.

    ![Enable secure LDAP for an Azure AD DS managed domain in the Azure portal](./media/tutorial-configure-ldaps/enable-ldaps.png)

A notification is displayed that secure LDAP is being configured for the managed domain. You can't modify other settings for the managed domain until this operation is complete.

It takes a few minutes to enable secure LDAP for your managed domain. If the secure LDAP certificate you provide doesn't match the required criteria, the action to enable secure LDAP for the managed domain fails. Some common reasons for failure are if the domain name is incorrect, or the certificate expires soon or has already expired. You can re-create the certificate with valid parameters, then enable secure LDAP using this updated certificate.

## Lock down secure LDAP access to your managed domain over the internet

When you enable secure LDAP access over the internet to your Azure AD DS managed domain, it creates a security threat. The managed domain is reachable from the internet on TCP port 636. It's recommend to restrict access to the managed domain to specific known IP addresses for your environment. An Azure network security group rule can be used to limit access to secure LDAP.

Let's create a rule to allow inbound secure LDAP access over TCP port 636 from a specified set of IP addresses. A default *DenyAll* rule with a lower priority applies to all other inbound traffic from the internet, so only the specified addresses can reach your Azure AD DS managed domain using secure LDAP.

1. In the Azure portal, select *Resource groups* on the left-hand side navigation.
1. Choose you resource group, such as *myResourceGroup*, then select your network security group, such as *AADDS-contoso100.com-NSG*.
1. The list of existing inbound and outbound security rules are displayed. On the left-hand side of the network security group windows, choose **Security > Inbound security rules**.
1. Select **Add**, then create a rule to allow *TCP* port *636*. For improved security, choose the source as *IP Addresses* and then specify your own valid IP address or range for your organization.

    | Setting                           | Value        |
    |-----------------------------------|--------------|
    | Source                            | IP Addresses |
    | Source IP addresses / CIDR ranges | A valid IP address or range for your environment |
    | Source port ranges                | *            |
    | Destination                       | Any          |
    | Destination port ranges           | 636          |
    | Protocol                          | TCP          |
    | Action                            | Allow        |
    | Priority                          | 401          |
    | Name                              | AllowLDAPS   |

1. When ready, select **Add** to save and apply the rule.

    ![Create a network security group rule to secure LDAPS access over the internet](./media/tutorial-configure-ldaps/create-inbound-nsg-rule-for-ldaps.png)

## Configure DNS to access the managed domain from the internet

With secure LDAP access enabled over the internet, update the DNS settings so that client computers can find this managed domain. The *Secure LDAP external IP address* is listed on the **Properties** tab for your Azure AD DS managed domain:

![View the secure LDAP external IP address for your Azure AD DS managed domain in the Azure portal](./media/tutorial-configure-ldaps/ldaps-external-ip-address.png)

Configure your external DNS provider to create a host record, such as *ldaps*, to resolve to this external IP address. To test locally on your machine first, you can create an entry in the Windows hosts file. To successfully edit the hosts file on your local machine, open *Notepad* as an administrator, then open the file *C:\Windows\System32\drivers\etc*.

The following example DNS entry, either with your external DNS provider or in the local hosts file, resolves traffic for *ldaps.contoso100.com* to the external IP address of *40.121.19.239*:

```
40.121.19.239    ldaps.contoso100.com
```

## Test queries to the managed domain using LDP.exe

Client computers must trust the issuer of the secure LDAP certificate to be able to connect successfully to the managed domain using LDAPS. If you use a public CA, the computer should automatically trust these certificate issuers. In this tutorial, you use a self-signed certificate, so let's install the public part of the self-signed certificate into the trusted certificate store on the client computer:

1. Go back to the MMC for *Certificates* where you exported the digital certificate in a previous step.
1. Expand the **Trusted Root Certification Authorities** node, followed by the **Certificates node**.
1. To copy the certificate, drag your certificate from the **Personal | Certificates** node, such as *contoso100.com*, to this **Trusted Root Certification Authorities | Certificates** node.
1. Verify that your certificate is now listed in the **Trusted Root Certification Authorities | Certificates** node before you try to connect using the *LDP.exe* tool.

To connect and bind to your Azure AD DS managed domain and search over LDAP, you use the *LDP.exe* too. This tool is included in the Remote Server Administration Tools (RSAT) package. For more information, see [install Remote Server Administration Tools][rsat].

1. Open *LDP.exe* and connect to the managed domain. Select **Connection**, then choose **Connect...**.
1. Enter the secure LDAP DNS domain name of your managed domain created in the previous step, such as *ldaps.contoso100.com*. To use secure LDAP, set **Port** to *636*, then check the box for **SSL**.
1. Select **OK** to connect to the managed domain.

Next, bind to your Azure AD DS managed domain. Users (and service accounts) can't perform LDAP simple binds if you have disabled NTLM password hash synchronization on your Azure AD DS instance. For more information on disabling NTLM password hash synchronization, see [Secure your Azure AD DS managed domain](secure-your-domain.md).

1. Select the **Connection** menu option, then choose **Bind...**.
1. Provide the credentials of a user account belonging to the *AAD DC Administrators* group, such as *contosoadmin*. Enter the user account's password, then enter your domain, such as *contoso100.com*.
1. For **Bind type**, choose the option for *Bind with credentials*.
1. Select **OK** to bind to your Azure AD DS managed domain.

To see of the objects stored in your Azure AD DS managed domain:

1. Select the **View** menu option, and then choose **Tree**.
1. Leave the *BaseDN* field blank, then select **OK**.
1. Choose a container, such as *AADDC Users*, then right-select the container and choose **Search**.
1. Leave the pre-populated fields set, then select **Run**. The results of the query are shown in the right-hand window.

    ![Search for objects in your Azure AD DS managed domain using LDP.exe](./media/tutorial-configure-ldaps/ldp-query.png)

To directly query a specific container, from the **View > Tree** menu, you can specify a **BaseDN** such as *OU=AADDC Users,DC=CONTOSO100,DC=COM* or *OU=AADDC Computers,DC=CONTOSO100,DC=COM*. For more information on how to format and create queries, see [LDAP query basics](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter).

## Clean up resources

If you no longer wish to use secure LDAP for your Azure AD DS managed domain, disable the feature using the following steps:

1. In the [Azure portal](https://portal.azure.com), search for *domain services* in the **Search resources** box. Select **Azure AD Domain Services** from the search result. Select your Azure AD DS domain, such as *contoso100.com*.
1. On the left-hand side of the Azure AD DS window, choose **Secure LDAP**.
1. Toggle **Secure LDAP** to **Disable**.
1. If desired, delete the local *.PFX* file exported during this tutorial, and delete the certificate from the personal and trusted stores in the MMC.

## Next steps

In this tutorial, you learned how to:

> [!div class="checklist"]
> * Create a digital certificate for use with Azure AD DS
> * Enable secure LDAP for Azure AD DS
> * Configure secure LDAP for use over the public internet
> * Bind and test secure LDAP for an Azure AD DS managed domain

> [!div class="nextstepaction"]
> [Join a Windows Server virtual machine to your managed domain](join-windows-vm.md)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md

<!-- EXTERNAL LINKS -->
[rsat]: /windows-server/remote/remote-server-administration-tools
