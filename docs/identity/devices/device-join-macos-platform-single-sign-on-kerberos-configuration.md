---
title: Enable Kerberos SSO to On-Premises Active Directory and Entra ID Kerberos Resources in Platform SSO
description: How administrators can set up macOS Platform Single Sign-on to support Kerberos authentication to on-premises Active Directory and Entra ID kerberos-integrated resources.

ms.service: entra-id
ms.subservice: devices
ms.topic: tutorial
ms.date: 05/13/2024

ms.author: miepping
author: miepping
manager: 
ms.reviewer: brianmel
#Customer intent: As a user I want to understand how to set up a Mac device with macOS Platform Single Sign-on (PSSO) during the out of box experience. I want to know the difference betwwen setting up with secure enclave, smart card or password based authentication methods.
---

# Enable Kerberos SSO to On-Premises Active Directory and Entra ID Kerberos Resources in Platform SSO

Mac users can join their new device to Microsoft Entra ID during the first-run out-of-box experience (OOBE). The macOS Platform single sign-on (PSSO) is a capability on macOS that is enabled using the [Microsoft Enterprise Single Sign-on Extension](../../identity-platform/apple-sso-plugin.md). PSSO allows users to sign in to a Mac device using a hardware-bound key, smart card or their Microsoft Entra ID password.

This tutorial shows you how to configure Platform SSO to support Kerberos-based SSO to on-premises and cloud resources, in addition to SSO to Entra ID. This is an optional capability within Platform SSO, but it is recommended if users still need to access on-premises Active Directory resources that use Kerberos for authentication.

## Prerequisites

- A minimum version of **macOS 14.5 Sonoma**. Earlier versions of macOS will not support Kerberos SSO combined with Platform SSO or are known to have bugs.
- [Microsoft Intune Company Portal](/mem/intune/apps/apps-company-portal-macos) version 5.2404.0 or later
- A Mac device enrolled in mobile device management (MDM) with Microsoft Intune.
- A configured SSO extension MDM payload with Platform SSO settings in Intune by an administrator, already deployed to the device. Refer to the [Platform SSO documentation](./macos-psso.md) or [Intune deployment guide](/mem/intune/configuration/platform-sso-macos) if Intune is your MDM.

## Set up your macOS device

Refer to the [Entra ID macOS Platform SSO documentation](./macos-psso.md) to learn how to configure and deploy Platform SSO. This must be performed regardless of whether you choose to deploy Kerberos SSO using this guide.

## Kerberos SSO MDM Profile Configuration

You must configure a Kerberos SSO MDM profile. It is recommended to use the following settings. Make sure that you replace all references to **contoso.com** and **Contoso** with the proper values for your environment:

| Configuration Key | Recommended Value | Note |
|-|-|-|
| preferredKDCs | <string>kkdcp://login.microsoftonline.com/contoso.com/kerberos</string> | Replace the **contoso.com** value with the value of one of your tenant domains or your tenant's GUID |
| Hosts | <string>contoso.com</string> | Replace **contoso.com** with your on-premises domain/forest name |
| Hosts | <string>*.contoso.com</string> | Replace **contoso.com** with your on-premises domain/forest name. Keep the preceding `*.` characters before your domain/forest name |
| PayloadOrganization | <string>Contoso</string> | Replace **Contoso** with the name of your organization |

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>PayloadContent</key>
    <array>
        <dict>
            <key>ExtensionData</key>
            <dict>
                <key>allowPasswordChange</key>
                <true/>
                <key>allowPlatformSSOAuthFallback</key>
                <true/>
                <key>performKerberosOnly</key>
                <true/>
                <key>pwReqComplexity</key>
                <true/>
                <key>syncLocalPassword</key>
                <true/>
                <key>usePlatformSSOTGT</key>
                <true/>
                <key>preferredKDCs</key>                         
                <array>
                <string>kkdcp://login.microsoftonline.com/contoso.com/kerberos</string>
                </array>
            </dict>
            <key>ExtensionIdentifier</key>
            <string>com.apple.AppSSOKerberos.KerberosExtension</string>
            <key>Hosts</key>
            <array>
                <string>contoso.com</string>
                <string>*.contoso.com</string>
                <string>windows.net</string>
                <string>*.windows.net</string>
                <string>KERBEROS.MICROSOFTONLINE.COM</string>
                <string>MICROSOFTONLINE.COM</string>
                <string>*.MICROSOFTONLINE.COM</string>
            </array>
            <key>PayloadDisplayName</key>
            <string>Single Sign-On Extensions Payload</string>
            <key>PayloadIdentifier</key>
            <string>985C1EDE-75A4-4248-B972-12E3E6A1E94C</string>
            <key>PayloadType</key>
            <string>com.apple.extensiblesso</string>
            <key>PayloadUUID</key>
            <string>985C1EDE-75A4-4248-B972-12E3E6A1E94C</string>
            <key>PayloadVersion</key>
            <integer>1</integer>
            <key>Realm</key>
            <string>KERBEROS.MICROSOFTONLINE.COM</string>
            <key>TeamIdentifier</key>
            <string>apple</string>
            <key>Type</key>
            <string>Credential</string>
            <key>URLs</key>
            <array/>
        </dict>
    </array>
    <key>PayloadDescription</key>
    <string></string>
    <key>PayloadDisplayName</key>
    <string>Kerberos SSO Extension for macOS</string>
    <key>PayloadEnabled</key>
    <true/>
    <key>PayloadIdentifier</key>
    <string>A05AE53E-FC8E-472F-9821-FD21CBEC04C8</string>
    <key>PayloadOrganization</key>
    <string>Contoso</string>
    <key>PayloadRemovalDisallowed</key>
    <true/>
    <key>PayloadScope</key>
    <string>System</string>
    <key>PayloadType</key>
    <string>Configuration</string>
    <key>PayloadUUID</key>
    <string>A05AE53E-FC8E-472F-9821-FD21CBEC04C8</string>
    <key>PayloadVersion</key>
    <integer>1</integer>
</dict>
</plist>
```

Once you have updated the configuration to use the proper values for your environment, save the configuration using a text editor as a mobileconfig file, such as kerberos.mobileconfig

### Intune Configuration Steps

If you use Intune as your MDM you can perform the following steps to deploy the profile. Make sure you follow the [previous instructions](#kerberos-sso-mdm-profile-configuration) about replacing **contoso.com** values with the proper values for your organization.

1. Sign in to the [Microsoft Intune admin center](https://go.microsoft.com/fwlink/?linkid=2109431).
2. Select **Devices** > **Configuration** > **Create** > **New policy**.
3. Enter the following properties:

    - **Platform**: Select **macOS**.
    - **Profile type**: Select **Templates**.

4. Choose the **Custom** template
5. Select **Create**.
6. In **Basics**, enter the following properties:

    - **Name**: Enter a descriptive name for the policy. Name your policies so you can easily identify them later. For example, name the policy **macOS - Platform SSO Kerberos**.
    - **Description**: Enter a description for the policy. This setting is optional, but recommended.

7. Select **Next**.
8. Enter a name in the **Custom configuration profile name** box.
9. Choose a **Deployment channel**. Device channel is recommended.
10. Click the folder icon to upload your **Configuration profile file**. Choose the kerberos.mobileconfig file you [saved previously](#kerberos-sso-mdm-profile-configuration) after customizing the template.
11. Select **Next**.
12. In **Scope tags** (optional), assign a tag to filter the profile to specific IT groups, such as `US-NC IT Team` or `JohnGlenn_ITDepartment`. For more information about scope tags, see [Use RBAC roles and scope tags for distributed IT](/mem/intune/fundamentals/scope-tags).

    Select **Next**.

13. In **Assignments**, select the users or user groups that will receive your profile. Platform SSO policies are user-based policies. Don't assign the platform SSO policy to devices.

    For more information on assigning profiles, see [Assign user and device profiles](/mem/intune/configuration/device-profile-assign).

    Select **Next**.

14. In **Review + create**, review your settings. When you select **Create**, your changes are saved, and the profile is assigned. The policy is also shown in the profiles list.

The next time the device checks for configuration updates, the settings you configured are applied.

## Testing Kerberos SSO

Once the profile has been assigned to the device you can check that your device has Kerberos tickets by running the following command in the Terminal app:

> ```console
> app-sso platform -s
> ```

You should have two Kerberos tickets, one for your on-premises AD with the ticketKeyPath value of **tgt_ad** and one for your Entra ID tenant with the ticketKeyPath value of **tgt_cloud**. The output should resemble the following:

:::image type="content" source="media/device-registration-macos-platform-single-sign-on/psso-kerberos-terminal-output.png" alt-text="Screenshot of the output of app-sso platform -s in the macOS Terminal app.":::

Validate your configuration is working by testing with appropriate Kerberos-capable resources:

1. Test on-premises Active Directory functionality by accessing an on-premises AD-integrated file server using Finder or a web application using Safari.
2. Test Entra ID Kerberos functionality by accessing an Azure Files share enabled for Entra ID cloud kerberos. Refer to [this guide](/azure/storage/files/storage-files-identity-auth-hybrid-identities-enable) if you need to configure a cloud file share in Azure Files.

## See also

- [Join a Mac device with Microsoft Entra ID using Company Portal](./device-join-microsoft-entra-company-portal.md)
- [Passwordless authentication options for Microsoft Entra ID](../authentication/concept-authentication-passwordless.md)
- [Plan a passwordless authentication deployment in Microsoft Entra ID](../authentication/howto-authentication-passwordless-deployment.md)
- [Microsoft Enterprise SSO plug-in for Apple devices](../../identity-platform/apple-sso-plugin.md)