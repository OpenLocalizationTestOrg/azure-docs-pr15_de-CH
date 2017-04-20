## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Vorbereiten der Azure-Ressourcenmanager authentifizieren

Sie müssen alle Vorgänge, die mit der [Azure-Ressourcen-Manager] durchgeführt authentifizieren[ lnk-authenticate-arm] mit Azure Active Directory (AD). Die einfachste Möglichkeit hierzu ist PowerShell oder Azure CLI verwenden.

[Azure PowerShell 1.0] installieren[ lnk-powershell-install] oder höher, bevor Sie fortfahren.

Die folgenden Schritte anzeigen zum Einrichten einer AD Anwendung PowerShell Kennwortauthentifizierung Sie können diese Befehle in ein standard-PowerShell-Sitzung ausführen.

1. Melden Sie sich bei Ihrem Azure-Abonnement mit dem folgenden Befehl:

    ```
    Login-AzureRmAccount
    ```

2. Notieren Sie sich die **TenantId** und **SubscriptionId**. Diese werden später benötigt werden.

3. Erstellen Sie eine neue Active Directory Azure-Anwendung mit dem folgenden Befehl die Platzhalter ersetzen:

    - **{Display Name}:** einen Anzeigenamen für die Anwendung wie **MySampleApp**
    - **{URL der Homepage}:** der URL der Homepage der Anwendung wie **http://mysampleapp/home**. Diese URL muss nicht auf einer echten Anwendung.
    - **{Anwendungsbezeichner}:** Ein eindeutiger Bezeichner wie **http://mysampleapp**. Diese URL muss nicht auf einer echten Anwendung.
    - **{Kennwort}:** Ein Kennwort, mit denen Sie authentifizieren Ihre App.

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. Notieren Sie die **ApplicationId** der erstellten Anwendung. Sie werden diese später benötigen.

5. Erstellen Sie einen neuen Dienst principal mithilfe des folgenden Befehls **ApplicationId** aus dem vorherigen Schritt **{MyApplicationId}** ersetzen:

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. Einrichten einer Zuweisung der Sicherheitsrolle mit dem folgenden Befehl die **ApplicationId** **{MyApplicationId}** ersetzen.

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
Sie haben jetzt erstellen Azure AD-Anwendung, die Sie aus Ihrer benutzerdefinierten C# Anwendung authentifizieren kann. Später in diesem Lernprogramm benötigen Sie die folgenden Werte:

- TenantId
- SubscriptionId
- ApplicationId
- Kennwort

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md
