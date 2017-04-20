<properties
   pageTitle="Dynamische Verwaltungsansichten mit SQL Azure-Datenbank überwachen | Microsoft Azure"
   description="Informationen Sie zum Erkennen und zu diagnostizieren häufig auftretende Leistungsprobleme mit dynamische Verwaltungsansichten Microsoft Azure SQL-Datenbank überwachen."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Überwachung über dynamische Verwaltungsansichten Azure SQL-Datenbank

Microsoft Azure SQL-Datenbank ermöglicht eine Teilmenge der dynamische Verwaltungsansichten Leistungsprobleme diagnostizieren, die durch blockierte oder lang andauernde Abfragen Ressourcenengpässe, ungeeignete Abfragepläne, und entstehen kann. Dieses Thema enthält Informationen zum Performance-Probleme mit dynamischen Verwaltungsansichten erkennen.

SQL-Datenbank unterstützt teilweise dynamische Verwaltungsansichten drei Kategorien:

- Datenbankbezogene dynamische Verwaltungsansichten.
- Ausführung bezogenen dynamische Verwaltungsansichten.
- Buchung dynamische Verwaltungsansichten.

Detaillierte Informationen für dynamische Ansichten finden Sie unter [dynamische Verwaltungsansichten und Funktionen (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in der SQL Server-Onlinedokumentation.

## <a name="permissions"></a>Berechtigungen

SQL Datenbank erfordert eine dynamische Ansicht Abfragen **ANSICHTSZUSTAND Datenbank** Berechtigungen. Die **VIEW DATABASE STATE** -Berechtigung gibt Informationen über alle Objekte in der aktuellen Datenbank zurück.
Um einem bestimmten Datenbankbenutzer die **VIEW DATABASE STATE** -Berechtigung erteilen, führen Sie die folgende Abfrage:

```GRANT VIEW DATABASE STATE TO database_user; ```

In lokalen SQL Server-Instanz zurückgeben dynamische Verwaltungsansichten Serverinformationen. SQL-Datenbank zurückgegeben Informationen über die aktuelle logische.

## <a name="calculating-database-size"></a>Berechnung der Datenbankgröße

Die folgende Abfrage gibt die Größe der Datenbank (in MB):

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Die folgende Abfrage gibt die Größe der einzelnen Objekte (in MB) in der Datenbank:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Verbindungen

[Sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) -Ansicht können Sie Informationen über die Verbindungen zu einem bestimmten Azure SQL-Datenbank-Server und die Details jeder Verbindung abzurufen. Darüber hinaus ist die [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) Ansicht hilfreich beim Abrufen von Informationen über alle aktiven Verbindungen und interne Vorgänge.
Die folgende Abfrage ruft Informationen über die aktuelle Verbindung:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Bei **dm_exec_requests** und **sys.dm_exec_sessions Ansichten** **VIEW DATABASE STATE** Berechtigung in der Datenbank alle ausgeführten Sessions in der Datenbank angezeigt. Andernfalls wird die aktuelle Sitzung angezeigt.

## <a name="monitoring-query-performance"></a>Abfrageperformance überwachen

Langsam oder lange Ausführung von Abfragen kann signifikante Systemressourcen belegen. Dieser Abschnitt veranschaulicht, wie dynamische Verwaltungsansichten einige allgemeine Abfrage Leistungsprobleme zu erkennen. Eine ältere aber dennoch hilfreich für die Problembehandlung findet Artikel [Problembehandlung bei Leistungsproblemen in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) in Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Suchen von Top N-Abfragen

Das folgende Beispiel gibt Informationen zu den oberen fünf Abfragen durchschnittliche CPU-Zeit Rangfolge. In diesem Beispiel aggregiert Abfragen nach deren Hash Abfrage, deren kumulative Ressourcenverbrauch logisch äquivalent Abfragen gruppiert werden.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Überwachen von blockierten Abfragen

Langsame oder lang andauernde Abfragen können zu übermäßiger Ressourcenverbrauch und aus blockierten Abfragen. Die Ursache für die Sperrung kann schlechtes Anwendungsdesign, fehlerhaften Abfrageplänen fehlen nützlicher Indizes und So weiter. Sys.dm_tran_locks-Ansicht können Sie Informationen zum aktuellen Sperraktivität Ihrer Azure SQL-Datenbank abrufen. Code, siehe [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in der SQL Server-Onlinedokumentation.

### <a name="monitoring-query-plans"></a>Überwachen von Abfrageplänen

Ineffizienten Abfrageplanes kann auch CPU-Verbrauch erhöhen. Das folgende Beispiel verwendet die [dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) bestimmen, welche Abfrage die kumulative CPU verwendet.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Siehe auch

[Einführung in SQL-Datenbank](sql-database-technical-overview.md)
