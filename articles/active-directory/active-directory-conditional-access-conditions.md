---
title: What are conditions in Azure Active Directory conditional access? | Microsoft Docs
description: Learn how conditions are used in Azure Active Directory conditional access to trigger a policy.
services: active-directory
keywords: conditional access to apps, conditional access with Azure AD, secure access to company resources, conditional access policies
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''

ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb

#Customer intent: As a IT admin, I need to understand the conditions in conditional access so that I can set them according to my business needs

---

# What are conditions in Azure Active Directory conditional access? 

You can control how authorized users access your cloud apps by using [Azure Active Directory (Azure AD) conditional access](active-directory-conditional-access-azure-portal.md). In a conditional access policy, you define the response to the reason for triggering your policy. An example response is **do this**. An example reason is **when this happens**.

![Control](./media/active-directory-conditional-access-conditions/10.png)


In the context of conditional access, **When this happens** is called **conditions**. **Then do this** is called **access controls**. The combination of your conditions with your access controls represents a conditional access policy.

![Control](./media/active-directory-conditional-access-conditions/61.png)


Conditions you haven't configured in a conditional access policy aren't applied. Some conditions are [mandatory](active-directory-conditional-access-best-practices.md#whats-required-to-make-a-policy-work) to apply a conditional access policy to an environment. 

This article is an overview of the conditions and how they're used in a conditional access policy. 

## Users and groups

The users and groups condition is mandatory in a conditional access policy. In your policy, you can either select **All users** or select specific users and groups.

![Control](./media/active-directory-conditional-access-conditions/111.png)

When you select **All users**, your policy is applied to all users in the directory. This includes guest users.

When you **Select users and groups**, you can set the following options:

* **All guest users**. Enables you to target a policy to B2B guest users. This condition matches any user account that has the *userType* attribute set to *guest*. You can use this setting in cases where a policy needs to be applied as soon as the account is created in an invite flow in Azure AD.

* **Directory roles**. Enables you to target a policy based on a user’s role assignment. This condition supports directory roles like *Global administrator* or *Password administrator*.

* **Users and groups**. Enables you to target specific sets of users. For example, you can select a group that contains all members of the HR department when you have an HR app selected as the cloud app. A group can be any type of group in Azure AD, including dynamic or assigned security and distribution groups.

You can also exclude specific users or groups from a policy. One common use case is service accounts if your policy enforces multifactor authentication (MFA). 

Targeting specific sets of users is useful for the deployment of a new policy. In a new policy, you should target only an initial set of users to validate the policy behavior. 



## Cloud apps 

A cloud app is a website or service. This includes websites protected by the Azure AD Application Proxy. For a detailed description of the supported cloud apps, see [cloud apps assignment](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments). 	

The cloud apps condition is mandatory in a conditional access policy. In your policy, you can either select **All cloud apps** or select specific apps.

![Control](./media/active-directory-conditional-access-conditions/03.png)

- Select **All cloud apps** to baseline policies to apply to the entire organization. Use this selection for policies that require multifactor authentication when sign-in risk is detected for any cloud app. A policy applied to **All cloud apps** applies to access to all websites and services. This setting isn't limited to the cloud apps that appear on the **Select Cloud apps** list.

- Select individual cloud apps to target specific services by policy. For example, you can require users to have a [compliant device](active-directory-conditional-access-policy-connected-applications.md) to access SharePoint Online. This policy is also applied to other services when they access SharePoint content. An example is Microsoft Teams. 

You can exclude specific apps from a policy. However, these apps are still subject to the policies applied to the services they access. 



## Sign-in risk

A sign-in risk is an indicator of the high, medium, or low likelihood that a sign-in attempt wasn't done by the legitimate owner of a user account. Azure AD calculates the sign-in risk level during a user's sign in. The calculated sign-in risk level can be a condition in a conditional access policy. 

![Conditions](./media/active-directory-conditional-access-conditions/22.png)

To use this condition, you need to have [Azure Active Directory Identity Protection](active-directory-identityprotection.md) enabled.
 
Common use cases for this condition are policies with the following protections:

- Block users with a high sign-in risk. This protection prevents potentially non-legitimate users from accessing your cloud apps. 
- Require multifactor authentication for users with a medium sign-in risk. By enforcing multifactor authentication, you can provide additional confidence that the sign in is done by the legitimate owner of an account.

For more information, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).  

## Device platforms

The device platform is characterized by the operating system that runs on your device. Azure AD identifies the platform by using information provided by the device, such as user agent. This information is unverified. We recommend that all platforms have a policy applied to them. The policy should either block access, require compliance with Intune policies, or require the device be domain joined. The default is to apply a policy to all device platforms. 


![Conditions](./media/active-directory-conditional-access-conditions/24.png)

For a complete list of the supported device platforms, see [device platform condition](active-directory-conditional-access-technical-reference.md#device-platform-condition).


A common use case for this condition is a policy that restricts access to your cloud apps to [managed devices](active-directory-conditional-access-policy-connected-applications.md#managed-devices). For more scenarios including the device platform condition, see [Azure Active Directory app-based conditional access](active-directory-conditional-access-mam.md).



## Device state

The device state condition allows hybrid Azure AD joined devices and devices marked as compliant to be excluded from a conditional access policy. This condition is useful when a policy should only apply to an unmanaged device to provide additional session security. For example, only enforce the Microsoft Cloud App Security session control when a device is unmanaged. 


![Conditions](./media/active-directory-conditional-access-conditions/112.png)

If you want to block access for unmanaged devices, implement [device-based conditional access](active-directory-conditional-access-policy-connected-applications.md).


## Locations

By using locations, you have the option to define conditions based on where a connection attempt was initiated. 
     
![Conditions](./media/active-directory-conditional-access-conditions/25.png)

Common use cases for this condition are policies with the following protections:

- Require multifactor authentication for users accessing a service when they're off the corporate network.  

- Block access for users accessing a service from specific countries or regions. 

For more information, see [What is the location condition in Azure Active Directory conditional access?](active-directory-conditional-access-locations.md).


## Client apps

By using the client apps condition, you can apply a policy to different types of applications. Examples are websites and services and mobile apps and desktop applications. 



An application is classified as follows:

- A web site or service if it uses the web SSO protocols, SAML, WS-Fed, or OpenID Connect for a confidential client.

- A mobile app or desktop application if it uses the mobile app OpenID Connect for a native client.

For a complete list of the client apps you can use in your conditional access policy, see [Client apps condition](active-directory-conditional-access-technical-reference.md#client-apps-condition) in the Azure Active Directory Conditional Access technical reference.

Common use cases for this condition are policies with the following protections:

- Require a [compliant device](active-directory-conditional-access-policy-connected-applications.md) for mobile and desktop applications that download large amounts of data to the device. At the same time, allow browser access from any device.

- Block access from web applications but allow access from mobile and desktop applications.

In addition to using web SSO and modern authentication protocols, you can apply this condition to mail applications that use Exchange ActiveSync, like the native mail apps on most smartphones. Currently, client apps using legacy protocols need to be secured using Azure AD Federation Services.

You can only select the client apps condition if **Office 365 Exchange Online** is the only cloud app you've selected.

![Cloud apps](./media/active-directory-conditional-access-conditions/32.png)

Selecting **Exchange ActiveSync** as a client apps condition is supported only if you don't have other conditions  in a policy configured. However, you can narrow down the scope of this condition to apply only to supported platforms.

 
![Supported platforms](./media/active-directory-conditional-access-conditions/33.png)

Applying this condition only to supported platforms is equal to all device platforms in a [device platform condition](active-directory-conditional-access-conditions.md#device-platforms).

![Supported platforms](./media/active-directory-conditional-access-conditions/34.png)


 For more information, see these articles:

- [Set up SharePoint Online and Exchange Online for Azure Active Directory conditional access](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory app-based conditional access](active-directory-conditional-access-mam.md) 


### Legacy authentication  

Conditional access now applies to older Office clients that do not support modern authentication as well as clients that use mail protocols like POP, IMAP, SMTP, etc. This allows you to configure policies like **block access from other clients**.


![Legacy authentication](./media/active-directory-conditional-access-conditions/160.png)  


#### Known issues

- Configuring a policy for **Other clients** blocks the entire organization from certain clients like SPConnect. This is due to these older clients authenticating in unexpected ways. This issue does not apply to the major Office applications like the older Office clients. 

- It can take up to 24 hours for the policy to take effect. 


#### Frequently asked questions

**Will this block Exchange Web Services (EWS)?**

It depends on the authentication protocol that EWS is using. If the EWS application is using modern authentication, it will be covered by the "Mobile apps and desktop clients" client app. If the EWS application is using basic authentication, it will be covered by the “Other clients” client app.


**What controls can I use for Other clients**

Any control can be configured for "Other clients". However, the end user experience will be block access for all cases. "Other clients" do not support controls like MFA, compliant device, domain join, etc. 
 
**What conditions can I use for Other clients?**

Any conditions can be configured for "Other clients".

**Does Exchange ActiveSync support all conditions and controls?**

No. Here is the summary of Exchange ActiveSync (EAS) support:

- EAS only supports user and group targeting. It doesn’t support guest, roles. If guest/role condition is configured, all users will get blocked since we cannot determine if the policy should apply to the user or not.

- EAS only works with Exchange as the cloud app. 

- EAS does not support any condition except client app itself.

- EAS can be configured with any control (all except device compliance will lead to block).

**Do the policies apply to all client apps by default going forward?**

No. There is no change in the default policy behavior. The policies continue to apply to browser and mobile applications/desktop clients by default.



## Next steps

- If you want to know how to configure a conditional access policy, see [Require MFA for specific apps with Azure Active Directory conditional access](active-directory-conditional-access-app-based-mfa.md).

- If you are ready to configure conditional access policies for your environment, see the [best practices for conditional access in Azure Active Directory](active-directory-conditional-access-best-practices.md). 

