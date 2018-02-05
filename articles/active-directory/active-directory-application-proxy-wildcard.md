---
title: How to provide secure remote access to on-premises apps
description: Covers how to use Azure AD Application Proxy to provide secure remote access to your on-premises apps.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman

ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro

---

# Wildcard application publishing

Configuring a large number of applications can quickly become unmanageable and introduces unnecessary risks for configuration errors if many of them require the same settings. With [Azure AD Application Proxy](active-directory-application-proxy-get-started.md), you can address this issue by using wildcard application publishing to publish and manage many applications at once. This is a solution that allows you to:

-	Simplify your administrative overhead
-	Reduce the number of configuration errors
-	Enable your users to securely access more resources

This article provides you with the information you need to configure wildcard application publishing in your environment.



## Prerequisites

To use wildcards, you need the following: 

- Custom domains
- CNAME entry 

### Custom domains

You need to configure [custom domains](active-directory-application-proxy-custom-domains.md). Creating custom domains requires you to:

1. Create a verified domain within Azure 
2. Upload an SSL certificate in the PFX format to your application proxy.

You should consider using a wildcard certificate to match the application you plan to create. Alternatively, you can also use a certificate that only lists specific applications.

For security reasons, this is a hard requirement and we will not support wildcards for applications that cannot use a custom domain for the external URL.


### CNAME entry

When using custom domains, you need to create a DNS entry with a CNAME record pointing the external URL of the application proxy endpoint. For wildcard applications, the CNAME record needs to point to the relevant external URLs:

> `<yourAADTenantId>.tenant.runtime.msappproxy.net`

To confirm that you have configured your CNAME correctly, you can use [nslookup](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/nslookup) on one of the target endpoints, for example, `expenses.adventure-works.com`.  Your response should include the already mentioned alias (`<Id.tenant>.runtime.msappproxy.net`).


## Publishing the Application 

Creating a wildcard application is based on the same [application publishing flow](application-proxy-publish-azure-portal.md) that is available for all other applications. The only differences are in the URLs and potentially in the SSO configuration.

You can publish applications with wildcards if both, the internal and external URLs are in the following format:

> http(s)://*.\<domain\>

For example: `http(s)://*.adventure-works.com`. 

The internal and external URLs must both have a wildcard, otherwise you see an error.

**Remarks**:

- All wildcard applications must have the same configurations – including the users who can access them and the SSO method. You must publish exceptions as separate applications to overwrite the defaults set for the wildcard.

- For applications using a [KCD as the SSO method](active-directory-application-proxy-sso-using-kcd.md), the SPN listed for the SSO method may also need a wildcard. For example, the SPN could be: `HTTP/*.adventure-works.com`. You still need to have the individual SPNs configured on your backend servers (for example, `http://expenses.adventure-works.com and HTTP/travel.adventure-works.com`).

## What you should know

### Accepted formats

Your **Internal URL** must be formatted as `http(s)://*.<domain>`. 

![AppId](./media/active-directory-application-proxy-wildcard\22.png)


When you configure an **External URL**, you must use the following format: `https://*.<custom domain>` 

![AppId](./media/active-directory-application-proxy-wildcard\21.png)

Other positions of the wildcard, multiple wildcards, or other regex strings are not supported and are causing errors.

### Setting the homepage URL for the MyApps panel

The wildcard application is represented with just one tile in the [MyApps panel](https://myapps.microsoft.com). By default this tile is hidden. To show the tile and have users land on a specific page:

1. Follow the guidelines for [setting a homepage URL](application-proxy-office365-app-launcher.md).
2. Set **Show Application** to **true** on the application properties page.

### Excluding applications from the wildcard

To exclude applications from the wildcard application, you need to to publish them as a separate applications. As a best practice, you should publish the excluded applications before the wildcard applications to ensure that your exceptions are enforced from the beginning.

You can also limit the wildcard to only work for specific applications through your DNS management. As a best practice, you should create a CNAME entry that includes a wildcard and matches the format of the external URL you have configured. However, you can also point specific application URLs to the wildcards. For example, instead of `*.adventure-works.com`, point `hr.adventure-works.com`, `expenses.adventure-works.com` and `travel.adventure-works.com individually` to `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net`. 

If you use this option, you also need another CNAME entry for the value `AppId.domain`, for example, `00000000-1a11-22b2-c333-444d4d4dd444.adventure-works.com`, also pointing to the same location. You can find the **AppId** on the application properties page:

![AppId](./media/active-directory-application-proxy-wildcard\01.png)


## Scenario 1: General wildcard application

In this scenario, you have three different applications you want to publish:

- `expenses.adventure-works.com`
- `hr.adventure-works.com`
- `travel.adventure-works.com`

All three applications:

- Are used by all your users
- Use *Integrated Windows Authentication* 
- Have the same properties

You can implement this scenario using a wildcard application. 
In this example, your tenant ID is: `000aa000-11b1-2ccc-d333-4444eee4444e` 

**Step 1**: As a first step, you net to create the `adventure-works.com` verified domain in your tenant.

**Step 2**: In your DNS, you need to create a **CNAME** entry that points `*.adventure-works.com` to `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net`.

**Step 3**: Create application proxy 

Following the documented steps, you create a new application proxy application in your tenant. In this example, the wildcard is in the following fields:

- Internal URL:

    ![AppId](./media/active-directory-application-proxy-wildcard\42.png)


- External URL:

    ![AppId](./media/active-directory-application-proxy-wildcard\43.png)

 
- Internal Application SPN: 

    ![AppId](./media/active-directory-application-proxy-wildcard\44.png)


By publishing the wildcard application, you can now access your three applications by navigating to the URLs you are used to (for example, `travel.adventure-works.com`).

The configuration implements the following structure:

![AppId](./media/active-directory-application-proxy-wildcard\05.png)

| Color | Description |
| ---   | ---         |
| Blue  | Applications explicitly published and visible in the Azure Portal. |
| Gray  | Applications you can accessible through the parent application. |




## Scenario 2: Specific applications

In this scenario, you have in addition to the three general applications another application, `finance.adventure-works.com`, which should only be accessible by Finance division. With the current application structure, your finance application would be accessible through the wildcard application and by all employees. To change this, you publish Finance as a separate application with more restrictive permissions.

**Step 1**: As a first step, you net to create the `finance.adventure-works.com` verified domain in your tenant.

**Step 2**: In your DNS, you need to create a **CNAME** entry that points `finance.adventure-works.com` to the application specific endpoint you have specified in the application proxy configuration. 

**Step 3**: Create application proxy 


- In the **Internal URL**, you set set **finance** instead of a wildcard. 

    ![AppId](./media/active-directory-application-proxy-wildcard\52.png)

- In the **External URL**, you set set **finance** instead of a wildcard. 

    ![AppId](./media/active-directory-application-proxy-wildcard\53.png)

- Internal Application SPN you set set **finance** instead of a wildcard.

    ![AppId](./media/active-directory-application-proxy-wildcard\54.png)


**Step 4**: Complete the steps of the general wildcard application scenario

This configuration implements the following scenario:

![AppId](./media/active-directory-application-proxy-wildcard\09.png)

Because `finance.adventure-works.com` is a more specific URL than `*.adventure-works.com`, it takes precedence. Users navigating to `finance.adventure-works.com` will thus have the experience specified in the Finance Resources application. In this case, only finance employees are able to access `finance.adventure-works.com`.

If you have multiple applications published for finance and you have `finance.adventure-works.com` as a verified domain, you could publish another wildcard application `*.finance.adventure-works.com`. Because this is more specific than the generic `*.adventure-works.com`, it takes precedence if a user accesses an application in the finance domain.

## Scenario 3: Mixed appications

In this example, you have several applications that don’t follow the `*.adventure-works.com` format, have different SSO methods, and / or don’t match a verified domain in your tenant. You can still publish these applications separately using the same flow. In this case, you want to publish the applications to your `awcycleshome.com` website:

- In the **Internal URL**, you set set **finance** instead of a wildcard. 

    ![AppId](./media/active-directory-application-proxy-wildcard\62.png)

- In the **External URL**, you set set **finance** instead of a wildcard. 

    ![AppId](./media/active-directory-application-proxy-wildcard\63.png)

- Internal Application SPN you set set **finance** instead of a wildcard.

    ![AppId](./media/active-directory-application-proxy-wildcard\12.png)


This configuration implements the following scenario:


![AppId](./media/active-directory-application-proxy-wildcard\13.png)


### What you should know

#### Accepted formats

The only acceptable external URL format is to have a wildcard (*) in the empty text box, and a custom domain. The internal URL must similarly be formatted as http(s)://*.domain . Other positions for the wildcard, multiple wildcards, or other regexs are not supported and will cause errors.

#### Setting the homepage URL for the MyApps panel

The wildcard application is represented with just one tile in the MyApps panel (`https://myapps.microsoft.com`). By default, this tile is hidden. To show the tile and have users land on a specific page, please first follow the guidelines for setting a homepage URL, and then set the **Show Application** toggle to **true** on the application properties page.

#### Excluding applications from the wildcard

You can exclude applications from the wildcard application by publishing them as a separate application. As a best practice, you should publish the exceptions before the wildcard application, to ensure that your exceptions are enforced from the beginning.

You can also limit the wildcard to only work for specific applications through your DNS management. As a best practice, you should create a CNAME entry that includes a wildcard and matches the format of the external URL you configured. However, you can instead point specific application URLs to the wildcards. For exampl, instead of having `*.adventure-works.com` point to hr.adventure-works.com, expenses.adventure-works.com and travel.adventure-works.com individually to 000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net). If you use this option, you will also need another CNAME entry for the value AppId.domain (ex: 00000000-1a11-22b2-c333-444d4d4dd444.adventure-works.com ) also pointing to the same location. The AppId can be found in the application properties page: