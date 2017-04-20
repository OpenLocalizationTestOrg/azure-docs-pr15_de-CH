<properties
    pageTitle="Einen Pool für elastische Datenbanken mit C# erstellen | Microsoft Azure"
    description="Verwenden Sie C# Datenbank Entwicklungstechniken Ressourcen über viele Datenbanken nutzen können skalierbare elastische Datenbankpool in Azure SQL-Datenbank erstellen."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="create-an-elastic-database-pool-with-cx23"></a>Erstellen Sie einen elastischen Datenbankpool mit C & #x 23;

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Dieser Artikel beschreibt, wie C# mit einen elastischen Datenbankpool Azure SQL [Azure SQL-Datenbank-Bibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)erstellt. Zum Erstellen einer eigenständigen SQL-Datenbank finden Sie unter [C# verwenden zum Erstellen einer SQL-Datenbank mit der SQL-Datenbank-Bibliothek für .NET](sql-database-get-started-csharp.md).

Azure SQL-Datenbank-Bibliothek für .NET bietet ein [Ressourcenmanager Azure](../azure-resource-manager/resource-group-overview.md)-basierte API, die der [Ressourcen-Manager-basierte SQL Datenbank REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx)umschließt.

>[AZURE.NOTE] Viele neue Funktionen der SQL-Datenbank werden nur unterstützt, wenn [Ressourcenmanager Azure Bereitstellungsmodell](../azure-resource-manager/resource-group-overview.md)Verwendung also stets die neuesten sollten Sie **Azure SQL-Datenbank-Management-Bibliothek für .NET ([Dokumente](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Die älteren [klassischen Bereitstellungsmodell basierende Bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) werden für Abwärtskompatibilität, wir empfehlen die neueren Ressourcenmanager Basis-Bibliotheken verwenden.

Die Schritte in diesem Artikel benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie Azure-Abonnement lediglich **Kostenloses Konto** am oberen Rand dieser Seite klicken Sie und dann wieder zu diesem Artikel.
- Visual Studio. Eine kostenlose Kopie von Visual Studio finden Sie in der [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs) -Downloadseite.


## <a name="create-a-console-app-and-install-the-required-libraries"></a>Erstellen Sie einer Konsole und installieren Sie die erforderlichen Bibliotheken

1. Starten Sie Visual Studio.
2. Klicken Sie auf **Datei** > **neue** > **Projekt**.
3. Erstellen Sie eine C#- **Konsolenanwendungsprojekt** und nennen Sie es: *SqlElasticPoolConsoleApp*


Laden Sie zum Erstellen einer SQL-Datenbank mit C# die erforderlichen Management-Bibliotheken (mit der [Paket-Manager-Konsole](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klicken Sie auf **Extras** > **NuGet Paket-Manager** > **Paket-Manager-Konsole**.
2. Typ `Install-Package Microsoft.Azure.Management.Sql –Pre` [Microsoft Azure SQL Management Library](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)installieren.
3. Typ `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` [Microsoft Azure-Ressourcen-Manager-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)installieren.
4. Typ `Install-Package Microsoft.Azure.Common.Authentication –Pre` [Microsoft Azure allgemeine Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication)installieren. 



> [AZURE.NOTE] Die Beispiele in diesem Artikel verwenden eine synchrone jede API-Anforderung und blockiert, bis Ende der REST vom zugrunde liegenden Dienst aufrufen. Asynchrone Methoden stehen zur Verfügung.


## <a name="create-a-sql-elastic-database-pool---c-example"></a>Erstellen Sie ein SQL elastische Datenbankpool - C#-Beispiel

Im folgende Beispiel erstellt eine Ressourcengruppe Server Firewallregel, elastische Pool und erstellt dann eine SQL-Datenbank im Pool. Siehe [Erstellen Dienstprinzipal Zugriff auf Ressourcen](#create-a-service-principal-to-access-resources) zu den `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` Variablen.

Ersetzen Sie den Inhalt von **"Program.cs"** mit den folgenden und aktualisieren die `{variables}` mit Ihren app (enthalten nicht die `{}`).


```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
{
    class Program
        {

        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    ElasticPoolName = poolName
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
    }
}
```





## <a name="create-a-service-principal-to-access-resources"></a>Erstellen Sie einen Service principal auf Ressourcen zugreifen

PowerShell-Skript erstellt die Anwendung Active Directory (AD) und Dienstprinzipalnamen, die wir der C#-Anwendung authentifizieren müssen. Das Skript gibt die Werte für die obigen C#-Beispiel. Ausführliche Informationen finden Sie unter [Verwenden Azure PowerShell Dienstprinzipal Zugriff auf Ressourcen zu erstellen](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret


  

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten der](sql-database-elastic-pool-manage-csharp.md)
- [Elastische Aufträge erstellen](sql-database-elastic-jobs-overview.md): elastische Aufträge können Sie T-SQL-Skripts für eine beliebige Anzahl von Datenbanken in einem Pool ausgeführt werden.
- [Dezentrales Skalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md): Verwenden Sie elastische Datenbanktools skalieren.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure Resource Management APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx)
