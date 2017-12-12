
## Register your application (express method)
Register your application by doing the following:
1. Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure).
2. Select **Add an app**.
3. In the **Application Name** box, enter a name for your application.
4. Select the **Guided Setup** check box, and then select **Create**.
5. Obtain the application ID, and paste it into your code.

### Add your application registration information to your solution (advanced method)
To add registration information to your solution, do the following:
1. If you haven't already registered your application, go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app).
2. Select **Add an app**.
3. In the **Application Name** box, enter a name for your application. 
4. Ensure that the **Guided Setup** check box is cleared, and then select **Create**.
5. Select **Add Platform**, select **Native Application**, and then select **Save**.
6. In the **Application ID** box, copy the GUID.
7. Go to Visual Studio, open the *App.xaml.cs* file, and then replace `your_client_id_here` with the Application ID you just registered and copied.

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```
