---
title: Sign in users to a Vanilla JS Single-page application using Microsoft Entra - Configure an application User Interface and Sign-In
description: Learn how to configure vanilla JavaScript single-page app (SPA) to sign in and sign out users with your CIAM tenant
services: active-directory
author: OwenRichards1
manager: CelesteDG

ms.author: owenrichards
ms.service: active-directory
ms.workload: identity
ms.subservice: ciam
ms.topic: how-to
ms.date: 04/17/2023
ms.custom: developer

#Customer intent: As a developer, I want to learn how to configure vanilla JavaScript single-page app (SPA) to sign in and sign out users with my CIAM tenant.
---

# Configure a Single-page application User Interface and Sign-In

This how-to guide describes how to configure a single-page application (SPA) to sign in and sign out users with your CIAM tenant. To build the user interface (UI) for the application, you'll use [Bootstrap](https://getbootstrap.com/). Next, you'll configure the application to sign in users by using [Microsoft Entra](https://docs.microsoft.com/azure/active-directory/develop/msal-js-v2).

In this article:

> * Create the project
> * Create the index.html & styles.css file
> * Create the app.js file

## Prerequisites

* Completion of the prerequisites and steps in [Prepare your application](how-to-single-page-app-vanillajs-prepare-app.md).

## Create the index.html file

Index.html is the main page of the application and is the first page that is loaded when the application is started. It's also the page that is loaded when the user select the **Sign Out** button.

1. Right-click the project folder and select **New File**. Name the file *index.html*.
1. Open *index.html* and add the following code snippet:

   ```html
   <!DOCTYPE html>
    <html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">
        <title>Microsoft identity platform</title>
        <link rel="SHORTCUT ICON" href="./favicon.svg" type="image/x-icon">
        <link rel="stylesheet" href="./styles.css">
        <!-- msal.min.js can be used in the place of msal.js; included msal.js to make debug easy -->
        <script id="load-msal" src="https://alcdn.msauth.net/browser/2.31.0/js/msal-browser.js"
            integrity="sha384-BO4qQ2RTxj2akCJc7t6IdU9aRg6do4LGIkVVa01Hm33jxM+v2G+4q+vZjmOCywYq"
            crossorigin="anonymous"></script>
        <!-- adding Bootstrap 5 for UI components  -->
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet"
            integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
    </head>
    
    <body>
        <nav class="navbar navbar-expand-sm navbar-dark bg-primary navbarStyle">
            <a class="navbar-brand" href="/">Microsoft identity platform</a>
            <div class="navbar-collapse justify-content-end">
                <button type="button" id="signIn" class="btn btn-secondary" onclick="signIn()">Sign-in</button>
                <button type="button" id="signOut" class="btn btn-success d-none" onclick="signOut()">Sign-out</button>
            </div>
        </nav>
        <br>
        <h5 id="title-div" class="card-header text-center">Vanilla JavaScript single-page application secured with MSAL.js
        </h5>
        <h5 id="welcome-div" class="card-header text-center d-none"></h5>
        <br>
        <div class="table-responsive-ms" id="table">
            <table id="table-div" class="table table-striped d-none">
                <thead id="table-head-div">
                    <tr>
                        <th>Claim Type</th>
                        <th>Value</th>
                        <th>Description</th>
                    </tr>
                </thead>
                <tbody id="table-body-div">
                </tbody>
            </table>
        </div>
        <!-- importing bootstrap.js and supporting js libraries -->
        <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"
            integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous">
            </script>
        <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.11.6/dist/umd/popper.min.js"
            integrity="sha384-oBqDVmMz9ATKxIep9tiCxS/Z9fNfEXiDAYTujMAeBAsjFuCZSmKbSSUnQlmh/jp3"
            crossorigin="anonymous"></script>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js"
            integrity="sha384-OERcA2EqjJCMA+/3y+gxIOqMEjwtxJY7qPCqsdltbNJuaOe923+mo//f6V8Qbsw3"
            crossorigin="anonymous"></script>
        <!-- importing app scripts (load order is important) -->
        <script type="text/javascript" src="./authConfig.js"></script>
        <script type="text/javascript" src="./ui.js"></script>
        <script type="text/javascript" src="./claimUtils.js"></script>
        <!-- <script type="text/javascript" src="./authRedirect.js"></script> -->
        <!-- uncomment the above line and comment the line below if you would like to use the redirect flow -->
        <script type="text/javascript" src="./authPopup.js"></script>
    </body>
    </html>
    ```

1. Save the file.

## Create the signout.html file

1. Right-click the project folder and select **New File**. Name the file *signout.html*.
1. Open *signout.html* and add the following code snippet:

    ```html
    <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Azure AD | Vanilla JavaScript SPA</title>
            <link rel="SHORTCUT ICON" href="./favicon.svg" type="image/x-icon">
        
            <!-- adding Bootstrap 4 for UI components  -->
            <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
        </head>
        <body>
            <div class="jumbotron" style="margin: 10%">
                <h1>Goodbye!</h1>
                <p>You have signed out and your cache has been cleared.</p>
                <a class="btn btn-primary" href="/" role="button">Take me back</a>
            </div>
        </body>
        </html>
    ```

1. Save the file.

## Create the ui.js file

1. Right-click the project folder and select **New File**. Name the file *ui.js*.
1. Open *ui.js* and add the following code snippet:

    ```javascript
    // Select DOM elements to work with
    const signInButton = document.getElementById('signIn');
    const signOutButton = document.getElementById('signOut');
    const titleDiv = document.getElementById('title-div');
    const welcomeDiv = document.getElementById('welcome-div');
    const tableDiv = document.getElementById('table-div');
    const tableBody = document.getElementById('table-body-div');
    
    function welcomeUser(username) {
        signInButton.classList.add('d-none');
        signOutButton.classList.remove('d-none');
        titleDiv.classList.add('d-none');
        welcomeDiv.classList.remove('d-none');
        welcomeDiv.innerHTML = `Welcome ${username}!`;
    };
    
    function updateTable() {
        /**
         * In order to obtain the ID Token in the cached obtained previously, you can initiate a
         * silent token request by passing the current user's account and the scope "openid".
         */
        myMSALObj.acquireTokenSilent({
                account: myMSALObj.getAccountByUsername(username),
                scopes: [],
            })
            .then((response) => {
                tableDiv.classList.remove('d-none');
    
                const tokenClaims = createClaimsTable(response.idTokenClaims);
    
                Object.keys(tokenClaims).forEach((key) => {
                    let row = tableBody.insertRow(0);
                    let cell1 = row.insertCell(0);
                    let cell2 = row.insertCell(1);
                    let cell3 = row.insertCell(2);
                    cell1.innerHTML = tokenClaims[key][0];
                    cell2.innerHTML = tokenClaims[key][1];
                    cell3.innerHTML = tokenClaims[key][2];
                });
            });
        };
    ```

1. Save the file.

## Create the styles.css file

1. Right-click the project folder and select **New File**. Name the file **styles.css**.
1. Open *styles.css* and add the following code snippet:

    ```css
    .navbarStyle {
    padding: .5rem 1rem !important;
    }
    
    .table-responsive-ms {
        max-height: 39rem !important;
        padding-left: 10%;
        padding-right: 10%;
    }
    ```

1. Save the file.

## Run your project and sign in

All the required code snippets have been added, so the application can now be called and tested in a web browser.

1. Open a new terminal by selecting Terminal > New Terminal.
1. Run the following command to start your express web server.

    ```powershell
    npm start
    ```

1. Open a web browser and navigate to the port specified in [Prepare a Single-page application for authentication](how-to-single-page-app-vanillajs-sign-in-sign-out.md). For example, <http://localhost:3000/>.

<!-- SCREENSHOT -->

1. Select the Sign In button. For the purposes of this tutorial, choose the Sign in using Popup option.

<!-- SCREENSHOT -->

1. After the popup window appears with the sign-in options, select the account with which to sign-in.

<!-- SCREENSHOT -->

1. A second window may appear indicating that a code will be sent to your email address. If this happens, select Send code. Open the email from the sender Microsoft account team, and enter the 7-digit single-use code. Once entered, select Sign in.

<!-- SCREENSHOT -->

1. For Stay signed in, you can select either No or Yes.

<!-- SCREENSHOT -->

1. The app will now ask for permission to sign-in and access data. Select Accept to continue.

<!-- SCREENSHOT -->

1. The SPA will now display a button saying Request Profile Information. Select it to display the Microsoft Graph profile data acquired from the Microsoft Graph API.

## Next steps

Learn how to use the Microsoft Authentication Library (MSAL) for JavaScript to sign in users and acquire tokens to call Microsoft Graph.