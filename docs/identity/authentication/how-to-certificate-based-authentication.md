---
title: Tutorial to configure cloud-native certificate-based authentication without federation (Preview) - Azure Active Directory
description: Tutorial that shows how to configure cloud-native certificate-based authentication in Azure Active Directory

services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 01/21/2022

ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: tommma

ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
---
# Tutorial: Configure cloud-native certificate-based authentication in Azure Active Directory (Preview)

Cloud-native certificate-based authentication (CBA) enables customers to configure their Azure AD tenants to allow or require users to authenticate with X.509 certificates verified against their Enterprise Public Key Infrastructure (PKI) for app and browser sign-in. This feature enables customers to adopt passwordless and authenticate with an x.509 certificate. 
 
During sign-in, users will see an option to authenticate with a certificate instead of entering a password. 
If multiple matching certificates are present on the device, the user can pick which one to use. The certificate is validated, the binding to the user account is checked, and if successful, they are signed in.

<!---Clarify plans that are covered --->
This topic covers how to configure and use certificate-based authentication for tenants in Office 365 Enterprise, US Government plans. You should already have a [public key infrastructure (PKI)](https://aka.ms/securingpki) configured.

Follow these instructions to configure and use cloud-native certificate-based authentication (CBA) for tenants in Azure Active Directory.

>[!NOTE]
>Cloud-native certificate-based authentication is currently in public preview. Some features might not be supported or have limited capabilities. For more information about previews, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

## Prerequisites

Make sure that the following prerequisites are in place.

- Configure at least one certificate authority (CA) and any intermediate certificate authorities in Azure Active Directory.
- The user must have access to a user certificate (issued from a trusted Public Key Infrastructure configured on the tenant) intended for client authentication to authenticate against Azure AD. 

>[!IMPORTANT]
>Each CA should have a certificate revocation list (CRL) that can be referenced from internet-facing URLs. If the trusted CA does not have a CRL configured, Azure AD will not perform any CRL checking, revocation of user certificates will not work, and authentication will not be blocked.

## Steps to configure and test cloud-native CBA

There are some configuration steps to complete before enabling cloud-native CBA. First, an admin must configure the trusted CAs that issue user certificates. As seen in the following diagram, we use role-based access control to make sure only least-privileged administrators make changes. Configuring the certificate authority is done only by the [Privileged Authentication Administrator](../roles/permissions-reference.md#privileged-authentication-administrator) role.

Optionally, you can also configure authentication bindings to map certificates to single-factor or multifactor and configure username bindings to map certificate field to a user object attribute. Configuring user-related settings can be done by [Authentication Policy Administrators](../roles/permissions-reference.md#authentication-policy-administrator). Once all the configurations are complete, enable cloud-native CBA on the tenant. 

:::image type="content" border="false" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/steps.png" alt-text="steps to enable cloud-native certificate-based authentication works in Azure AD.":::

## Step 1: Configure the certificate authorities

To configure your certificate authorities in Azure Active Directory, follow the steps to [Configure Certificate Authorities](active-directory-certificate-based-authentication-get-started.md#step-2-configure-the-certificate-authorities).

>[!NOTE]
>Only one Certificate Distribution Point (CDP) for a trusted CA is supported.
>The CDP can be only HTTP URLs. We do not support Online Certificate Status Protocol (OCSP) or Lightweight Directory Access Protocol (LDAP) URLs.

## Step 2: Configure authentication binding policy 

The authentication binding policy helps determine the strength of authentication to either a single factor or multi factor. An admin can change the default value from single-factor to multifactor and configure custom policy rules by mapping to issuer Subject or policy OID fields in the certificate.

To enable the certificate-based authentication and configure user bindings in the Azure portal, complete the following steps:

1. Sign in to the [Azure portal](https://portal.azure.com) as an Authentication Policy Administrator.
1. Select **Azure Active Directory**, then choose **Security** from the menu on the left-hand side.
1. Click **Authentication methods** > **Policies**.
1. Under **Manage**, select **Authentication methods** > **Certificate-based Authentication**.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/policy.png" alt-text="Screenshot of Authentication policy.":::


1. Click **Configure** to set up authentication binding and username binding.
1. The protection level attribute has a default value of **Single-factor authentication**. Select **Multi-factor authentication** to change the default value to MFA. 

   >[!NOTE] 
   >The default protection level value will be in effect if no custom rules are added. If custom rules are added, the protection level defined at the rule level will be honored instead.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/change-default.png" alt-text="Screenshot of how to change the default policy to MFA.":::

1. You can also set up custom authentication binding rules to help determine the protection level for client certificates. It can be configured using either the issuer Subject or Policy OID fields in the certificate.

   Authentication binding rules will map the certificate attributes (issuer or Policy OID) to a value, and select default protection level for that rule. Multiple rules can be created.

   To add custom rules, click on **Add rule**.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/add-rule.png" alt-text="Screenshot of how to add a rule.":::

   To create a rule by certificate issuer, click **Certificate issuer**.

   1. Select a **Certificate issuer identifier** from the list box.
   1. Click **Multi-factor authentication**.

      :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/multifactor-issuer.png" alt-text="Screenshot of multifactor authentication policy.":::


   To create a rule by Policy OID, click **Policy OID**.

   1. Enter a value for **Policy OID**.
   1. Click **Multi-factor authentication**.

      :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/multifactor-policy-oid.png" alt-text="Screenshot of mapping to Policy OID.":::

   Click **Ok** to save any custom rule.

## Step 3: Configure username binding policy

The username binding policy helps determine the user in the tenant. By default, we map Principal Name in the certificate to onPremisesUserPrincipalName in the user object to determine the user.

An admin can override the default and create a custom mapping. Currently, we support two certificate fields, SAN (Subject Alternate Name) Principal Name and SAN RFC822Name, to map against the user object attribute userPrincipalName and onPremisesUserPrincipalName.

1. Create the username binding by selecting one of the X.509 certificate fields to bind with one of the user attributes. The username binding order represents the priority level of the binding. The first one has the highest priority and so on.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/username-binding-policy.png" alt-text="Screenshot of a username binding policy.":::

   If the specified X.509 certificate field is found on the certificate, but Azure AD doesn’t find a user object using that value, the authentication fails. Azure AD doesn’t try the next binding in the list.

   The next priority is attempted only if the X.509 certificate field is not in the certificate.

1. Click **Save** to save the changes. 

Currently supported set of username bindings:

- SAN Principal Name > userPrincipalName
- SAN Principal Name > onPremisesUserPrincipalName
- SAN RFC822Name > userPrincipalName
- SAN RFC822Name > onPremisesUserPrincipalName

>[!NOTE]
>If the RFC822Name binding is evaluated and if no RFC822Name is specified in the certificate Subject Alternative Name, we will fall back on legacy Subject Name "E=user@contoso.com" if no RFC822Name is specified in the certificate we will fall back on legacy Subject Name E=user@contoso.com.

The final configuration will look like this image:

:::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/final.png" alt-text="Screenshot of the final configuration.":::

## Step 4: Enable CBA on the tenant

To enable the certificate-based authentication in the Azure MyApps portal, complete the following steps:

1. Sign in to the [MyApps portal](https://myapps.microsoft.com/) as an Authentication Policy Administrator.
1. Select **Azure Active Directory**, then choose **Security** from the menu on the left-hand side.
1. Under **Manage**, select **Authentication methods** > **Certificate-based Authentication**.
4.	Under **Basics**, select **Yes** to enable CBA.
1. CBA can be enabled for a targeted set of users.
   1. Click **All users** to enable all users.
   1. Click **Select users** to enable selected users or groups. 
   1. Click **+ Add users**, select specific users and groups.
   1. Click **Select** to add them.

1. Select **Sign in with a certificate**.
1. Pick the correct user certificate in the client certificate picker UI and click **OK**.
 
Once certificate-based authentication is enabled on the tenant, all users in the tenant will see the option to sign in with a certificate. Only users who are enabled for certificate-based authentication will be able to authenticate using the X.509 certificate. 

## Step 5: Test your configuration

This section covers how to test your certificate and custom authentication binding rules.

### Testing your certificate

As a first configuration test, you should try to sign in to the [MyApps portal](https://myapps.microsoft.com/) using your on-device browser.

1. Enter your User Principal Name (UPN).

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/name.png" alt-text="Screenshot of the User Principal Name.":::

1. Click **Next**.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/certificate.png" alt-text="Screenshot of sign in with certificate.":::

   If you have enabled other authentication methods like Phone sign-in or FIDO2, users may see a different sign-in screen.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/alternative.png" alt-text="Screenshot of the alternative sign in.":::

1. Select **Sign in with a certificate**.

1.	Pick the correct user certificate in the client certificate picker UI and click **Ok**.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/picker.png" alt-text="Screenshot of the certificate picker UI.":::

1. Users should be signed into [MyApps portal](https://myapps.microsoft.com/). 

If your sign in is successful, then you know that:

- The user certificate has been provisioned into your test device.
- Azure Active Directory is configured correctly with trusted CAs.
- Username binding is configured correctly, and the user is found and authenticated.

### Testing custom authentication binding rules

Let's walk through a scenario where we will validate strong authentication by creating two authentication policy rules, one via issuer subject satisfying single factor and one via policy OID satisfying multi factor. 

1. Create an issuer Subject rule with protection level as single factor authentication and value set to your CAs Subject value. For example: 

   `CN=ContosoCA,DC=Contoso,DC=org`

1. Create a policy OID rule, with protection level as multi-factor authentication and value set to one of the policy OID’s in your certificate. For example, 1.2.3.4.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/policy-oid-rule.png" alt-text="Screenshot of the Policy OID rule.":::

1. Create a conditional access policy for the user to require multi-factor authentication by following steps at [Conditional Access - Require MFA](../conditional-access/howto-conditional-access-policy-all-users-mfa.md#create-a-conditional-access-policy).
1. Navigate to [MyApps portal](https://myapps.microsoft.com/). Enter your UPN and click **Next**.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/name.png" alt-text="Screenshot of the User Principal Name.":::

1. Select **Sign in with a certificate**.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/certificate.png" alt-text="Screenshot of sign in with certificate.":::

   If you have enabled other authentication methods like Phone sign-in or FIDO2, users may see a different sign-in screen.

   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/alternative.png" alt-text="Screenshot of the alternative sign in.":::

1.	Select the client certificate and click **Certificate Information**. 
   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/client-picker.png" alt-text="Screenshot of the client picker.":::
   The certificate will be shown, and you can verify the issuer and policy OID values. 
   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/issuer.png" alt-text="Screenshot of the issuer.":::

1. To see Policy OID values, click **Details**.
   :::image type="content" border="true" source="./media/tutorial-enable-cloud-native-certificate-based-authentication/authentication-details.png" alt-text="Screenshot of the authentication details.":::

1. Select the client certificate and click **Ok**.
1. The policy OID in the certificate matches the configured value of **1.2.3.4** and it will satisfy multifactor authentication. Similarly, the issuer in the certificate matches the configured value of **CN=ContosoCA,DC=Contoso,DC=org** and it will satisfy single-factor authentication.
1. Because policy OID rule takes precedence over issuer rule, the certificate will satisfy multifactor authentication.
1. The conditional access policy for the user requires MFA and the certificate satisfies multifactor, so the user will be authenticated into the application.

### Enable cloud-native CBA using Microsoft Graph API

To enable the certificate-based authentication and configure username bindings using Graph API, complete the following steps.

>[!NOTE]
>The following steps will not work in US Government. US Government tenants with MS Graph will need to use the postman tool.

1. Go to [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer).
1. Click **Sign into Graph Explorer** and sign in to your tenant.
1. Click **Settings** > **Select permission**.
1. Enter "auth" in the search bar and consent all related permissions.
1. GET all authentication methods

   ```http
   GET  https://graph.microsoft.com/beta/policies/authenticationmethodspolicy
   ```

1. GET X509Certificate authentication method

   ```http
   GET https://graph.microsoft.com/beta/policies/authenticationmethodspolicy/authenticationMetHodConfigurations/X509Certificate
   ```

1. To update policy run a PATCH command.

   PATCH X509Certificate strong auth with sample authentication rules with certificate user bindings and certificate rules.
    
    Request body:


   ```http
   {
"@odata.context": https://graph.microsoft-ppe.com/testppebetatestx509certificatestrongauth/$metadata#authenticationMethodConfigurations/$entity,
      "@odata.type": "#microsoft.graph.x509CertificateAuthenticationMethodConfiguration",
      "id": "X509Certificate",
      "state": "disabled",
      "certificateUserBindings": [{
                               "x509CertificateField": "PrincipalName",
                               "userProperty": "onPremisesUserPrincipalName",
                               "priority": 1
                  },
                  {
                               "x509CertificateField": "RFC822Name",
                               "userProperty": "userPrincipalName",
                               "priority": 2
                  }
      ],
      "authenticationModeConfiguration": {
                  "x509CertificateAuthenticationDefaultMode": "x509CertificateSingleFactor",
                  "rules": [{
                                            "x509CertificateRuleType": "issuerSubject",
                                            "identifier": "CN=ContosoCA,DC=Contoso,DC=org ",
                                     "x509CertificateAuthenticationMode": "x509CertificateMultiFactor"
                               },
                               {
                                            "x509CertificateRuleType": "policyOID",
                                            "identifier": "1.2.3.4",
                                     "x509CertificateAuthenticationMode": "x509CertificateMultiFactor"
                               }
                  ]
      },
      includeTargets@odata.context: https://graph.microsoft-ppe.com/testppebetatestx509certificatestrongauth/$metadata#policies/authenticationMethodsPolicy/authenticationMethodConfigurations('X509Certificate')/microsoft.graph.x509CertificateAuthenticationMethodConfiguration/includeTargets,
      "includeTargets": [{
                  "targetType": "group",
                  "id": "all_users",
                  "isRegistrationRequired": false
      }]
}

   ```

   You will get a 204 response and re-run the GET command to make sure the policies are updated correctly.

1. Test the configuration by signing in with a certificate that satisfies the policy.
 

## Next steps 

- [Overview of cloud-native certificate-based authentication](concept-cloud-native-certificate-based-authentication.md)
- [Technical deep dive for cloud-native certificate-based authentication](concept-cloud-native-certificate-based-authentication-technical-deep-dive.md)   
- [Limitations with cloud-native certificate-based authentication](concept-cloud-native-certificate-based-authentication-limitations.md)
- [FAQ](cloud-native-certificate-based-authentication-faq.yml)
- [Troubleshoot cloud-native certificate-based authentication](troubleshoot-cloud-native-certificate-based-authentication.md)

