---
title: Plan conditional access policies in Azure Active Directory | Microsoft Docs
description: In this article, you learn how to plan conditional access policies for Azure Active Directory.
services: active-directory
author: MarkusVi
manager: mtillman
tags: azuread
ms.service: active-directory
ms.component: conditional-access
ms.topic: conceptual
ms.workload: identity
ms.date: 12/13/2018
ms.author: markvi
ms.reviewer: martincoetzer
---

# How To: Plan your conditional access deployment in Azure Active Directory


Why am I supposed to read this?


## Prerequisites

Read the [overview](overview.md). 


## What you should know

Conditional access is more a framework than a stand-alone feature. Consequently, some conditional access settings require additional features to be configured. For example, you can configure a policy that responds to a specific [sign-in risk level](../identity-protection/howto-sign-in-risk-policy.md#what-is-the-sign-in-risk-policy). However, a policy that is based on a sign-in risk level requires [Azure Active Directory identity protection](../identity-protection/overview.md) to be enabled. 

If additional features are required, you might also need to get related licenses. For example, while conditional access is Azure AD P1 capability, identity protection requires an Azure AD P2 license.  

There are two types of conditional access policies: custom and baseline. A [baseline policy](baseline-protection.md) is a predefined conditional access policy. The goal of these policies is to ensure that you have at least the baseline level of security enabled. While managing custom conditional access policies requires an Azure AD Premium license, baseline policies are available in all editions of Azure AD. Baseline policies provide only limited customization options. If a scenario requires more flexibility, disable the baseline policy, and implement your requirements in a custom policy. 



## Draft policies

Azure Active Directory conditional access enables you to bring the protection of your cloud apps to a new level. In this new level, how you can access a cloud app is based on a dynamic policy evaluation instead of a static access configuration. With a conditional access policy, you define a response (**do this**) to an access condition (**when this happens**).

![Reason and response](./media/plan-conditional-access/10.png)

At a minimum, **when this happens** defines the principal (**who**) that attempts to access a cloud app (**what**). If required, you can also include **how** an access attempt is performed. In conditional access, the elements that define the who, what, and how are known as conditions. For more information, see [What are conditions in Azure Active Directory conditional access?](conditions.md) 

With **then do this**, you define the response of your policy to an access condition. In your response, you either block or grant access with additional requirements, for example, multi-factor authentication (MFA). For a complete overview, see [What are access controls in Azure Active Directory conditional access?](controls.md)  
 

The combination of conditions with your access controls represents a conditional access policy.

![Reason and response](./media/plan-conditional-access/51.png)


When planning your conditional access policies, use this model to draft your requirements:
    

|When *this* happens:|Then do *this*:|
|-|-|
|An access attempt is made:<br>- To a cloud app*<br>- By users and groups*<br>Using:<br>- Condition 1 (for example, outside Corp network)<br>- Condition 2 (for example, sign-in risk)|Block access to the application|
|An access attempt is made:<br>- To a cloud app*<br>- By users and groups*<br>Using:<br>- Condition 1 (for example, outside Corp network)<br>- Condition 2 (for example, sign-in risk)|Grant access with (AND):<br>- Requirement 1 (for example, MFA)<br>- Requirement 2 (for example, Device compliance)|
|An access attempt is made:<br>- To a cloud app*<br>- By users and groups*<br>Using:<br>- Condition 1 (for example, outside Corp network)<br>- Condition 2 (for example, sign-in risk)|Grant access with (OR):<br>- Requirement 1 (for example, MFA)<br>- Requirement 2 (for example, Device compliance)|

For more information, see [what's required to make a policy work](best-practices.md#whats-required-to-make-a-policy-work).

## Plan policies

When planning your solution, assess whether you need to create policies for the following considerations. 


### Block access

The option to block access is very powerful because it:

- Trumps all other assignments for a user

- Has the power to block your entire organization from signing on to your tenant
 
If you want to block access for all users, you should at least exclude one user from the policy. For more information, see [select users and groups](block-legacy-authentication.md#select-users-and-cloud-apps).  
    

### Require MFA

To simplify the sign-in experience of your users, you might want to allow them to sign in to your cloud apps using a user name and a password. However, typically, there are at least some scenarios for which it is advisable to require a stronger form of account verification. With a conditional access policy, you can limit the requirement for MFA to certain scenarios. 

Common use cases to require MFA are access:

- [By admins](baseline-protection.md#require-mfa-for-admins)
- [To specific apps](app-based-mfa.md) 
- [From network locations you don't trust](untrusted-networks.md).


### Compromised accounts

With conditional access polices, you can implement automated responses to sign-ins from potentially compromised identities. The probability that an account has been compromised is expressed in form of risk levels. There are two risk levels calculated by identity protection: sign-in risk and user risk. To implement a response to a sign-in risk, you have two options:

- [The sign-in risk condition](conditions.md#sign-in-risk) in conditional access policy
- [The sign-in risk policy](../identity-protection/howto-sign-in-risk-policy.md) in identity protection 

Because the sign-in risk condition provides you with more flexibility, use this condition if you need to implement a response to a sign-in risk level. 

The user risk level is only available as [user risk policy](../identity-protection/howto-user-risk-policy.md) in identity protection. 


### Managed devices

The proliferation of supported devices to access your cloud resources helps to improve the productivity of your users. On the flip side, you probably don't want certain resources in your environment to be accessed by devices with an unknown protection level. For the affected resources, you should require that users can only access them using a managed device. For more information, see [How to require managed devices for cloud app access with conditional access](require-managed-devices.md). 

### Approved client apps

One of the first decisions you need to make for bring your own devices (BYOD) scenarios, is whether you need to manage the entire device or just the data on it. Your employees use mobile devices for both personal and work tasks. While making sure your employees can be productive, you also want to prevent data loss. With Azure Active Directory (Azure AD) conditional access, you can restrict access to your cloud apps to approved client apps that can protect your corporate data. For more information, see [How to require approved client apps for cloud app access with conditional access](app-based-conditional-access.md).


### Legacy authentication

Azure AD supports several of the most widely used authentication and authorization protocols including legacy authentication. How can you prevent apps using legacy authentication from accessing your tenant's resources? The recommendation is to just block them with a conditional access policy. If necessary, you allow only certain users and specific network locations to use apps that are based on legacy authentication. For more information, see [How to block legacy authentication to Azure AD with conditional access](block-legacy-authentication.md).


## Test your policy

Before rolling out a policy into production, you should test is to verify that it behaves as expected.

1. Create test users

2. Create a test plan

3. Configure the policy

4. Evaluate a simulated sign-in

5. Test your policy

6. Cleanup



### Create test users

To test a policy, create a set of users representing users in your environment. 
(Can we beef this up a bit?)

### Create a test plan

The test plan is important to have a comparison between the expected results and the actual results. You should always have an expectation before testing something. The following table outlines example test cases. Adjust the scenarios and expected results based on how your CA policies are configured.

|Policy |Scenario |Expected Result | Result |
|---|---|---|---|
|[Require MFA when not at work](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)|Authorized user signs into *App* while on a trusted location / work|User isn't prompted to MFA| |
|[Require MFA when not at work](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)|Authorized user signs into *App* while not on a trusted location / work|User is prompted to MFA and can sign in successfully| |
|[Require MFA (for admin)](https://docs.microsoft.com/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins)|Global Admin signs into *App*|Admin is prompted to MFA| |
|[Risky Sign-Ins](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-sign-in-risk-policy)|User signs into *App* using a [Tor browser](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-playbook)|Admin is prompted to MFA| |
|[Device Management](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)|Authorized user attempts to sign in from an authorized device|Access Granted| |
|[Device Management](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)|Authorized user attempts to sign in from an unauthorized device|Access blocked| |
|[Password Change for risky users](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy)|Authorized user attempts to sign in with compromised credentials (high risk sign in)|User is prompted to change password or access is blocked based on your policy| |


### Configure the policy

To create your policy:

1. Sign in to your [Azure portal](https://portal.azure.com) as global administrator, security administrator, or a conditional access administrator.

2. Navigate to [Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview).

3. On the **Azure Active Directory** page, in the **Security** section, click **Conditional Access**.


4. On the **Conditional Access** page, in the toolbar on the top, click **New policy**.

5. Configure the test users, apps, conditions, and controls.

6. In the **Enable policy** section, click **On**.


![Picture 26](media/plan-conditional-access/azure-ad-ca-deployment-image1.png)


### Evaluate a simulated sign-in

Now that you have configured your conditional access policy, you probably want to know whether it works as expected. As a first step, use the conditional access [what if policy tool](what-if-tool.md) to simulate a sign-in of your test user. The simulation estimates the impact this sign-in has on your policies and generates a simulation report.

>[!NOTE]
> While a simulated run gives you impression of the impact a conditional access policy has, it does not replace an actual test run.


### Test your policy

Run test cases according to your test plan.

== Can we beef this up a bit?



### Cleanup

The cleanup procedure consists of the following steps:

1. Disable the policy.

2. Remove the assigned users and groups.

3. Delete the test users.  




## Move to production

When you are ready to deploy a new policy into your environment, you should do this in phases:

1. Apply a policy to a small set of users and verify it behaves as expected.

2. When you expand a policy to include more users, continue to exclude all administrators from the policy. This ensures that administrators still have access and can update a policy if a change is required.

3. Apply a policy to all users only if this is really required.

As a best practice, create at least one user account that is:

- Dedicated to policy administration

- Excluded from all your policies

 Provide internal change communication to end users.



## Rollback steps

Use the following options to roll back a Conditional Access policy

1. **Disable the policy** - Disabling a policy makes sure it doesn't apply when a user tries to sign in. You can always come back and enable the policy when you’d like to use it.

    ![Picture 27](media/plan-conditional-access/azure-ad-ca-deployment-image2.png)

2. **Exclude a user / group from a policy** - If a user is unable to access the app, you can choose to exclude the user from the policy

    >[!NOTE]
    > This option should be used sparingly, only in situations where the user is trusted. The user should be added back into the policy or group as soon as possible.

    ![Picture 30](media/plan-conditional-access/azure-ad-ca-deployment-image3.png)

3. **Delete the policy** - If the policy is no longer required, delete it.

## Next steps

* Conditional access [technical reference](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference)
