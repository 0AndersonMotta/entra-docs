---
title: What's new? Release notes
description: Learn what is new with Microsoft Entra ID, such as the latest release notes, known issues, bug fixes, deprecated functionality, and upcoming changes.
author: owinfreyATL
manager: amycolannino
featureFlags:
 - clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/31/2023
ms.author: owinfrey
ms.reviewer: dhanyahk
ms.custom: it-pro, has-azure-ad-ps-ref
ms.collection: M365-identity-device-management
---

# What's new in Microsoft Entra ID?

>Get notified about when to revisit this page for updates by copying and pasting this URL: `https://learn.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Active+Directory%22&locale=en-us` into your ![RSS feed reader icon](./media/whats-new/feed-icon-16x16.png) feed reader.

Microsoft Entra ID (previously known as Azure AD) receives improvements on an ongoing basis. To stay up to date with the most recent developments, this article provides you with information about:

- The latest releases
- Known issues
- Bug fixes
- Deprecated functionality
- Plans for changes

> [!NOTE] 
> If you're currently using Azure AD today or are have previously deployed Azure AD in your organizations, you can continue to use the service without interruption. All existing deployments, configurations, and integrations continue to function as they do today without any action from you.

This page updates monthly, so revisit it regularly. If you're looking for items older than six months, you can find them in [Archive for What's new in Azure Active Directory](whats-new-archive.md).

## November 2023

### General Availability - Microsoft Entra Cloud Sync now supports ability to enable Exchange Hybrid configuration for Exchange customers

**Type:** New feature   
**Service category:** Provisioning                     
**Product capability:**  AAD Connect Cloud Sync             

Exchange hybrid capability allows for the coexistence of Exchange mailboxes both on-premises and in Microsoft 365. Microsoft Entra Cloud Sync synchronizes a specific set of Exchange-related attributes from Microsoft Entra ID back into your on-premises directory and to any forests that's disconnected (no network trust needed between them). With this capability, existing customers who have this feature enabled in Microsoft Entra Connect sync can now migrate, and apply, this feature with Microsoft Entra cloud sync. For more information, see: [Exchange hybrid writeback with cloud sync](../identity/hybrid/exchange-hybrid-writeback.md).

---

### General Availability - Guest Governance: Inactive Guest Insights

**Type:** New feature   
**Service category:** Reporting                     
**Product capability:**  Identity Governance             

Monitor guest accounts at scale with intelligent insights into inactive guest users in your organization. Customize the inactivity threshold depending on your organization’s needs, narrow down the scope of guest users you want to monitor, and identify the guest users that might be inactive. For more information, see: [Monitor and clean up stale guest accounts using access reviews](../identity/users/clean-up-stale-guest-accounts.md).

---

### Public Preview - lastSuccessfulSignIn property in signInActivity API

**Type:** New feature   
**Service category:** MS Graph                     
**Product capability:**  End User Experiences             

An extra property has been added to signInActivity API to display the last **successful** sign in time for a specific user, regardless if the sign in was interactive or non-interactive. The data won't be backfilled for this property, so you should expect to be returned only successful signIn data starting on 12/8/2023.

---

### General Availability - Auto-rollout of Conditional Access policies

**Type:** New feature   
**Service category:** Conditional Access                     
**Product capability:**  Access Control             

Starting in November 2023, Microsoft will begin automatically protecting customers with Microsoft managed Conditional Access policies. These are policies that Microsoft creates and enables in customer tenants. The following policies are rolled out to all eligible tenants, who will be notified prior to policy creation :

1. Multi-factor Authentication for admin portals: This policy covers privileged admin roles and requires multi-factor authentication when an admin signs into a Microsoft admin portal.
1. Multi-factor Authentication for per-user multi-factor authentication users: This policy covers users with per-user multi-factor authentication and requires multi-factor authentication for all cloud apps.
1. Multi-factor authentication for high-risk sign-ins: This policy covers all users and requires multi-factor authentication and reauthentication for high-risk sign-ins.

For more information, see:

- [Automatic Conditional Access policies in Microsoft Entra streamline identity protection](https://www.microsoft.com/security/blog/2023/11/06/automatic-conditional-access-policies-in-microsoft-entra-streamline-identity-protection/)
- [Microsoft-managed policies](../identity/conditional-access/managed-policies.md)

---

### General Availability -  Chrome's CloudAPAuthEnabled available for device-based conditional access

**Type:** New feature   
**Service category:** Conditional Access                     
**Product capability:**  3rd Party Integration             

Enterprises can enable Chrome's CloudAPAuthEnabled on Windows 10+ devices to meet Microsoft Entra ID device-based conditional access policies. When enabled, it configures automatic user sign-in to web properties accessed via Chrome for accounts signed into the Windows 10+ computer. Automatic sign-in from the device also improves single sign-on experience for end users if third-party cookies are disabled in the browser. For more information, see: [CloudAPAuthEnabled](https://chromeenterprise.google/policies/#CloudAPAuthEnabled).

---

### General Availability - Custom security attributes in Microsoft Entra ID

**Type:** New feature   
**Service category:** Directory Management                     
**Product capability:**  Directory             

Custom security attributes in Microsoft Entra ID are business-specific attributes (key-value pairs) that you can define and assign to Microsoft Entra objects. These attributes can be used to store information, categorize objects, or enforce fine-grained access control over specific Azure resources. Custom security attributes can be used with [Azure attribute-based access control (Azure ABAC)](/azure/role-based-access-control/conditions-overview). For more information, see: [What are custom security attributes in Microsoft Entra ID?](./custom-security-attributes-overview.md).

Changes were made to custom security attribute audit logs for general availability that might impact your daily operations. If you have been using custom security attribute audit logs during the preview, there are the actions you must take before February 2024 to ensure your audit log operations aren't disrupted. For more information, see: [Custom security attribute audit logs](./custom-security-attributes-manage.md#step-6-assign-roles).

---

### Public Preview - New provisioning connectors in the Azure AD Application Gallery - November 2023

**Type:** New feature   
**Service category:** App Provisioning               
**Product capability:** 3rd Party Integration    

We've added the following new applications in our App gallery with Provisioning support. You can now automate creating, updating, and deleting of user accounts for these newly integrated apps:

- [Colloquial](../identity/saas-apps/colloquial-provisioning-tutorial.md)
- [Diffchecker](../identity/saas-apps/diffchecker-provisioning-tutorial.md)
- [M-Files](../identity/saas-apps/m-files-provisioning-tutorial.md)
- [XM Fax and XM SendSecure](../identity/saas-apps/xm-fax-and-xm-send-secure-provisioning-tutorial.md)
- [Rootly](../identity/saas-apps/rootly-provisioning-tutorial.md)
- [Simple In/Out](../identity/saas-apps/simple-in-out-provisioning-tutorial.md)
- [Team Today](../identity/saas-apps/team-today-provisioning-tutorial.md)
- [YardiOne](../identity/saas-apps/yardione-provisioning-tutorial.md)

For more information about how to better secure your organization by using automated user account provisioning, see: [Automate user provisioning to SaaS applications with Azure AD](~/identity/app-provisioning/user-provisioning.md).

---

### General Availability - New Federated Apps available in Azure AD Application gallery - November 2023

**Type:** New feature   
**Service category:** Enterprise Apps                
**Product capability:** 3rd Party Integration          

In November 2023 we've added the following 10 new applications in our App gallery with Federation support:

[Citrix Cloud](https://www.citrix.com/), [Freight Audit](/entra/identity/saas-apps/freight-audit-tutorial), [Movement by project44](/entra/identity/saas-apps/movement-by-project44-tutorial), [Alohi](/entra/identity/saas-apps/alohi-tutorial), [AMCS Fleet Maintenance](https://www.amcsgroup.com/solutions/fleet-maintenance/), [Real Links Campaign App](https://reallinks.io/), [Propely](https://www.propely.io/), [Contentstack](/entra/identity/saas-apps/contentstack-tutorial), [Jasper AI](/entra/identity/saas-apps/jasper-ai-tutorial), [IANS Client Portal](/entra/identity/saas-apps/ians-client-portal-tutorial), [Avionic Interface Technologies LSMA](https://aviftech.com/), [CultureHQ](/entra/identity/saas-apps/culturehq-tutorial), [Hone](/entra/identity/saas-apps/hone-tutorial), [Collector Systems](/entra/identity/saas-apps/collector-systems-tutorial), [NetSfere](/entra/identity/saas-apps/netsfere-tutorial), [Spendwise](https://www.spendwise.com/), [Stage and Screen](/entra/identity/saas-apps/stage-and-screen-tutorial)

You can also find the documentation of all the applications from here https://aka.ms/AppsTutorial.

For listing your application in the Azure AD app gallery, read the details here https://aka.ms/AzureADAppRequest

---

### General Availability - Microsoft Authenticator on Android is FIPS 140-3 compliant

**Type:** New feature   
**Service category:** Microsoft Authenticator App                     
**Product capability:**  User Authentication             

Beginning with version 6.2310.7174, Microsoft Authenticator for Android is compliant with Federal Information Processing Standard (FIPS 140-3) for all Microsoft Entra authentications, including phishing-resistant device-bound passkeys, push multi-factor authentication (MFA), passwordless phone sign-in (PSI) and time-based one-time passcodes (TOTP). For organizations using Intune Company Portal, it is required to have minimum CP version 5.0.6043.0 in addition to Microsoft Authenticator version 6.2310.7174. Microsoft Authenticator on iOS is already FIPS 140 compliant, as announced last year. For more information, see: [Authentication methods in Microsoft Entra ID - Microsoft Authenticator app](../identity/authentication/concept-authentication-authenticator-app.md).

---


## October 2023

### Public Preview - Managing and Changing Passwords in My Security Info 

**Type:** New feature   
**Service category:** My Profile/Account                     
**Product capability:** End User Experiences            

My Sign Ins ([My Sign-Ins (microsoft.com)](https://mysignins.microsoft.com/)) now supports end users managing and changing their passwords. Administrators are able to use Conditional Access registration policies with authentication strengths targeting My Security Info to control the end user experience for changing passwords. Based on the Conditional Access policy, users are able to change their password by entering their existing password, or if they authenticate with MFA and satisfy the Conditional Access policy, can change the password without entering the existing password.

For more information, see: [Combined security information registration for Microsoft Entra overview](~/identity/authentication/concept-registration-mfa-sspr-combined.md).

---

### Public Preview - Govern AD on-premises applications (Kerberos based) using Microsoft Entra Governance

**Type:** New feature   
**Service category:** Provisioning                     
**Product capability:** AAD Connect Cloud Sync            

Security groups provisioning to AD (also known as Group Writeback) is now publicly available through Microsoft Entra Cloud Sync. With this new capability, you can easily govern AD based on-premises applications (Kerberos based apps) using Microsoft Entra Governance.

For more information, see: [Govern on-premises Active Directory based apps (Kerberos) using Microsoft Entra ID Governance](~/identity/hybrid/cloud-sync/govern-on-premises-groups.md)

---

### Public Preview - Microsoft Entra Permissions Management: Permissions Analytics Report PDF for multiple authorization systems

**Type:** Changed feature   
**Service category:**                      
**Product capability:** Permissions Management            

The Permissions Analytics Report (PAR) lists findings relating to permissions risks across identities and resources in Permissions Management. The PAR is an integral part of the risk assessment process where customers discover areas of highest risk in their cloud infrastructure. This report can be directly viewed in the Permissions Management UI, downloaded in Excel (XSLX) format, and exported as a PDF. The report is available for all supported cloud environments: Amazon Web Services (AWS), Microsoft Azure, and Google Cloud Platform (GCP).  

The PAR PDF has been redesigned to enhance usability, align with the product UX redesign effort, and address various customer feature requests. [You can download the PAR PDF for up to 10 authorization systems](~/permissions-management/product-permissions-analytics-reports.md).

---

### General Availability - Enhanced Devices List Management Experience

**Type:** Changed feature   
**Service category:** Device Access Management                     
**Product capability:** End User Experiences            

Several changes have been made to the **All Devices** list since announcing public preview, including:

- Prioritized consistency and accessibility across the different components
- Modernized the list and addressed top customer feedback
    - Added infinite scrolling, column reordering, and the ability to select all devices
    - Added filters for OS Version and Autopilot devices
- Created more connections between Microsoft Entra and Intune
    - Added links to Intune in Compliant and MDM columns
    - Added Security Settings Management column

For more information, see: [View and filter your devices](~/identity/devices/manage-device-identities.md#view-and-filter-your-devices).

---

### General Availability - Windows MAM

**Type:** New feature   
**Service category:** Conditional Access                     
**Product capability:** Access Control            

Windows MAM is the first step toward Microsoft management capabilities for unmanaged Windows devices. This functionality comes at a critical time when we need to ensure the Windows platform is on par with the simplicity and privacy promise we offer end users today on the mobile platforms. End users can access company resources without needing the whole device to be MDM managed.

For more information, see: [Require an app protection policy on Windows devices](~/identity/conditional-access/how-to-app-protection-policy-windows.md).

---

### General Availability - Microsoft Security email update and Resources for Azure AD rename to Microsoft Entra ID 

**Type:** Plan for change   
**Service category:** Other                     
**Product capability:** End User Experiences            

Microsoft Entra ID is the new name for Azure Active Directory (Azure AD). The rename and new product icon are now being deployed across experiences from Microsoft. Most updates will be complete by mid-November of this year. As previously announced, this is just a new name, with no impact on deployments or daily work. There are no changes to capabilities, licensing, terms of service, or support.

From October 15 to November 15, Azure AD emails previously sent from azure-noreply@microsoft.com will start being sent from MSSecurity-noreply@microsoft.com. You may need to update your Outlook rules to match this.  

Additionally, we'll update email content to remove all references of Azure AD where relevant, and include an informational banner that announces this change.

Here are some resources to guide you rename your own product experiences or content where necessary:

- [How to: Rename Azure AD](how-to-rename-azure-ad.md)
- [New name for Azure Active Directory](new-name.md)

---

### General Availability - End users will no longer be able to add password SSO apps in My Apps. 

**Type:** Deprecated   
**Service category:** My Apps                     
**Product capability:** End User Experiences            

Effective November 15, 2023, end users will no longer be able to add password SSO Apps to their gallery in My Apps. However, admins can still add password SSO apps following [these instructions](~/identity/enterprise-apps/configure-password-single-sign-on-non-gallery-applications.md). Password SSO apps previously added by end users remain available in My Apps.

For more information, see: [Discover applications](~/identity/enterprise-apps/myapps-overview.md#discover-applications).

---

### General Availability - Restrict Microsoft Entra ID Tenant Creation To Only Paid Subscription  

**Type:** Changed feature   
**Service category:** Managed identities for Azure resources                    
**Product capability:** End User Experiences            

The ability to create new tenants from the Microsoft Entra admin center allows users in your organization to create test and demo tenants from your Microsoft Entra ID tenant, [Learn more about creating tenants](/microsoft-365/education/deploy/intro-azure-active-directory). When used incorrectly this feature can allow the creation of tenants that aren't managed or viewable by your organization. We recommend that you restrict this capability so that only trusted admins can use this feature, [Learn more about restricting member users' default permissions](users-default-permissions.md#restrict-member-users-default-permissions). We also recommend you use the Microsoft Entra audit log to monitor for the Directory Management: Create Company event that signals a new tenant has been created by a user in your organization.  

To further protect your organization, Microsoft is now limiting this functionality to only paid customers. Customers on trial subscriptions won't be able to create additional tenants from the Microsoft Entra admin center. Customers in this situation who need a new trial tenant can sign up for a [Free Azure Account](https://azure.microsoft.com/free/).

---

### General Availability - Users can't modify GPS location when using location based access control

**Type:** Plan for change   
**Service category:** Conditional Access                     
**Product capability:** User Authentication            

In an ever-evolving security landscape, the Microsoft Authenticator is updating its security baseline for Location Based Access Control (LBAC) conditional access policies to disallow authentications where the user may be using a different location than the actual GPS location of the mobile device. Today, it's possible for users to modify the location reported by the device on iOS and Android devices. The Authenticator app will start to deny LBAC authentications where we detect that the user isn't using the actual location of the mobile device where the Authenticator is installed.

In the November 2023 release of the Authenticator app, users who are modifying the location of their device will see a denial message in the app when doing an LBAC authentication. To ensure that users aren’t using older app versions to continue authenticating with a modified location, beginning January 2024, any users that are on Android Authenticator 6.2309.6329 version or prior and iOS Authenticator version 6.7.16 or prior will be blocked from using LBAC. To determine which users are using older versions of the Authenticator app, you may use [our MSGraph APIs](/graph/api/resources/microsoftauthenticatorauthenticationmethod).

---

### Public Preview - Overview page in My Access portal

**Type:** New feature   
**Service category:** Entitlement Management                     
**Product capability:** Identity Governance            

Today, when users navigate to myaccess.microsoft.com, they land on a list of available access packages in their organization. The new Overview page provides a more relevant place for users to land. The Overview page points them to the tasks they need to complete and helps familiarize users with how to complete tasks in My Access.  

Admins can enable/disable the Overview page preview by signing into the Entra portal and navigating to Entitlement management > Settings > Opt-in Preview Features and locating My Access overview page in the table.

For more information, see: [My Access Overview page (preview)](~/id-governance/my-access-portal-overview.md#overview-page-preview).

---

### Public Preview - New provisioning connectors in the Azure AD Application Gallery - October 2023

**Type:** New feature   
**Service category:** App Provisioning               
**Product capability:** 3rd Party Integration    
      

We've added the following new applications in our App gallery with Provisioning support. You can now automate creating, updating, and deleting of user accounts for these newly integrated apps:

- [Amazon Business](~/identity/saas-apps/amazon-business-provisioning-tutorial.md)
- [Bustle B2B Transport Systems](~/identity/saas-apps/bustle-b2b-transport-systems-provisioning-tutorial.md)
- [Canva](~/identity/saas-apps/canva-provisioning-tutorial.md)
- [Cybozu](~/identity/saas-apps/cybozu-provisioning-tutorial.md)
- [Forcepoint Cloud Security Gateway - User Authentication](~/identity/saas-apps/forcepoint-cloud-security-gateway-provisioning-tutorial.md)
- [Hypervault](~/identity/saas-apps/hypervault-provisioning-tutorial.md)
- [Oneflow](~/identity/saas-apps/oneflow-provisioning-tutorial.md)

For more information about how to better secure your organization by using automated user account provisioning, see: [Automate user provisioning to SaaS applications with Azure AD](~/identity/app-provisioning/user-provisioning.md).

---

### Public Preview - Microsoft Graph Activity Logs

**Type:** New feature   
**Service category:** Microsoft Graph                     
**Product capability:** Monitoring & Reporting            

The MicrosoftGraphActivityLogs provides administrators full visibility into all HTTP requests accessing your tenant’s resources through the Microsoft Graph API. These logs can be used to find activity from compromised accounts, identify anomalous behavior, or investigate application activity. For more information, see: [Access Microsoft Graph activity logs (preview)](/graph/microsoft-graph-activity-logs-overview).

---

### Public Preview - Microsoft Entra Verified ID quick setup

**Type:** New feature   
**Service category:** Other                     
**Product capability:** Identity Governance            

Quick Microsoft Entra Verified ID setup, available in preview, removes several configuration steps an admin needs to complete with a single click on a Get started button. The quick setup takes care of signing keys, registering your decentralized ID, and verifying your domain ownership. It also creates a Verified Workplace Credential for you. For more information, see: [Quick Microsoft Entra Verified ID setup](~/verified-id/verifiable-credentials-configure-tenant-quick.md).

---

## September 2023

### Public Preview - Changes to FIDO2 authentication methods and Windows Hello for Business

**Type:**  Changed feature            
**Service category:**  Authentications (Logins)                                
**Product capability:**  User Authentication                        

Beginning **January 2024**, Microsoft Entra ID will support [device-bound passkeys](https://passkeys.dev/docs/reference/terms/#device-bound-passkey) stored on computers and mobile devices as an authentication method in public preview, in addition to the existing support for FIDO2 security keys. This enables your users to perform phishing-resistant authentication using the devices that they already have.  

We'll expand the existing FIDO2 authentication methods policy, and end user experiences, to support this preview release. For your organization to opt in to this preview, you'll need to enforce key restrictions to allow specified passkey providers in your FIDO2 policy. Learn more about FIDO2 key restrictions [here](~/identity/authentication/howto-authentication-passwordless-security-key.md).

In addition, the existing end user sign-in option for Windows Hello and FIDO2 security keys will be renamed to “*Face, fingerprint, PIN, or security key*”. The term “passkey” will be mentioned in the updated sign-in experience to be inclusive of passkey credentials presented from security keys, computers, and mobile devices.

---

### General Availability - Recovery of deleted application and service principals is now available

**Type:**  New feature            
**Service category:**  Enterprise Apps                                
**Product capability:**  Identity Lifecycle Management                        

With this release, you can now recover applications along with their original service principals, eliminating the need for extensive reconfiguration and code changes ([Learn more](~/identity/enterprise-apps/delete-recover-faq.yml)). It significantly improves the application recovery story and addresses a long-standing customer need. This change is beneficial to you on:

- **Faster Recovery**: You can now recover their systems in a fraction of the time it used to take, reducing downtime and minimizing disruptions.
- **Cost Savings**: With quicker recovery, you can save on operational costs associated with extended outages and labor-intensive recovery efforts.
- **Preserved Data**: Previously lost data, such as SMAL configurations, is now retained, ensuring a smoother transition back to normal operations.
- **Improved User Experience**: Faster recovery times translate to improved user experience and customer satisfaction, as applications are back up and running swiftly.

---

### Public Preview - New provisioning connectors in the Azure AD Application Gallery - September 2023

**Type:** New feature   
**Service category:** App Provisioning               
**Product capability:** 3rd Party Integration    
      

We've added the following new applications in our App gallery with Provisioning support. You can now automate creating, updating, and deleting of user accounts for these newly integrated apps:

- [Datadog](~/identity/saas-apps/datadog-provisioning-tutorial.md)
- [Litmos](~/identity/saas-apps/litmos-provisioning-tutorial.md)
- [Postman](~/identity/saas-apps/postman-provisioning-tutorial.md)
- [Recnice](~/identity/saas-apps/recnice-provisioning-tutorial.md)

For more information about how to better secure your organization by using automated user account provisioning, see: [Automate user provisioning to SaaS applications with Azure AD](~/identity/app-provisioning/user-provisioning.md).

---

### General Availability - Web Sign-In for Windows

**Type:** Changed feature              
**Service category:** Authentications (Logins)                                 
**Product capability:** User Authentication    

We're thrilled to announce that as part of the Windows 11 September moment, we're releasing a new Web Sign-In experience that will expand the number of supported scenarios and greatly improve security, reliability, performance, and overall end-to-end experience for our users.

Web Sign-In (WSI) is a credential provider on the Windows lock/sign-in screen for AADJ joined devices that provide a web experience used for authentication and returns an auth token back to the operating system to allow the user to unlock/sign-in to the machine.

Web Sign-In was initially intended to be used for a wide range of auth credential scenarios; however, it was only previously released for limited scenarios such as: [Simplified EDU Web Sign-In](/education/windows/federated-sign-in?tabs=intune) and recovery flows via [Temporary Access Password (TAP)](~/identity/authentication/howto-authentication-temporary-access-pass.md).

The underlying provider for Web Sign-In has been re-written from the ground up with security and improved performance in mind. This release moves the Web Sign-in infrastructure from the Cloud Host Experience (CHX) WebApp to a newly written Login Web Host (LWH) for the September moment. This release provides better security and reliability to support previous EDU & TAP experiences and new workflows enabling using various Auth Methods to unlock/login to the desktop.                    

---

### General Availability - Support for Microsoft admin portals in Conditional Access

**Type:** New feature             
**Service category:** Conditional Access                                   
**Product capability:** Identity Security & Protection    

When a Conditional Access policy targets the Microsoft Admin Portals cloud app, the policy is enforced for tokens issued to application IDs of the following Microsoft administrative portals:

- Azure portal
- Exchange admin center
- Microsoft 365 admin center
- Microsoft 365 Defender portal
- Microsoft Entra admin center
- Microsoft Intune admin center
- Microsoft Purview compliance portal                   

For more information, see: [Microsoft Admin Portals](~/identity/conditional-access/concept-conditional-access-cloud-apps.md#microsoft-admin-portals).

---

## August 2023

### General Availability - Tenant Restrictions V2

**Type:** New feature         
**Service category:** Authentications (Logins)                              
**Product capability:** Identity Security & Protection                    

**Tenant Restrictions V2 (TRv2)** is now generally available for authentication plane via proxy.  

TRv2 allows organizations to enable safe and productive cross-company collaboration while containing data exfiltration risk. With TRv2, you can control what external tenants your users can access from your devices or network using externally issued identities and provide granular access control on a per org, user, group, and application basis.    

TRv2 uses the cross-tenant access policy, and offers both authentication and data plane protection. It enforces policies during user authentication, and on data plane access with Exchange Online, SharePoint Online, Teams, and MSGraph.  While the data plane support with Windows GPO and Global Secure Access is still in public preview, authentication plane support with proxy is now generally available. 

Visit https://aka.ms/tenant-restrictions-enforcement for more information on tenant restriction V2 and Global Secure Access client-side tagging for TRv2 at [Universal tenant restrictions](/entra/global-secure-access/how-to-universal-tenant-restrictions).   

---

### Public Preview - Cross-tenant access settings supports custom RBAC roles and protected actions

**Type:** New feature         
**Service category:** B2B                               
**Product capability:** B2B/B2C                    

Cross-tenant access settings can be managed with custom roles defined by your organization. This enables you to define your own finely-scoped roles to manage cross-tenant access settings instead of using one of the built-in roles for management. [Learn more about creating your own custom roles](~/external-id/cross-tenant-access-overview.md#custom-roles-for-managing-cross-tenant-access-settings).

You can also now protect privileged actions inside of cross-tenant access settings using Conditional Access. For example, you can require MFA before allowing changes to default settings for B2B collaboration. Learn more about [Protected actions](~/identity/role-based-access-control/protected-actions-overview.md).

---

### General Availability - Additional settings in Entitlement Management auto-assignment policy
 
**Type:** Changed feature    
**Service category**: Entitlement Management    
**Product capability:** Entitlement Management    

In the Entra ID Governance entitlement management auto-assignment policy, there are three new settings. This allows a customer to select to not have the policy create assignments, not remove assignments, and to delay assignment removal.

---

### Public Preview - Setting for guest losing access

**Type:** Changed feature          
**Service category:** Entitlement Management                             
**Product capability:** Entitlement Management                    

An administrator can configure that when a guest brought in through entitlement management has lost their last access package assignment, they're deleted after a specified number of days. For more information, see: [Govern access for external users in entitlement management](~/id-governance/entitlement-management-external-users.md).

---

### Public Preview - Real-Time Strict Location Enforcement

**Type:** New feature         
**Service category:** Continuous Access Evaluation                              
**Product capability:** Access Control                    

Strictly enforce Conditional Access policies in real-time using Continuous Access Evaluation.  Enable services like Microsoft Graph, Exchange Online, and SharePoint Online to block access requests from disallowed locations as part of a layered defense against token replay and other unauthorized access. For more information, see blog: [Public Preview: Strictly Enforce Location Policies with Continuous Access Evaluation](https://techcommunity.microsoft.com/t5/microsoft-entra-azure-ad-blog/public-preview-strictly-enforce-location-policies-with/ba-p/3773133) and documentation:
[Strictly enforce location policies using continuous access evaluation (preview)](~/identity/conditional-access/concept-continuous-access-evaluation-strict-enforcement.md).

---

### Public Preview - New provisioning connectors in the Azure AD Application Gallery - August 2023

**Type:** New feature   
**Service category:** App Provisioning               
**Product capability:** 3rd Party Integration    
      

We've added the following new applications in our App gallery with Provisioning support. You can now automate creating, updating, and deleting of user accounts for these newly integrated apps:

- [Airbase](~/identity/saas-apps/airbase-provisioning-tutorial.md)
- [Airtable](~/identity/saas-apps/airtable-provisioning-tutorial.md)
- [Cleanmail Swiss](~/identity/saas-apps/cleanmail-swiss-provisioning-tutorial.md)
- [Informacast](~/identity/saas-apps/informacast-provisioning-tutorial.md)
- [Kintone](~/identity/saas-apps/kintone-provisioning-tutorial.md)
- [O'reilly learning platform](~/identity/saas-apps/oreilly-learning-platform-provisioning-tutorial.md)
- [Tailscale](~/identity/saas-apps/tailscale-provisioning-tutorial.md)
- [Tanium SSO](~/identity/saas-apps/tanium-sso-provisioning-tutorial.md)
- [Vbrick Rev Cloud](~/identity/saas-apps/vbrick-rev-cloud-provisioning-tutorial.md)
- [Xledger](~/identity/saas-apps/xledger-provisioning-tutorial.md)


For more information about how to better secure your organization by using automated user account provisioning, see: [Automate user provisioning to SaaS applications with Azure AD](~/identity/app-provisioning/user-provisioning.md).


---

### General Availability - Continuous Access Evaluation for Workload Identities available in Public and Gov clouds

**Type:** New feature         
**Service category:** Continuous Access Evaluation                              
**Product capability:** Identity Security & Protection                    

Real-time enforcement of risk events, revocation events, and Conditional Access location policies is now generally available for workload identities.
Service principals on line of business (LOB) applications are now protected on access requests to Microsoft Graph. For more information, see: [Continuous access evaluation for workload identities (preview)](~/identity/conditional-access/concept-continuous-access-evaluation-workload.md).

---

## July 2023

### General Availability: Azure Active Directory (Azure AD) is being renamed.

**Type:** Changed feature       
**Service category:**  N/A                          
**Product capability:** End User Experiences                

**No action is required from you, but you may need to update some of your own documentation.**

Azure AD is being renamed to Microsoft Entra ID. The name change rolls out across all Microsoft products and experiences throughout the second half of 2023. 

Capabilities, licensing, and usage of the product isn't changing. To make the transition seamless for you, the pricing, terms, service level agreements, URLs, APIs, PowerShell cmdlets, Microsoft Authentication Library (MSAL) and developer tooling remain the same.   

Learn more and get renaming details: [New name for Azure Active Directory](new-name.md).

---

### General Availability - Include/exclude My Apps in Conditional Access policies

**Type:** Fixed       
**Service category:** Conditional Access                            
**Product capability:** End User Experiences                  

My Apps can now be targeted in Conditional Access policies. This solves a top customer blocker. The functionality is available in all clouds. GA also brings a new app launcher, which improves app launch performance for both SAML and other app types. 

Learn More about setting up Conditional Access policies here: [Azure AD Conditional Access documentation](~/identity/conditional-access/index.yml).

---

### General Availability - Conditional Access for Protected Actions

**Type:** New feature         
**Service category:** Conditional Access                            
**Product capability:** Identity Security & Protection                    

Protected actions are high-risk operations, such as altering access policies or changing trust settings, that can significantly impact an organization's security. To add an extra layer of protection, Conditional Access for Protected Actions lets organizations define specific conditions for users to perform these sensitive tasks. For more information, see: [What are protected actions in Azure AD?](~/identity/role-based-access-control/protected-actions-overview.md).

---

### General Availability - Access Reviews for Inactive Users

**Type:** New feature         
**Service category:** Access Reviews                            
**Product capability:** Identity Governance                      

This new feature, part of the Microsoft Entra ID Governance SKU, allows admins to review and address stale accounts that haven’t been active for a specified period. Admins can set a specific duration to determine inactive accounts that weren't used for either interactive or non-interactive sign-in activities. As part of the review process, stale accounts can automatically be removed. For more information, see: [Microsoft Entra ID Governance Introduces Two New Features in Access Reviews](https://techcommunity.microsoft.com/t5/microsoft-entra-azure-ad-blog/microsoft-entra-id-governance-introduces-two-new-features-in/ba-p/2466930).

---

### General Availability - Automatic assignments to access packages in Microsoft Entra ID Governance

**Type:** Changed feature       
**Service category:** Entitlement Management                          
**Product capability:** Entitlement Management                

Microsoft Entra ID Governance includes the ability for a customer to configure an assignment policy in an entitlement management access package that includes an attribute-based rule, similar to dynamic groups, of the users who should be assigned access. For more information, see: [Configure an automatic assignment policy for an access package in entitlement management](~/id-governance/entitlement-management-access-package-auto-assignment-policy.md).

---

### General Availability - Custom Extensions in Entitlement Management 

**Type:** New feature       
**Service category:** Entitlement Management                          
**Product capability:** Entitlement Management                

Custom extensions in Entitlement Management are now generally available, and allow you to extend the access lifecycle with your organization-specific processes and business logic when access is requested or about to expire. With custom extensions you can create tickets for manual access provisioning in disconnected systems, send custom notifications to additional stakeholders, or automate additional access-related configuration in your business applications such as assigning the correct sales region in Salesforce. You can also leverage custom extensions to embed external governance, risk, and compliance (GRC) checks in the access request.

For more information, see:

- [Microsoft Entra ID Governance Entitlement Management New Generally Available Capabilities](https://techcommunity.microsoft.com/t5/microsoft-entra-azure-ad-blog/microsoft-entra-id-governance-entitlement-management-new/ba-p/2466929)
- [Trigger Logic Apps with custom extensions in entitlement management](~/id-governance/entitlement-management-logic-apps-integration.md)

---

### General Availability - Conditional Access templates

**Type:** Plan for change         
**Service category:** Conditional Access                             
**Product capability:** Identity Security & Protection                  

Conditional Access templates are predefined set of conditions and controls that provide a convenient method to deploy new policies aligned with Microsoft recommendations. Customers are assured that their policies reflect modern best practices for securing corporate assets, promoting secure, optimal access for their hybrid workforce. For more information, see: [Conditional Access templates](~/identity/conditional-access/concept-conditional-access-policy-common.md).

---

### General Availability - Lifecycle Workflows

**Type:** New feature       
**Service category:** Lifecycle Workflows                            
**Product capability:** Identity Governance                  

User identity lifecycle is a critical part of an organization’s security posture, and when managed correctly, can have a positive impact on their users’ productivity for Joiners, Movers, and Leavers. The ongoing digital transformation is accelerating the need for good identity lifecycle management. However, IT and security teams face enormous challenges managing the complex, time-consuming, and error-prone manual processes necessary to execute the required onboarding and offboarding tasks for hundreds of employees at once. This is an ever present and complex issue IT admins continue to face with digital transformation across security, governance, and compliance.

Lifecycle Workflows, one of our newest Microsoft Entra ID Governance capabilities is now generally available to help organizations further optimize their user identity lifecycle. For more information, see: [Lifecycle Workflows is now generally available!](https://techcommunity.microsoft.com/t5/microsoft-entra-azure-ad-blog/lifecycle-workflows-is-now-generally-available/ba-p/2466931)

---

### General Availability - Enabling extended customization capabilities for sign-in and sign-up pages in Company Branding capabilities.

**Type:** New feature       
**Service category:** User Experience and Management                            
**Product capability:** User Authentication                

Update the Microsoft Entra ID and Microsoft 365 sign in experience with new Company Branding capabilities. You can apply your company’s brand guidance to authentication experiences with predefined templates. For more information, see: [Company Branding](~/fundamentals/how-to-customize-branding.md) 

---

### General Availability - Enabling customization capabilities for the Self-Service Password Reset (SSPR) hyperlinks, footer hyperlinks and browser icons in Company Branding.

**Type:** Changed feature       
**Service category:** User Experience and Management                            
**Product capability:** End User Experiences                  

Update the Company Branding functionality on the Microsoft Entra ID/Microsoft 365 sign in experience to allow customizing Self Service Password Reset (SSPR) hyperlinks, footer hyperlinks, and a browser icon. For more information, see: [Company Branding](~/fundamentals/how-to-customize-branding.md) 

---

### General Availability - User-to-Group Affiliation recommendation for group Access Reviews

**Type:** New feature       
**Service category:** Access Reviews                            
**Product capability:** Identity Governance                  

This feature provides Machine Learning based recommendations to the reviewers of Azure AD Access Reviews to make the review experience easier and more accurate. The recommendation leverages machine learning based scoring mechanism and compares users’ relative affiliation with other users in the group, based on the organization’s reporting structure. For more information, see:  [Review recommendations for Access reviews](~/id-governance/review-recommendations-access-reviews.md) and [Introducing Machine Learning based recommendations in Azure AD Access reviews](https://techcommunity.microsoft.com/t5/microsoft-entra-azure-ad-blog/introducing-machine-learning-based-recommendations-in-azure-ad/ba-p/2466923)

---

### Public Preview - Inactive guest insights

**Type:** New feature       
**Service category:** Reporting                            
**Product capability:** Identity Governance                  

Monitor guest accounts at scale with intelligent insights into inactive guest users in your organization. Customize the inactivity threshold depending on your organization’s needs, narrow down the scope of guest users you want to monitor and identify the guest users that may be inactive. For more information, see: [Monitor and clean up stale guest accounts using access reviews](~/identity/users/clean-up-stale-guest-accounts.md).

---

### Public Preview - Just-in-time application access with PIM for Groups

**Type:** New feature       
**Service category:** Privileged Identity Management                            
**Product capability:** Privileged Identity Management                  

You can minimize the number of persistent administrators in applications such as [AWS](~/identity/saas-apps/aws-single-sign-on-provisioning-tutorial.md#just-in-time-jit-application-access-with-pim-for-groups-preview)/[GCP](~/identity/saas-apps/g-suite-provisioning-tutorial.md#just-in-time-jit-application-access-with-pim-for-groups-preview) and get JIT access to groups in AWS and GCP. While PIM for Groups is publicly available, we’ve released a public preview that integrates PIM with provisioning and reduces the activation delay from 40+ minutes to 1 – 2 minutes.

---

### Public Preview - Graph beta API for PIM security alerts on Azure AD roles

**Type:** New feature       
**Service category:** Privileged Identity Management                            
**Product capability:** Privileged Identity Management                 

Announcing API support (beta) for managing PIM security alerts for Azure AD roles. [Azure Privileged Identity Management (PIM)](~/id-governance/privileged-identity-management/index.yml) generates alerts when there's suspicious or unsafe activity in your organization in Azure Active Directory (Azure AD), part of Microsoft Entra. You can now manage these alerts using REST APIs. These alerts can also be [managed through the Azure portal](~/id-governance/privileged-identity-management/pim-resource-roles-configure-alerts.md). For more information, see:  [unifiedRoleManagementAlert resource type](/graph/api/resources/unifiedrolemanagementalert).

---

### General Availability - Reset Password on Azure Mobile App

**Type:** New feature       
**Service category:** Other                            
**Product capability:** End User Experiences                

The Azure mobile app has been enhanced to empower admins with specific permissions to conveniently reset their users' passwords. Self Service Password Reset will not be supported at this time. However, users can still more efficiently control and streamline their own sign-in and auth methods. The mobile app can be downloaded for each platform here:

- Android: https://aka.ms/AzureAndroidWhatsNew
- IOS: https://aka.ms/ReferAzureIOSWhatsNew

---

### Public Preview - API-driven inbound user provisioning 

**Type:** New feature       
**Service category:** Provisioning                              
**Product capability:** Inbound to Azure AD                  

With API-driven inbound provisioning, Microsoft Entra ID provisioning service now supports integration with any system of record. Customers and partners can use any automation tool of their choice to retrieve workforce data from any system of record for provisioning into Entra ID and connected on-premises Active Directory domains. The IT admin has full control on how the data is processed and transformed with attribute mappings. Once the workforce data is available in Entra ID, the IT admin can configure appropriate joiner-mover-leaver business processes using Entra ID Governance Lifecycle Workflows. For more information, see: [API-driven inbound provisioning concepts (Public preview)](~/identity/app-provisioning/inbound-provisioning-api-concepts.md).

---

### Public Preview - Dynamic Groups based on EmployeeHireDate User attribute

**Type:** New feature       
**Service category:** Group Management                                
**Product capability:** Directory                    

This feature enables admins to create dynamic group rules based on the user objects' employeeHireDate attribute. For more information, see: [Properties of type string](~/identity/users/groups-dynamic-membership.md#properties-of-type-string).

---

### General Availability - Enhanced Create User and Invite User Experiences

**Type:** Changed feature       
**Service category:** User Management                                  
**Product capability:** User Management                      

We have increased the number of properties admins are able to define when creating and inviting a user in the Entra admin portal, bringing our UX to parity with our Create User APIs. Additionally, admins can now add users to a group or administrative unit, and assign roles. For more information, see: [Add or delete users using Azure Active Directory](./add-users.md).

---

### General Availability - All Users and User Profile

**Type:** Changed feature       
**Service category:** User Management                                  
**Product capability:** User Management                   

The All Users list now features an infinite scroll, and admins can now modify more properties in the User Profile. For more information, see: [How to create, invite, and delete users](~/fundamentals/how-to-create-delete-users.md).

---

### Public Preview - Windows MAM

**Type:** New feature       
**Service category:** Conditional Access                                
**Product capability:** Identity Security & Protection                    

“*When will you have MAM for Windows?*” is one of our most frequently asked customer questions. We’re happy to report that the answer is: “Now!” We’re excited to offer this new and long-awaited MAM Conditional Access capability in Public Preview for Microsoft Edge for Business on Windows.

Using MAM Conditional Access, Microsoft Edge for Business provides users with secure access to organizational data on personal Windows devices with a customizable user experience. We’ve combined the familiar security features of app protection policies (APP), Windows Defender client threat defense, and Conditional Access, all anchored to Azure AD identity to ensure un-managed devices are healthy and protected before granting data access. This can help businesses to improve their security posture and protect sensitive data from unauthorized access, without requiring full mobile device enrollment.

The new capability extends the benefits of app layer management to the Windows platform via Microsoft Edge for Business. Admins are empowered to configure the user experience and protect organizational data within Microsoft Edge for Business on un-managed Windows devices.

For more information, see: [Require an app protection policy on Windows devices (preview)](~/identity/conditional-access/how-to-app-protection-policy-windows.md).

---

### General Availability - New Federated Apps available in Azure AD Application gallery - July 2023

**Type:** New feature   
**Service category:** Enterprise Apps                
**Product capability:** 3rd Party Integration          

In July 2023 we've added the following 10 new applications in our App gallery with Federation support:    

[Gainsight SAML](~/identity/saas-apps/gainsight-tutorial.md), [Dataddo](https://www.dataddo.com/), [Puzzel](https://www.puzzel.com/), [Worthix App](~/identity/saas-apps/worthix-app-tutorial.md), [iOps360 IdConnect](https://iops360.com/iops360-id-connect-azuread-single-sign-on/), [Airbase](~/identity/saas-apps/airbase-tutorial.md), [Couchbase Capella - SSO](~/identity/saas-apps/couchbase-capella-sso-tutorial.md), [SSO for Jama Connect®](~/identity/saas-apps/sso-for-jama-connect-tutorial.md), [mediment (メディメント)](https://mediment.jp/), [Netskope Cloud Exchange Administration Console](~/identity/saas-apps/netskope-cloud-exchange-administration-console-tutorial.md), [Uber](~/identity/saas-apps/uber-tutorial.md), [Plenda](https://app.plenda.nl/), [Deem Mobile](~/identity/saas-apps/deem-mobile-tutorial.md), [40SEAS](https://www.40seas.com/), [Vivantio](https://www.vivantio.com/), [AppTweak](https://www.apptweak.com/), [Vbrick Rev Cloud](~/identity/saas-apps/vbrick-rev-cloud-tutorial.md), [OptiTurn](~/identity/saas-apps/optiturn-tutorial.md), [Application Experience with Mist](https://www.mist.com/), [クラウド勤怠管理システムKING OF TIME](~/identity/saas-apps/cloud-attendance-management-system-king-of-time-tutorial.md), [Connect1](~/identity/saas-apps/connect1-tutorial.md), [DB Education Portal for Schools](~/identity/saas-apps/db-education-portal-for-schools-tutorial.md), [SURFconext](~/identity/saas-apps/surfconext-tutorial.md), [Chengliye Smart SMS Platform](~/identity/saas-apps/chengliye-smart-sms-platform-tutorial.md), [CivicEye SSO](~/identity/saas-apps/civic-eye-sso-tutorial.md), [Colloquial](~/identity/saas-apps/colloquial-tutorial.md), [BigPanda](~/identity/saas-apps/bigpanda-tutorial.md), [Foreman](https://foreman.mn/)

You can also find the documentation of all the applications from here https://aka.ms/AppsTutorial.

For listing your application in the Azure AD app gallery, read the details here https://aka.ms/AzureADAppRequest

---

### Public Preview - New provisioning connectors in the Azure AD Application Gallery - July 2023

**Type:** New feature   
**Service category:** App Provisioning               
**Product capability:** 3rd Party Integration    
      

We've added the following new applications in our App gallery with Provisioning support. You can now automate creating, updating, and deleting of user accounts for these newly integrated apps:

- [Albert](~/identity/saas-apps/albert-provisioning-tutorial.md)
- [Rhombus Systems](~/identity/saas-apps/rhombus-systems-provisioning-tutorial.md)
- [Axiad Cloud](~/identity/saas-apps/axiad-cloud-provisioning-tutorial.md)
- [Dagster Cloud](~/identity/saas-apps/dagster-cloud-provisioning-tutorial.md)
- [WATS](~/identity/saas-apps/wats-provisioning-tutorial.md)
- [Funnel Leasing](~/identity/saas-apps/funnel-leasing-provisioning-tutorial.md)


For more information about how to better secure your organization by using automated user account provisioning, see: [Automate user provisioning to SaaS applications with Azure AD](~/identity/app-provisioning/user-provisioning.md).


---

### General Availability - Microsoft Authentication Library for .NET 4.55.0

**Type:** New feature       
**Service category:** Other                                    
**Product capability:** User Authentication                     

Earlier this month we announced the release of [MSAL.NET 4.55.0](https://www.nuget.org/packages/Microsoft.Identity.Client/4.55.0), the latest version of the [Microsoft Authentication Library for the .NET platform](/entra/msal/dotnet/). The new version introduces support for user-assigned [managed identity](/entra/msal/dotnet/advanced/managed-identity) being specified through object IDs, CIAM authorities in the `WithTenantId` API, better error messages when dealing with cache serialization, and improved logging when using the [Windows authentication broker](/entra/msal/dotnet/acquiring-tokens/desktop-mobile/wam).

---

### General Availability - Microsoft Authentication Library for Python 1.23.0

**Type:** New feature       
**Service category:** Other                                    
**Product capability:** User Authentication                  

Earlier this month, the Microsoft Authentication Library team announced the release of [MSAL for Python version 1.23.0](https://pypi.org/project/msal/1.23.0/). The new version of the library adds support for better caching when using client credentials, eliminating the need to request new tokens repeatedly when cached tokens exist.

To learn more about MSAL for Python, see: [Microsoft Authentication Library (MSAL) for Python](/entra/msal/python/).

---

## June 2023

### Public Preview - New provisioning connectors in the Azure AD Application Gallery - June 2023

**Type:** New feature   
**Service category:** App Provisioning               
**Product capability:** 3rd Party Integration    
      
We've added the following new applications in our App gallery with Provisioning support. You can now automate creating, updating, and deleting of user accounts for these newly integrated apps:

- [Headspace](~/identity/saas-apps/headspace-provisioning-tutorial.md)
- [Humbol](~/identity/saas-apps/humbol-provisioning-tutorial.md)
- [LUSID](~/identity/saas-apps/lusid-provisioning-tutorial.md)
- [Markit Procurement Service](~/identity/saas-apps/markit-procurement-service-provisioning-tutorial.md)
- [Moqups](~/identity/saas-apps/moqups-provisioning-tutorial.md)
- [Notion](~/identity/saas-apps/notion-provisioning-tutorial.md)
- [OpenForms](~/identity/saas-apps/openforms-provisioning-tutorial.md)
- [SafeGuard Cyber](~/identity/saas-apps/safeguard-cyber-provisioning-tutorial.md)
- [Uni-tel A/S](~/identity/saas-apps/uni-tel-as-provisioning-tutorial.md)
- [Vault Platform](~/identity/saas-apps/vault-platform-provisioning-tutorial.md)
- [V-Client](~/identity/saas-apps/v-client-provisioning-tutorial.md)
- [Veritas Enterprise Vault.cloud SSO-SCIM](~/identity/saas-apps/veritas-provisioning-tutorial.md)

For more information about how to better secure your organization by using automated user account provisioning, see: [Automate user provisioning to SaaS applications with Azure AD](~/identity/app-provisioning/user-provisioning.md).

---

### General Availability - Include/exclude Entitlement Management in Conditional Access policies

**Type:** New feature       
**Service category:** Entitlement Management                          
**Product capability:** Entitlement Management                

The Entitlement Management service can now be targeted in the Conditional Access policy for inclusion or exclusion of applications. To target the Entitlement Management service, select “Azure AD Identity Governance - Entitlement Management” in the cloud apps picker. The Entitlement Management app includes the entitlement management part of My Access, the Entitlement Management part of the Entra and Azure portals, and the Entitlement Management part of MS Graph. For more information, see:  [Review your Conditional Access policies](~/id-governance/entitlement-management-external-users.md#review-your-conditional-access-policies).

---

### General Availability - Azure Active Directory User and Group capabilities on Azure Mobile are now available

**Type:** New feature       
**Service category:** Azure Mobile App                             
**Product capability:** End User Experiences                  

The Azure Mobile app now includes a section for Azure Active Directory. Within Azure Active Directory on mobile, user can search for and view more details about user and groups. Additionally, permitted users can invite guest users to their active tenant, assign group memberships and ownerships for users, and view user sign-in logs. For more information, see: [Get the Azure mobile app](https://azure.microsoft.com/get-started/azure-portal/mobile-app/).

---

### Plan for change - Modernizing Terms of Use Experiences

**Type:** Plan for change         
**Service category:** Terms of Use                               
**Product capability:** AuthZ/Access Delegation                   

Recently we announced the modernization of terms of use end-user experiences as part of ongoing service improvements. As previously communicated the end user experiences will be updated with a new PDF viewer and are moving from https://account.activedirectory.windowsazure.com to https://myaccount.microsoft.com. 

Starting today the modernized experience for viewing previously accepted terms of use is available via https://myaccount.microsoft.com/termsofuse/myacceptances. We encourage you to check out the modernized experience, which follows the same updated design pattern as the upcoming modernization of accepting or declining terms of use as part of the sign-in flow. We would appreciate your [feedback](https://forms.microsoft.com/r/NV0msbrqtF) before we begin to modernize the sign-in flow.

---

### General Availability - Privileged Identity Management for Groups

**Type:** New feature       
**Service category:** Privileged Identity Management                               
**Product capability:** Privileged Identity Management                 

Privileged Identity Management for Groups is now generally available. With this feature, you have the ability to grant users just-in-time membership in a group, which in turn provides access to Azure Active Directory roles, Azure roles, Azure SQL, Azure Key Vault, Intune, other application roles, and third-party applications. Through one activation, you can conveniently assign a combination of permissions across different applications and RBAC systems.

PIM for Groups offers can also be used for just-in-time ownership. As the owner of the group, you can manage group properties, including membership. For more information, see: [Privileged Identity Management (PIM) for Groups](~/id-governance/privileged-identity-management/concept-pim-for-groups.md).

---

### General Availability - Privileged Identity Management and Conditional Access integration 

**Type:** New feature       
**Service category:** Privileged Identity Management                               
**Product capability:** Privileged Identity Management                    

The Privileged Identity Management (PIM) integration with Conditional Access authentication context is generally available. You can require users to meet various requirements during role activation such as:

- Have specific authentication method through [Authentication Strengths](~/identity/authentication/concept-authentication-strengths.md)
- Activate from a compliant device 
- Validate location based on GPS
- Not have certain level of sign-in risk identified with Identity Protection
- Meet other requirements defined in Conditional Access policies

The integration is available for all providers: PIM for Azure AD roles, PIM for Azure resources, PIM for groups. For more information, see:
- [Configure Azure AD role settings in Privileged Identity Management](~/id-governance/privileged-identity-management/pim-how-to-change-default-settings.md)
- [Configure Azure resource role settings in Privileged Identity Management](~/id-governance/privileged-identity-management/pim-resource-roles-configure-role-settings.md)
- [Configure PIM for Groups settings](~/id-governance/privileged-identity-management/groups-role-settings.md)

---

### General Availability - Updated look and feel for Per-user MFA

**Type:** Plan for change    
**Service category:** MFA                       
**Product capability:** Identity Security & Protection              

As part of ongoing service improvements, we're making updates to the per-user MFA admin configuration experience to align with the look and feel of Azure. This change doesn't include any changes to the core functionality and will only include visual improvements. For more information, see: [Enable per-user Azure AD Multi-Factor Authentication to secure sign-in events](~/identity/authentication/howto-mfa-userstates.md).

---

### General Availability - Converged Authentication Methods in US Gov cloud

**Type:** New feature   
**Service category:** MFA                      
**Product capability:** User Authentication              

The Converged Authentication Methods Policy enables you to manage all authentication methods used for MFA and SSPR in one policy, migrate off the legacy MFA and SSPR policies, and target authentication methods to groups of users instead of enabling them for all users in the tenant. Customers should migrate management of authentication methods off the legacy MFA and SSPR policies before September 30, 2024. For more information, see: [Manage authentication methods for Azure AD](~/identity/authentication/concept-authentication-methods-manage.md).

---

### General Availability - Support for Directory Extensions using Azure AD cloud sync

**Type:** New feature
**Service category:** Provisioning
**Product capability:** Azure AD Connect cloud sync

Hybrid IT Admins can now sync both Active Directory and Azure AD Directory Extensions using Azure AD Connect cloud sync. This new capability adds the ability to dynamically discover the schema for both Active Directory and Azure Active Directory, thereby, allowing customers to map the needed attributes using the attribute mapping experience of cloud sync. For more information, see [Directory extensions and custom attribute mapping in cloud sync](~/identity/hybrid/cloud-sync/custom-attribute-mapping.md).

---

### Public Preview - Restricted Management Administrative Units

**Type:** New feature   
**Service category:** Directory Management                        
**Product capability:** Access Control               

Restricted Management Administrative Units allow you to restrict modification of users, security groups, and device in Azure AD so that only designated administrators can make changes.  Global Administrators and other tenant-level administrators can't modify the users, security groups, or devices that are added to a restricted management admin unit. For more information, see: [Restricted management administrative units in Azure Active Directory (Preview)](~/identity/role-based-access-control/admin-units-restricted-management.md).

---

### General Availability - Report suspicious activity integrated with Identity Protection

**Type:** Changed feature   
**Service category:** Identity Protection                        
**Product capability:** Identity Security & Protection              

Report suspicious activity is an updated implementation of the MFA fraud alert, where users can report a voice or phone app MFA prompt as suspicious. If enabled, users reporting prompts have their user risk set to high, enabling admins to use Identity Protection risk based policies or risk detection APIs to take remediation actions.  Report suspicious activity operates in parallel with the legacy MFA fraud alert at this time. For more information, see: [Configure Azure AD Multi-Factor Authentication settings](~/identity/authentication/howto-mfa-mfasettings.md).

---
