---
title: Azure Multi-Factor Authentication user data collection
description: What information is used to help authenticate users by Azure Multi-Factor Authentication?
services: multi-factor-authentication

ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi

ms.service: multi-factor-authentication
ms.workload: identity
ms.topic: article
ms.date: 05/01/2018

---
# Azure Multi-Factor Authentication user data collection

This document explains how to find user information collected by Azure Multi-Factor Authentication Server (MFA Server) and Azure MFA (Cloud-based) in the event you would like to remove it.

[!INCLUDE [gdpr-intro-sentence](../../../includes/gdpr-dsr-and-stp-note.md)]

## Information collected

MFA server, the NPS Extension, and the Windows Server 2016 Azure MFA AD FS Adapter collect and store the following information for 90 days.

Authentication Attempts (used for reporting and troubleshooting):

- Timestamp
- Username
- First Name
- Last Name
- Email Address
- User Group
- Authentication Method (Phone Call, Text Message, Mobile App, OATH Token)
- Phone Call Mode (Standard, PIN)
- Text Message Direction (One-Way, Two-Way)
- Text Message Mode (OTP, OTP + PIN)
- Mobile App Mode (Standard, PIN)
- OATH Token Mode (Standard, PIN)
- Authentication Type
- Application Name
- Primary Call Country Code
- Primary Call Phone Number
- Primary Call Extension
- Primary Call Authenticated
- Primary Call Result
- Backup Call Country Code
- Backup Call Phone Number
- Backup Call Extension
- Backup Call Authenticated
- Backup Call Result
- Overall Result Timestamp
- Overall Authenticated
- Overall Result
- Results
- Result Timestamp
- Authenticated
- Result
- Initiating IP Address
- Devices
- Notification Timestamp
- Device Token
- Device Type
- Mobile App Version
- OS Version
- Result Timestamp
- Authenticated
- Result
- Used Check for Notification

Activations (attempts to activate an account in the Microsoft Authenticator mobile app)
- Username
- Account Name
- Get Activation Code Timestamp
- Get Activation Code Result
- Activate Timestamp
- Activate Success
- Activate Error
- Activation Status Timestamp
- Activation Status Result
- Device  Name
- Device Type
- App Version
- OATH Token Enabled

Blocks (used to determine blocked state and for reporting):

- Block Timestamp
- Block By Username
- Username
- Country Code
- Phone Number
- Phone Number Formatted
- Extension
- Clean Extension
- Blocked
- Block Reason
- Completion Timestamp
- Completion Reason
- Account Lockout
- Account Lockout Email Sent
- Fraud Alert
- Fraud Alert Email Sent
- Fraud Alert Not Blocked
- Language

Bypasses (used for reporting):

- Bypass Timestamp
- Bypass Seconds
- Bypass By Username
- Username
- Country Code
- Phone Number
- Phone Number Formatted
- Extension
- Clean Extension
- Bypass Reason
- Completion Timestamp
- Completion Reason
- Bypass Used
- Bypass Email Sent

Changes (used to sync user changes to MFA Server or AAD):

- Change Timestamp
- Username
- New Country Code
- New Phone Number
- New Extension
- New Backup Country Code
- New Backup Phone Number
- New Backup Extension
- New PIN
- PIN Change Required
- Old Device Token
- New Device Token

## Gather data from MFA Server

For MFA Server version 8.0 or higher the following process allows administrators to export all data for users:

- Log in to your MFA Server, navigate to the **Users** tab, find, and select the user in question and click the **Edit** button. Take screenshots (Alt-PrtScn) of each tab to provide the user their current MFA settings.
- From the command line of the MFA Server, run the following command changing the path according to your installation `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` to produce a JSON formatted file.
- Search `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` and any backups for `"<username>"` (include the quotes in the search) to find all instances of the user record being added or changed.
   - These records can be limited (but not eliminated) by unchecking **“Log user changes”** in the MFA Server UX, Logging section, Log Files tab.
   - If syslog is configured, and **“Log user changes”** is checked in the MFA Server UX, Logging section, Syslog tab, then the log entries can be gathered from syslog instead.
- Other occurrences of the username in MultiFactorAuthSvc.log and other MFA Server log files pertaining to authentication attempts are considered operational and duplicative to the information provided using MultiFactorAuthGdpr.exe export or Web Service SDK GetUserGdpr.
- For MFA Server administrators and User Portal administrators, this export process includes records for other users that the administrator changed.
- Inform the user that if they have used the Microsoft Authenticator app to send logs to Microsoft in the past 30 days, they can also view these logs in the app to know what data was sent. Microsoft deletes these logs after 30 days.
- Administrators can also use the Web Service SDK GetUserGdpr operation as an option to export all MFA cloud service information collected for a given user or use the Web Service SDK GetUserGdpr operation to incorporate your MFA Server into a larger solution.

## Delete data from MFA Server

From the command line of the MFA Server, run the following command changing the path according to your installation `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` to delete all MFA cloud service information collected for this user.

- Data included in the export is deleted in real time, but it may take up to 30 days for operational or duplicative data to be fully removed.
- Delete the user record from the MFA Server UX, Users section.
- Delete all MFA Server log files that contain data older than 30 days.
   - This includes logs on all MFA Servers, servers with the MFA Server AD FS Adapter, and servers with MFA Server User Portal.
- Administrators can also use the Web Service SDK DeleteUserGdpr operation as an option to delete all MFA cloud service information collected for a given user or use the Web Service SDK DeleteUserGdpr operation to incorporate your MFA Server into a larger solution.

## Gather data from NPS Extension

Use the [Microsoft Privacy Portal](https://portal.azure.com/?feature.customportal=false&feature.userPrivacy=true#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Privacy) to make a request for Export.

- MFA information is included in the export, which may take hours or days to complete.
- Occurrences of the username in the AzureMfa/AuthN/AuthNOptCh, AzureMfa/AuthZ/AuthZAdminCh, and AzureMfa/AuthZ/AuthZOptCh event logs are considered operational and duplicative to the information provided in the export.

## Delete data from NPS Extension

Use the [Microsoft Privacy Portal](https://portal.azure.com/?feature.customportal=false&feature.userPrivacy=true#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Privacy) to make a request for Account Close to delete all MFA cloud service information collected for this user.

- It may take up to 30 days for data to be fully removed.
- Delete all AzureMfa/AuthN/AuthNOptCh, AzureMfa/AuthZ/AuthZAdminCh, and AzureMfa/AuthZ/AuthZOptCh event logs older than 30 days.

## Gather data from Windows Server 2016 Azure MFA AD FS Adapter

Use the [Microsoft Privacy Portal](https://portal.azure.com/?feature.customportal=false&feature.userPrivacy=true#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Privacy) to make a request for Export. 

- MFA information is included in the export, which may take hours or days to complete.
- Occurrences of the username in the AD FS Tracing/Debug event logs (if enabled) are considered operational and duplicative to the information provided in the export.

## Delete data from Windows Server 2016 Azure MFA AD FS Adapter

Use the [Microsoft Privacy Portal](https://portal.azure.com/?feature.customportal=false&feature.userPrivacy=true#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Privacy) to make a request for Account Close to delete all MFA cloud service information collected for this user.

- It may take up to 30 days for data to be fully removed.
- Delete all AD FS Tracing/Debug event logs older than 30 days (if enabled).

## Gather data for Azure MFA

Use the [Microsoft Privacy Portal](https://portal.azure.com/?feature.customportal=false&feature.userPrivacy=true#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Privacy) to make a request for Export.

   - MFA information is included in the export, which may take hours or days to complete.

## Delete Data for Azure MFA

Use the [Microsoft Privacy Portal](https://portal.azure.com/?feature.customportal=false&feature.userPrivacy=true#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Privacy) to make a request for Account Close to delete all MFA cloud service information collected for this user.

- It may take up to 30 days for data to be fully removed.

## Next steps

[MFA Server reporting](howto-mfa-reporting.md)
