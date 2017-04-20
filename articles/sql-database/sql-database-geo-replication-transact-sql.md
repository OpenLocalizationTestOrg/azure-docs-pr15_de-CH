<properties
    pageTitle="Geo-Replikation für SQL Azure-Datenbank mit Transact-SQL konfigurieren | Microsoft Azure"
    description="Konfigurieren Sie Geo-Replikation mit Transact-SQL Azure SQL-Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Konfigurieren Sie Geo-Replikation für SQL Azure-Datenbank mit Transact-SQL

> [AZURE.SELECTOR]
- [Übersicht](sql-database-geo-replication-overview.md)
- [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Dieser Artikel veranschaulicht die aktive Geo-Replikation für eine Transact-SQL-Datenbank SQL Azure konfigurieren.

Zum Initiieren Failover mit Transact-SQL finden Sie unter [Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Aktive Geo-Replikation (lesbare sekundäre) steht jetzt für alle Datenbanken in alle Dienstebenen. Im April 2017 nicht lesbaren sekundären Typs zurückgezogen und Datenbanken nicht lesbar, lesbare sekundäre werden automatisch aktualisiert.

Aktive Geo-Replikation konfigurieren Transact-SQL verwenden, benötigen Sie:

- Ein Azure-Abonnement.
- Logische Azure SQL-Datenbankserver <MyLocalServer> und einer SQL-Datenbank <MyDB> -die primäre Datenbank, die Sie replizieren möchten.
- Ein oder mehrere logische Azure SQL-Datenbank Server < MySecondaryServer(n) > - die logischen Server, die die Partnerserver in denen sekundäre Datenbanken erstellt werden.
- Ein Benutzername, der auf dem primären DBManager ist haben Db_ownership Geo-Replikation wird die lokale Datenbank und DBManager Partner Server, Sie Geo-Replikation konfigurieren.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Sekundäre Datenbank hinzufügen

**ALTER DATABASE** -Anweisung können Sie eine geografisch repliziert sekundäre Datenbank auf einem Partnerserver erstellen. Ausführen dieser Anweisung in der master-Datenbank des Servers mit der Datenbank repliziert werden. Die Geo replizierten Datenbank die "primäre" haben denselben Namen wie die Datenbank repliziert und standardmäßig haben dieselben Service als primäre Datenbank. Die sekundäre Datenbank gelesen oder nicht gelesen werden kann, und kann eine einzelne Datenbank oder elastischen Databbase. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Dienstebenen](sql-database-service-tiers.md).
Nachdem die sekundäre Datenbank erstellt und ausgesät, beginnen die Daten asynchron aus der primären Datenbank replizieren. In den nachfolgenden Schritten wird beschrieben, wie Geo-Replikation konfigurieren mit Management Studio. Nicht lesbare und lesbar entweder mit einer einzelnen Datenbank oder eine Datenbank elastische sekundäre erstellen dienen.

> [AZURE.NOTE] Wenn eine Datenbank auf dem Server angegebene Partner mit demselben Namen wie die primäre Datenbank vorhanden ist, schlägt der Befehl fehl.


### <a name="add-non-readable-secondary-single-database"></a>Nicht lesbare sekundäre (einzelne Datenbank) hinzufügen

Gehen Sie nicht lesbare sekundäre als einzelne Datenbank erstellen.

1. Mit Version 13.0.600.65 oder höher von SQL Server Management Studio.

     > [AZURE.IMPORTANT] Downloaden Sie die [neueste](https://msdn.microsoft.com/library/mt238290.aspx) Version von SQL Server Management Studio. Es wird empfohlen, immer die neueste Version von Management Studio mit Updates Azure-Portal synchronisiert bleiben.


2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **master**und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie **ALTER DATABASE** -Anweisung zu einer lokalen Datenbank in Geo-Replikation mit einem nicht lesbaren sekundäre Datenbank MySecondaryServer1 primären MySecondaryServer1 angezeigter Servername ist.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.


### <a name="add-readable-secondary-single-database"></a>Lesbare sekundäre (einzelne Datenbank) hinzufügen
Gehen Sie folgendermaßen vor, eine lesbare sekundäre als einzelne Datenbank erstellen.

1. In Management Studio eine Verbindung mit der logischen Server Azure SQL-Datenbank.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **master**und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie **ALTER DATABASE** -Anweisung zu einer lokalen Datenbank in eine Geo-Replikation Primär mit eine lesbare sekundäre Datenbank auf dem sekundären Server.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.



### <a name="add-non-readable-secondary-elastic-database"></a>Nicht lesbare sekundäre (elastische Datenbank) hinzufügen

Gehen Sie nicht lesbare sekundäre als elastische Datenbank erstellen.

1. In Management Studio eine Verbindung mit der logischen Server Azure SQL-Datenbank.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **master**und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie **ALTER DATABASE** -Anweisung zu einer lokalen Datenbank in eine Geo-Replikation mit nicht lesbaren sekundären Datenbank auf einem sekundären Server in einem Pool elastische primäre.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.



### <a name="add-readable-secondary-elastic-database"></a>Lesbare sekundäre (elastische Datenbank) hinzufügen
Gehen Sie folgendermaßen vor, eine lesbare sekundäre als elastische Datenbank erstellen.

1. In Management Studio eine Verbindung mit der logischen Server Azure SQL-Datenbank.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **master**und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie **ALTER DATABASE** -Anweisung zu einer lokalen Datenbank in eine Geo-Replikation mit einer lesbaren sekundären Datenbank auf einem sekundären Server in einem Pool elastische primäre.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.



## <a name="remove-secondary-database"></a>Sekundäre Datenbank entfernen

**ALTER DATABASE** -Anweisung können Sie Replikationspartnerschaft zwischen einer sekundären Datenbank und den primären endgültig beenden. Diese Anweisung ist für die master-Datenbank auf die primäre Datenbank befindet. Nach Beendigung Beziehung wird die sekundäre Datenbank eine reguläre Datenbank schreibgeschützt. Wenn die Verbindung mit der sekundären Datenbank beschädigt ist wird der Befehl erfolgreich ausgeführt wird aber der sekundären Lese-/ Schreibzugriff nach Konnektivität wiederhergestellt ist. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Dienstebenen](sql-database-service-tiers.md).

Gehen Sie geometrische Replikationspartnerschaft Geo repliziert sekundäre entfernen.

1. In Management Studio eine Verbindung mit der logischen Server Azure SQL-Datenbank.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **master**und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie **ALTER DATABASE** -Anweisung, um einen sekundären Geo repliziert entfernen.

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

## <a name="monitor-geo-replication-configuration-and-health"></a>Geo-Replikationskonfiguration und Gesundheit überwachen

Überwachungsaufgaben sind Überwachung des Geo-Replikationskonfiguration und Überwachung Daten-Replikation.  **Sys.dm_geo_replication_links** dynamische Ansicht können in der master-Datenbank Sie Informationen über alle vorhandenen Replikationslinks für jede Datenbank auf dem logischen Server Azure SQL-Datenbank zurück. Diese Ansicht enthält eine Zeile für jede Verknüpfung Replikation zwischen primären und sekundären Datenbanken. **Sys.dm_replication_link_status** dynamische Ansicht können Sie eine Zeile für jede Azure SQL-Datenbank zurück, die derzeit an einer Replikation Replikationslink. Dies schließt primäre und sekundäre Datenbanken. Wenn mehr als eine fortlaufende Replikation Verknüpfung für eine primäre Datenbank vorhanden ist, enthält diese Tabelle eine Zeile für jede Beziehung. Die Ansicht ist in allen Datenbanken, einschließlich des logischen Masters erstellt. Dieser auf dem logischen Master Abfragen werden jedoch eine leere Menge zurückgegeben. **Sys.dm_operation_status** dynamische Ansicht können Sie den Status für alle Datenbankvorgänge einschließlich des Status der Replikationslinks anzuzeigen. Weitere Informationen finden Sie unter [sys.geo_replication_links (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575504.aspx)und [sys.dm_operation_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn270022.aspx).

Gehen Sie folgendermaßen vor, um geometrische Replikationspartnerschaft überwachen.

1. In Management Studio eine Verbindung mit der logischen Server Azure SQL-Datenbank.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **master**und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie folgende Anweisung alle Datenbanken mit Geo-Replikation Links angezeigt.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.
5. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **MyDB**und klicken Sie dann auf **Neue Abfrage**.
6. Verwenden Sie die folgende Anweisung Replikation lag und Letzte Replizierung von meinem sekundären Datenbanken von MyDB.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.
8. Verwenden Sie die folgende Anweisung die neuesten Geo-Replikationsvorgänge Datenbank MyDB angezeigt.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Aktualisierung eine sekundäre nicht lesbar lesbar

Im April 2017 nicht lesbaren sekundären Typs zurückgezogen und Datenbanken nicht lesbar, lesbare sekundäre werden automatisch aktualisiert. Wenn Sie heute nicht lesbare sekundäre verwendet, sie gelesen werden möchten können Sie die folgenden einfachen Schritte für jedes sekundär.

> [AZURE.IMPORTANT] Gibt keine benutzerverantwortliche direkte Aktualisierung eine sekundäre nicht lesbar lesbar. Wenn Sie Ihr nur sekundäre ablegen, bleibt die primäre Datenbank ungeschützt bis die neue sekundäre Datenbank vollständig synchronisiert. Wenn SLA für die Anwendung erfordert, dass die primäre immer geschützt ist, sollten Sie eine parallele sekundäre auf einem anderen Server vor der Anwendung der oben genannten Schritte erstellen. Hinweis jede Grundfarbe bis zu 4 sekundäre Datenbanken verwalten kann.


1. Zunächst auf den *sekundären* Server herstellen und die nicht lesbare sekundäre Datenbank:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Jetzt zum *primären* Server her und fügen Sie eine neue lesbare sekundäre

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über aktive Geo-Replikation finden Sie unter - [Active Geo-Replikation](sql-database-geo-replication-overview.md)
- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
