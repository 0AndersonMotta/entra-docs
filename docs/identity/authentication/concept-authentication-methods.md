---
title: Azure AD authentication methods
description: What authentication methods are available in Azure AD for MFA and SSPR

services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 05/14/2018

ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry, richagi

---
# What are authentication methods?

Azure AD self-service password reset (SSPR) and Multi-Factor Authentication (MFA) may ask for additional information known as authentication methods to confirm you are who you say you are when using the associated features.

Administrators can define in policy which authentication methods are available to users of SSPR and MFA. Some authentication methods may not be available to all features.

Microsoft highly recommends Administrators enable users to select more than the minimum required number of authentication methods in case they do not have access to one.

|Authentication Method|Usage|
| --- | --- |
| Password | MFA and SSPR |
| Security questions | SSPR Only |
| Email address | SSPR Only |
| Microsoft Authenticator App | MFA Only |
| SMS | MFA and SSPR |
| Voice call | MFA and SSPR |

## Password

Your Azure AD password is considered an authentication method. It is the one method that **cannot be disabled**.

## Security questions

Security questions are available **only in Azure AD self-service password reset**.

### Predefined questions

* In what city did you meet your first spouse/partner?
* In what city did your parents meet?
* In what city does your nearest sibling live?
* In what city was your father born?
* In what city was your first job?
* In what city was your mother born?
* What city were you in on New Year's 2000?
* What is the last name of your favorite teacher in high school?
* What is the name of a college you applied to but didn't attend?
* What is the name of the place in which you held your first wedding reception?
* What is your father's middle name?
* What is your favorite food?
* What is your maternal grandmother's first and last name?
* What is your mother's middle name?
* What is your oldest sibling's birthday month and year? (e.g. November 1985)
* What is your oldest sibling's middle name?
* What is your paternal grandfather's first and last name?
* What is your youngest sibling's middle name?
* What school did you attend for sixth grade?
* What was the first and last name of your childhood best friend?
* What was the first and last name of your first significant other?
* What was the last name of your favorite grade school teacher?
* What was the make and model of your first car or motorcycle?
* What was the name of the first school you attended?
* What was the name of the hospital in which you were born?
* What was the name of the street of your first childhood home?
* What was the name of your childhood hero?
* What was the name of your favorite stuffed animal?
* What was the name of your first pet?
* What was your childhood nickname?
* What was your favorite sport in high school?
* What was your first job?
* What were the last four digits of your childhood telephone number?
* When you were young, what did you want to be when you grew up?
* Who is the most famous person you have ever met?

All of the predefined security questions are translated and localized into the full set of Office 365 languages based on the user's browser locale.

### Custom security questions

Custom security questions are not localized. All custom questions are displayed in the same language as they are entered in the administrative user interface, even if the user's browser locale is different. If you need localized questions, you should use the predefined questions.

The maximum length of a custom security question is 200 characters.

### Security question requirements

* The minimum answer character limit is three characters.
* The maximum answer character limit is 40 characters.
* Users can't answer the same question more than one time.
* Users can't provide the same answer to more than one question.
* Any character set can be used to define the questions and the answers, including Unicode characters.
* The number of questions defined must be greater than or equal to the number of questions that were required to register.

## Email address

Email address is available **only in Azure AD self-service password reset**.

Microsoft recommends the use of an email account that would not require the user's Azure AD password to access.

## Microsoft Authenticator app

The Microsoft Authenticator app provides an additional level of security to your Azure AD work or school account or your Microsoft account.

The Microsoft Authenticator app is available for [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594), and [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

### Notification through mobile app

The Microsoft Authenticator app can help prevent unauthorized access to accounts and stop fraudulent transactions by pushing a notification to your smartphone or tablet. Users view the notification, and if it's legitimate, select Verify. Otherwise, they can select Deny.

### Verification code from mobile app

The Microsoft Authenticator app or other third-party apps can be used as a software token to generate an OAuth verification code. After entering your username and password, you enter the code provided by the app into the sign-in screen. The verification code provides a second form of authentication.

## Mobile phone

Two options are available to users with mobile phones.

### Text message

An SMS is sent to the mobile phone number containing a verification code. Enter the verification code provided in the sign-in interface to continue.

### Phone call

An automated voice call is made to the phone number you provide. Answer the call and press # in the phone keypad to authenticate

## Office phone

An automated voice call is made to the phone number you provide. Answer the call and presses # in the phone keypad to authenticate.

## Next steps

[Enable self service password reset for your organization](quickstart-sspr.md)

[Enable Azure Multi-Factor Authentication for your organization](quickstart-mfa.md)