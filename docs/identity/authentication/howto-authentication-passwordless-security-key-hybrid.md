---
title: Passwordless security key sign in hybrid Azure AD joined devices (preview) - Azure Active Directory
description: Enable passwordless security key sign in to hybrid Azure AD joined devices using FIDO2 security keys (preview)

services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/05/2019

ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown

ms.collection: M365-identity-device-management
---
# Enable passwordless security key sign in for hybrid Azure AD joined devices (preview)

This document focusses on enabling passwordless authentication for environments with **hybrid Azure AD joined devices**. This functionality provides seamless single sign-on to on-premises resources using Microsoft-compatible security keys. It allows Azure AD authentication on Windows 10 devices for both Azure AD joined and hybrid Azure AD joined devices using FIDO2 security keys. 

For enterprises that use passwords today and have a shared PC environment, security keys provide a seamless way for workers to authenticate without entering a username or password. Unlike passwords, these security keys have lower IT management costs, provide improved productivity for workers, and have better security.

## Requirements

- Azure Multi-Factor Authentication
- Combined security information registration preview enabled
- Compatible FIDO2 security keys
- Microsoft Intune or Group Policy
- Windows 10 Insider Build 18945 or newer
- Windows Server Insider skip ahead build or fully patched Windows Server 2016/2019 (Only required for accessing on-premises resources with NTLM authentication)
- Hybrid Azure AD joined devices
- Azure AD Connect version X.y.z

## Known limitations

- The scenario is supported for hybrid Azure AD joined devices. 
   - Includes SSO to cloud resources (Office 365 and other SAML enabled applications).
   - Includes SSO to on-premises resources, and Windows Integrated authentication to web sites will work including web sites and SharePoint sites that require IIS Authentication and/or use NTLM.

- Windows Server Active Directory Domain Services (AD DS) domain joined (on-premises only devices) deployment **not supported**.
- RDP, VDI, and Citrix scenarios are **not supported** using security key.
- S/MIME is **not supported** using security key.
- “Run as“ is **not supported** using security key.
- Login to a sever using security key is **not supported**.

## Prepare devices for preview

Devices that you will be piloting with must be running Windows 10 Insider Build 18945 or newer. 

## Create Kerberos server object

You will use a private set of tools that includes a new PowerShell module to create an Azure AD Kerberos Server object in your on-premises directory. 

1. Copy and extract the AzureADKerberosManagement.zip to a new directory on the server where you run Azure AD Connect. 

   > [!NOTE] 
   > This will not upgrade or modify your existing Azure AD Connect installation, but the tools do need to run on the Azure AD Connect server.

1. Open an elevated PowerShell prompt and navigate to \AzureADKerberosManagement\Microsoft Azure Active Directory Connect\AzureADKerberos
1. Run the following PowerShell commands to create a new Azure AD Kerberos server object in both your on-premises Active Directory domain and Azure Active Directory tenant.

> [!NOTE]
> Replace "contoso.corp.com" in the following example with your on-premises Active Directory domain name.

```PowerShell
Import-Module ".\AzureAdKerberos.psd1"

# Specify the on-premises Active Directory domain. A new Azure AD
# Kerberos Server object will be created in this Active Directory domain.
$domain = "contoso.corp.com"

# Enter an Azure Active Directory global administrator username and password.
$cloudCred = Get-Credential

# Enter a domain administrator username and password.
$domainCred = Get-Credential

# Create the new Azure AD Kerberos Server object in Active Directory
# and then publish it to Azure Active Directory.
Set-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

### Viewing and verifying the Azure AD Kerberos Server

You can view and verify the newly created Azure AD Kerberos Server using the following command. 

```PowerShell
Get-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

This command will output the properties of the Azure AD Kerberos Server. You can review the properties to verify that everything is in good order.

| Property | Description |
| --- | --- |
| ID | The unique Id of the AD Domain Controller object. This is sometimes referred to as it’s “slot” or it’s “branch Id”. |
| DomainDnsName | The DNS domain name of the Active Directory Domain. |
| ComputerAccount | The computer account object of the Azure AD Kerberos Server object (The DC). |
| UserAccount | The disabled user account object that holds the Azure AD Kerberos Server TGT encryption key. The DN of this account will be: <br> CN=krbtgt_AzureAD,CN=Users,<Domain-DN> |
| KeyVersion | The key version of the Azure AD Kerberos Server TGT encryption key. The version is assigned when the key is created. The version is then incremented every time the key is rotated. The increments are based on replication meta-data and will likely be greater than one. For example, the initial KeyVersion could be 192272. The first time the key is rotated, the version could advance to 212621. The important thing to verify is that the KeyVersion for the on-premises object and the CloudKeyVersion for the cloud object are the same. |
| KeyUpdatedOn | The date and time that the Azure AD Kerberos Server TGT encryption key was updated/created. |
| KeyUpdatedFrom | The Domain Controller where the Azure AD Kerberos Server TGT encryption key was last updated. |
| CloudId | The Id from the Azure AD Object. Must match the Id above. |
| CloudDomainDnsName | The DomainDnsName from the Azure AD Object. Must match the DomainDnsName above. |
| CloudKeyVersion | The KeyVersion from the Azure AD Object. Must match the KeyVersion above. |
| CloudKeyUpdatedOn | The KeyUpdatedOn from the Azure AD Object. Must match the KeyUpdatedOn above. |

### Rotating the Azure AD Kerberos Server key

The Azure AD Kerberos Server encryption krbtgt keys should be rotated on a regular basis. It’s recommended you follow the same schedule you use to rotate all other Active Directory Domain Controller krbtgt keys. 

> [!WARNING]
> There are other tools that could rotate the krbtgt keys, however, you must use the tools mentioned in this document to rotate the krbtgt keys of your Azure AD Kerberos Server. This ensures the keys are updated in both on-premises AD and Azure AD.

```PowerShell
Set-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred -RotateServerKey
```

### Removing the Azure AD Kerberos Server

If you would like to revert the scenario and remove the Azure AD Kerberos Server from both on-premises Active Directory and Azure Active Directory, run the following command.  

```PowerShell
Remove-AzureADKerberosServer -Domain $domain -CloudCredential $cloudCred -DomainCredential $domainCred
```

## Enable security keys for Windows sign in

Organizations may choose to use one or more of the following methods to enable the use of security keys for Windows sign in.


### Enable credential provider via Intune




 





	8.2 If your enterprise uses NTLM auth, update at least one Domain Controller with Vibranium build
How do I know if my enterprise uses NTLM Auth?
•	Recommend using this link to assess

In our experience, most enterprises use NTLM auth.
1.	Please install the Insider skip ahead - Vibranium server build <BUILD# 7/23 or later> on your DC

If updating a DC is not feasible, you can try this feature without NTLM. Flip the following reg keys to skip the NTLM aspect of the preview:
•	Key: HKLM\System\CurrentControlSet\Control\Lsa\Kerberos\Parameters
•	Value: KeyListReqSupportOverride: 0 – turn off NTLM, 1 or unset – turn on NTLM

	8.3 Enable FIDO for your Azure AD tenant via Azure AD Portal
1.	Use the new Authentication methods blade in Azure AD admin portal that allows you to assign passwordless credentials using FIDO2 security keys
2.	Enable the converged registration portal for users to create and manage FIDO2 security keys

	8.4 Push FIDO cred prov on clients to enable logon with security keys via Intune / Group Policy
Intune (Azure AD joined): 
1.	Go to your Azure AD Portal
2.	Search for Intune
3.	For Tenant wide configuration:
a.	Navigate to Device Enrollment > Profiles> Windows Hello for Business > Settings
i.	Set “Security key for sign-in” to “Enabled”

 
4.	For targeting specific device groups, use the following custom settings via Intune
a.	Follow instructions here
b.	Use the following for FIDO setup:
i.	Name: Turn on FIDO Security Keys for Windows Sign-In
ii.	Description: Enables FIDO Security Keys to be used during Windows Sign In
iii.	OMA-URI:
./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
iv.	Data Type: Integer
v.	Value: 1 
 
Group Policy (hybrid Azure AD joined): 
You can configure the following Group Policy settings to enable FIDO for your enterprise. These are available for both User configuration and Computer configuration under Policies > Administrative Templates > System > Logon
Policy	Options
AllowSecurityKeySignIn 	Not Configured: Security key is not available as an option for sign in
Enabled: Security key is available as an option for sign in
Disabled: Security key is not available as an option for sign in
	8.5 Update the PCs you will be piloting to latest Windows Insider Vibranium Build 
	Make sure the Client PC you are planning to try the scenarios out are running the Windows Insider Vibranium Build #18945 or newer, available starting 7/23.

## Test scenarios

This section includes test scenarios from an end user persona that you may consider using and test the feature.
	9.1.  Provision a security key 
1.	Make sure you have a Microsoft-compatible FIDO2 security key
2.	Browse to https://myprofile.microsoft.com
3.	Sign in if not already
4.	Click Security Info 
a.	If the user already has at least one Azure Multi-Factor Authentication method registered, they can immediately register a FIDO2 security key.
b.	If they don’t have at least one Azure Multi-Factor Authentication method registered, they must add one.
5.	Add a FIDO2 Security key by clicking Add method and choosing Security key
6.	Choose USB device or NFC device
7.	Have your key ready and choose Next
8.	A box will appear and ask you to create/enter a PIN for your security key, then perform the required gesture for your key either biometric or touch.
9.	You will be returned to the combined registration experience and asked to provide a meaningful name for your token so you can identify which one if you have multiple. Click Next.
10.	Click Done to complete the process

	9.2.  First login to Azure AD joined PC and verify Single Sign-On (SSO) to cloud resources and On-prem resources (Kerberos and NTLM)
1.	Make sure you have network / internet access
2.	On the lock screen, choose the FIDO sign-in option
3.	Follow instructions on the screen to proceed to login
4.	Access cloud resources to confirm you get SSO
•	E.g office.com
5.	Access on-premises resources to confirm you get SSO
•	Kerberos based resources
•	NTLM based resources
6.	Lock screen (Windows + L)

Note: Windows Hello Face is the intended best experience for a device where a user has it enrolled. But you can turn off Hello Face sign in by removing your Face Enrollment in Settings Sign-In Options if it's preventing you from trying the FIDO logon scenario.
	9.3.  First login to hybrid Azure AD joined PC and verify Single Sign-On (SSO) to cloud resources and On-prem resources (Kerberos and NTLM)
1.	Make sure you have network / internet access
2.	For hybrid Azure AD joined setup, make sure you are connected to your corporate network (have line of sight to your domain controller)
3.	On the lock screen, choose the FIDO sign-in option
4.	Follow instructions on the screen to proceed to login
5.	Access cloud resources to confirm you get SSO
•	E.g office.com
6.	Access on-premises resources to confirm you get SSO
•	Kerberos based resources
•	NTLM based resources
7.	Lock screen (Windows + L)

Note: Windows Hello Face is the intended best experience for a device where a user has it enrolled. But you can turn off Hello Face sign in by removing your Face Enrollment in Settings Sign-In Options if it's preventing you from trying the FIDO logon scenario.

	 9.4. Unlock PC using a security key
1.	Make sure you have signed into this PC before
2.	On the lock screen, choose the FIDO sign-in option
3.	Follow instructions on the screen to proceed to login
4.	Access cloud resources to confirm you get SSO
•	E.g office.com
5.	Access on-premises resources to confirm you get SSO
•	Kerberos based resources
•	NTLM based resources
6.	Lock screen (Windows + L)

	 9.5. Manage your security key via Settings
1.	Once you’ve unlocked your machine using FIDO, open the inbox “Settings” app
2.	Navigate to Accounts > Sign-in options > Security Key
3.	Click Manage and touch your security key.
4.	Based on the capability of your security key, various options may light up
 
5.	Try changing your security key PIN
6.	If your security key has biometric capability, try enrolling fingerprints
7.	Try resetting your security key by following instructions on the screen
•	Note: Resetting your key will wipe off ALL data and restore it to factory default
•	Resetting of keys may require you to unplug, re-plug and touch the blinking security key couple of times

	 9.6. Offline unlock
1.	Make sure you have signed into this PC before
2.	Turn off the network / put the PC in airplane mode
•	For hybrid Azure AD joined machines, you can disconnect from your corporate network
3.	On the lock screen, choose the FIDO sign-in option
4.	Follow instructions on the screen to proceed to login
5.	You should be able to get to your desktop but not have access to cloud or on-premises resources
6.	Lock screen (Windows + L)

## Troubleshooting and feedback

If you would like to share feedback or encounter issues while previewing this feature, please share them via Feedback Hub

1. Launch Feedback Hub and make sure you're signed in
1. Submit feedback under 
   1. Category: Security and Privacy 
   1. Subcategory: FIDO
1. To capture logs use, Recreate my Problem

11.	Additional Material
	Scenario Webcast: https://www.yammer.com/wsscengineering/#/files/109518749

## Under the hood

How SSO works for on prem resources using FIDO2 keys – Under the hood
You may choose to have Azure Active Directory issue Kerberos Ticket Granting Tickets (TGTs) for one or more of your Active Directory domains. This allows you to sign into Windows with modern credentials like FIDO and access your traditional Active Directory based resources. Kerberos Service Tickets and authorization will continue to be controlled by your on-premises Active Directory Domain Controllers.
An Azure AD Kerberos Server object will be created in your on-premises Active Directory and then securely published to Azure Active Directory. The object is not associated with any physical servers. It is simply a representation of a Domain Controller that can be used by Azure Active Directory to generate Kerberos Ticket Granting Tickets (TGTs) for your Active Directory Domain.
The object appears in the directory as a Read Only Domain Controller object. The same rules and restrictions used for Read Only Domain Controllers apply to the Azure AD Kerberos Server object.  

## Known issues

Logon with FIDO will be blocked if your password has expired. expectation for user to reset password over lock screen before being able to login using FIDO

## Frequently asked questions

### Does this work in my on-premises environment?

This feature does NOT work for a pure on-premises environment.

### My organization requires two factor authentication to access resources, what can I do to support this?

Security keys come in a variety of form factors. Please contact the device manufacturer of interest to discuss how their devices can be enabled with a PIN or biometric as a second factor.

### Can admins set up security keys?

We are working on this capability for GA of this feature.

### Where can I go to find compliant Security Keys?

The following vendors provide security keys that work with our services. Please work with these vendors to get the correct set of keys for validation.

- Yubico
- Feitian
- HID
- Ensurity
- eWBM

Check out this page to get a list of vendors that provide compatible keys

### Can I use a phone as a Windows Hello security key?

We are investigating support for phones for future releases.

### What do I do if I lose my Security Key?

You can remove keys from the Azure portal, by navigating to the security info page and removing the security key 

### What do I do if Windows Hello Face is too quick and preventing me from trying FIDO?

Windows Hello Face is the intended best experience for a device where a user has it enrolled. But you can turn off Hello Face sign in by removing your Face Enrollment in Settings Sign-In Options if it's preventing you from trying the FIDO logon scenario.

### I’m not able to use FIDO immediately after I create a hybrid Azure AD joined machine

If clean installing a hybrid Azure AD joined machine, post domain join and restart you must sign in with a password and wait for policy to sync before being able to use FIDO to sign in

- Check your current status by typing dsregcmd /status into a command window and check that both AzureAdJoined and DomainJoined are showing YES.
- This is a known limitation for domain joined devices and not FIDO specific.

### I’m unable to get SSO to my NTLM network resource after signing in with FIDO and get a credential prompt

There are known limitations based on how many servers have the new build and which will respond in time to service your resource request. To check if you can see a server that is running the feature, check the output of nltest /dsgetdc:redmond /keylist /kdc

## Next steps
