<properties
    pageTitle="Azure SQL-Datenbank mit PowerShell verwalten | Microsoft Azure"
    description="Azure SQL-Datenbank-Management mit PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Azure SQL-Datenbank mit PowerShell verwalten


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

Dieses Thema zeigt die PowerShell-Cmdlets, mit denen viele Aufgaben zur Azure SQL-Datenbank. Eine vollständige Liste finden Sie unter [Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/mt574084.aspx).


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine Ressourcengruppe für unsere SQL-Datenbank und zugehörige Azure Ressourcen mit dem [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837.aspx) -Cmdlet.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Weitere Informationen finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).
Ein Beispielskript finden Sie unter [Erstellen einer SQL-Datenbank PowerShell-Skript](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## <a name="create-a-sql-database-server"></a>Erstellen Sie einen SQL-Datenbankserver

Erstellen Sie einen SQL-Datenbankserver mit dem [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715.aspx) -Cmdlet. Ersetzen Sie *server1* mit dem Namen des Servers ein. Servernamen müssen auf allen Servern Azure SQL-Datenbank eindeutig sein. Wenn der Servername bereits vergeben ist, erhalten Sie Fehler. Dieser Befehl kann einige Minuten dauern. Die Ressourcengruppe muss in Ihrem Abonnement vorhanden.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Weitere Informationen finden Sie unter [SQL-Datenbank](sql-database-technical-overview.md). Ein Beispielskript finden Sie unter [Erstellen einer SQL-Datenbank PowerShell-Skript](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-server-firewall-rule"></a>Erstellen einer Datenbank SQL Server Firewall-Regel

Erstellen Sie eine Firewallregel Zugriff auf den Server mit dem [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860.aspx) -Cmdlet. Führen Sie den folgenden Befehl Start- und IP-Adressen für den Client durch gültige Werte ersetzen. Die Ressourcengruppe und Server ist in Ihrem Abonnement.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Andere Azure Services auf dem Server eine Firewall-Regel erstellen und Festlegen der `-StartIpAddress` und `-EndIpAddress` auf **0.0.0.0**. Dieser spezielle Firewallregel kann alle Azure Datenverkehr auf den Server zugreifen.

Weitere Informationen finden Sie in [Azure SQL-Datenbank-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx). Ein Beispielskript finden Sie unter [Erstellen einer SQL-Datenbank PowerShell-Skript](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-blank"></a>Erstellen Sie eine SQL-Datenbank (leer)

Erstellen Sie eine Datenbank mit dem [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) -Cmdlet. Die Ressourcengruppe und Server ist in Ihrem Abonnement. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Weitere Informationen finden Sie unter [SQL-Datenbank](sql-database-technical-overview.md). Ein Beispielskript finden Sie unter [Erstellen einer SQL-Datenbank PowerShell-Skript](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="change-the-performance-level-of-a-sql-database"></a>Ändern der Leistung einer SQL-Datenbank

Skalieren Sie die Datenbank nach oben oder unten mit dem [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx) -Cmdlet. Die Ressourcengruppe, Server und Datenbank müssen bereits in Ihrem Abonnement vorhanden sein. Legen Sie die `-RequestedServiceObjectiveName` in ein einzelnes Leerzeichen (wie im folgenden Codeausschnitt) für grundlegende Ebene. Legen sie auf *S0*, *S1*, *P1*, *P6*usw., wie im vorangehenden Beispiel für andere Ebenen.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Weitere Informationen finden Sie unter [SQL-Datenbank und Leistung: verstehen, was jeder Dienstebene für](sql-database-service-tiers.md). Ein Beispielskript finden Sie unter [Beispiel PowerShell-Skript der Service-Tier und Leistung Ihrer SQL-Datenbank ändern](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf demselben server

Kopieren einer SQL-Datenbank auf demselben Server mit dem [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644.aspx) -Cmdlet. Legen Sie die `-CopyServerName` und `-CopyResourceGroupName` , dieselben Werte wie der Datenbank-Server und Ressourcen Gruppe.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Weitere Informationen finden Sie unter [einer Azure SQL-Datenbank kopieren](sql-database-copy.md). Ein Beispielskript finden Sie in der [SQL-Datenbank PowerShell-Skript zu kopieren](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>Löschen einer SQL-Datenbank

Löschen Sie eine SQL-Datenbank mit dem [Remove-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368.aspx) -Cmdlet. Die Ressourcengruppe, Server und Datenbank müssen bereits in Ihrem Abonnement vorhanden sein.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Einen SQL Server löschen

Löschen eines Servers mit dem [Remove-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488.aspx) -Cmdlet.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Erstellen und Verwalten von elastischen datenbankpools mithilfe von PowerShell

Details zum Erstellen von Pools mit PowerShell elastische Datenbanken finden Sie unter [Erstellen einer neuen elastische Datenbankpool mit PowerShell](sql-database-elastic-pool-create-powershell.md).

Informationen zum Verwalten von Pools mit PowerShell elastische Datenbanken finden Sie unter [Überwachen und Verwalten einer elastischen Datenbank mit PowerShell](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Informationen

- [SQL Azure-Datenbank Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx)
- [Azure-Cmdlet-Referenz](https://msdn.microsoft.com/library/azure/dn708514.aspx)
