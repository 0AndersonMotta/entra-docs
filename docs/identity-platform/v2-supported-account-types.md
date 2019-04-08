# Supported account types

<!-- This section can be in an include for many of the scenarios (SPA, Web App signing-in users, protecting a Web API, Desktop (depending on the flows), Mobile -->

## Supported accounts types in Microsoft Identity platform applications

In the Microsoft Azure public Cloud Web Apps can sign in users with any audience:

- if you're writing a Line of Business (LOB) application, you can sign in users in your own organization. Such an application is sometimes named **single tenant**.
- if you're an ISV, you can write an application which signs-in users:

  - in any organization. Such an application is named a **multi-tenant** web application. You'll sometimes read that it signs-in users with their work or school accounts.
  - with their work or school or personal Microsoft account.
  - with only personal Microsoft account.
    > [!NOTE]
    > Currently the Microsoft identity platform supports personal Microsoft accounts only by registering an app for work or school or Microsoft personal accounts, and then, restrict sign-in in the code for the application by specifying an an Azure AD authority such as `https://login.onmicrosoftonline.com/organizations`. In the future it will be possible to express the constraint directly at the application registration, and no longer in code.

- If you're writing a business to consumers application, you can also sign in users with their social identities, using Azure AD B2C.

## Certain authentication flows don't support all the account types

Some account types can't be used with certain authentication flows. For instance, in desktop and UWP applications:

- You can only use the Integrated Windows Authentication flow with work or school accounts (in your organization or any organization). Indeed, Integrated Windows Authentication works with domain accounts, and requires the machines to be domain joined or AAD joined. This flow doesn't make sense for personal Microsoft Accounts.
- The [Resource Owner Password Grant](./v2-oauth-ropc.md) (Username/Password), can't be used with personal Microsoft accounts. Indeed, personal Microsoft accounts require that the user consents to accessing personal resources at each sign-in session. Therefore this behavior isn't compatible with non-interactive flows.
- Device code flow doesn't yet work with personal Microsoft accounts
  > [!NOTE]
  > There is ongoing work to support device code flow with personal Microsoft accounts;

## Supported account types in national clouds

 Apps can also sign in users in national and sovereign clouds. However, Microsoft personal accounts aren't supported in these clouds (by definition of these clouds). Therefore the supported account types are reduced for these clouds to your organization or any organizations (single tenants and multi-tenant applications).

## Next Steps

- Learn more about [Tenancy in Azure Active Directory](./single-and-multi-tenant-apps.md)
- Learn more about [National Clouds](./authentication-national-cloud.md)