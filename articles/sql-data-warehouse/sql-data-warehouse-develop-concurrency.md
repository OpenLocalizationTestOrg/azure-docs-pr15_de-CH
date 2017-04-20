<properties
   pageTitle="Parallelität und Arbeitslast-Management im SQL Data Warehouse | Microsoft Azure"
   description="Kennen Sie Azure SQL Data Warehouse Parallelität und Arbeitslast-Management Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Parallelität und Arbeitslast-Management im SQL Data Warehouse

Um vorhersagbare Leistung Ebene Microsoft Azure SQL Data Warehouse können steuern Gleichzeitigkeitsgrade und Zuweisung von Ressourcen wie Speicher und CPU-Prioritäten. Dieser Artikel stellt Ihnen die Konzepte von Parallelität und Arbeitslast erklärt, wie Features implementiert wurden und wie Sie sie in Ihr Datawarehouse steuern können. SQL Data Warehouse Arbeitslast-Management soll Sie Mehrbenutzer-Umgebung unterstützen. Es ist nicht vorgesehen für Multi-Tenant-Arbeitslasten.

## <a name="concurrency-limits"></a>Parallelitätsgrenzen

SQL Data Warehouse können bis zu 1.024 gerätebasierte. Alle 1.024 Verbindungen können Abfragen gleichzeitig senden. Jedoch kann zur Optimierung des Durchsatzes SQL Data Warehouse Warteschlange einige Abfragen, um sicherzustellen, dass jede Abfrage minimal Arbeitsspeicher erhält. Queuing tritt bei Ausführung der Abfrage. Warteschlangen-Abfragen Parallelitätsgrenzen erreicht, können SQL Data Warehouse Gesamtdurchsatz dadurch, dass aktive Abfragen dringend benötigten Arbeitsspeicher zugreifen.  

Parallelitätsgrenzen unterliegen zwei Konzepte: *gleichzeitige Abfragen* und *Parallelität Steckplätze*. Für eine Abfrage ausführen müssen sie Query Parallelität Limit und Parallelität Zeitnischen ausführen.

- Gleichzeitige Abfragen sind Abfragen gleichzeitig ausführen. SQL Data Warehouse unterstützt bis zu 32 gleichzeitige Abfragen auf das größere DWU.
- Parallelität Steckplätze sind basierend auf DWU zugewiesen. Jede 100 DWU bietet 4 Parallelität Steckplätze. Beispielsweise ein DW100 4 Steckplätze Parallelität und zuweist DW1000 40. Jede Abfrage benötigt mindestens Parallelität Steckplätze, die [Klasse](#resource-classes) der Abfrage abhängig. Abfragen in die Klasse "smallrc" nutzen eine Parallelität Steckplatz. Abfragen in einer höheren Ressourcenklasse verbrauchen mehr Parallelität Slots.

Die folgende Tabelle beschreibt die Grenzwerte für gleichzeitige Abfragen und Parallelität Steckplätze mit verschiedenen DWU-Größen.

### <a name="concurrency-limits"></a>Parallelitätsgrenzen

|  DWU   | Max. gleichzeitige Abfragen  | Parallelität Umsetzungsplätzen |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120            |
| DW6000 |           32            |              240            |

Wenn einer der beiden Schwellenwerte erreicht wird, werden neue Abfragen in der Warteschlange und First in, First durchgeführt.  Ein Abfragen abgeschlossen ist und die Anzahl der Abfragen und Steckplätze fällt unter die Grenzwerte sind in der Warteschlange Abfragen veröffentlicht. 

> [AZURE.NOTE]  *Wählen Sie* Abfragen ausführen ausschließlich für dynamische Verwaltungsansichten (DMVs) oder Katalogsichten keine die Parallelitätsgrenzen unterliegen. Überwachen Sie das System unabhängig von der Anzahl der Abfragen auf ausführen.

## <a name="resource-classes"></a>Ressourcenklassen

Ressource Klassen unterstützt Sie Speicher reservieren und CPU-Zyklen einer Abfrage angegeben. Sie können Benutzer in Form von *Datenbankrollen*vier Ressourcenklassen zuweisen. Die vier Ressourcenklassen sind **"smallrc"**, **Mediumrc**, **Largerc**und **Xlargerc**. Benutzer in "smallrc" erhalten weniger Arbeitsspeicher und bessere Parallelität nutzen. Im Gegensatz dazu Xlargerc zugewiesene Benutzer erhalten viel Speicher und weniger Abfragen können daher gleichzeitig ausgeführt werden.

Standardmäßig ist jeder Benutzer ein Mitglied der kleinen Ressourcenklasse "smallrc". Die Prozedur `sp_addrolemember` zur Erhöhung der Ressourcenklasse und `sp_droprolemember` wird verwendet, um die Klasse zu verringern. Dieser Befehl würde z. B. die Klasse von Loaduser zu Largerc erhöhen:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Eine gute Vorgehensweise ist eine Ressourcenklasse als ändern ihre Ressourcenklassen dauerhaft Benutzer zuweisen. Beispielsweise erstellen auf gruppierte Columnstore Tabellen Indizes höherer Qualität, wenn mehr Arbeitsspeicher belegt. Um sicherzustellen, dass Lasten in höhere Speicher zugreifen, erstellen einen Benutzer speziell zum Laden von Daten und dauerhaft eine höhere Klasse dieser Benutzer zuweisen.

Es gibt einige Typen von Abfragen, die nicht aus einem größeren Speicher profitieren. Das System ignoriert Zuweisung ihrer Klasse und immer Ausführen dieser Abfragen in der kleinen Ressourcenklasse. Wenn diese Abfragen immer die kleine Klasse ausführen, führen sie Parallelität Steckplätze sind und sie wird nicht mehr als Steckplätze belegen. Weitere Informationen finden Sie in der [Resource-Klasse Ausnahmen](#query-exceptions-to-concurrency-limits) .

Weitere Ressourcen-Klasse:

- Berechtigung *Ändern Rolle* muss die Klasse eines Benutzers ändern.  
- Obwohl Benutzer mindestens eine höheren Ressourcenklassen hinzufügen können dauert Benutzer die Attribute des höchsten Ressourcenklasse, die sie zugeordnet sind. Wenn Mediumrc und Largerc ein Benutzer zugewiesen wird, also die höhere Klasse (Largerc) die Ressourcenklasse berücksichtigt wird.  
- Die Klasse der System-Administrator kann nicht geändert werden.

Ein ausführliches Beispiel finden Sie unter [Changing User Ressource Beispiel](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Speicher belegen

Es gibt vor- und Nachteile des Benutzers Klasse erhöhen. Eine Klasse für einen Benutzer erhöhen geben ihre Abfragen mehr Speicher dies bedeuten kann, dass Abfragen schneller ausgeführt.  Höhere Ressourcenklassen verringern jedoch auch die Anzahl gleichzeitiger Abfragen ausführen können. Dies ist das Verhältnis zwischen große Speichermengen zu einer einzelnen Abfrage zuweisen oder das andere Abfragen, die Speicherbereiche gleichzeitig ausgeführt. Wird ein Benutzer hohe Umlagen Speicher für eine Abfrage angegeben, werden anderen Benutzern nicht, denselben Speicher zum Ausführen einer Abfrage zugreifen.

In der folgenden Tabelle wird jede Verteilung durch DWU und Ressource Klasse belegten Speicher zugeordnet.

### <a name="memory-allocations-per-distribution-mb"></a>Speicherbereiche pro Verteilung (MB)

|  DWU   | "smallrc" | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1600   |
| DW500  |   100   |    400   |    800  |  1600   |
| DW600  |   100   |    400   |    800  |  1600   |
| DW1000 |   100   |    800   |  1600  |  3200   |
| DW1200 |   100   |    800   |  1600  |  3200   |
| DW1500 |   100   |    800   |  1600  |  3200   |
| DW2000 |   100   |  1600   |  3200  |  6400   |
| DW3000 |   100   |  1600   |  3200  |  6400   |
| DW6000 |   100   |  3200   |  6400  |  12800  |

Aus der obigen Tabelle sehen Sie, dass eine Abfrage auf ein DW2000 in die Xlargerc Klasse 6.400 MB Speicher 60 verteilten Datenbanken zugreifen.  SQL Data Warehouse sind 60-Distributionen. Daher sollten insgesamt Speicher für eine Abfrage in einer bestimmten Klasse zu berechnen, die oben genannten Werte 60 multipliziert.

### <a name="memory-allocations-system-wide-gb"></a>Speicher Umlagen systemweiten (GB)

|  DWU   | "smallrc" | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375    |
| DW3000 |    6    |    94    |   188   |   375    |
| DW6000 |    6    |   188    |   375   |   750    |

In dieser Tabelle systemweiten Speicherbereiche können sehen, dass eine Abfrage auf ein DW2000 in die Xlargerc Klasse insgesamt 375 GB Speicher reserviert (6.400 MB * 60 Distributionen / 1024 GB konvertieren) über die gesamte SQL Data Warehouse.

## <a name="concurrency-slot-consumption"></a>Parallelität Steckplatz Verbrauch

SQL Data Warehouse wird mehr Speicher Abfragen in höheren Ressourcenklassen gewährt. Speicher ist eine feste Ressource aus.  Daher kann pro Abfrage mehr Speicher, weniger gleichzeitigen Abfragen ausführen. In der folgenden Tabelle wiederholt alle vorherigen Konzepte in einer Ansicht, die die Anzahl der Parallelität durch DWU und die von jeder Klasse verwendet.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Verteilung und Verbrauch der Parallelität Steckplätze

|  DWU   | Maximale Anzahl gleichzeitiger Abfragen  | Parallelität Umsetzungsplätzen | "Smallrc" verwendete Steckplätze |  Steckplätze Mediumrc verwendet |  Steckplätze Largerc verwendet |  Steckplätze Xlargerc verwendet |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


Aus dieser Tabelle sehen Sie, dass SQL Data Warehouse ausgeführt wie DW1000 maximal 32 gleichzeitige Abfragen und insgesamt 40 Parallelität Steckplätze belegt. Wenn alle Benutzer in "smallrc" ausführen, darf 32 gleichzeitige Abfragen da jede Abfrage 1 Parallelität Platz belegen. Wenn alle Benutzer einer DW1000 in Mediumrc ausgeführt wurden, jede Abfrage wäre 800 MB pro Verteilung für eine Zuordnung Gesamtspeicher 47 GB pro Abfrage zugewiesen Parallelität wäre auf 5 Benutzer begrenzt (40 Parallelität Steckplätze / 8 Steckplätze pro Mediumrc Benutzer).

## <a name="query-importance"></a>Abfrage Bedeutung

SQL Data Warehouse implementiert Ressourcenklassen mit Arbeitsauslastungsgruppen. Es gibt insgesamt acht Arbeitsauslastungsgruppen, die das Verhalten der Ressourcenklassen in verschiedenen Größen DWU steuern. Für alle DWU verwendet SQL Data Warehouse vier der acht Arbeitsauslastungsgruppen. Dies ist sinnvoll, da jede Arbeitsauslastungsgruppe vier Ressourcenklassen zugeordnet ist: "smallrc", Mediumrc, Largerc, oder Xlargerc. Verstehen der Arbeitsauslastungsgruppen ist, dass einige dieser Arbeitsauslastungsgruppen *Bedeutung*fest. Bedeutung für CPU verwendet planen. Abfragen mit hoher Priorität ausgeführt, drei Mal mehr CPU-Zyklen als mittlere Bedeutung erhalten. Daher ermitteln Parallelität Steckplatz Mappings CPU-Priorität. Eine Abfrage mindestens 16 Steckplätze belegt, wird mit hoher Wichtigkeit.

Die folgende Tabelle zeigt die Bedeutung Mappings für jede Arbeitsauslastungsgruppe.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Workload Group Mappings Parallelität Steckplätze und Bedeutung

| Arbeitsauslastungsgruppen | Parallelität Steckplatz Zuordnung | MB / Verteilung | Zuordnung von Bedeutung |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Mittel       |
| SloDWGroupC01   |            2             |         200       |       Mittel       |
| SloDWGroupC02   |            4             |         400       |       Mittel       |
| SloDWGroupC03   |            8             |         800       |       Mittel       |
| SloDWGroupC04   |           16             |       1600       |       Hoch         |
| SloDWGroupC05   |           32             |       3200       |       Hoch         |
| SloDWGroupC06   |           64             |       6400       |       Hoch         |
| SloDWGroupC07   |          128             |      12800       |       Hoch         |

Aus dem Diagramm **Zuordnung und Verwendung von Parallelität Steckplätze** finden Sie unter einem DW500 1, 4, 8 oder 16 Parallelität Steckplätze für "smallrc", Mediumrc, Largerc und Xlargerc, bzw. verwendet. Sie können diese Werte im vorangegangenen Bedeutung für jede Klasse zu nachschlagen.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>DW500 Zuordnung Ressourcenklassen Bedeutung

| Klasse | Arbeitsauslastungsgruppe | Parallelität Steckplätze verwendet | MB / Verteilung | Bedeutung |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| "smallrc"        | SloDWGroupC00  |           1            |         100       |   Mittel   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Mittel   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Mittel   |
| xlargerc       | SloDWGroupC04  |          16            |        1600      |   Hoch     |


Die folgende Abfrage DMV können die Unterschiede hinsichtlich der Ressourcenkontrolle in Speicherressourcen im Detail betrachten oder aktiv und Verlaufsdaten Verwendung der Arbeitsauslastungsgruppen analysieren bei der Problembehandlung.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Abfragen, die Parallelitätsgrenzen berücksichtigen

Die meisten Abfragen unterliegen Ressourcenklassen. Diese Abfragen müssen sowohl die parallele Abfrage Parallelität Steckplatz Schwellenwert einpassen. Benutzer kann nicht wählen, Steckplatz Parallelitätsmodell Abfrage ausgeschlossen.

Zur Bestätigung berücksichtigt die folgenden Aussagen Ressourcenklassen:

- AUSWAHL EINFÜGEN
- UPDATE
- LÖSCHEN
- (Beim Abfragen von Tabellen) auswählen
- ALTER INDEX NEU ERSTELLEN
- ALTER INDEX NEU ORGANISIEREN
- ALTER TABELLE NEU ERSTELLEN
- INDEX ERSTELLEN
- COLUMNSTORE GRUPPIERTEN INDEX ERSTELLEN
- ERSTELLEN DER TABELLE ALS OPTION (CTAS)
- Laden von Daten
- Daten verschieben Operationen von Daten Bewegung Service (DMS)

## <a name="query-exceptions-to-concurrency-limits"></a>Abfrage Ausnahmen Parallelitätsgrenzen

Einige Abfragen berücksichtigt nicht die Klasse, die Benutzer zugeordnet ist. Diese Ausnahmen die Parallelitätsgrenzen erfolgen, wenn für einen bestimmten Befehl erforderlichen Speicherressourcen niedrig, oftmals ist der Befehl einen Metadatenvorgang. Das Ziel dieser Ausnahmen ist zu größeren Speicherbereiche für Abfragen, die sie nicht benötigen. In diesen Fällen wird die standardmäßige kleine Klasse ("smallrc") immer unabhängig von der tatsächlichen Benutzer zugewiesen verwendet. Beispielsweise `CREATE LOGIN` wird immer in "smallrc" ausgeführt. Zu diesen Vorgang erforderlichen Ressourcen sind sehr niedrig, damit es nicht sinnvoll, die Abfrage in Steckplatz Parallelitätsmodell aufnehmen.  Diese Abfragen werden auch nicht durch 32 Benutzeranzahl Parallelität beschränkt, eine unbegrenzte Anzahl Abfragen kann das Sitzungslimit 1.024 Sitzungen starten.

Die folgenden Aussagen berücksichtigt nicht Ressourcenklassen:

- Erstellen oder DROP TABLE
- ALTER TABLE... SWITCH, teilen oder PARTITION ZUSAMMENFÜHREN
- ALTER INDEX DISABLE
- INDEX LÖSCHEN
- Erstellen, aktualisieren oder DROP STATISTICS
- TRUNCATE TABLE
- ALTER-BERECHTIGUNG
- ANMELDUNG ERSTELLEN
- CREATE, ALTER oder DROP USER
- CREATE, ALTER oder DROP PROCEDURE
- Erstellen oder DROP VIEW
- WERTE EINFÜGEN
- Ansichten und DMVs auswählen
- ERKLÄREN
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Ein Beispiel für Benutzer Ressource ändern

1. **Login erstellen:** Öffnen Sie eine Verbindung mit der **master** -Datenbank auf dem SQL Server SQL Data Warehouse-Datenbank und führen Sie die folgenden Befehle.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] In diesem Zusammenhang empfiehlt es sich, einen Benutzer in der master-Datenbank Azure SQL Data Warehouse-Benutzer erstellen. Erstellen eines Benutzers in Master kann Benutzer sich mit Tools wie SSMS ohne einen Datenbanknamen.  Außerdem können sie mit Objekt-Explorer an alle Datenbanken auf einem SQL Server.  Weitere Informationen zum Erstellen und Verwalten von Benutzern finden Sie unter [sichere Datenbank im SQL Data Warehouse][].

2. **SQL Data Warehouse erstellen Benutzer:** Öffnen Sie eine Verbindung zu **SQL Data Warehouse** -Datenbank und führen Sie den folgenden Befehl.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Berechtigungen:** Im folgenden Beispiel wird gewährt `CONTROL` im **SQL Data Warehouse** -Datenbank. `CONTROL`in der Datenbank entspricht Ebene Db_owner in SQL Server.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Erhöhen Klasse:** Verwenden Sie die folgende Abfrage eine höhere Auslastung Management Rolle einen Benutzer hinzufügen.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Verringern Klasse:** Verwenden Sie die folgende Abfrage zum Entfernen eines Benutzers aus einer Arbeitslast-Management-Rolle.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Es ist nicht möglich, einen Benutzer aus "smallrc" entfernen.

## <a name="queued-query-detection-and-other-dmvs"></a>Verzögerte Abfrage Erkennung und andere DMVs

Sie können die `sys.dm_pdw_exec_requests` DMV Abfragen identifizieren, die in eine Warteschlange Parallelität. Parallelität Steckplatz warten Abfragen haben den Status **angehalten**.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Arbeitslast-Management-Funktionen mit angezeigt werden `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Die folgende Abfrage zeigt die Rolle jedes Benutzers zugewiesen ist.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse hat die folgenden Typen warten:

- **LocalQueriesConcurrencyResourceType**: Abfragen, die Parallelität Steckplatz Rahmen befinden. DMV Abfragen und System Funktionen wie `SELECT @@VERSION` sind Beispiele für lokale Abfragen.
- **UserConcurrencyResourceType**: Parallelität Steckplatz Framework innerhalb Abfragen. Abfragen für Endbenutzer Tabellen sind Beispiele, die diesen Ressourcentyp verwenden.
- **DmsConcurrencyResourceType**: aus Bewegung Datenoperationen wartet.
- **BackupConcurrencyResourceType**: Wartezeit gibt an, dass eine Datenbank gesichert wird. Der maximale Wert für diesen Ressourcentyp ist 1. Wenn mehrere Backups gleichzeitig angefordert wurde, werden den anderen Warteschlange.

Die `sys.dm_pdw_waits` DMV werden kann, welche Ressourcen finden eine Anforderung wartet.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

Die `sys.dm_pdw_resource_waits` DMV zeigt nur die Ressource wartet, die von einer bestimmten Abfrage verwendet. Ressourcenzeit misst nur warten Ressourcen bereitgestellt werden im Gegensatz zu Signal Wartezeit ist die Zeit dauert die zugrunde liegende SQL Server die Abfrage auf die CPU planen.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

Die `sys.dm_pdw_wait_stats` DMV für historisch Trendanalyse wartet verwendet werden kann.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwalten von Datenbankbenutzern und Sicherheit finden Sie unter [sichere Datenbank im SQL Data Warehouse][]. Weitere Informationen wie größere Ressourcen Klassen Columnstore gruppierter Index Qualität, finden Sie unter [Rebuilding Indizes Segment Qualität zu verbessern].

<!--Image references-->

<!--Article references-->
[Sichern einer Datenbank im SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Neuerstellen von Indizes Segment zu verbessern]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Sichern einer Datenbank im SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
