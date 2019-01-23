---

title: Add B2B collaboration users in the Azure portal - Azure Active Directory | Microsoft Docs
description: Shows how an admin can add guest users to their directory from a partner organization using Azure Active Directory (Azure AD) B2B collaboration.

services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 12/14/2018

ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal

---

# Use one-time passcodes to authenticate B2B guest users

The one-time passcode feature authenticates B2B guest users when they can't be authenticated through other means like Google federation, Azure AD, or a Microsoft Account. With the one-time passcode feature, when a guest user redeems their invitation or accesses the shared resource, they receive a temporary code via email that they enter while signing in.

This feature is currently available for preview (see [Opting in to the preview](#opting-in-to-the-preview) below). After preview, this feature will be turned on by default for all tenants.

> [!NOTE]
> One-time passcode users must sign in using a link that includes the tenant context, for example `https://myapps.microsoft.com/<tenant id>`. Direct links to applications and resources also work as long as they include the tenant context. Guest users are currently unable to sign in using endpoints that have no tenant context. For example, using `https://myapps.microsoft.com`, `https://portal.azure.com`, or the Teams common endpoint will result in an error. 

## User experience for the invited B2B user using an email one-time passcode 
With one-time passcode authentication, the guest user can redeem your invitation by clicking a direct link or by using the invitation email. In either case, a message in the browser indicates that a code will be sent to the guest user's email address. The guest user selects **Send code**:
 
   ![Access Panels manage app](media/one-time-passcode/otp-send-code.png)
 
A passcode is sent to the user’s email address. The user retrieves the passcode from the email and enters it in the browser window:
 
   ![Access Panels manage app](media/one-time-passcode/otp-enter-code.png)
 
The guest user is now authenticated, and they can see the shared resource or continue signing in. 

> [!NOTE]
> One-time passcodes are valid for 30 minutes. After 30 minutes, that specific one-time passcode is no longer valid for sign-in, and the user must request a new one. User sessions expire after 24 hours. After that time, the guest user receives a new passcode when they access the resource. Session expiration provides added security, especially after a guest user no longer needs access.

## When does a guest user get a one-time passcode?

When a guest user redeems an invitation or uses a link to a resource that has been shared with them, they’ll receive a one-time passcode if 
- An Alternate ID (private preview feature) has not been set up for their account 
- They do not have an Azure AD account 
- They do not have a Microsoft Account (MSA) 
- The inviting tenant did not set up Google federation for @gmail.com and @googlemail.com users or direct federation (private preview feature) for the user’s domain name 

At the time of invitation, there's no indication that the user you're inviting will get a one-time passcode. But when the guest user signs in, the one-time passcode will be the fallback method for authentication if other methods can't be used. 

> [!NOTE]
> When a user redeems a one-time passcode and later obtains a Microsoft-backed or federated account, they'll continue to be authenticated using a one-time passcode. If you want to update their authentication method, you can delete their guest user account and reinvite them.

### Examples
- Guest user alexdoe@gmail.com is invited to Fabrikam, which does not have Google federation set up. Alex does not have a Microsoft account. He'll receive a one-time passcode for authentication.
- Guest user jane@firstupconsultants.com is invited to Contoso, which does not have direct federation set up with firstupconsultants.com. Jane will receive a one-time passcode for authentication.	

## Opting in to the preview 
It may take a few minutes for the opt-in action to take effect.

### To opt in using the Azure AD portal
1.	Sign in to the Azure portal as an Azure AD global administrator.
2.	In the navigation pane, select **Azure Active Directory**.
3.	Under **Manage**, select **Organizational Relationships**.
4.	Select **Settings**.
5.	Under **Enable Email One-Time Passcode for guests (Preview)**, select **Yes**.
 
### To opt in using PowerShell
1. Install the latest AzureADPreview module if you don’t have it already by running the following command:

````powershell 
Install-module AzureADPreview
````
2. Run the following command to connect to the tenant domain: 

````powershell 
Connect-AzureAD 
````
3. When prompted for your credentials, sign in with the Global Administrator account. 
4. Depending on whether you currently have B2B policies set up, choose one of the following sets of commands.

#### If B2B policies don't currently exist

````powershell 
// Get current policy
$currentpolicy =  Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1

// Check if policy is null
$currentpolicy -eq $null

// If policy is null then define new policy value with OneTimePasscode
$policyValue=@("{`"B2BManagementPolicy`":{`"PreviewPolicy`":{`"Features`":[`"OneTimePasscode`"]}}}")

// Create new policy
New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true`
````

#### If B2B policies already exist

````powershell 
// Get current policy
$currentpolicy =  Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
 
// Check policy is not null
$currentPolicy -ne $null
 
// If policy is not null, then get policy details
$policy = $currentpolicy.Definition | ConvertFrom-Json

// Add preview policy - inline
$features=[PSCustomObject]@{'Features'=@('OneTimePasscode')}; $policy.B2BManagementPolicy | Add-Member 'PreviewPolicy' $features; $policy.B2BManagementPolicy
 
// Finally update the policy
$updatedPolicy = $policy | ConvertTo-Json -Depth 3
Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id

````

## Opting out of the preview after opting in
It may take a few minutes for the opt-out action to take effect. If you turn off the preview, any guest users who have redeemed a one-time passcode will not be able to sign in. You can delete the guest user and reinvite the user to enable them to sign in again.

### To turn off preview using the Azure AD portal
1.	Sign in to the Azure portal as an Azure AD global administrator.
2.	In the navigation pane, select **Azure Active Directory**.
3.	Under **Manage**, select **Organizational Relationships**.
4.	Select **Settings**.
5.	Under **Enable Email One-Time Passcode for guests (Preview)**, select **No**.

### To turn off preview using PowerShell
1. Install the latest AzureADPreview module if you don’t have it already by running the following command:

````powershell 
Install-module AzureADPreview
````
2. Run the following command to connect to the tenant domain: 

````powershell 
Connect-AzureAD 
````
3. When prompted for your credentials, sign in with the Global Administrator account. 
4. Run the following commands:

````powershell 
// Get current policy
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy' -and $_.IsOrganizationDefault -eq $true} | select -First 1
 
// Check if policy not null and has OneTimePasscode enabled
($currentPolicy -ne $null) -and ($currentPolicy.Definition -like "*OneTimePasscode*")
 
// If policy is not null and has OneTimePasscode enabled, then get policy details
$policy = $currentpolicy.Definition | ConvertFrom-Json
 
// Remove OneTimePasscode from preview features
$policy.B2BManagementPolicy.PreviewPolicy.Features = $policy.B2BManagementPolicy.PreviewPolicy.Features.Where({$_ -ne "OneTimePasscode"})
 
// Finally update the policy
$updatedPolicy = $policy | ConvertTo-Json -Depth 3
Set-AzureADPolicy -Definition $updatedPolicy -Id $currentpolicy.Id
````

