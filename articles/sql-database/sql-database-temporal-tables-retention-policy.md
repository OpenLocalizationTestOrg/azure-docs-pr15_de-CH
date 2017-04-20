<properties
   pageTitle="Verwalten von Verlaufsdaten in temporäre Tabellen mit Aufbewahrungsrichtlinie | Microsoft Azure"
   description="Informationen Sie zum zeitlichen Aufbewahrungsrichtlinie Verlaufsdaten unter Ihre Kontrolle zu verwenden."
   services="sql-database"
   documentationCenter=""
   authors="bonova"
   manager="drasumic"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="10/12/2016"
   ms.author="bonova"/>

#<a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Verwalten von Verlaufsdaten in temporäre Tabellen mit Aufbewahrungsrichtlinien

Temporäre Tabellen können Datenbank mehr als normale Tabellen erhöhen, besonders wenn Verlaufsdaten für längere Zeit beibehalten. Aufbewahrungsrichtlinie Verlaufsdaten ist daher ein wichtiger Aspekt der Planung und Verwaltung des Lebenszyklus aller temporären Tabellen. Temporäre Tabellen in Azure SQL-Datenbank sind mit Halterung, mit der Sie diese Aufgabe einfach zu verwenden.

Zeitliche Verlauf beibehalten kann Richtlinien ermöglicht Benutzern das Erstellen von flexiblen Alterung Ebene einzelne Tabelle konfiguriert. Zeitliche Aufbewahrung angewendet ist einfach: Es erfordert nur einen Parameter bei Erstellung oder Schema Tabelle festgelegt werden.

Nach dem Definieren der Aufbewahrungsrichtlinie startet Azure SQL-Datenbank regelmäßig überprüft wird, ob historisch Zeilen, die für die automatische Bereinigung. Kennung der übereinstimmenden Zeilen und aus der Tabelle auftreten transparent Hintergrundtasks, die geplant und vom System ausgeführt. Alter Bedingung für die Tabellenzeilen Verlauf wird basierend auf der Spalte für Ende SYSTEM_TIME überprüft. Wenn Aufbewahrungszeit, z. B. sechs Monate festzulegen, erfüllen Tabellenzeilen für Bereinigung folgende Bedingung:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

Im obigen Beispiel davon ausgegangen, dass Ende SYSTEM_TIME **ValidTo** Spalte entspricht.

##<a name="how-to-configure-retention-policy"></a>So konfigurieren Sie Aufbewahrungsrichtlinie?

Vor dem Konfigurieren der Aufbewahrungsrichtlinie für eine temporäre Tabelle zuerst, ob zeitliche historisch Aufbewahrung aktiviert *auf Datenbankebene*ist.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Datenbank-Flag **Is_temporal_history_retention_enabled** ist standardmäßig auf, aber Benutzer können mit ALTER DATABASE-Anweisung ändern. Es ist auch nach [Zeitpunkt wiederherstellen](sql-database-point-in-time-restore-portal.md) -Vorgang auf OFF festgelegt. Um zeitliche Aufbewahrung Verlaufscleanup für die Datenbank zu aktivieren, führen Sie folgende Anweisung:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [AZURE.IMPORTANT] Archivierung für temporäre Tabellen können auch wenn **Is_temporal_history_retention_enabled** ist deaktiviert automatische Bereinigung alter Zeilen nicht in diesem Fall ausgelöst werden.


Aufbewahrungsrichtlinie wird beim Erstellen der Tabelle Wert für den Parameter HISTORY_RETENTION_PERIOD konfiguriert:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Azure SQL-Datenbank können Sie unterschiedliche Einheiten mit Aufbewahrungsdauer angeben: Tage, Wochen, Monate und Jahre. HISTORY_RETENTION_PERIOD fehlt, wird UNENDLICH Aufbewahrung. Sie können auch explizit INFINITE-Schlüsselwort verwenden.

In einigen Szenarien Aufbewahrung nach der Erstellung von Tabellen konfigurieren möchten oder zum Ändern zuvor konfigurierten Wert. Verwenden Sie in diesem Fall ALTER TABLE-Anweisung:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [AZURE.IMPORTANT]  SYSTEM_VERSIONING *behält nicht* aufbewahrungswert OFF festlegen. SYSTEM_VERSIONING auf ON festgelegt, ohne HISTORY_RETENTION_PERIOD Ergebnisse in die unbegrenzte Aufbewahrungsdauer explizit angegeben.

Überprüfen Sie Stand der Aufbewahrungsrichtlinie mithilfe der folgenden Abfrage, die zeitliche Aufbewahrung Enablement-Flag auf Datenbankebene Aufbewahrungszeiträume für einzelne Tabellen verknüpft:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


##<a name="how-sql-database-deletes-aged-rows"></a>Wie SQL Datenbank löscht altern Zeilen?

Der Cleanup-Prozess hängt das Indexlayout der Tabelle. Es ist wichtig zu beachten, dass *nur Verlaufstabellen mit einem gruppierten Index (B-Struktur oder Columnstore) begrenzte Aufbewahrungsrichtlinie konfiguriert haben*. Hintergrund wird für alle temporären Tabellen mit begrenzten Aufbewahrungsdauer Aufräumarbeiten veralteter Daten erstellt.
Bereinigungslogik für den gruppierten Index Rowstore (B-Struktur) löscht veraltete Zeile in kleineren Segmenten (bis zu 10 K) Druck auf die Datenbank und e/a-Subsystem minimieren. Obwohl Bereinigungslogik erforderlichen B-Baum-Index Reihenfolge von Löschvorgängen für die Zeilen, die älter nutzt als die Aufbewahrungsdauer fest garantiert werden kann. Daher *werden keine Abhängigkeit von der Bereinigung Reihenfolge in Ihrer Anwendung*.

Die Cleanuptask für gruppierte Columnstore entfernt gesamte [Zeilengruppen](https://msdn.microsoft.com/library/gg492088.aspx) gleichzeitig (in der Regel enthalten 1 Million Zeilen), das ist sehr effizient, insbesondere, wenn Daten in einem hohen Tempo generiert werden.

![Gruppierte Columnstore Aufbewahrung](./media/sql-database-temporal-tables-retention-policy/cciretention.png)


Hervorragende Datenkompression und effiziente Archivierung die Bereinigung clustered-Indexes Columnstore ideal für Szenarien Wenn Ihre Arbeitslast schnell viel Daten generiert. Dieses Muster ist für Überarbeitungen und Überwachung, Trendanalyse und IoT Daten Einnahme intensive [transaktionale Verarbeitung Arbeitslasten, die temporäre Tabellen verwenden](https://msdn.microsoft.com/library/mt631669.aspx) .

##<a name="index-considerations"></a>Index-Hinweise

Die Cleanuptask für Tabellen mit Rowstore gruppierter Index erfordert Index zu Ende SYSTEM_TIME entsprechende Spalte. Wenn dieser Index existiert möglich nicht begrenzte Aufbewahrungsdauer konfigurieren:

*Msg 13765, Ebene 16, Status 1 <br> </br> Einstellung begrenzte Aufbewahrungsdauer fehlgeschlagen System Versionsangabe temporäre Tabelle 'temporalstagetestdb.dbo.WebsiteUserInfo' die Tabelle 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' nicht erforderlichen gruppierten Index enthalten. Erstellen einer gruppierten Columnstore oder B-Baumstruktur mit der Spalte, die Ende des SYSTEM_TIME entspricht Periode in der Tabelle.*

Es ist wichtig zu beachten, dass die Standardwert Tabelle Azure SQL-Datenbank bereits erstellt Index clustered hat die Aufbewahrungsrichtlinie entspricht. Beim Entfernen dieses Indexes für eine Tabelle mit begrenzten Aufbewahrungsdauer fehl mit Fehler:

*Msg 13766, Ebene 16, Status 1 <br> </br> den gruppierten Index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' kann nicht gelöscht werden, da sie für die automatische Bereinigung von veralteten Daten verwendet wird. Legen Sie HISTORY_RETENTION_PERIOD unendlich in der entsprechenden System Versionsangabe temporären Tabelle möchten Sie diesen Index löschen.*

Cleanup für gruppierte Columnstore-Index funktioniert optimal, wenn Verlaufsdaten in aufsteigender Reihenfolge (Ende der Spalte angegebenen Periode bestellt), Einfügen von Zeilen immer der Fall ist, die Tabelle exklusiv durch SYSTEM_VERSIONIOING Mechanismus ausgefüllt wird. Wenn Zeilen in der Tabelle nicht nach Ende der Spalte angegebenen Periode bestellt werden (was der Fall ist, wenn Sie vorhandene Daten zur Versionsgeschichte migriert), sollten Sie Columnstore gruppierter Index auf B-Baumstruktur Rowstore neu erstellen, die ordnungsgemäß sortiert um optimale Leistung zu erzielen.

Vermeiden Sie gruppierte Columnstore-Index auf der Tabelle mit den begrenzten Aufbewahrungsdauer neu, da variieren natürlich auferlegten der Operation System Versioning Zeilengruppen bestellt. Benötigen Sie gruppierte Columnstore-Index auf der Tabelle neu erstellen, dazu Neuerstellen auf kompatibel B-Baum-Index für reguläre Daten bereinigen Rowgroups Reihenfolge beibehalten. Der gleiche Ansatz muss ausgeführt werden, wenn zeitliche Tabelle mit Verlauf, der Spaltenindex ohne garantierte Daten Cluster hat:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Begrenzte Aufbewahrungsdauer für die Tabelle mit den gruppierten Columnstore konfiguriert, erstellen keine zusätzlichen nicht gruppierten B-Baum-Indizes auf Tabelle:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Ausführung über-Anweisung fehl mit Fehler:

*Msg 13772, Ebene 16, Status 1 <br> </br> kann nicht gruppierten Index erstellt eine temporäre Tabelle 'WebsiteUserInfoHistory' begrenzte Aufbewahrungsdauer und Columnstore gruppierter Index definiert.*

##<a name="querying-tables-with-retention-policy"></a>Abfragen in Tabellen mit Aufbewahrungsrichtlinien

Alle Abfragen in der temporären Tabelle filtern automatisch historisch Ergebniszeilen begrenzte Aufbewahrungsrichtlinie zu unvorhersehbaren und inkonsistente Ergebnisse, da veraltete Zeilen von der Cleanuptask *jederzeit und in beliebiger Reihenfolge*gelöscht werden können.

Die folgende Abbildung zeigt den Abfrageplan für eine einfache Abfrage:

````
SELECT * FROM dbo.WebsiteUserInfo FROM SYSTEM_TIME ALL;
````

Abfrageplan enthält zusätzlichen Filter angewendet, Ende der Periode Spalte (ValidTo) in den Operator "Clustered Index Scan" auf die Tabelle (hervorgehoben). Angenommen, 1 Monat Aufbewahrungsdauer WebsiteUserInfo Tabelle festgelegt wurde.

![Abfragefilter Aufbewahrung](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Wenn Sie Tabelle direkt Abfragen, können Sie Zeilen sehen, die älter als angegebenen einzubehaltenden Periode, ohne Gewähr für wiederholbare Abfrageergebnisse. Das folgende Bild zeigt Abfrageausführungsplan für die Abfrage für die Tabelle ohne zusätzliche Filter angewendet:

![Abfragen ohne Aufbewahrung filter](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Nicht auf Ihre Geschäftslogik Tabelle über Aufbewahrungsdauer erhalten Sie möglicherweise inkonsistente oder unerwartete Ergebnisse lesen. Es wird empfohlen, zeitliche Abfragen zum Analysieren von Daten in temporäre Tabellen für SYSTEM_TIME-Klausel verwenden.

##<a name="point-in-time-restore-considerations"></a>Point-in-Time wiederherstellen Aspekte

Beim Erstellen der neuen Datenbank durch [Wiederherstellen der Datenbank zu einem bestimmten Zeitpunkt](sql-database-point-in-time-restore-portal.md)hat zeitliche Aufbewahrung auf Datenbankebene deaktiviert. (**Is_temporal_history_retention_enabled** -Flag auf OFF festgelegt). Diese Funktionalität ermöglicht Ihnen alle Verlaufsdaten Zeilen bei der Wiederherstellung, ohne, dass Alter Zeilen entfernt werden, bevor Sie diese Abfrage untersuchen. Sie können überprüfen, *Verlaufsdaten über konfigurierte Aufbewahrungsdauer*.

Angenommen Sie, dass eine temporäre Tabelle einen Monat Archivierung des angegebenen Zeitraums. Wenn Ihre Datenbank in Premium-Service-Tier erstellt wurde, würde Sie Datenbankkopie mit den Zustand der Datenbank bis zu 35 Tagen in der Vergangenheit erstellen können. Dadurch effektiv können Sie Verlaufsdaten Zeilen analysieren, die bis zu 65 Tage die Tabelle direkt abgefragt werden.

Wenn zeitliche Aufbewahrung Bereinigung aktivieren möchten, führen Sie die folgende Transact-SQL-Anweisung nach Punkt Zeitpunkt:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

##<a name="next-steps"></a>Nächste Schritte

Temporäre Tabellen in der Anwendung Auschecken zum Erlernen [Einstieg in temporäre Tabellen in Azure SQL-Datenbank](sql-database-temporal-tables.md).

Besuchen Sie Channel 9 eine [echte Erfolgsstory der zeitliche Implementierung](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) und sehen Sie sich eine [live-Demonstration zeitliche](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Detaillierte Informationen zu temporären Tabellen finden Sie in [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn935015.aspx).
