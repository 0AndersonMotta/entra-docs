---
title: Sign in users to a Vanilla JS Single-page application using Microsoft Entra - Prepare your Single-page application
description: Learn how to configure vanilla JavaScript single-page app (SPA) to prepare for authentication with your CIAM tenant.
services: active-directory
author: OwenRichards1
manager: CelesteDG

ms.author: owenrichards
ms.service: active-directory
ms.workload: identity
ms.subservice: ciam
ms.topic: how-to
ms.date: 04/25/2023
ms.custom: developer

#Customer intent: As a developer, I want to learn how to configure vanilla JavaScript single-page app (SPA) to sign in and sign out users with my CIAM tenant.
---

# Prepare a Single-page application for authentication

After registering an application and creating a user flow in a CIAM tenant, a vanilla JavaScript (JS) single-page application (SPA) can be created using an integrated development environment (IDE) or a code editor. In this article, you'll create a vanilla JS SPA and a server to host the application

In this article:

> [!div class="checklist"]
>
> * Create a single-page application (SPA) using `npm`
> * Configure the settings for the application
> * Add authentication code to the application

## Prerequisites

- Completion of the prerequisites and steps in [Sign in users to a vanilla JS Single-page application using Microsoft Entra](how-to-single-page-app-vanillajs-prepare-tenant.md).
- Although any IDE that supports Vanilla JS applications can be used, **Visual Studio Code** is used for this guide. It can be downloaded from the [Downloads](https://visualstudio.microsoft.com/downloads) page.
- [Node.js](https://nodejs.org/en/download/).

## Create a new Vanilla JS project and install dependencies

1. Open a terminal in your IDE and navigate to the location in which to create your project.
1. Run the following command to create a new vanilla JS project

    ```powershell
    npm init -y
    ```

1. In the **Terminal**, run the following command to install the required dependencies for the project:

    ```powershell
    npm install express morgan
    ```

## Create the server file

**Express** is a web application framework for **Node.js**. It's used to create a server that hosts the application. **Morgan** is the middleware that logs HTTP requests to the console. The server file is used to host these dependencies and contains the routes for the application.

1. In your IDE, create a new file and call it *server.js*.
1. Add the following code snippet to the *server.js* file:

    ```javascript
    const express = require('express');
    const morgan = require('morgan');
    const path = require('path');
    
    const DEFAULT_PORT = process.env.PORT || 3000;
    
    // initialize express.
    const app = express();
    
    // Configure morgan module to log all requests.
    app.use(morgan('dev'));
    
    // Setup app folders.
    app.use(express.static('public'));
    
    // Set up a route for signout.html
    app.get('/signout', (req, res) => {
        res.sendFile(path.join(__dirname + '/public/signout.html'));
    });
    
    // set up a route for redirect.html
    app.get('/redirect', (req, res) => {
        res.sendFile(path.join(__dirname + '/public/redirect.html'));
    });
    
    // Set up a route for index.html
    app.get('/', (req, res) => {
        res.sendFile(path.join(__dirname + '/index.html'));
    });
    
    app.listen(DEFAULT_PORT, () => {
        console.log(`Sample app listening on port ${DEFAULT_PORT}!`);
    });
    
    module.exports = app;
    ```

## Next steps

> [!div class="nextstepaction"]
> [Configure application for authentication](how-to-single-page-app-vanillajs-configure-authentication.md)