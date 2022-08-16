---
title: Add branding to your organization's sign-in page (preview) - Azure AD
description: Instructions about how to add your organization's branding to the sign-in experience.
services: active-directory
author: barclayn
manager: rkarlin

ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 07/03/2021
ms.author: barclayn
ms.reviewer: kexia
ms.custom: "it-pro, seodec18, fasttrack-edit"
ms.collection: M365-identity-device-management
---

# Configure your company branding

Create a consistent experience when users sign into your organization's web-based apps that use Azure Active Directory (Azure AD) as your identity provider, such as Microsoft 365. The sign-in process can include your company logo and customized experiences based on browser language.

## License requirements

Adding custom branding requires one of the following licenses:

- Azure AD Premium 1
- Azure AD Premium 2
- Office 365 (for Office apps)

For more information about licensing and editions, see the [Sign up for Azure AD Premium](active-directory-get-started-premium.md) article.

Azure AD Premium editions are available for customers in China using the worldwide instance of Azure AD. Azure AD Premium editions aren't currently supported in the Azure service operated by 21Vianet in China

## Before you begin

You can customize the sign-in pages that use Azure AD as the identity provider when users sign in to your organization's tenant-specific apps, such as `https://outlook.com/woodgrove.com`, or when passing a domain variable, such as `https://passwordreset.microsoftonline.com/?whr=woodgrove.com`.

Custom branding appears after users sign in. Users that start the sign-in process at a site like www\.office.com won't see the branding. After users sign in, the branding may take at least 15 minutes to appear.

**All branding elements are optional. Default settings will remain, if left unchanged.** For example, if you specify a banner logo but no background image, the sign-in page shows your logo with a default background image from the destination site such as Microsoft 365. Additionally, sign-in page branding doesn't carry over to personal Microsoft accounts. If your users or guests sign in using a personal Microsoft account, the sign-in page won't reflect the branding of your organization.

**Images have different image and file size requirements.** Take note of the image requirements for each option. You may need to use a photo editor to create the right-sized images. The preferred image type for all images is PNG, but JPG is accepted. 

1. Sign in to the [Azure portal](https://portal.azure.com/) using a Global administrator account for the directory.

2. Select **Azure Active Directory** > **Company branding** > **Customize**.

The sign-in experience process is grouped into sections. At the end of each section, select the **Review + create** button to review what you have selected and submit your changes or the **Next** button to move to the next section.

## Basics

- **Favicon**: Select a PNG or JPG of your logo that appears in the web browser tab.

- **Background image**: Select a PNG or JPG to display as the main image on your sign-in page. This image will scale and crop according to the window size, but may be partially blocked by the sign-in prompt.

- **Page background color**: If the background image isn't able to load because of a slower connection, this background color appears instead.

## Layout

- **Visual Templates**: Customize the layout of your sign-in page using templates or custom CSS. Choose one of two **Templates**: Full-screen or partial-screen background. The full-screen background could obscure your background image, so choose the partial-screen background if your background image is important. The details of the **Header** and **Footer** options are set on the next two sections of the process.

- **Custom CSS**: Upload custom CSS to replace the Microsoft default style of the page. For a CSS template and more information on using your own CSS, view the [HELPFUL LINK HERE]().

## Header

If you haven't enabled the header, go to the **Layout** section and select **Show header**. Select a PNG or JPG to display in the header of the sign-in page.

## Footer

If you haven't enabled the footer, go to the **Layout** section and select **Show footer**. Select a PNG or JPG to display in the header of the sign-in page.

- **Show 'Privacy & Cookies'**: This option is selected by default and displays the [Microsoft 'Privacy & Cookies'](https://privacy.microsoft.com/privacystatement) link. Uncheck this option to hide the default Microsoft link. Optionally provide your own **Display text** and **URL**. The text and links do not have to be related to privacy and cookies.

- **Show 'Terms of Use'**: This option is also elected by default and displays the [Microsoft 'Terms of Use'](https://www.microsoft.com/servicesagreement/) link. Uncheck this option to hide the default Microsoft link. Optionally provide your own **Display text** and **URL**. The text and links do not have to be your terms of use.

    >[!IMPORTANT]
    >The default Microsoft 'Terms of Use' link is not the same as the Conditional Access Terms of Use. Seeing the terms here doesn't mean you've accepted those terms and conditions. 

## Sign-in form

- **Banner logo**: Select a PNG or JPG image file of a banner-sized logo (short and wide) to appear on the sign-in pages that use Azure AD as the identity provider and the Access Panel service.

- **Square logo (light theme)**: Select a square PNG or JPG image file of your logo to be used in browsers that are using a light color theme. This logo is used to represent your organization on the Azure AD web interface and in Windows.

- **Square logo (dark theme)** Select a square PNG or JPG image file of your logo to be used in browsers that are using a dark color theme. This logo is used to represent your organization on the Azure AD web interface and in Windows. If your logo looks good on light and dark backgrounds there's no need to add a dark theme logo.

- **Username hint text**: Enter hint text for the username input field on the sign-in page. If guests use the same sign-in page, we don't recommend using hint text here.

- **Sign-in page text**: Enter text that appears on the bottom of the sign-in page. You can use this text to communicate additional information, such as the phone number to your help desk or a legal statement. This page is public, so don't provide sensitive information here. This text must be Unicode and can't exceed 1024 characters.

    To begin a new paragraph, use the enter key twice. You can also change text formatting to include bold, italics, an underline or clickable link. Use the following syntax to add formatting to text: 

    > Hyperlink: `[text](link)` 
        
    > Bold: `**text**` or `__text__` 
          
    > Italics: `*text*` or `_text_` 
          
    > Underline: `++text++` 
         
    > [!IMPORTANT]
    > Hyperlinks that are added to the sign-in page text render as text in native environments, such as desktop and mobile applications.

- **Self-service password reset**:
    - Show self-service password reset (SSPR): Select the checkbox to turn on SSPR. 
    - Common URL: Enter the destination URL for where your users will reset their passwords. This URL appears on the username and password collection screens.
    - Username collection display text: 
    - Password collection display text:

## Review

All of the available options appear in one list so you can review everything you've customized or left at the default setting. When you're done, click the **Create** button. 

Once your default sign-in experience is created, you will start at the **Default sign-in** section instead of **Getting started.** Click the **Edit** button to make any changes. You can't delete a default sign-in experience after it is created.

## Customize the sign-in experience by browser language

To create an inclusive experience for all of your users, you can customize the sign-in experience based on browser language.

1. Sign in to the [Azure portal](https://portal.azure.com/) using a Global administrator account for the directory.

2. Select **Azure Active Directory** > **Company branding** > **Add browser language**.

The process for customizing the experience is the same as the [default sign-in experience](#basics) process, except you must select a language from the dropdown list in the **Basics** section. We recommend adding custom text in the same areas as your default sign-in experience. The available languages are pulled from the list of all available Azure languages. 

## Next steps

- Learn more about [default user permissions in Azure AD](../fundamentals/users-default-permissions.md)
- [Configure sign-in frequency settings with Conditional Access](../conditional-access/howto-conditional-access-session-lifetime.md)