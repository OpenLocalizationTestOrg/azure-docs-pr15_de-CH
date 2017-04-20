<properties
    pageTitle="Überwachen und Verwalten einer elastischen Datenbank mit C# | Microsoft Azure"
    description="Verwenden Sie C#-Datenbank Entwicklungstechniken zum Verwalten eines elastischen Datenbankpool Azure SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Überwachen Sie und verwalten Sie einer elastischen Datenbank mit C & #x 23; 

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Informationen Sie zu einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md) mit C & #x 23;. 

>[AZURE.NOTE] Viele neue Funktionen der SQL-Datenbank werden nur unterstützt, wenn [Ressourcenmanager Azure Bereitstellungsmodell](../azure-resource-manager/resource-group-overview.md)Verwendung also stets die neuesten sollten Sie **Azure SQL-Datenbank-Management-Bibliothek für .NET ([Dokumente](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Wir empfehlen die neueren Ressourcenmanager Basis-Bibliotheken verwenden werden nur Abwärtskompatibilität älteren [classic Bereitstellung modellbasierten Bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) unterstützt.

Die Schritte in diesem Artikel benötigen Sie Folgendes:

- Elastische Pool (Pool verwalten möchten) Zum Erstellen eines Pools finden Sie unter [Erstellen einer elastischen Datenbankpool mit C#](sql-database-elastic-pool-create-csharp.md).
- Visual Studio. Eine kostenlose Kopie von Visual Studio finden Sie in der [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs) -Downloadseite.


## <a name="move-a-database-into-an-elastic-pool"></a>Eine Datenbank in einer elastischen Pool verschieben

Sie können eigenständige Datenbank in oder aus einem Pool verschieben.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Datenbanken im elastischen Pool auflisten

Rufen Sie alle Datenbanken in einem Pool rufen Sie die [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) -Methode auf.

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Ändern der Leistung eines Pools

Rufen Sie vorhandene Pool-Eigenschaften ab. Ändern Sie die Werte und die CreateOrUpdate-Methode auszuführen.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Latenz elastischen Pool Vorgänge

- Min eDTUs pro Datenbank oder Max eDTUs pro Datenbank normalerweise ändern in fünf Minuten abgeschlossen.
- Die Poolgröße (eDTUs) geändert hängt die Gesamtgröße aller Datenbanken im Pool. Ändert durchschnittlich 90 Minuten oder weniger pro 100 GB. Wenn beispielsweise der gesamte Speicherplatz aller Datenbanken im Pool 200 GB, dann der erwarteten Latenz ist für eDTU Pool pro Pool ändern 3 Stunden oder weniger.




## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbank-Verbindungsfehler und andere Probleme](sql-database-develop-error-messages.md).
- [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure Resource Management APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Erstellen Sie einen neuen Datenbankpool elastische mit C#](sql-database-elastic-pool-create-csharp.md)
- [Wenn werden ein Datenbankpool elastische soll verwendet?](sql-database-elastic-pool-guidance.md)
- [Dezentrales Skalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md)finden Sie unter: elastische Datenbanktools Dezentrales Skalieren, Verschieben von Daten verwenden, oder erstellen Sie Transaktionen.

