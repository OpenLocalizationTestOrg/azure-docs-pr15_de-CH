<properties
   pageTitle="Erhalten Sie die erforderlichen Werte zur Authentifizierung einer Anwendung Zugriff von Code auf SQL Datenbank | Microsoft Azure"
   description="Erstellen Sie einen Dienstprinzipalnamen für Code SQL-Datenbank zugreifen."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Erhalten Sie die erforderlichen Werte für die Authentifizierung einer Anwendung Code SQL-Datenbank zugreifen

Zum Erstellen und Verwalten der SQL-Datenbank von Code müssen Sie Ihre Anwendung in Azure Active Directory (AAD) Domäne im Abonnement registrieren, Azure-Ressourcen erstellt wurden.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Erstellen Sie einen Dienst principal Zugriff auf die Ressourcen einer Anwendung

Sie müssen die neuesten [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) installiert und ausgeführt haben. Ausführliche Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

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




## <a name="see-also"></a>Siehe auch

- [Erstellen einer SQL-Datenbank mit C#](sql-database-get-started-csharp.md)
- [Herstellen einer Verbindung mit Datenbank SQL Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md)


