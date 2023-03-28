---
title: Clean up unmanaged Azure Active Directory accounts
description: Clean up unmanaged accounts using email one-time password and PowerShell modules in Azure AD
services: active-directory 
author: gargi-sinha
ms.author: gasinh
manager: martinco
ms.date: 03/28/2023
ms.topic: how-to
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.custom: it-pro
ms.collection: M365-identity-device-management
---

# Clean up unmanaged Azure Active Directory accounts

Prior to August 2022, Azure Acticve Directory B2B (Azure AD B2B) supported self-service sign-up for email-verified users. With this feature, users create Azure AD accounts, when they verify email ownership. These accounts were created in unmanaged (or viral) tenants: users created accounts with an organization domain, not under IT team management. Access persists after users leave the organization. 

To learn more, see, [What is self-service sign-up for Azure AD?](./directory-self-service-signup.md)

   > [!NOTE]
   > Unmanaged Azure AD accounts via Azure AD B2B are deprecated. As of August 2022, new B2B invitations can't be redeemed. However, invitations prior to August 2022 were redeemable with unmanaged Azure AD accounts. 

## Remove unmanaged Azure AD accounts

Use the following guidance to remove unmanaged Azure AD accounts from your Azure AD tenants. Tool features help identify viral users in your Azure AD tenant. You can reset the user redemption status.

* Use the sample application in [Azure-samples/Remove-unmanaged-guests](https://github.com/Azure-Samples/Remove-Unmanaged-Guests)
* Use PowerShell cmdlets in [AzureAD/MSIdentityTools](https://github.com/AzureAD/MSIdentityTools/wiki/)  

After you run a tool, users with unmanaged Azure AD accounts access the tenant, and re-redeem their invitations. However, Azure AD prevents users from redeeming with an unmanaged Azure AD account. They’ll redeem with another account type. Google Federation and SAML/WS-Fed are not enabled by default. Therefore, users redeem with a Microsoft account (MSA) or email one-time password (OTP). MSA is recommended. 

Learn more: [Invitation redemption flow](../external-identities/redemption-experience.md#invitation-redemption-flow).

## Overtaken tenants and domains

Some tenants created as unmanaged tenants can be taken over and
converted to a managed tenant. See, [take over an unmanaged directory as
administrator in Azure AD](./domains-admin-takeover.md).

In some cases, overtaken domains might not be updated, for example, missing a DNS TXT record and therefore become flagged as unmanaged. Implications are:

- For guest users who belong to formerly unmanaged tenants, redemption status is reset and one consent prompt appears. Redemption occurs with same account as before.

- After unmanaged user redemption status is reset, the tool might identify unmanaged users that are false positives.

## Reset redemption using a sample application

Use the sample application on
    [Azure-Samples/Remove-Unmanaged-Guests](https://github.com/Azure-Samples/Remove-Unmanaged-Guests).

## Reset redemption using MSIdentityTools PowerShell Module

MSIdentityTools PowerShell Module is a collection of cmdlets and
scripts. They are for use in the Microsoft identity platform and Azure
AD; they augment capabilities in the PowerShell SDK. See, [Microsoft
Graph PowerShell
SDK](https://github.com/microsoftgraph/msgraph-sdk-powershell).

Run the following cmdlets:

- `Install-Module Microsoft.Graph -Scope CurrentUser`

- `Install-Module MSIdentityTools`

- `Import-Module msidentitytools,microsoft.graph`

To identify unmanaged Azure AD accounts, run:

- `Connect-MgGraph -Scope User.ReadAll`

- `Get-MsIdUnmanagedExternalUser`

To reset unmanaged Azure AD account redemption status, run:

- `Connect-MgGraph -Scopes User.ReadWriteAll`

- `Get-MsIdUnmanagedExternalUser | Reset-MsIdExternalUser`

To delete unmanaged Azure AD accounts, run:

- `Connect-MgGraph -Scopes User.ReadWriteAll`

- `Get-MsIdUnmanagedExternalUser | Remove-MgUser`

## Next steps

Examples of using
[Get-MSIdUnmanagedExternalUser](https://github.com/AzureAD/MSIdentityTools/wiki/Get-MsIdUnmanagedExternalUser)
