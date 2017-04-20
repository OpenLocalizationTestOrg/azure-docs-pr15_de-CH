<properties
    pageTitle="Verwaltung und Problembehandlung Stretch Datenbank | Microsoft Azure"
    description="Informationen Sie zur Verwaltung und Fehlerbehebung Stretch-Datenbank."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Verwaltung und Problembehandlung von Stretch-Datenbank

Verwalten und Strecken Datenbank beheben, Werkzeuge und Methoden in diesem Thema beschrieben.

## <a name="manage-local-data"></a>Verwalten von lokalen Daten

### <a name="LocalInfo"></a>Informationen Sie über lokale Datenbanken und Tabellen für Strecken Datenbank aktiviert
Öffnen Katalog Ansichten **sys.databases** und **' sys.Tables '** Info Strecken an\-SQL Server-Datenbanken und Tabellen aktiviert. Weitere Informationen finden Sie unter [sys.databases (Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) und [' sys.Tables ' (Transact-SQL)](https://msdn.microsoft.com/library/ms187406.aspx).

Auf wie viel Speicherplatz ein\-aktivierte Tabelle in SQL Server führen Sie folgende Anweisung verwenden.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Verwalten der Datenmigration

### <a name="check-the-filter-function-applied-to-a-table"></a>Überprüfen Sie die Filterfunktion auf eine Tabelle angewendet
Öffnen Sie die Katalogansicht **sys.remote\_Daten\_Archiv\_Tabellen** und den Wert der **Filter\_Prädikat** Spalte zu Strecken Datenbank migrieren Zeilenauswahl verwendet die Funktion. Wenn der Wert null ist, kann die gesamte Tabelle migriert werden. Weitere Informationen finden Sie unter [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) , und [Wählen Sie Zeilen mithilfe einer Filterfunktion migrieren](sql-server-stretch-database-predicate-function.md).

### <a name="Migration"></a>Überprüfen Sie den Status der Datenmigration
Wählen Sie **Aufgaben | Stretch | Monitor** für eine Datenbank in SQL Server Management Studio Datenmigration Abschnitt Datenbank Monitor überwachen. Weitere Informationen finden Sie unter [Überwachung und Problembehandlung bei der Datenmigration (Stretch Datenbank)](sql-server-stretch-database-monitor.md).

Öffnen Sie die dynamische Verwaltungsansicht **sys.dm\_Db\_rda\_Migration\_Status** zu sehen, wie viele Batches und Datenzeilen migriert wurden.

### <a name="Firewall"></a>Problembehandlung bei der Datenmigration
Vorschläge zur Problembehandlung, finden Sie unter [Überwachung und Problembehandlung bei der Datenmigration (Stretch Datenbank)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Verwalten von remote-Daten

### <a name="RemoteInfo"></a>Informationen Sie zu entfernten Datenbanken und Tabellen Stretch-Datenbank
Öffnen Sie die Katalogsichten **sys.remote\_Daten\_Archiv\_Datenbanken** und **sys.remote\_Daten\_Archiv\_Tabellen** , Informationen über die entfernten Datenbanken und Tabellen in der migrierte Daten gespeichert. Weitere Informationen finden Sie unter [sys.remote_data_archive_databases (Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) und [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

Um zu sehen, wie viel Speicherplatz wird mithilfe einer Tabelle Strecken aktiviert in Azure, führen Sie folgende Anweisung.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Löschen von migrierten Daten  
Möchten Sie Daten löschen, die in Azure migriert wurde, befolgen Sie die Schritte im [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx).

## <a name="manage-table-schema"></a>Tabellenschema verwalten

### <a name="dont-change-the-schema-of-the-remote-table"></a>Das Schema der entfernten Tabelle nicht ändern
Ändern Sie nicht das Schema einer entfernten Azure Tabelle, die mit SQL Server konfiguriert Stretch-Datenbank verknüpft ist. Insbesondere ändern Sie nicht den Namen oder den Datentyp einer Spalte. Der Stretch Datenbank-Funktion können verschiedene Annahmen über das Schema der entfernten Tabelle in Bezug auf das Schema der SQL Server-Tabelle. Wenn Sie das entfernte Schema ändern, funktioniert Stretch-Datenbank für die geänderte Tabelle.

### <a name="reconcile-table-columns"></a>Abstimmen von Tabellenspalten  
Spalten in der entfernten Tabelle versehentlich gelöscht haben, führen Sie **Sp_rda_reconcile_columns** zum Hinzufügen von Spalten zu, die in der entfernten Tabelle\-SQL Server-Tabelle aktiviert, jedoch nicht in der entfernten Tabelle. Weitere Informationen finden Sie unter [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx).  

  > [!IMPORTANT] Wenn **Sp_rda_reconcile_columns** Spalten, die versehentlich aus der entfernten Tabelle gelöscht erstellt, wird er nicht gelöschten Spalten zuvor die Daten wiederherstellen.

**Sp_rda_reconcile_columns** löscht keine Spalten aus der entfernten Tabelle in der entfernten Tabelle jedoch nicht in der vorhanden\-SQL Server-Tabelle aktiviert. Spalten in der entfernten Tabelle Azure, die nicht mehr in der existieren\-ermöglicht SQL Server Tabelle diese zusätzlichen Spalten nicht verhindern Stretch Datenbank normal. Sie können die zusätzlichen Spalten manuell entfernen.  

## <a name="manage-performance-and-costs"></a>Verwalten von Performance und Kosten  

### <a name="troubleshoot-query-performance"></a>Problembehandlung bei Leistung
Abfragen, Strecken\-aktivierte Tabellen sollen langsamer als zuvor Tabellen für Abschnitt aktiviert wurden. Wenn Leistung deutlich beeinträchtigt, überprüfen Sie folgende mögliche Probleme.

-   Ist der Azure-Server in einer anderen geographischen Region als Ihr SQL Server? Konfigurieren Sie Ihren Azure-Server in derselben geographischen Region als Ihr SQL Server Netzwerkwartezeit zu reduzieren.

-   Die Netzwerkprobleme können beeinträchtigt haben. Erhalten Sie vom Netzwerkadministrator Informationen über neue Probleme oder Ausfälle.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Azure Leistung erhöhen Ressource\-intensive Vorgänge wie Indizierung
Berücksichtigen Sie beim Erstellen, neu erstellen oder neu organisieren einen Index für eine große Tabelle Stretch-Datenbank konfiguriert ist und erwarten schwere Abfragen von migrierten Daten in Azure während dieser Zeit erhöhen die Leistung der entsprechenden Azure Remotedatenbank für die Dauer des Vorgangs. Weitere Informationen zu Leistung und Preise finden Sie in der [SQL Server Stretch Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Den SQL Server Stretch-Datenbankdienst in Azure kann nicht angehalten werden.  
 Stellen Sie sicher, dass angemessene Performance und Preisstufe auswählen. Erhöht die Leistungsfähigkeit für eine Ressource\-aufwändiger Vorgang nach Abschluss des Vorgangs auf dem vorherigen Niveau wiederherzustellen. Weitere Informationen zu Leistung und Preise finden Sie in der [SQL Server Stretch Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  

## <a name="change-the-scope-of-queries"></a>Ändern des Bereichs Abfragen  
 Abfragen in Abschnitt\-aktivierte Tabellen standardmäßig sowohl lokale als auch Daten zurückgeben. Sie können die Abfragen für alle Abfragen alle Benutzer oder nur für eine einzelne Abfrage vom Administrator ändern.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Ändern des Bereichs für alle Abfragen alle Benutzer Abfragen  
 Aller Abfragen alle Benutzer ändern, führen Sie die gespeicherte Prozedur **sys.sp_rda_set_query_mode**. Reduzieren Sie den Bereich lokale Daten Abfragen, alle Abfragen deaktivieren oder die Standardeinstellung wiederherstellen. Weitere Informationen finden Sie unter [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx).  

### <a name="queryHints"></a>Ändern des Bereichs von Abfragen für eine einzelne Abfrage durch den administrator  
 Hinzufügen einer einzelnen Abfrage von einem Mitglied der Db_owner-Rolle ändern, die * *mit \( REMOTE_DATA_ARCHIVE_OVERRIDE = *Wert* \)** Abfragehinweis zur SELECT-Anweisung. Der Abfragehinweis REMOTE_DATA_ARCHIVE_OVERRIDE kann die folgenden Werte aufweisen.  
 -   **LOCAL_ONLY**. Lokale Daten Abfragen.  

 -   **REMOTE_ONLY**. Entfernte Daten Abfragen.  

 -   **STAGE_ONLY**. Abfrage nur die Daten in der Tabelle, dehnen Datenbank Zeilen für die Migration und behält migrierte Zeilen für den angegebenen Zeitraum nach der Migration. Dieser Abfragehinweis ist die einzige Möglichkeit zum Abfragen der Stagingtabelle.  

Beispielsweise gibt die folgende Abfrage lokales Ergebnisse.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Administrative Updates und löscht  
 Standardmäßig können nicht aktualisiert oder Zeilen löschen, die für die Migration oder Zeilen, die bereits, in einem migriert wurden\-Tabelle aktiviert. Wenn Sie ein Problem beheben müssen, kann Mitglied der Rolle Db_owner durch Hinzufügen eine Update- oder DELETE-Operation Ausführen der * *mit \( REMOTE_DATA_ARCHIVE_OVERRIDE = *Wert* \)** Abfragehinweis der Anweisung. Der Abfragehinweis REMOTE_DATA_ARCHIVE_OVERRIDE kann die folgenden Werte aufweisen.  
 -   **LOCAL_ONLY**. Aktualisieren oder Löschen von lokalen Daten.  

 -   **REMOTE_ONLY**. Aktualisieren oder Löschen von remote-Daten.  

 -   **STAGE_ONLY**. Aktualisieren oder Löschen der Daten in der Tabelle, dehnen Datenbank Zeilen für die Migration und behält migrierte Zeilen für den angegebenen Zeitraum nach der Migration.  

## <a name="see-also"></a>Siehe auch

[Stretch-Monitor-Datenbank](sql-server-stretch-database-monitor.md)

[Sicherungsdatenbanken Strecken aktiviert](sql-server-stretch-database-backup.md)

[Stretch-fähigen Datenbanken wiederherstellen](sql-server-stretch-database-restore.md)
