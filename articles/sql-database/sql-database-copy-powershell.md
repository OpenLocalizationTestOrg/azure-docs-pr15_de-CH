<properties 
    pageTitle="Kopieren eine Azure SQL-Datenbank mit PowerShell | Microsoft Azure" 
    description="Kopie einer Azure SQL-Datenbank mit PowerShell" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Kopieren einer SQL Azure-Datenbank mithilfe von PowerShell


> [AZURE.SELECTOR]
- [Übersicht](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Dieser Artikel veranschaulicht das Kopieren einer SQL-Datenbank mit PowerShell auf demselben Server auf einen anderen Server oder eine Datenbank in einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md)kopieren. Des Kopiervorgangs verwendet das [Neue AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) -Cmdlet. 


Um dieses Artikels abzuschließen, benötigen Sie Folgendes:

- Eine SQL Azure-Datenbank (Datenbank kopieren). Wenn Sie keinen SQL-Datenbank erstellen einen der Schritte in diesem Artikel: [Erstellen Sie Ihre erste Azure SQL-Datenbank](sql-database-get-started.md).
- Die neueste Version von Azure PowerShell. Ausführliche Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).


Viele neue Funktionen der SQL-Datenbank werden nur unterstützt, wenn [Ressourcenmanager Azure Bereitstellungsmodell](../azure-resource-manager/resource-group-overview.md)Verwendung Beispiele [Azure SQL-Datenbank-PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) für den Ressourcen-Manager verwenden. Vorhandenen klassischen Bereitstellungsmodell [Azure SQL-Datenbank (classic) Cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) zur Abwärtskompatibilität unterstützt, jedoch sollten Sie die Ressourcen-Manager-Cmdlets verwenden.


>[AZURE.NOTE] Die Größe der Datenbank kann der Kopiervorgang einige Zeit dauern.


## <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf demselben server

Zum Erstellen der Kopie auf dem Server lassen die `-CopyServerName` Parameter (oder auf demselben Server).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Kopieren einer SQL-Datenbank auf einen anderen server

Um die Kopie auf einem anderen Server enthalten die `-CopyServerName` Parameter und auf einem anderen Server. *Kopie* -Server muss bereits vorhanden sein. Wenn es in einer anderen Ressourcengruppe, dann muss auch die `-CopyResourceGroupName` Parameter.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Kopieren einer SQL-Datenbank in einem Datenbankpool elastische

Legen Sie zum Erstellen einer Kopie einer SQL-Datenbank in einem Pool der `-ElasticPoolName` Parameter zu einem vorhandenen Pool.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Benutzernamen auflösen

Um Benutzernamen zu beheben, nachdem der Kopiervorgang abgeschlossen ist, finden Sie unter [Benutzernamen auflösen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>PowerShell-Beispielskript

Das folgende Skript übernimmt alle Ressourcengruppen, Servern und Pool vorhanden (die Variablenwerte mit vorhandenen Ressourcen ersetzen). Alles bestehen, außer die Datenbankkopie.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Nächste Schritte

- Finden Sie einen Überblick über das Kopieren einer Azure SQL-Datenbank [eine SQL Azure-Datenbank kopieren](sql-database-copy.md) .
- Anzeigen Sie kopieren eine Datenbank mithilfe von Azure Portal [kopieren eine SQL Azure-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md)
- Anzeigen Sie kopieren eine Datenbank mithilfe von Transact-SQL- [Kopie einer Azure SQL-Datenbank mithilfe von T-SQL](sql-database-copy-transact-sql.md)
- Finden Sie unter [Verwalten von Azure SQL-datenbanksicherheit nach Wiederherstellung](sql-database-geo-replication-security-config.md) Informationen zum Verwalten von Benutzern und Benutzernamen beim Kopieren einer Datenbank auf einem anderen logischen Server.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Neue AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [AzureRmSqlDatabaseActivity abrufen](https://msdn.microsoft.com/library/mt603687.aspx)
- [Verwalten von Benutzernamen](sql-database-manage-logins.md)
- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine T-SQL-Abfrage](sql-database-connect-query-ssms.md)
- [Die Datenbank in ein BACPAC exportieren](sql-database-export.md)
- [Business Continuity – Überblick](sql-database-business-continuity.md)
- [Dokumentation zur SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
- [SQL Azure Datenbankverweis PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt574084.aspx)
