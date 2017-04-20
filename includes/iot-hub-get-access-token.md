## <a name="obtain-an-azure-resource-manager-token"></a>Einen Azure-Ressourcenmanager Token abrufen

Azure Active Directory authentifizieren müssen alle Aufgaben, die mit der Azure-Ressourcen-Manager durchgeführt. Dieses Beispiel verwendet Kennwortauthentifizierung, andere Ansätze finden Sie unter [Authentifizierung Azure Resource Manager benötigt][lnk-authenticate-arm].

1. Fügen Sie folgenden Code der **Main** -Methode in Program.cs einen Token von Azure AD mit der Anwendung abgerufen.

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. Erstellen Sie ein **ResourceManagementClient** -Objekt, das das Token verwendet, indem Sie am Ende der **Main** -Methode den folgenden Code hinzufügen:

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Erstellen Sie oder beantragen Sie einen Verweis auf die Ressourcengruppe, die Sie verwenden:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx