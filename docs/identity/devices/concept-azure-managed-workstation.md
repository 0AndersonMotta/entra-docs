---
title: Understand secure workstations - Azure Active Directory
description: Learn about secure, Azure-managed workstations and understand why they're important.

services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019

ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim

# Customer intent: As an administrator, I want to provide staff with secured workstations to reduce the risk of breach due to misconfiguration or compromise.

ms.collection: M365-identity-device-management
---

# Understand secure, Azure-managed workstations

Secured, isolated workstations are critically important for the security of sensitive roles like administrators, developers, and critical service operators. If client workstation security is compromised, many security controls and assurances can fail or be ineffective .

This document explains what you need for building a secure workstation, often known as a privileged access workstation (PAW). The article also contains detailed instructions to set up initial security controls. This guidance describes how cloud-based technology can manage the service. It's an approach that relies on security capabilities introduced in Windows 10RS5, Microsoft Defender ATP, Azure Active Directory, and Intune.

> [!NOTE]
> This article explains the concept of a secure workstation and its importance. If you are already familiar with the concept and would like to skip to deployment, visit [Deploy a Secure Workstation](https://docs.microsoft.com/azure/active-directory/devices/howto-azure-managed-workstation).

## Why secure workstation access is important

The rapid adoption of cloud services and the ability to work from anywhere has created a new exploitation method. By exploiting weak security controls on devices where administrators work, attackers can gain access to privileged resources.

Privileged misuse and supply chain attacks are among the top five methods that attackers use to breach organizations. They are also the second most commonly detected tactics in incidents reported in 2018 according to the [Verizon Threat report](https://enterprise.verizon.com/resources/reports/dbir/), and [Security Intelligence Report](https://aka.ms/sir).

Most attackers follow these steps:

1. Reconnaissance to find a way in, often specific to an industry.
1. Analysis to collect information and identify the best way to infiltrate a workstation that is perceived as low value.
1. Persistence to look for a means to move [laterally](https://en.wikipedia.org/wiki/Network_Lateral_Movement).
1. Exfiltration of confidential and sensitive data.

During reconnaissance, attackers frequently infiltrate devices that seem low risk or undervalued. They use these vulnerable devices to locate an opportunity for lateral movement and to find administrative users and devices. After they gain access to privileged user roles, attackers identify high value data and successfully exfiltrate that data.

![Typical compromise pattern](./media/concept-azure-managed-workstation/typical-timeline.png)

This document describes a solution that can  help protect your computing devices from such lateral attacks. The solution accomplishes this by isolating management and services from less valuable productivity devices, breaking the chain prior to infiltration of the device used to manage or access sensitive cloud resources. The solution uses native Azure services that are part of the Microsoft 365 Enterprise stack:

* Intune for device management and a safe list of applications and URLs
* Autopilot for device setup, deployment, and refresh
* Azure AD for user management, conditional access, and multi-factor authentication
* Windows 10 (current version) for device health attestation and user experience
* Microsoft Defender Advanced Threat Protection (ATP) for cloud-managed endpoint protection, detection, and response
* Azure AD PIM for managing authorization and just-in-time (JIT) privileged access to resources

## Who benefits from a secure workstation?

All users and operators benefit when using a secure workstation. An attacker who compromises a PC or device can impersonate all cached accounts. When logged in to the device, they can also use credentials and tokens. This risk makes it very important to secure devices that are used for privileged roles, including administrative rights. Devices with privileged accounts are targets for lateral movement and privilege escalation attacks. These accounts can be used for a variety of assets such as:

* Administrator of on-premises or cloud-based systems
* Developer workstation for critical systems
* Social media account administrator with high exposure
* Highly sensitive workstation, such as a SWIFT payment terminal
* Workstation handling trade secrets

To reduce risk, you should implement elevated security controls for privileged workstations that make use of these accounts. For additional guidance, see the [Azure Active Directory feature deployment guide](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2), [Office 365 roadmap](https://aka.ms/o365secroadmap), and [Securing Privileged Access roadmap](https://aka.ms/sparoadmap)).

## Why use dedicated workstations?

While it is possible to add security to an existing device, it is better to start with a secure foundation. To put your organization in the best position to maintain a high security level, start with a device you know is secure and implement a set of known security controls.

A growing number of attack vectors through email and web browsing makes increasingly hard to be sure that a device can be trusted. This guides assumes that a dedicated workstation is isolated from standard productivity, browsing, and email. Removal of productivity, web browsing, and email from a device can have a negative impact on productivity. However, this safeguard is typically acceptable for scenarios where the job tasks don’t explicitly require it and risk of a security incident is high.

> [!NOTE]
> Web browsing here refers to general access to arbitrary websites which can be a high risk activity. Such browsing is distinctly different from using a web browser to access a small number of well-known administrative websites for services like Azure, Office 365, other cloud providers, and SaaS applications.

Containment strategies provide increased security by increasing the number and type of controls that an attacker must overcome to access sensitive assets. The model described in this article uses a tiered privilege design and restricts administrative privileges to specific devices.

## Supply chain management

Essential to a secured workstation is a supply chain solution where you use a trusted workstation called the 'root of trust'. For this solution, the root of trust uses [Microsoft Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-autopilot) technology. To secure a workstation, Microsoft Autopilot lets you  leverage Microsoft OEM-optimized Windows 10 devices. These devices come in a known good state from the manufacturer. Instead of re-imaging a potentially insecure device, Microsoft Autopilot can transform a Windows device into a “business-ready” state. It applies settings and policies, installs apps, and even changes the edition of Windows 10. For example, Autopilot can switch devices from Windows 10 Pro to Windows 10 Enterprise to support advanced features.

![Secure workstation Levels](./media/concept-azure-managed-workstation/supplychain.png)

## Device roles and profiles

Throughout the guidance, multiple security profiles and roles will be addressed to achieve a more secure solution for users, developers, and IT operations staff. These profiles have been aligned to support common users in organizations that can benefit from an enhanced, or secure workstation, while balancing usability and risk. The guidance will provide configuration of settings based on industry accepted standards. This guidance is used to illustrate a method in hardening Windows 10 and reducing the risks associated with device or user compromise using policy and technology to help manage security features and risks.
![Secure workstation Levels](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Low Security** – A managed standard workstation provides a good starting point for most home, and small business use. These devices are Azure AD registered and Intune managed. The profile permits users to run any applications and browse any website. An anti-malware solution like [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) should be enabled.
* **Enhanced Security** – Is an entry level protected solution, good for home users, small business users, as well as general developers.
   * The Enhanced workstation provides a policy based means to enhance the security of the Low Security profile. This profile allows for a secure means to work with customer data, and be able to use productivity tools such as checking email and web browsing. An Enhanced workstation can be used to audit user behavior, and profile use of a workstation by enabling audit policies, and logging to Intune. In this profile, the workstation will enable security controls and policies described in the content, and deployed in the Enhanced Workstation - Windows10 (1809) script. The deployment also takes advantage of advanced malware protection using [Advanced Threat Protection (ATP)](https://docs.microsoft.com/office365/securitycompliance/office-365-atp)
* **High Security** – The most effective means to reduce the attack surface of a workstation is to remove the ability to self-administer the workstation. Removing local administrative rights is a step that enhances security and can impact productivity if implemented incorrectly. The High Security profile builds on the enhanced security profile with one considerable change, the removal of the local admin. This profile is designed to help with users that maybe a high profile user such as an executive or users that may have contact with sensitive data such as payroll, or approval of services, and processes.
   * The High Security user profile demands a higher controlled environment while still being able to perform their productivity activity, such as mail, and web browsing while maintaining a simple to use experience. The users expect features such as cookies, favorites, and other shortcuts available to operate. However these users may not require the ability to modify, or debug their device, and will not need to install drivers. The High Security profile is deployed using the High Security - Windows10 (1809) script.
* **Specialized** – Developers and IT administrators are an attractive target to attackers as these roles can alter systems of interest to the attackers. The Specialized workstation takes the effort deployed in the High Security workstation, and further emphases its security by managing local applications, limiting internet web sites, and restricting productivity capabilities that are high risk such as ActiveX, Java, browser plugin's, and several other known high risk controls on a Windows device. In this profile, the workstation will enable security controls and policies described in the content, and deployed in the DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline  script.
* **Secured** – An attacker who can compromise an administrative account can typically cause significant business damage by data theft, data alteration, or service disruption. In this hardened state, the workstation will enable all the security controls and policies that restrict direct control of local application management, and productivity tools are removed. As a result, compromising the device is made more difficult as mail and social media are blocked which reflect the most common way phishing attacks can succeed.  The secured workstation can be deployed with the Secure Workstation - Windows10 (1809) SecurityBaseline  script.

   ![Secured workstation](./media/concept-azure-managed-workstation/secure-workstation.png)

   A secure workstation  provides an administrator a hardened workstation that has clear application control, and application guard. The workstation will use credential, device, and exploit guard to protect the host from malicious behavior. Additionally all local disks are encrypted with Bitlocker encryption.

* **Isolated** – This custom offline scenario represents the extreme end of the spectrum (no installation scripts are provided for this case). Organizations may need to manage an isolated business critical function such as a high value production line or life support systems that requires unsupported/unpatched legacy operating systems. Because security is critical and cloud services are unavailable, organizations may either manually manage/update these computers or use an isolated Active Directory forest architecture (like the Enhanced Security Admin Environment (ESAE)) to manage them. In these circumstance removing all access except basic Intune, and ATP health checking should be considered.
   * [Intune network communications requirement](https://docs.microsoft.com/intune/network-bandwidth-use)
   * [ATP network communications requirement](https://docs.microsoft.com/azure-advanced-threat-protection/configure-proxy)

## Next steps

[Deploying a secure Azure-managed workstation](howto-azure-managed-workstation.md)
