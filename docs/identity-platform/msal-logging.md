---
title: Logging in MSAL applications | Azure
description: Learn about logging in Microsoft Authentication Library (MSAL) applications.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''

ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
#Customer intent: As an application developer, I want to learn about logging so I can diagnose and troubleshoot my apps.
ms.collection: M365-identity-device-management
---

# Logging
Micrsoft Authentication Libary (MSAL) apps to generate log messages that can help diagnose issues and provide details. An app can configure logging with a few lines of code, and have custom control over the level of detail and whether or not Personal Identifiable Information (PII) or Organizational Identifiable Information (OII) is logged. It is recommended that you set an MSAL logging callback and provide a way for users to submit logs when they are having authentication issues.

## Logging levels

MSAL's logger allows for several levels of detail to be capture:

- Error: Events that indicate something has gone wrong and an error was generated. Use for debugging and identifying problems.
- Warning: Events that are of question and the app needs more information on. There hasn't necessarily been an error or failure, but intended for diagnostic and pinpointing problems.
- Info: MSAL will log events intended for informational purposes not necessarily intended for debugging.
- Verbose: Default. MSAL will log a large amount of information and give full details into what library behavior.

## Personal and organizational identifiable information 
By default, the MSAL logger does not capture any personal identifiable information (PII) & organizational identifiable information (OII).  By turning on the collection of PII or OII, the app takes responsibility for safely handling highly-sensitive data and complying with any regulatory requirements and in particular [GDPR](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr/resources).

## Logging in MSAL.NET
In MSAL 3.x, logging is set per application at app creation using the `.WithLogging` builder modifier. This method takes optional parameters:

- *Level* enables you to decide which level of logging you want. Setting it to Errors will only get errors
- *PiiLoggingEnabled* enables you to decide if you want to log PII or not. By default this is set to false, so that your application is compliant with GDPR.
- *LogCallback* is set to a delegate that does the logging. If PiiLoggingEnabled is true, this method will receive the messages twice: once with the containsPii parameter equals false and the message without PII, and a second time with the containsPii parameter equals to true and the message might contain PII. In some cases (when the message does not contain PII), the message will be the same.
- *DefaultLoggingEnabled* enables the default logging for the platform. By default it's false. If you set it to true it uses Event Tracing in Desktop/UWP applications, NSLog on iOS and logcat on Android.

```csharp
class Program
 {
  private static void Log(LogLevel level, string message, bool containsPii)
  {
     if (containsPii)
     {
        Console.ForegroundColor = ConsoleColor.Red;
     }
     Console.WriteLine($"{level} {message}");
     Console.ResetColor();
  }

  static void Main(string[] args)
  {
    var scopes = new string[] { "User.Read" };

    var application = PublicClientApplicationBuilder.Create("<clientID>")
                      .WithLogging(Log, LogLevel.Info, true)
                      .Build();

    AuthenticationResult result = application.AcquireTokenInteractive(scopes)
                                             .ExecuteAsnc();
  }
 }
 ```

 ## Logging in MSAL.js
 You can enable logging in MSAL.js by passing a logger object when creating a `UserAgentApplication` instance. This method takes the following parameters:

- *logger*: a Callback instance that can be provided by the developer to consume and publish logs in a custom manner. Implement the loggerCallback method depending on how you want to redirect logs.

- *level* (optional): the configurable log level. The supported log levels are: Error, Warning, Info, Verbose. Default value is Info.

- *piiLoggingEnabled* (optional): enables/disables the logging of PII data. PII logs are never written to default outputs like Console, Logcat or NSLog. Default is set to false.

- *correlationId* (optional): a unique identifier, used to map the request with the response for debugging purposes. Defaults to RFC4122 version 4 guid (128 bits).

```javascript
var logger = new Msal.Logger(loggerCallback, { level: Msal.LogLevel.Verbose, correlationId: '1234' });

var clientApplication = new Msal.UserAgentApplication(clientID, authority, authCallback, { logger: logger });

function loggerCallback(logLevel, message, piiLoggingEnabled) {
   console.log(message);
}
```