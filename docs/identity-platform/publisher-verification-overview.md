---
title: Microsoft identity platform accounts & tenant profiles on Android | Azure
description: An overview of publisher verification for Microsoft identity platform.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/30/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
---

# Publisher verfication

Publisher Verification allows developers with a verified (or “vetted”) Microsoft Partner Center (MPN) account to mark one or more app registrations as Publisher Verified.   

## Benefits
Publisher verification provides the following benefits:
Transparency and risk reduction for customers- this capability helps customers understand which apps being used in their organizations are published by developers they trust. 

UX differentiation- a “verified” badge will appear on the Azure AD consent prompt, Enterprise Apps page, and additional UX surfaces used by end-users and admins. 

Smoother enterprise adoption- admins will soon be able to configure new User Consent Policies, and Publisher Verification status will be one of the primary policy criteria. 

Risk evaluation- Microsoft’s detections for “risky” consent requests will include Publisher Verification as a signal. 

## What types of apps is this relevant for? 

This feature is primarily targeted at developers building multi-tenant apps that leverage OAuth 2.0 and OpenID Connect with the Microsoft identity platform (Azure AD). These apps may simply sign users in using OpenID Connect, or they may use OAuth to request access to data using APIs like Microsoft Graph. 

## Requirements
Publisher Verification is a simple process. There are a few pre-requisites, some of which will have already been completed by many Microsoft partners. They include: 

1. An MPN ID for a valid Microsoft Partner Network account that has completed the verification process. This MPN account must be the Partner global account (PGA) for your organization. 

1. An Azure AD tenant with a DNS-verified custom domain, which must match the domain of the email address used during verification in step #1. 

1. An app registered in an Azure AD tenant, with a Publisher Domain configured using the same domain as above. 

1. The user performing verification must be authorized to make changes to both the app registration in Azure AD and the MPN account in Partner Center. 

  1. In Azure AD this user must either be the Owner of the app or have one of the following roles: Application Admin, Cloud Application Admin, Global Admin. 

  1. In Partner Center this user must have of the following roles: MPN Admin, Accounts Admin, or a Global Admin (this is a shared role mastered in Azure AD). 

Developers who have already met these pre-requisites can get verified in a matter of minutes. If they have not been met, getting set up should be a quick process which is free of charge from Microsoft. 

If you cannot currently meet these requirements, or don’t know if you can, you can still participate in the Private Preview! We would like to hear your feedback and help you understand how you can satisfy these requirements. 

## Frequently Asked Questions 

When will the verified badge start showing up on the consent screen? Admins and end-users will be able to see the badge even during the Private Preview phase. Users who get prompted to consent to your app will start seeing the badge soon after you have gone through the process successfully, although it may take some time for this to replicate throughout the system. This will generally be a few minutes but could be a few hours. 

When will other experiences start showing the badge or using verification status?  

The Enterprise Apps experience will start showing an indication of Publisher Verified apps by Public Preview in mid-May. Admins will be able to set policies using this information in a similar timeframe. 

Dates on additional UI surfaces are still TBD. 

Microsoft’s Risk Based Consent capability will start factoring in verification status immediately. 

I don’t know my MPN ID- where do I find it? Please see the Microsoft Partner Network FAQ for guidance on how to find your organization’s MPN ID. 

How do I make sure my MPN verification (a.k.a. vetting/authorization) is complete in Partner Center? If you are unsure whether vetting has been completed, you can check the Partner profile page in Partner Center. It will say “Verification status: Authorized”. 

I do not have an MPN account and need to create one. How long will verification take? This process is often very quick, especially if you have enrolled in other Microsoft programs before. However, at times of increased load, you should expect 3 weeks for a net-new vetting to be processed. See Verify your account information or Vetting process for Partners in Partner Center for more details. 

Do I need to complete this process again for each app that my organization publishes? Most of the steps in this process only need to be performed once. For example, you will only need a single verified/vetted MPN ID. However, this MPN ID will need to be associated with each app you want to mark as Publisher Verified. You can use PowerShell or Microsoft Graph to help mark multiple apps verified in bulk. 

Will I have to go through parts of this process again in the future to renew my app’s Publisher Verification status? As of Private Preview, the renewal/attestation process is not yet defined. Microsoft reserves the right to add a required renewal process in the future, which may involve one or more steps needing to be performed again. 

I successfully marked my app Publisher Verified, but the verified Publisher display name is showing up incorrectly. Why? This property is pulled directly from your MPN account in Partner Center when you mark your app registration as Publisher Verified. If it is incorrect, that means it was set incorrectly in Partner Center. You can confirm this by visiting the Partner profile page in Partner Center. See below for instructions on updating it, if necessary. 

My organization’s name has changed, or was incorrect initially, and needs to be updated. How do I do this? You can update your organization’s name on the Partner profile page in Partner Center. After doing this, you will need to perform the initial verification process again. After that completes, you will need to update your Verified Publisher information in Azure AD using the “update” button on the Branding blade where you initially performed verification. 

My MPN ID has changed, how do I update it on my app(s) in Azure AD? You will need to remove your old MPN ID from each app that was previously marked as Publisher Verified and go through the binding process again with the new one. The same requirements will apply as the initial process.  

How much does this cost? Does it require any license? Microsoft does not charge developers for Publisher Verification and it does not require any specific license. 

Is this the same thing as Microsoft 365 Publisher Attestation? What about Microsoft 365 App Certification? These are not the same things. Publisher Verification is a complementary but separate feature to both Microsoft 365 Publisher Attestation and Microsoft 365 App Certification. For more info: 

Publisher Attestation is a voluntary program allows you to complete a self-assessment of your app's security, data handling, and compliance practices. The information you provide will be processed and presented to your potential customers so they can better evaluate your app before enabling it for their organization. 

Microsoft 365 App Certification offers assurance and confidence to enterprise organizations that data and privacy are adequately secured and protected when your Microsoft Word, Excel, PowerPoint, Outlook, Teams integration, or Graph API app is introduced to the Microsoft 365 platform. Certification confirms that an app solution is compatible with Microsoft technologies, compliant with cloud app security best practices, and supported by Microsoft, a trusted partner. 

You will need to complete the Publisher Verification process independently of participation in those programs. 

Is this the same thing as the Azure AD Application Gallery? No- Publisher Verification is a complementary but separate feature to the Azure Active Directory application gallery. You will need to complete the Publisher Verification process independently of participation in that program. 

As a customer using apps built on the Microsoft identity platform, what assurances does Publisher Verification provide? 

The primary goal of Publisher Verification (Preview) is to help customers understand the authenticity of app developers integrating with the Microsoft identity platform. When an application is marked as Publisher Verified, it means that the developer of the application has verified their identity using their Microsoft Partner Network (MPN) account and has associated this MPN account with their app registration. 

The Publisher Verification process is specifically about transparency and authenticity. When an app is marked Publisher Verified this does not indicate whether the application or its developer has achieved any specific certifications, complies with industry standards, adheres to best practices, etc. Other Microsoft programs do provide these assurances, including Microsoft 365 App Certification 

Publisher Verification is performed on an app by app basis. For a developer to mark an app as Publisher Verified, the following requirements must be met: 

The developer must have a valid Microsoft Partner Network account that  

This account must have completed the MPN account verification process.  

This account must be the Partner global account (PGA) for the developer’s organization. 

The app must be registered in an Azure AD tenant  

A publisher domain must be set on the app 

The domain used for email verification in the partner’s MPN account must match either: 

A DNS-verified custom domain in the tenant where the app is registered  

The app publisher domain from #5 

The user performing verification must be authorized to make changes to both the app registration in Azure AD and the MPN account in Partner Center. 

In Azure AD this user must either be the Owner of the app or have one of the following roles: Application Admin, Cloud Application Admin, Global Admin. 

In Partner Center this user must have of the following roles: MPN Admin, Accounts Admin, or a Global Admin (this is a shared role mastered in Azure AD). 

## Next steps
* Learn how to mark an app as publisher verified
* Troubleshoot publisher verification