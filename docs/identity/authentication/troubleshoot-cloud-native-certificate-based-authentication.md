---
title: Troubleshoot cloud native certificate-based authentication without federation (Preview) - Azure Active Directory
description: Learn how to troubleshoot cloud native certificate-based authentication in Azure Active Directory

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
# Troubleshoot cloud native certificate-based authentication in Azure Active Directory (Preview)

This topic covers how to troubleshoot cloud native certificate-based authentication (CBA) in Azure Active Directory.

>[!NOTE]
>Cloud-native certificate-based authentication is currently in public preview. Some features might not be supported or have limited capabilities. For more information about previews, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

## Why don't I see an option to sign in using certificates against Azure Active Directory after I enter my username?

An administrator needs to enable CBA for the tenant to make the sign in with certificate option available for users. Link to getting started doc step 2

## User-facing sign-in error messages

If the user is unable to sign in using Certificate-based Authentication, they may see one of the following user-facing errors on the Azure AD sign-in screen.

**ADSTS1001000 - Unable to acquire certificate policy from tenant**

This is a server-side error that occurs when the server could not fetch an authentication policy for the user using the SAN Principal Name/SAN RFC822Name field of the user certificate. Make sure that the authentication policy rules are correct, a valid certificate is used, and retry. 

**AADSTS1001003 – User sign-in fails with "Unable To Acquire Value Specified In Binding From Certificate"**

This error is returned if the user selects the wrong user certificate from the list while signing in.

Make sure the certificate is valid and works for the user binding and authentication policy configuration.

**AADSTS50034 - Users sign in fails with “Your account or password is incorrect. If you don't remember your password, reset it now.**

Make sure the user is trying to sign in with the correct username. This error happens when a unique user can't be found in the certificate fields. 

- Make sure user bindings are set correctly and the certificate field is mapped to the correct user Attribute.
- Make sure the user Attribute contains the correct value that matches the certificate field value.

Link to Getting started configure user binding

If the user is a federated user moving to Azure AD and if the user binding configuration is Principal Name > onPremisesUserPrincipalName:

- Make sure the onPremisesUserPrincipalName is being synchronized, and ALT IDs are enabled in Azure AD Connect. 
- Make sure the value of onPremisesUserPrincipalName is correct and synchronized in Azure AD Connect.

>[!NOTE]
>There is a known issue that this scenario is not logged into the sign-in logs.

**AADSTS130501 - Users sign in fails with "Sign in was blocked due to User Credential Policy"**

This error happens when the target user is not in scope for the Certificate-based authentication. Make sure the user is listed in the **target** attribute of CBA.

Link to Getting started configuring enable CBA (step 4)

## User sign-in failed but not much diagnostic information

There is a known issue when the authentication sometimes fails, the failure screen may not have an error message or troubleshooting information.

For example, if a user certificate is revoked and is part of a Certificate Revocation List, then authentication fails correctly. However, instead of the error message, you might see the following screen:

:::image type="content" border="false" source="./media/troubleshoot-cloud-native-certificate-based-authentication/failed.png" alt-text="Error message for cloud-native certificate-based authentication works in Azure AD.":::

To get more diagnostic information, look in **Sign-in logs**. If a user authentication fails due to CRL validation for example, sign-in logs show the error information correctly.

:::image type="content" border="true" source="./media/troubleshoot-cloud-native-certificate-based-authentication/details.png" alt-text="Screenshot of Authentication Details." lightbox="./media/troubleshoot-cloud-native-certificate-based-authentication/details.png":::

## Why didn't my changes to authentication policy changes take effect?

The authentication policy is cached. After a policy update, it may take up to an hour for the changes to be effective. Try after an hour to make sure the policy caching is not the cause.

## I see a valid Certificate Revocation List (CRL) endpoint set, but why don't I see any CRL revocation?

- Make sure the CRL distribution point is set to a valid HTTP URL.
- Make sure the CRL distribution point is accessible via an internet-facing URL.
- Make sure the CRL sizes are within the limit for public preview. For more information about the maximum CRL size, see [What is the maximum size for downloading a CRL?](cloud-native-certificate-based-authentication-faq.yml#is-there-a-limit-for-crl-size-).

## Next steps 

- [Overview of cloud native CBA](concept-cloud-native-certificate-based-authentication.md)
- [Technical deep dive for cloud-native CBA](concept-cloud-native-certificate-based-authentication-technical-deep-dive.md)   
- [Limitations with cloud native CBA](concept-cloud-native-certificate-based-authentication-limitations.md)
- [How to configure cloud native CBA](how-to-certificate-based-authentication.md)
- [FAQ](cloud-native-certificate-based-authentication-faq.yml)


