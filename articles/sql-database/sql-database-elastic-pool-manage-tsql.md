<properties 
    pageTitle="Erstellen oder verschieben eine Azure SQL-Datenbank in einer elastischen Pool mit T-SQL | Microsoft Azure" 
    description="Mit T-SQL Azure SQL-Datenbank in einem elastischen Pool erstellen. Oder T-SQL und Pools die Datenbank verschoben." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Überwachen Sie und verwalten Sie einer elastischen Datenbank mit Transact-SQL  

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Verwenden Sie die Befehle [Erstellen Datenbank Azure SQL](https://msdn.microsoft.com/library/dn268335.aspx) und [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) erstellen und Datenbanken in und aus elastischen Pools. Elastische Pool muss vorhanden sein, bevor Sie diese Befehle verwenden können. Diese Befehle betreffen nur Datenbanken. Erstellung neuer Pools sowie von der Einstellung der Eigenschaften (wie min und Max eDTUs) mit T-SQL-Befehle möglich.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Erstellen Sie eine neue Datenbank im elastischen pool
Verwenden Sie den Befehl CREATE DATABASE mit der Option SERVICE_OBJECTIVE.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Alle Datenbanken im elastischen Pool erben die Dienstebene elastischen Pool (Basic, Standard, Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Verschieben einer Datenbank zwischen elastische
Verwenden Sie den Befehl ALTER DATABASE mit modifizieren und Dienst\_Ziel Option ELASTISCHEN\_POOL; Legen Sie den Namen, den Namen des Zielpool.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Eine Datenbank in einer elastischen Pool verschieben 
Verwenden Sie den Befehl ALTER DATABASE mit modifizieren und Dienst\_Ziel Option ELASTIC_POOL; Legen Sie den Namen, den Namen des Zielpool.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Verschieben einer Datenbank aus einer elastischen
Verwenden Sie den Befehl ALTER DATABASE und die SERVICE_OBJECTIVE auf einen der Leistung (S0 S1 usw.).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Datenbanken im elastischen Pool auflisten
Verwenden der [sys.database\_Service \_Ziele anzeigen](https://msdn.microsoft.com/library/mt712619) Liste aller Datenbanken in einer elastischen Pool. Melden Sie sich an den Master Datenbank die Sicht Abfragen.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Abrufen von Daten für einen pool

Verwenden der [sys.elastic\_Pool \_Resource \_Statistiken anzeigen](https://msdn.microsoft.com/library/mt280062.aspx) die Ressource Verwendungsstatistik elastischen Pool auf einem logischen Server zu. Melden Sie sich an den Master Datenbank die Sicht Abfragen.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Ressourcenverwendung für elastische Datenbank abrufen

Verwenden der [sys.dm\_ Db\_ Resource\_Statistiken anzeigen](https://msdn.microsoft.com/library/dn800981.aspx) oder [sys.resource \_Statistiken anzeigen](https://msdn.microsoft.com/library/dn269979.aspx) zu einer Datenbank in einer elastischen Pool Statistik zur Ressource. Dieser Vorgang ähnelt der Ressourcenverwendung für jede einzelne Datenbank Abfragen.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen eines elastischen Datenbankpool, können Sie elastische Arbeitsplätze elastische Datenbanken im Pool verwalten. Elastische Aufträge erleichtern ausgeführten T-SQL-Skripts für eine beliebige Anzahl von Datenbanken im Pool. Weitere Informationen finden Sie unter [elastische Datenbank Aufträge (Übersicht)](sql-database-elastic-jobs-overview.md). 

[Dezentrales Skalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md)finden Sie unter: elastische Datenbanktools Dezentrales Skalieren, Verschieben von Daten verwenden, oder erstellen Sie Transaktionen.
