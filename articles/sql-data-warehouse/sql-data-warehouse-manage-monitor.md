<properties
   pageTitle="Überwachen Sie Ihre Arbeitslast mit DMVs | Microsoft Azure"
   description="Erfahren Sie, wie Ihre Arbeitslast mit DMVs überwachen."
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
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Überwachen Sie Ihre Arbeitslast mit DMVs

Dieser Artikel beschreibt wie Sie dynamische Verwaltungsansichten (DMVs) Ihre Arbeitsbelastung überwachen und untersuchen in Azure SQL Data Warehouse Abfragen auszuführen.

## <a name="permissions"></a>Berechtigungen

In diesem Artikel DMVs abgefragt benötigen Sie die Berechtigung VIEW DATABASE STATE oder Steuerelement. Gewährung ANSICHTSZUSTAND Datenbank ist normalerweise die bevorzugte Berechtigung wesentlich restriktiver ist.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Überwachen der Verbindung

Alle Benutzernamen in SQL Data Warehouse werden in [sys.dm_pdw_exec_sessions][]protokolliert.  Diese DMV enthält mindestens 10.000 Benutzernamen.  Die Nummer der Primärschlüssel und nacheinander für jeden neuen Benutzer zugewiesen.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Überwachung der Ausführung von Abfragen

Alle Abfragen im SQL Data Warehouse ausgeführt werden [sys.dm_pdw_exec_requests][]protokolliert.  Diese DMV enthält mindestens 10.000 Abfragen ausgeführt.  Die Request_id eindeutig identifiziert jede Abfrage und der primäre Schlüssel für diese DMV.  Die Request_id für jede neue Abfrage nacheinander und vorangestellt QID steht für Abfrage-ID.  Diese DMV für eine bestimmte Nummer Abfragen zeigt alle Abfragen für eine bestimmte Anmeldung.

>[AZURE.NOTE] Gespeicherte Prozeduren verwenden mehrere IDs anfordern.  Anforderung-IDs werden in sequenzieller Reihenfolge zugewiesen. 

Hier werden Schritte Abfrageausführungspläne und Zeitangaben für eine bestimmte Abfrage untersuchen.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>Schritt 1: Ermitteln der Abfrage zu untersuchen

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Aus den vorangegangenen Abfrageergebnissen **Notiz ID anfordern** der Abfrage, die Sie untersuchen möchten.

Abfragen in den Status " **angehalten** " werden durch Parallelitätsgrenzen gesammelt. Diese Abfragen werden auch die sys.dm_pdw_waits wartet Abfrage mit UserConcurrencyResourceType. Näheres [Parallelität und Arbeitslast-Management][] auf Parallelitätsgrenzen. Abfragen können auch aus anderen Gründen wie Objektsperren warten.  Wenn die Abfrage auf eine Ressource wartet, siehe [Untersuchung Abfragen warten auf Ressourcen][] weiter unten in diesem Artikel.

Verwenden Sie zur Vereinfachung der Suche einer Abfrage in der sys.dm_pdw_exec_requests-Tabelle [Beschriftung][] an sys.dm_pdw_exec_requests nachgeschlagen werden kann die Abfrage einen Kommentar zuweisen.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>Schritt 2: Überprüfen Sie den Abfrageplan

Verwenden Sie ID anfordern der verteilten SQL (DSQL) Abfrageplan aus [sys.dm_pdw_request_steps][]abgerufen.

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Wenn ein Plan DSQL länger als erwartet dauert, können komplexe Plan mit vielen DSQL Schritte oder einem Schritt Zeit verursachen.  Sollten Sie der Plan viele Schritte mit mehreren verschieben optimieren Ihre Tabelle Distributionen um Daten reduzieren. [Verteilung der Tabelle][] Artikel erklärt, warum Daten verschoben werden müssen, um eine Abfrage zu lösen und erläutert Verteilungsstrategien um Daten zu minimieren.

Weitere Details zu einem Schritt der *Operation_type* Spalte langer Abfrage Schritt und **Schritt Index**beachten:

- Fahren Sie mit Schritt 3a für **SQL-Vorgänge**: OnOperation RemoteOperation, ReturnOperation.
- Fahren Sie mit Schritt 3 **Datentransfer Operationen**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>Schritt 3a: SQL verteilten Datenbanken überprüfen

Verwenden Sie ID anfordern und Schritt Index Details aus [sys.dm_pdw_sql_requests][]abgerufen, die Ausführung des Schrittes Abfrage aller verteilten Datenbanken Informationen.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Beim Ausführen des Abfrage Schritt kann [DBCC PDW_SHOWEXECUTIONPLAN][] zum Abrufen von SQL Server geschätzten Plans aus dem SQL Server für eine bestimmte Verteilung unter Schritt verwendet werden.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>Schritt 3: Datentransfer auf verteilten Datenbanken überprüfen

Verwenden Sie ID anfordern und Index Schritt zum Abrufen von Informationen über eine bewegungsschritt auf jede Verteilung [sys.dm_pdw_dms_workers][]ausgeführt.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Überprüfen Sie die Spalte *Total_elapsed_time* , um festzustellen, ob eine bestimmte Verteilung wesentlich länger als für Daten dauert.
- Überprüfen Sie für die Verteilung langer *Rows_processed* Spalte die Anzahl der Zeilen von Verteilung erheblich größer als die andere ist. In diesem Fall kann der zugrunde liegenden Daten neigen sein.

Wenn die Abfrage ausgeführt wird, kann [DBCC PDW_SHOWEXECUTIONPLAN][] zum SQL Server geschätzten Plan aus dem SQL Server für den aktuell ausgeführten SQL Schritt innerhalb einer bestimmten abrufen.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Wartende Abfragen überwachen

Wenn Sie feststellen, dass die Abfrage nicht ausgeführt, da eine Ressource wartet, ist hier eine Abfrage, die alle Ressourcen zeigt eine Abfrage wartet.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Wenn die Abfrage aktiv auf Ressourcen aus einer anderen Abfrage wartet, werden der Status **AcquireResources**.  Wenn die Abfrage die erforderlichen Ressourcen verfügt, werden der Status **gewährt**.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu DMVs finden Sie unter [Ansichten][] .
Finden Sie weitere Informationen zu bewährten [SQL Data Warehouse best practices][]

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[Bewährte SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Ansichten]: ./sql-data-warehouse-reference-tsql-system-views.md
[Tabellenverteilung]: ./sql-data-warehouse-tables-distribute.md
[Parallelität und Arbeitslast-management]: ./sql-data-warehouse-develop-concurrency.md
[Warten auf Ressourcen Abfragen untersuchen]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[Sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[Sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[Sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[Sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[Sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[BEZEICHNUNG]: https://msdn.microsoft.com/library/ms190322.aspx
