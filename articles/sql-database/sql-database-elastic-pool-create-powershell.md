<properties
    pageTitle="Erstellen einen neuen elastische Datenbankpool mit PowerShell | Microsoft Azure"
    description="Erfahren Sie mit PowerShell Azure SQL-Datenbank skalieren Ressourcen durch Erstellen eines Pools skalierbare elastische Datenbank zur Verwaltung von Datenbanken."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Erstellen eines neuen Pools elastische Datenbank mit PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Informationen Sie zum Erstellen einer [elastischen Datenbankpool](sql-database-elastic-pool.md) mithilfe von PowerShell-Cmdlets. 

Allgemeine Fehlercodes finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbank-Verbindungsfehler und andere Probleme](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Elastische Pools sind allgemein verfügbar (GA) in allen Azure Regionen außer US North Central und West Indien, gerade in der Vorschau wird.  GA elastische Pools in diese Bereiche erfolgt so bald wie möglich. Auch unterstützen elastische Pools derzeit nicht Datenbanken [im Arbeitsspeicher OLTP- oder im Arbeitsspeicher Analytics](sql-database-in-memory.md).


Sie müssen Azure PowerShell 1.0 oder höher ausgeführt werden. Ausführliche Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Erstellen eines neuen Pools

[Neu-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) -Cmdlet erstellt einen neuen Pool. Die Werte für eDTU pro Pool, min und max Dtus werden durch den Wert der Service-Ebene (Basic, Standard oder Premium) eingeschränkt. Finden Sie unter [eDTU und Speicher für elastische Pools und elastische Datenbanken beschränkt](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Erstellen Sie eine neue elastische Datenbank in einem pool

Verwenden Sie das Cmdlet [Neu AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) und den **ElasticPoolName** -Parameter an den Zielpool. Um eine vorhandene Datenbank in einem Pool verschieben, finden Sie unter [Verschieben einer Datenbank in einer elastischen Pool](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Erstellen eines Pools und mit neuen Datenbanken 

Erstellung einer großen Anzahl von Datenbanken in einem Pool kann dauern Abschluss über das Portal oder PowerShell-Cmdlets, die jeweils nur eine Datenbank erstellen. Zum Automatisieren der Erstellung in einem neuen Pool finden Sie unter [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Beispiel: Erstellen eines Pools mit PowerShell 

Dieses Skript erstellt eine neue Azure Ressourcengruppe und einen neuen Server. Wenn Sie aufgefordert werden, geben Sie einen Benutzernamen und das Kennwort für den neuen Server (nicht der Azure-Anmeldeinformationen).

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Nächste Schritte

- [Verwalten der](sql-database-elastic-pool-manage-powershell.md)
- [Elastische Aufträge erstellen](sql-database-elastic-jobs-overview.md) Elastische Aufträge können Sie T-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool ausgeführt werden.
- [Dezentrales Skalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md): verwenden elastische Datenbanktools zu skalieren.

