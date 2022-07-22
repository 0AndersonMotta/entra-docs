---
title: Use additional context in Microsoft Authenticator notifications (Preview) - Azure Active Directory
description: Learn how to use additional context in MFA notifications
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/22/2022
ms.author: justinha
author: mjsantani
ms.collection: M365-identity-device-management

# Customer intent: As an identity administrator, I want to encourage users to use the Microsoft Authenticator app in Azure AD to improve and secure user sign-in events.
---
# How to use additional context in Microsoft Authenticator notifications (Preview) - Authentication Methods Policy

This topic covers how to improve the security of user sign-in by adding the application name and geographic location of the sign-in to Microsoft Authenticator push and passwordless notifications.  

## Prerequisites

Your organization will need to enable Microsoft Authenticator push notifications for some users or groups using the new Authentication Methods Policy API.

>[!NOTE]
>Additional context can be targeted to only a single group, which can be dynamic or nested. On-premises synchronized security groups and cloud-only security groups are supported for the Authentication Method Policy.

## Passwordless phone sign-in and multifactor authentication 

When a user receives a passwordless phone sign-in or MFA push notification in the Authenticator app, they'll see the name of the application that requests the approval and the location based on the IP address where the sign-in originated from.

:::image type="content" border="false" source="./media/howto-authentication-passwordless-phone/location.png" alt-text="Screenshot of additional context in the MFA push notification.":::

The additional context can be combined with [number matching](how-to-mfa-number-match.md) to further improve sign-in security. 

:::image type="content" border="false" source="./media/howto-authentication-passwordless-phone/location-with-number-match.png" alt-text="Screenshot of additional context with number matching in the MFA push notification.":::

### Policy schema changes 

>[!NOTE]
>In Graph Explorer, ensure you've consented to the **Policy.Read.All** and **Policy.ReadWrite.AuthenticationMethod** permissions. 

You can enable and disable application name and geographic location separately. Under featureSettings, you can use the following mapping for the following features:

- Application name: displayAppInformationRequiredState
- Geographic location: displayLocationInformationRequiredState


Identify your single target group for each of the features. Then use the following API endpoint to change the displayAppInformationRequiredState or displayLocationInformationRequiredState properties under featureSettings to **enabled** and include or exclude the groups you want::

https://graph.microsoft.com/beta/authenticationMethodsPolicy/authenticationMethodConfigurations/MicrosoftAuthenticator

>[!NOTE]
>For Passwordless phone sign-in, the Authenticator app does not retrieve policy information just in time for each sign-in request. Instead, the Authenticator app does a best effort retrieval of the policy once every 7 days. We understand this limitation is less than ideal and are working to optimize the behavior. In the meantime, if you want to force a policy update to test using additional context with Passwordless phone sign-in, you can remove and re-add the account in the Authenticator app. 

#### MicrosoftAuthenticatorAuthenticationMethodConfiguration properties

**PROPERTIES**

| Property | Type | Description |
|---------|------|-------------|
| id | String | The authentication method policy identifier. |
| state | authenticationMethodState | Possible values are: **enabled**<br>**disabled** |
 
**RELATIONSHIPS**

| Relationship | Type | Description |
|--------------|------|-------------|
| includeTargets | [microsoftAuthenticatorAuthenticationMethodTarget](/graph/api/resources/passwordlessmicrosoftauthenticatorauthenticationmethodtarget) collection | A collection of users or groups who are enabled to use the authentication method. |
| featureSettings | [microsoftAuthenticatorFeatureSettings](/graph/api/resources/passwordlessmicrosoftauthenticatorauthenticationmethodtarget) collection | A collection of Microsoft Authenticator features. |
 
#### MicrosoftAuthenticator includeTarget properties
 
**PROPERTIES**

| Property | Type | Description |
|----------|------|-------------|
| authenticationMode | String | Possible values are:<br>**any**: Both passwordless phone sign-in and traditional second factor notifications are allowed.<br>**deviceBasedPush**: Only passwordless phone sign-in notifications are allowed.<br>**push**: Only traditional second factor push notifications are allowed. |
| id | String | Object ID of an Azure AD user or group. |
| targetType | authenticationMethodTargetType | Possible values are: **user**, **group**.<br>You can only set one group or user for additional context. |

#### MicrosoftAuthenticator featureSettings properties
 
**PROPERTIES**

| Property | Type | Description |
|----------|------|-------------|
| numberMatchingRequiredState | authenticationMethodFeatureConfiguration | Require number matching for MFA notifications. Value is ignored for phone sign-in notifications. |
| displayAppInformationRequiredState | authenticationMethodFeatureConfiguration | Determines whether the user is shown application name in Microsoft Authenticator notification. |
| displayLocationInformationRequiredState | authenticationMethodFeatureConfiguration | Determines whether the user is shown geographic location context in Microsoft Authenticator notification. |

#### Authentication Method Feature Configuration properties

**PROPERTIES**

| Property | Type | Description |
|----------|------|-------------|
| excludeTarget | featureTarget | A single entity that is excluded from this feature. |
| includeTarget | featureTarget | A single entity that is included in this feature. |
| State | advancedConfigState | Possible values are:<br>**enabled** explicitly enables the feature for the selected group.<br>**disabled** explicitly disables the feature for the selected group.<br>**default** allows Azure AD to manage whether the feature is enabled or not for the selected group. |

#### Feature Target properties

**PROPERTIES**

| Property | Type | Description |
|----------|------|-------------|
| id | String | ID of the entity targeted. |
| targetType | featureTargetType | The kind of entity targeted, such as group, role, or administrative unit. The possible values are: ‘group’, 'administrativeUnit’, ‘role’, unknownFutureValue’. |

#### Example of how to enable additional context for all users

In **featureSettings**, change **displayAppInformationRequiredState** and **displayLocationInformationRequiredState** from **default** to **enabled**. 

The value of Authentication Mode can be either **any** or **push**, depending on whether or not you also want to enable passwordless phone sign-in. In these examples, we'll use **any**, but if you do not want to allow passwordless, use **push**.

You might need to PATCH the entire schema to prevent overwriting any previous configuration. In that case, do a GET first, update only the relevant fields, and then PATCH. The following example shows how to update **displayAppInformationRequiredState** and **displayLocationInformationRequiredState** under **featureSettings**. 

Only users who are enabled for Microsoft Authenticator under Microsoft Authenticator’s **includeTargets** will see the application name or geographic location. Users who aren't enabled for Microsoft Authenticator won't see these features.

```json
//Retrieve your existing policy via a GET. 
//Leverage the Response body to create the Request body section. Then update the Request body similar to the Request body as shown below.
//Change the Query to PATCH and Run query
 
{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#authenticationMethodConfigurations/$entity",
    "@odata.type": "#microsoft.graph.microsoftAuthenticatorAuthenticationMethodConfiguration",
    "id": "MicrosoftAuthenticator",
    "state": "enabled",
    "featureSettings": {
        "displayAppInformationRequiredState": {
            "state": "enabled",
            "includeTarget": {
                "targetType": "group",
                "id": "all_users"
            },
            "excludeTarget": {
                "targetType": "group",
                "id": "00000000-0000-0000-0000-000000000000"
            }
        },
        "displayLocationInformationRequiredState": {
            "state": "enabled",
            "includeTarget": {
                "targetType": "group",
                "id": "all_users"
            },
            "excludeTarget": {
                "targetType": "group",
                "id": "00000000-0000-0000-0000-000000000000"
            }
        }
    },
    "includeTargets@odata.context": "https://graph.microsoft.com/beta/$metadata#authenticationMethodsPolicy/authenticationMethodConfigurations('MicrosoftAuthenticator')/microsoft.graph.microsoftAuthenticatorAuthenticationMethodConfiguration/includeTargets",
    "includeTargets": [
        {
            "targetType": "group",
            "id": "all_users",
            "isRegistrationRequired": false,
            "authenticationMode": "any",
        }
    ]
} 
```
 
 
#### Example of how to enable application name and geographic location for separate groups
 
In **featureSettings**, change **displayAppInformationRequiredState** and **displayLocationInformationRequiredState** from **default** to **enabled.** 
Inside the **includeTarget** for each featureSetting, change the **id** from **all_users** to the ObjectID of the group from the Azure AD portal.

You need to PATCH the entire schema to prevent overwriting any previous configuration. We recommend that you do a GET first, and then update only the relevant fields and then PATCH. The following example shows an update to **displayAppInformationRequiredState** and **displayLocationInformationRequiredState** under **featureSettings**. 

Only users who are enabled for Microsoft Authenticator under Microsoft Authenticator’s **includeTargets** will see the application name or geographic location. Users who aren't enabled for Microsoft Authenticator won't see these features.


```json
{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#authenticationMethodConfigurations/$entity",
    "@odata.type": "#microsoft.graph.microsoftAuthenticatorAuthenticationMethodConfiguration",
    "id": "MicrosoftAuthenticator",
    "state": "enabled",
    "featureSettings": {
        "displayAppInformationRequiredState": {
            "state": "enabled",
            "includeTarget": {
                "targetType": "group",
                "id": "44561710-f0cb-4ac9-ab9c-e6c394370823"
            },
            "excludeTarget": {
                "targetType": "group",
                "id": "00000000-0000-0000-0000-000000000000"
            }
        },
        "displayLocationInformationRequiredState": {
            "state": "enabled",
            "includeTarget": {
                "targetType": "group",
                "id": "a229e768-961a-4401-aadb-11d836885c11"
            },
            "excludeTarget": {
                "targetType": "group",
                "id": "00000000-0000-0000-0000-000000000000"
            }
        }
    },
    "includeTargets@odata.context": "https://graph.microsoft.com/beta/$metadata#authenticationMethodsPolicy/authenticationMethodConfigurations('MicrosoftAuthenticator')/microsoft.graph.microsoftAuthenticatorAuthenticationMethodConfiguration/includeTargets",
    "includeTargets": [
        {
            "targetType": "group",
            "id": "all_users",
            "isRegistrationRequired": false,
            "authenticationMode": "any",
        }
    ]
}
```
 
To verify, RUN GET again and verify the ObjectID
GET https://graph.microsoft.com/beta/authenticationMethodsPolicy/authenticationMethodConfigurations/MicrosoftAuthenticator
 

#### Example of error when enabling additional context for multiple groups

The PATCH request will fail with 400 Bad Request and the error will contain the following message: 

`Persistance of policy failed with error: You cannot enable multiple targets for feature 'Require Display App Information'. Choose only one of the following includeTargets to enable: aede0efe-c1b4-40dc-8ae7-2c402f23e312,aede0efe-c1b4-40dc-8ae7-2c402f23e317.`

### Turn off additional context

To turn off additional context, you'll need to PATCH **displayAppInformationRequiredState** and **displayLocationInformationRequiredState** from **enabled** to **disabled**/**default**.

```json
{
    "@odata.context": "https://graph.microsoft.com/beta/$metadata#authenticationMethodConfigurations/$entity",
    "@odata.type": "#microsoft.graph.microsoftAuthenticatorAuthenticationMethodConfiguration",
    "id": "MicrosoftAuthenticator",
    "state": "enabled",
    "featureSettings": {
        "displayAppInformationRequiredState": {
            "state": "disabled",
            "includeTarget": {
                "targetType": "group",
                "id": "44561710-f0cb-4ac9-ab9c-e6c394370823"
            },
            "excludeTarget": {
                "targetType": "group",
                "id": "00000000-0000-0000-0000-000000000000"
            }
        },
        "displayLocationInformationRequiredState": {
            "state": "disabled",
            "includeTarget": {
                "targetType": "group",
                "id": "a229e768-961a-4401-aadb-11d836885c11"
            },
            "excludeTarget": {
                "targetType": "group",
                "id": "00000000-0000-0000-0000-000000000000"
            }
        }
    },
    "includeTargets@odata.context": "https://graph.microsoft.com/beta/$metadata#authenticationMethodsPolicy/authenticationMethodConfigurations('MicrosoftAuthenticator')/microsoft.graph.microsoftAuthenticatorAuthenticationMethodConfiguration/includeTargets",
    "includeTargets": [
        {
            "targetType": "group",
            "id": "all_users",
            "isRegistrationRequired": false,
            "authenticationMode": "any",
        }
    ]
}
```

## Enable additional context in the portal

To enable additional context in the Azure AD portal, complete the following steps:

1. In the Azure AD portal, click **Security** > **Authentication methods** > **Microsoft Authenticator**.
1. On the **Basics** tab, click **Yes** and **All users** to enable the policy for everyone, and change **Authentication mode** to **Any**. Only users who are enabled for Microsoft Authenticator will see additional context. Anyone who isn't enabled for Microsoft Authenticator is unaffected.

   :::image type="content" border="true" source="./media/how-to-mfa-additional-context/enable-settings-additional-context.png" alt-text="Screenshot of how to enable Microsoft Authenticator settings for Any authentication mode.":::

1. On the **Configure** tab, for **Show application name in push and passwordless notifications (Preview)** and **Show geographic location in push and passwordless notifications (Preview)**, change **Status** to **Enabled**, choose who to include or exclude from the policy, and click **Save**.

   :::image type="content" border="true" source="./media/how-to-mfa-additional-context/additional-context.png" alt-text="Screenshot of how to enable additional context.":::

## Known issues

Additional context is not supported for Network Policy Server (NPS). 

## Next steps

[Authentication methods in Azure Active Directory - Microsoft Authenticator app](concept-authentication-authenticator-app.md)

