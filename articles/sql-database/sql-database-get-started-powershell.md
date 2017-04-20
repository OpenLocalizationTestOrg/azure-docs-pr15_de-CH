<properties
    pageTitle="Neue SQL-Datenbank-Setup mit PowerShell | Microsoft Azure"
    description="Lernen Sie eine SQL-Datenbank mit PowerShell erstellen. Setup Aufgaben können über PowerShell-Cmdlets verwaltet werden."
    keywords="Erstellen Sie neue Sql-Datenbank Datenbank-setup"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Erstellen Sie eine SQL-Datenbank und Aufgaben Sie häufige Datenbank Setup mit PowerShell-cmdlets


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Informationen Sie zum Erstellen einer SQL-Datenbank mithilfe von PowerShell-Cmdlets. (Elastische Datenbanken erstellen, finden Sie unter [Erstellen einer neuen elastische Datenbankpool mit PowerShell](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Datenbank-Setup: Erstellen Sie eine Ressourcengruppe, Server und Firewall-Regel

Haben Sie Zugriff auf ausgewählte Azure Abonnements Cmdlets ausführen, wird im nächste Schritt Ressourcengruppe einrichten mit dem Server, in dem die Datenbank erstellt werden. Sie können den nächsten Befehl alle gültigen Speicherort verwenden, wählen Sie bearbeiten. Führen Sie **(Get-AzureRmLocation | Where-Object {$_. Anbieter - Eq "Microsoft.Sql"}). Ort** eine Liste der gültigen Positionen abrufen.

Führen Sie den folgenden Befehl zum Erstellen einer Ressourcengruppe:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Erstellen eines Servers

SQL-Datenbanken werden in Azure SQL-Datenbankserver erstellt. Führen Sie [New-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) Server zu erstellen. Der Name des Servers muss auf alle Azure SQL-Datenbank-Server eindeutig sein. Wenn der Servername bereits vergeben ist, erhalten Sie Fehler. Erwähnenswert ist, dass dieser Befehl mehrere Minuten dauern kann. Den Befehl alle gültigen Speicherort verwenden, wählen Sie bearbeiten, aber verwenden Sie den Speicherort für die im vorherigen Schritt erstellten Ressourcengruppe verwendet.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Wenn Sie diesen Befehl ausführen, werden Ihr Benutzername und Kennwort aufgefordert werden. Geben Sie Ihre Azure Anmeldeinformationen nicht. Stattdessen geben Sie den Benutzernamen und das Kennwort als Serveradministrator erstellt. Das Skript am Ende dieses Artikels veranschaulicht, wie die Serveranmeldeinformationen im Code.

Die Serverdetails angezeigt werden, nachdem der Server erfolgreich erstellt wurde.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Konfigurieren Sie eine Firewallregel Server Zugriff auf den server

Zugriff auf den Server müssen Sie eine Firewall-Regel erstellen. Führen Sie [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) Befehl Start- und IP-Adressen durch gültige Werte ersetzen.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Regeldetails Firewall angezeigt, nachdem die Regel erstellt wird.

Damit andere Azure-Dienste auf den Server zugreifen können, fügen Sie eine Firewallregel und Start und EndIpAddress auf 0.0.0.0 festgelegt. Diese Regel ermöglicht Azure Datenverkehr von jeder Azure-Abonnement auf den Server zugreifen.

Weitere Informationen finden Sie in [Azure SQL-Datenbank-Firewall](sql-database-firewall-configure.md).


## <a name="create-a-sql-database"></a>Erstellen Sie eine SQL-Datenbank

Jetzt haben Sie eine Ressourcengruppe, ein Server und eine Firewall-Regel so konfiguriert, dass der Server zugreifen kann.

Die folgenden [New AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) Befehl erstellt eine (leere) Datenbank SQL Ebene Service mit S1 Leistung:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Die Details der Datenbank angezeigt, nachdem die Datenbank erfolgreich erstellt.

## <a name="create-a-sql-database-powershell-script"></a>Erstellen Sie eine SQL-Datenbank PowerShell-Skript

Das folgende PowerShell-Skript erstellt eine SQL-Datenbank und alle abhängigen Ressourcen. Ersetzen Sie den gesamten `{variables}` mit Werte und Ihre Ressourcen (entfernen **{** beim Festlegen der Werte).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Nächste Schritte
Erstellen Sie eine SQL-Datenbank und grundlegenden Setup Aufgaben können Sie Folgendes:

- [SQL-Datenbank mit PowerShell verwalten](sql-database-manage-powershell.md)
- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine T-SQL-Abfrage](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Azure SQL-Datenbank-Cmdlets] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [SQL Azure-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
