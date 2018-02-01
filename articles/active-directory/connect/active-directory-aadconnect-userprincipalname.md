---
title: Azure AD UserPrincipalName population
description: 
author: billmath
ms.author: billmath
ms.date: 01//2018
ms.topic: article
ms.workload: identity
ms.service: active-Directory
manager: mtillman
---

# Azure AD UserPrincipalName population

This article describes how the UserPrincipalName attribute is populated in Azure Active Directory (Azure AD).
The UserPrincipalName attribute value is the Azure AD username for the user accounts.

## UPN terminology
The following terminology is used in this article:

|Term|Description|
|-----|-----|
|Initial domain|This is the default domain (onmicrosoft.com) in the Azure AD Tenant. For example, contoso.onmicrosoft.com.|
|Microsoft Online Email Routing Address (MOERA)|Azure AD calculates the MOERA from Azure AD MailNickName attribute and Azure AD initial domain as &lt;MailNickName&gt;@&lt;initial domain&gt;.|
|On-premises mailNickName attribute|This is an attribute in Active Directory, the value of which represents the alias of a user in an Exchange organization.|
|On-premises mail attribute|This is an attribute in Active Directory, the value of which represents the email address of a user|
|Primary SMTP Address|This is the primary email address of an Exchange recipient object. For example, SMTP:user@contoso.com.|
|Alternate Login ID|This allows configuring source of Azure AD username to an on-premises attribute other than UserPrincipalName, such as mail attribute.|

## What is UserPrincipalName?
UserPrincipalName is an attribute that is an Internet-style login name for a user based on the Internet standard [RFC 822](http://www.ietf.org/rfc/rfc0822.txt). 

### UPN Format
A UPN consists of a UPN prefix (the user account name) and a UPN suffix (a DNS domain name). The prefix is joined with the suffix using the "@" symbol. For example, "someone@ example.com". A UPN must be unique among all security principal objects within a directory forest. This means the prefix of a UPN can be reused, just not with the same suffix.

A UPN suffix has the following restrictions:

- It must be the DNS name of a domain, but does not need to be the name of the domain that contains the user. 
- It must be the name of a domain in the current domain forest, or an alternate name listed in the upnSuffixes attribute of the Partitions container within the Configuration container.

### UPN Management
A UPN can be assigned, but is not required, when a user account is created. When a UPN is created, it is unaffected by changes to other attributes of the user object such as the user being renamed or moved. This allows the user to keep the same login name if a directory is restructured. However, an administrator can change a UPN. When you create a new user object, you should check the local domain and the global catalog for the proposed name to ensure it does not already exist.

When a user uses a UPN to log on to a domain, the UPN is validated by searching the local domain and then the global catalog. If the UPN is not found in the global catalog, the logon attempt fails.

## UPN in Azure AD 
The UPN is used by Azure AD to allow users to sign-in.  The UPN that a user can use, depends on whether or not the domain has been verified.  If the domain has been verified, then a user with that suffix will be allowed to sign-in to Azure AD.  

The attribute is synchronized by Azure AD Connect.  During installation you can view the domains that have been verified and those that have not.

   ![Unverified domains](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png) 

## Alternate Login ID
In some environments, due to corporate policy or on-premises line-of-business application dependencies, end users may only be aware of their email address and not their UPN. In some cases, the UPN is also non-routable (jdoe@contoso.local)and is only used for authenticating into applications on the corporate network.

Azure AD requires all user login IDs to be fully internet routable. If the on-premises UPN uses a non-routable domain (ex. Contoso.local), or the existing UPN cannot be changed due to local application dependencies, we recommend setting up alternate login ID. Alternate login ID allows you to configure a sign in experience where users can sign in with an attribute other than their UPN, such as mail.

However, the alternate login ID feature is only available for federated environments with AD FS deployed. It is not supported in the following scenarios:
- Non-routable domains (e.g. Contoso.local) that cannot be verified by Azure AD.
- Managed environments that do not have AD FS deployed.

When the alternate login ID feature is enabled, AD FS will try to authenticate the end user with alternate login ID first and then fall back to use UPN if it cannot find an account that can be identified by the alternate login ID. You should make sure there are no clashes between the alternate login ID and the UPN if you want to still support the UPN login. For example, setting one's mail attribute with the other's UPN will block the other user from signing in with his UPN.

To enable alternate login ID with Azure AD, no additional configurations steps are needed when using Azure AD Connect. Alternate ID can be configured directly from the wizard. See Azure AD sign-in configuration for your users under the section Sync. Under the **User Principal Name** drop-down, select the attribute for alternate logon id.

![Unverified domains](./media/active-directory-aadconnect-userprincipalname/altloginid.png)  

For more information see [Configure Alternate Login ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) and [Azure AD sign-in configuration](active-directory-aadconnect-get-started-custom#azure-ad-sign-in-configuration)

## Non-verified UPN Suffix
If the on-premises UserPrincipalName attribute/Alternate Login ID suffix is not verified with Azure AD Tenant, then Azure AD UserPrincipalName attribute value is set to MOERA. This means that Azure AD calculates the MOERA from the Azure AD MailNickName attribute and Azure AD initial domain as &lt;MailNickName&gt;@&lt;initial domain&gt;.

## Verified UPN Suffix
If the on-premises UserPrincipalName attribute/Alternate Login ID suffix is verified with the Azure AD Tenant, then the Azure AD UserPrincipalName attribute value is going to be the same as the on-premises UserPrincipalName attribute/Alternate Login ID value.

## Azure AD MailNickName Attribute Value Calculation
Because the Azure AD UserPrincipalName attribute value could be set to MOERA, it is important to understand how the Azure AD MailNickName attribute value, which is the MOERA prefix, is calculated.

When a user object is synchronized to an Azure AD Tenant for the first time, Azure AD checks the following in the given order and sets the MailNickName attribute value to the first existing one:

- On-premises mailNickName attribute
- Prefix of on-premises mail attribute
- Prefix of primary SMTP address
- Prefix of on-premises userPrincipalName attribute/Alternate Login ID

When the updates to a user object are synchronized to the Azure AD Tenant, Azure AD updates the MailNickName attribute value only in case there is an update to the on-premises mailNickName attribute value.

>[!IMPORTANT]
>Azure AD recalculates the UserPrincipalName attribute value only in case an update to the on-premises UserPrincipalName attribute/Alternate Login ID value is synchronized to the Azure AD Tenant. 
>
>In addition to this, whenever Azure AD recalculates the UserPrincipalName attribute, it also recalculates the MOERA. 

## UPN scenarios
The following are example scenarios of how the UPN is calculated based on the given scenario.

### Scenario 1: Non-Verified UPN Suffix – Initial Synchronization

On-Premises user object:
- mailNickName		: &lt;not set&gt;
- mail			: us1@contoso.com
- proxyAddresses		: {SMTP:us2@contoso.com}
- userPrincipalName	: us3@contoso.com`

Synchronized the user object to Azure AD Tenant for the first time
- Set Azure AD MailNickName attribute to on-premises mail attribute prefix.
- Set MOERA to  &lt;MailNickName&gt;@&lt;initial domain&gt;.
- Set Azure AD UserPrincipalName attribute to MOERA.

Azure AD Tenant user object:
- MailNickName		: us1			
- UserPrincipalName	: us1@contoso.onmicrosoft.com


### Scenario 2: Non-Verified UPN Suffix – Set On-Premises mailNickName Attribute

On-Premises user object:
- mailNickName		: us4
- mail			: us1@contoso.com
- proxyAddresses		: {SMTP:us2@contoso.com}
- userPrincipalName	: us3@contoso.com

Synchronize update on on-premises mailNickName attribute to Azure AD Tenant
- Update Azure AD MailNickName attribute with on-premises mailNickName attribute.
- As there is no update on on-premises userPrincipalName attribute, there will be no change to Azure AD UserPrincipalName attribute.

Azure AD Tenant user object:
- MailNickName		: us4
- UserPrincipalName	: us1@contoso.onmicrosoft.com

### Scenario 3: Non-Verified UPN Suffix – Update On-Premises userPrincipalName Attribute

On-Premises user object:
- mailNickName		: us4
- mail			: us1@contoso.com
- proxyAddresses		: {SMTP:us2@contoso.com}
- userPrincipalName	: us5@contoso.com

Synchronize update on on-premises userPrincipalName attribute to Azure AD Tenant
- Update on on-premises userPrincipalName attribute triggers recalculation of MOERA and Azure AD UserPrincipalName attribute.
- Set MOERA to &lt;MailNickName&gt;@&lt;initial domain&gt;.
- Set Azure AD UserPrincipalName attribute to MOERA.

Azure AD Tenant user object:
- MailNickName		: us4
- UserPrincipalName	: us4@contoso.onmicrosoft.com

### Scenario 4: Non-verified UPN Suffix – Update On-Premises mail attribute and Primary SMTP Address

On-Premises user object:
- mailNickName		: us4
- mail			: us6@contoso.com
- proxyAddresses		: {SMTP:us7@contoso.com}
- userPrincipalName	: us5@contoso.com

Synchronize update on on-premises mail attribute and primary SMTP address to Azure AD Tenant
- After the initial synchronization of the user object, updates to on-premises mail attribute and primary SMTP address does not affect neither Azure AD MailNickName nor UserPrincipalName attribute.

Azure AD Tenant user object:
- MailNickName		: us4
- UserPrincipalName	: us4@contoso.onmicrosoft.com

### Scenario 5: Verified UPN Suffix – Update On-Premises userPrincipalName Attribute Suffix

On-Premises user object:
- mailNickName		: us4
- mail			: us6@contoso.com
- proxyAddresses		: {SMTP:us7@contoso.comu
- serPrincipalName	: us5@verified.contoso.com

Synchronize update on on-premises userPrincipalName attribute to the Azure AD Tenant
- Update on on-premises userPrincipalName attribute triggers recalculation of Azure AD UserPrincipalName attribute.
- Set Azure AD UserPrincipalName attribute to on-premises userPrincipalName attribute as the UPN suffix is verified with the Azure AD Tenant.

Azure AD Tenant user object:
- MailNickName		: us4	  
- UserPrincipalName	: us5@verified.contoso.com
