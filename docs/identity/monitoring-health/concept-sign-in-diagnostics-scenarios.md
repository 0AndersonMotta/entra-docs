---

title: What is the sign-in diagnostic for Azure Active Directory?
description: Provides a general overview of the sign-in diagnostic in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''

ms.assetid: e2b3d8ce-708a-46e4-b474-123792f35526
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/07/2021
ms.author: markvi
ms.reviewer: tspring  

# Customer intent: As an Azure AD administrator, I want a tool that gives me the right level of insights into the sign-in activities in my system so that I can easily diagnose and solve problems when they occur.
ms.collection: M365-identity-device-management
---

# Sign-in diagnostics for Azure AD scenarios

You can use the sign-in diagnostic for Azure AD to analyze what happened during a sign-in attempt and get recommendations for resolving problems without needing to involve Microsoft support.  

This article gives you an overview of the types of scenarios you can identify and resolve when using this tool.

## Supported scenarios

The sign-in diagnostic for Azure AD provides you with support for the follwowing scenarios:

- **Conditional Access**

    - Blocked by conditional access  

    - Failed conditional access  

    - Multifactor authentication (MFA) from conditional access  

    - B2B Blocked Sign-In Due to Conditional Access 

- **Multi-Factor Authentication (MFA)**  

    - MFA from other requirements  

    - MFA proof up required  

    - MFA proof up required (risky sign-in location)  

- **Correct & Incorrect Credentials**  

    - Successful sign-in  

    - Account locked  

    - Invalid username or password  

- **Enterprise Apps**  

    - Enterprise apps service provider  

    - Enterprise apps configuration  

- **Other Scenarios**   

    - Security Defaults  
    
    - Error code insights  

    - Legacy Authentication  

    - Blocked by Risk Policy 

 

## Conditional Access  


### Blocked by conditional access 

In this scenario, a sign-in attempt has been blocked by a conditional access policy. 


Description automatically generatedShape Screenshot showing access configuration with Block access selected. 

The diagnostic section for this scenario shows details about the user sign-in event and the applied policies. 

 

### Failed conditional access 

This scenario is typically a result of a sign-in attempt that failed because the requirements of a conditional access policy were not satisfied. Common examples are: 

Graphical user interface, text, application

Description automatically generatedShape Screenshot showing access configuration with common policy examples and Grant access selected. 

- Require hybrid Azure AD joined device 

- Require approved client app 

- Require app protection policy 

The diagnostic section for this scenario shows details about the user sign-in attempt and the applied policies. 

 

### MFA from conditional access 

In this scenario, a conditional access policy has the requirement to sign in using multifactor authentication set. 

Graphical user interface, text, application, email

Description automatically generatedShape Screenshot showing access configuration with Require multifactor authentication selected. 

The diagnostic section for this scenario shows details about the user sign-in attempt and the applied policies. 

 

 

## Multi-Factor Authentication (MFA)  

### MFA from other requirements 

In this scenario, a multifactor authentication requirement wasn't enforced by a conditional access policy. For example, multifactor authentication on a per-user basis. 

Graphical user interface, text, application, email

Description automatically generatedShape Screenshot showing multifactor authentication per user configuration. 

The intent of this diagnostic scenario is to provide more details about: 

The source of the interrupted multifactor authentication 

The result of the client interaction 

You can also view all details of the user sign-in attempt. 

 

### MFA proof up required 

In this scenario, sign-in attempts were interrupted by requests to set up multifactor authentication. This setup is also known as proof up. 

 

Multifactor authentication proof up occurs when a user is required to use multifactor authentication but has not configured it yet, or an administrator has required the user to configure it. 

 

The intent of this diagnostic scenario is to reveal that the multifactor authentication interruption was due to lack of user configuration. The recommended solution is for the user to complete the proof up. 

 

### MFA proof up required (risky sign-in location) 

In this scenario, sign-in attempts were interrupted by a request to set up multifactor authentication from a risky sign-in location. 

 

The intent of this diagnostic scenario is to reveal that the multifactor authentication interruption was due to lack of user configuration. The recommended solution is for the user to complete the proof up, specifically from a network location that doesn't appear risky. 

 

An example of this scenario is when policy requires that the user setup MFA only from trusted network locations but the user is signing in from an untrusted network location. 

 

## Correct & Incorrect Credential

### Successful sign-in 

In this scenario, sign-in events were not interrupted by conditional access or multifactor authentication.  

 

This diagnostic scenario provides details about user sign-in events that were expected to be interrupted due to conditional access policies or multifactor authentication. 

 

### The account is locked 

In this scenario, a user signed-in with incorrect credentials too many times. This scenario happens when too many password-based sign-in attempts have occurred with incorrect credentials. The diagnostic scenario provides information for the admin to determine where the attempts are coming from and if they are legitimate user sign-in attempts or not. 

 

This diagnostic scenario provides details about the apps, the number of attempts, the device used, the operating system and the IP address. 

 

More information about this topic can be found in the Azure AD Smart Lockout documentation. 

 

 

### Invalid username or password 

In this scenario, a user tried to sign-in using an invalid username or password. The diagnostic is intended to allow an administrator to determine if the problem is with a user entering incorrect credentials, or a client and/or application(s) which have cached an old password and are resubmitting it. 

 

This diagnostic scenario provides details about the apps, the number of attempts, the device used, the operating system and the IP address. 

 

## Enterprise App 

In Enterprise Applications there are two points where problems may occur: at the Identity Provider (Azure AD) application configuration or at the Service Provider (application service, also known as SaaS application) side.  

 

Diagnostics for these problems address which side of the problem should be looked at for resolution and what to do. 

 

### Enterprise apps service provider 

In this scenario, a user tried to sign-in to an application. The sign-in failed due to a problem with the application (also known as service provider) side of the sign-in flow. Problems detected by this diagnosis typically must be resolved by changing the configuration or fixing problems on the application service.  

Resolution for this scenario means signing into the other service and changing some configuration per the diagnostic guidance. 

 

### Enterprise apps configuration 

In this scenario, a sign-in failed due to an application configuration issue for the Azure AD side of the application. 

 

Resolution for this scenario requires reviewing and updating the configuration of the application in the Enterprise Applications blade entry for the application. 

 

## Other Scenario Issues 

### Security Defaults 

This scenario covers sign-in events where the user’s sign-in was interrupted due to Security Defaults settings. Security Defaults enforce best practice security for your organization and will require Multi-Factor Authentication (MFA) to be configured and used in many scenarios to prevent password sprays, replay attacks and phishing attempts from being successful. 

More information can be found at this link. 

### Error code insights 

When an event does not have a contextual analysis in the Sign-in Diagnostic an updated error code explanation and relevant content may be shown. The error code insights will contain detailed text about the scenario, how to remediate the problem and any content to read regarding the problem. 

### Legacy Authentication 

This diagnostics scenario diagnosis a sign-in event which was blocked or interrupted since the client was attempting to use Basic (also known as Legacy) Authentication. 

Preventing legacy authentication sign-in is recommended as the best practice for security. Legacy authentication protocols like POP, SMTP, IMAP, and MAPI cannot enforce Multi-Factor Authentication (MFA) which makes them preferred entry points for adversaries to attack your organization. 

More information on this topic can be found at this link. 

### B2B Blocked Sign-in Due to Conditional Access 

This diagnostic scenario detects a blocked or interrupted sign-in due to the user being from another organization-a B2B sign-in-where a Conditional Access policy requires that the client's device is joined to the resource tenant. 

More information on this scenario can be found at this link. 

### Blocked by Risk Policy 

This scenario is where Identity Protection Policy blocks a sign-in attempt due to the sign-in attempt having been identified as risky. 

More information on this scenario can be found at this link. 

 

If your scenario is not covered in this list, provide feedback by contacting us through [[email]]. 

 



## Next steps

- [What are Azure Active Directory reports?](overview-reports.md)
- [What is Azure Active Directory monitoring?](overview-monitoring.md)
