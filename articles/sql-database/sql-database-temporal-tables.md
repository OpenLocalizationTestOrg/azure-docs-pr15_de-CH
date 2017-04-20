<properties
   pageTitle="Erste Schritte mit temporären Tabellen in SQL Azure-Datenbank | Microsoft Azure"
   description="Informationen Sie zum Einstieg temporäre Tabellen in Azure SQL-Datenbank."
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
   ms.workload="sql-database"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

#<a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Erste Schritte mit temporären Tabellen in SQL Azure-Datenbank

Temporäre Tabellen sind eine neue Programmierbarkeit von Azure SQL-Datenbank, mit der Sie zu überwachen und analysieren Sie das vollständige Änderungsprotokoll in Ihren Daten ohne benutzerdefinierte Codierung. Temporäre Tabellen Daten eng mit jedem gespeicherte Fakten interpretiert werden können, als gültig nur in bestimmten Zeitraum. Diese Eigenschaft der temporäre Tabellen ermöglicht effiziente Zeit und die erste Einblicke Daten Entwicklung.

##<a name="temporal-scenario"></a>Zeitliche Szenario

Dieser Artikel beschreibt die Schritte, um temporäre Tabellen in einem Anwendungsszenario nutzen. Nehmen Sie zum Nachverfolgen von Benutzeraktivitäten auf einer neuen Website von Grund auf neu entwickelt oder einer vorhandenen Website Benutzer Aktivität Analytics erweitern möchten. In diesem vereinfachten Beispiel wird davon ausgegangen, dass die Anzahl der besuchten Webseiten einen Zeitraum Indikator ist, erfasst und in der Website-Datenbank, die auf Azure SQL-Datenbank überwacht werden. Ziel der Historie der Benutzeraktivität ist zu Eingaben Website und bieten optimal für Besucher.

Das Datenbankmodell für dieses Szenario ist sehr einfach – Benutzer Aktivität Metrik wird mit einem einzelnen ganzzahligen Feld **PageVisited**dargestellt und sowie Informationen im Benutzerprofil erfasst. Darüber hinaus würde für zeitbasierte Analyse sollen eine Reihe von Zeilen für jeden Benutzer, wobei jede Zeile die Anzahl der Seiten ein bestimmtes Benutzers in einem bestimmten Zeitraum darstellt.

![Schema](./media/sql-database-temporal-tables/AzureTemporal1.png)

Glücklicherweise müssen Sie keine in Ihrer Anwendung verwalten diese Mühe. Mit temporären Tabellen ist dieser Prozess automatisiert, wodurch volle Flexibilität beim Webdesign und mehr Zeit der Analyse selbst -. Sie müssen lediglich sicherstellen, dass als **WebSiteInfo** Tabelle konfiguriert ist [zeitliche System Version](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). Temporäre Tabellen in diesem Szenario nutzen die genauen Schritte sind unten beschrieben.

##<a name="step-1-configure-tables-as-temporal"></a>Schritt 1: Konfigurieren von Tabellen als zeitliche

Je nachdem, ob neue Entwicklung oder vorhandene Anwendung aktualisieren Sie temporäre Tabellen erstellen oder vorhandene ändern, indem temporäre Attribute hinzufügen. Fall das Szenario im Allgemeinen kann eine Mischung aus diesen beiden Optionen. Diese Aktion mit [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS) in [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) oder anderen Transact-SQL-Entwicklungstool.


> [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


###<a name="create-new-table"></a>Neue Tabelle erstellen

Verwenden Sie Kontextmenü "Neues System Versionsangabe Table" im Objekt-Explorer SSMS zu den Abfrage-Editor mit einer zeitlichen Tabellenskript Vorlage öffnen "Geben Sie Werte für Vorlagenparameter" (STRG + UMSCHALT + M), füllen die Vorlage:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

"Temporäre Tabelle (System Version)" Vorlage ausgewählt in SSDT Wenn das Projekt neue Elemente hinzufügen. Werden Tabellen-Designer öffnen und aktivieren Sie einfach das Layout angeben:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Sie können auch eine temporäre Tabelle erstellen durch Angabe der Transact-SQL-Anweisung, wie im folgenden Beispiel gezeigt. Beachten Sie, dass die obligatorische Elemente aller temporären Tabellen die Definition der und die SYSTEM_VERSIONING-Klausel mit einer anderen Benutzertabelle, die historisch Zeilenversionen gespeichert werden:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Beim System Versionsangabe temporäre Tabelle erstellen, wird die zugehörige Tabelle in der Standardkonfiguration automatisch erstellt. Die Verlauf-Tabelle enthält einen gruppierten Index B-Struktur in den Periodenspalten (Ende, Anfang) mit aktivierter seitenkomprimierung. Diese Konfiguration ist für die meisten Szenarios, in denen temporäre Tabellen, insbesondere für die [Überwachung von Daten verwendet werden](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

In diesem Fall soll ausführt zeitbasierte Trendanalyse mehr Daten Verlauf und mit größeren Datenmengen Speicherauswahl für die Tabelle einen gruppierten Columnstore-Index ist. Gruppierte Columnstore bietet gute Komprimierung und Leistung für analytische Abfragen. Temporäre Tabellen können Sie Indizes für die Tabellen aktuellen und zeitlichen unabhängig konfigurieren. 

**Hinweis**: Columnstore Indizes sind nur in Premium-Service-Tier.

Das folgende Skript zeigt, wie Standardindex Tabelle gruppierte Columnstore geändert werden kann:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Temporäre Tabellen werden im Objekt-Explorer mit bestimmten Symbols zur leichteren Identifizierung dargestellt, während die Tabelle als untergeordnete Knoten angezeigt.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

###<a name="alter-existing-table-to-temporal"></a>Vorhandene Tabelle zeitliche ändern

Wir decken alternative Szenario in dem Tabelle WebsiteUserInfo bereits vorhanden, jedoch kein Änderungsprotokoll beibehalten soll. In diesem Fall können Sie die vorhandene Tabelle zu zeitliche, wie im folgenden Beispiel gezeigt einfach erweitern:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

##<a name="step-2-run-your-workload-regularly"></a>Schritt 2: Führen Sie Ihre Arbeit regelmäßig

Der Hauptvorteil von temporären Tabellen ist, nicht ändern oder Anpassen Ihrer Website an Änderungsprotokoll führen müssen. Erstellte, bleiben temporäre Tabellen transparent Vorgängerversionen Zeile jedes Mal Änderungen für Ihre Daten auszuführen. 

Um automatische Änderungsprotokoll für dieses spezielle Szenario nutzen wir nur Spalte aktualisiert **PagesVisited** jedes Mal, wenn Benutzer seine Sitzung auf der Website beendet:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Es ist wichtig zu beachten, dass die Update-Abfrage nicht beim Betrieb auftreten und wie Daten zur späteren Analyse erhalten die genaue Zeit kennen. Beide Aspekte werden von Azure SQL-Datenbank automatisch behandelt. Das folgende Diagramm veranschaulicht, wie Daten auf alle Updates generiert wird.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

##<a name="step-3-perform-historical-data-analysis"></a>Schritt 3: Daten analysieren

Wenn zeitliche System Versionskontrolle aktiviert ist, ist Verlaufsdaten Analyse nur eine Abfrage Weg. In diesem Artikel bieten einige Beispiele wir diese Adresse Analyse Szenarien - alle Informationen, die verschiedene Optionen mit der Klausel [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) eingeführt.

Um den Top 10-Benutzer die Anzahl der besuchten Webseiten ab Stunde bestellten anzuzeigen, führen Sie diese Abfrage:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Sie können diese Abfrage Besuche einen Tag vor einem Monat analysieren ändern oder jederzeit in der Vergangenheit soll.

Um grundlegende statistische Analysen für den vorherigen Tag auszuführen, verwenden Sie Folgendes Beispiel:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Für die Suche nach Aktivitäten eines bestimmten Benutzers in einem Zeitraum enthalten IN-Klausel verwenden:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Graphic Visualization ist besonders für zeitliche Abfragen Sie Trends und Nutzungsmuster in ein intuitives sehr leicht anzeigen können:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

##<a name="evolving-table-schema"></a>Tabellenschema entwickelt

In der Regel müssen Sie dabei Anwendungsentwicklung zeitliche Tabellenschema ändern. Wird führen Sie reguläre ALTER TABLE-Anweisung und Azure SQL-Datenbank entsprechend in die Historientabelle weiterleiten. Das folgende Skript zeigt, wie Sie zusätzliche Attribute für die Überwachung hinzufügen können:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Ebenso können Sie Spaltendefinition ändern, während die Arbeitslast aktiv ist:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Schließlich können Sie eine Spalte entfernen, die Sie nicht mehr benötigen.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````
    
Alternativ neueste [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) zeitliche Tabellenschema ändern, während die Verbindung mit der Datenbank (Onlinemodus) oder als Teil des Datenbankprojekts (offline-Modus).

##<a name="controlling-retention-of-historical-data"></a>Steuern der Aufbewahrung von Daten

Mit System Versionsangabe temporäre Tabellen erhöht die Tabelle der Datenbank mehr als normale Tabellen. Eine große und wachsende Tabelle werden ein Problem sowohl reine Speicherkosten sowie zur Einführung einer Leistung zeitliche Abfragen steuern. Entwickeln einer Archivierungsrichtlinie zur Verwaltung von Daten in der Tabelle ist daher ein wichtiger Aspekt der Planung und Verwaltung des Lebenszyklus aller temporären Tabellen. Azure SQL-Datenbank müssen Sie die folgenden Methoden zum Verwalten von Verlaufsdaten in der temporären Tabelle:

- [Tabellenpartitionierung](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
- [Benutzerdefinierte Bereinigungsskript](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

##<a name="next-steps"></a>Nächste Schritte

Ausführliche Informationen zu temporären Tabellen finden Sie [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn935015.aspx).
Besuchen Sie Channel 9 eine [echte Erfolgsstory der zeitliche Implementierung](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) und sehen Sie sich eine [live-Demonstration zeitliche](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).
