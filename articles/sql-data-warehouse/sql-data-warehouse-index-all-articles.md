<properties
    pageTitle="Alle Themen für SQL Data Warehouse Dienst | Microsoft Azure"
    description="Alle Themen Azure Service Namen SQL Data Warehouse auf http://azure.microsoft.com/documentation/articles/, Titel und Beschreibung."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Alle Themen für Azure SQL Data Warehouse-Dienst

Dieses Thema listet jedes Thema direkt auf den Dienst **SQL Data Warehouse** Azure angewendet wird. Diese Webseite Schlüsselwörter können mit **STRG + F**, die aktuellen Themen finden.




## <a name="new"></a>Neu

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [SQL Data Warehouse backups](sql-data-warehouse-backups.md) | Erfahren Sie mehr über SQL Data Warehouse integrierte Datenbank-Backups, die Sie einer Azure SQL Data Warehouse einer anderen geographischen Region zu einem Wiederherstellungspunkt wiederherstellen können. |


## <a name="updated-articles-sql-data-warehouse"></a>Aktualisierte Artikel SQL Data Warehouse

Dieser Abschnitt enthält Artikel, die kürzlich aktualisiert wurden, wurde das Update Groß oder erheblich. Für jede aktualisierte Artikel grober Ausschnitt hinzugefügten Abzug Text angezeigt. Die Artikel wurden innerhalb des Datumsbereichs **2016-08-22** **2016 10**05 aktualisiert.

| &nbsp; | Artikel | Aktualisierter Text, Ausschnitt | Aktualisiert, wenn |
| --: | :-- | :-- | :-- |
| 2 | [Laden von Daten aus Azure BLOB-Speicher in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | /-Zum Nachverfolgen Bytes und Dateien wählen Sie r.command, s.request_id, r.status, Count (distinct Input_name) als Nbr_files, sum (s.bytes_processed) 1024/1024 als Gb_processed aus sys.dm_pdw_exec_requests r inner Join sys.dm_pdw_dms_external_work auf r.request_id s.request_id = wo R. Label = "CTAS: Load Cso. DimProduct ' oder R. Label = "CTAS: Load Cso. FactOnlineSales' GROUP BY r.command s.request_id r.status ORDER BY Nbr_files Desc, Gb_processed Desc;  | 2016-09-07 |
| 3 | [SQL Data Warehouse wiederherstellen](sql-data-warehouse-restore-database-overview.md) | **Kann ein angehaltenes Datawarehouse werden wiederhergestellt?** Um ein Datawarehouse wiederherzustellen, der angehalten wurde, müssen Sie zuerst wieder online geschaltet. Sobald das Datawarehouse wieder online ist, haben Sie sieben Tage Wiederherstellungspunkte aus. **Geo-redundant Bereich wiederherstellen** Bei Verwendung von Geo-redundanten Speicher können Sie das Datawarehouse gepaarten Datenzentrum in einer anderen geographischen Region wiederherstellen. Das Datawarehouse wird von der letzten täglichen Sicherung wiederhergestellt. **Zeitachse wiederherstellen** In den letzten sieben Tagen können Sie eine Datenbank auf einen beliebigen Wiederherstellungspunkt wiederherstellen. Snapshots alle vier bis acht Stunden und 7 Tage zur Verfügung. Wenn ein Snapshot älter als sieben Tage ist, Ablauf und der Wiederherstellungspunkt ist nicht mehr verfügbar. **Wiederherstellen von Kosten** Speicher kostenlos wiederhergestellten Datawarehouse ist Rate Azure Premium Speicher berechnet. Anhalten einer wiederhergestellten Datawarehouse werden Sie Speicher Rate Azure Premium Speicher berechnet. Anhalten der Vorteil ist, dass Sie nicht kostenlos | 2016-09-29 |





## <a name="get-started"></a>Erste Schritte

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 4 | [Authentifizierung in SQL Azure Datawarehouse](sql-data-warehouse-authentication.md) | Azure Active Directory (AAD) und SQL Server-Authentifizierung Azure SQL Data Warehouse. |
| 5 | [Best Practices für Azure SQL Data Warehouse](sql-data-warehouse-best-practices.md) | Empfehlung und best Practices, die Sie kennen sollten Lösungen für Azure SQL Data Warehouse. Diese helfen Ihnen gelingen. |
| 6 | [Treiber für Azure SQL Datawarehouse](sql-data-warehouse-connection-strings.md) | Verbindungszeichenfolgen und Treiber für SQL Data Warehouse |
| 7 | [Verbinden Sie mit SQL Azure Datawarehouse](sql-data-warehouse-connect-overview.md) | Wie Sie den Server und die Verbindungszeichenfolge Zeichenfolge für Ihre Azure SQL Data Warehouse |
| 8 | [Analysieren von Daten mit Azure maschinelles lernen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Verwenden Sie Azure Machine Learning eine vorhersehbare Maschine Lernmodell auf Azure SQL Data Warehouse gespeicherten Daten basiert. |
| 9 | [Abfrage Azure SQL Data Warehouse (Sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Abfragen von Azure SQL Data Warehouse mit dem Befehlszeilenprogramm Sqlcmd. |
| 10 | [Erstellen Sie SQL Data Warehouse-Datenbank mithilfe von Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) | Informationen Sie zum Erstellen einer Azure SQL Data Warehouse mit TSQL |
| 11 | [Erstellen Sie ein Support-Ticket für SQL Data Warehouse](sql-data-warehouse-get-started-create-support-ticket.md) | Support-Ticket in Azure SQL Data Warehouse erstellen |
| 12 | [Daten mit Azure Daten laden](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Informationen Sie zum Laden von Daten mit Azure Data Factory |
| 13 | [Laden von Daten mit SQL Data Warehouse PolyBase](sql-data-warehouse-get-started-load-with-polybase.md) | Erfahren Sie, was PolyBase ist und für Datawarehousing-Szenarien verwenden. |
| 14 | [Erstellen einer Azure SQL Datawarehouse](sql-data-warehouse-get-started-provision.md) | Erstellen Sie ein Azure SQL Data Warehouse in Azure-portal |
| 15 | [Erstellen Sie SQL Data Warehouse mit PowerShell](sql-data-warehouse-get-started-provision-powershell.md) | Erstellen Sie mithilfe von PowerShell SQL Data Warehouse |
| 16 | [Daten Sie mit Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) | Daten Sie SQL Data Warehouse mit Power BI |
| 17 | [Abfrage SQL Azure Datawarehouse (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | Abfrage SQL Datawarehouse mit Visual Studio. |



## <a name="develop"></a>Entwickeln

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 18 | [Optimieren der Transaktionen für SQL Data Warehouse](sql-data-warehouse-develop-best-practices-transactions.md) | Best Practice-Anleitung zum Schreiben von effizienten Transaktionen Updates in Azure SQL Data Warehouse |
| 19 | [Parallelität und Arbeitslast-Management im SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) | Kennen Sie Azure SQL Data Warehouse Parallelität und Arbeitslast-Management Lösungen. |
| 20 | [Erstellen Sie Tabelle als auswählen (CTAS) im SQL Datawarehouse](sql-data-warehouse-develop-ctas.md) | Tipps zur Codierung mit Create Table (CTAS) SELECT-Azure SQL Data Warehouse Lösungen. |
| 21 | [Dynamisches SQL im SQL Datawarehouse](sql-data-warehouse-develop-dynamic-sql.md) | Tipps zur Verwendung von dynamic SQL Azure SQL Data Warehouse Lösungen. |
| 22 | [Gruppe von Optionen in SQL Data Warehouse](sql-data-warehouse-develop-group-by-options.md) | Tipps für die Implementierung von Gruppe von Optionen in Azure SQL Data Warehouse Lösungen. |
| 23 | [Beschriftung Instrument Abfragen im SQL Data Warehouse](sql-data-warehouse-develop-label.md) | Tipps zur Verwendung von Etiketten Instrument Abfragen in Azure SQL Data Warehouse Lösungen. |
| 24 | [Schleifen im SQL Datawarehouse](sql-data-warehouse-develop-loops.md) | Tipps für Transact-SQL-Loops und Ersetzen Cursor in Azure SQL Data Warehouse Lösungen. |
| 25 | [Gespeicherte Prozeduren in SQL Data Warehouse](sql-data-warehouse-develop-stored-procedures.md) | Tipps für die Implementierung von gespeicherter Prozeduren in Azure SQL Data Warehouse Lösungen. |
| 26 | [Transaktionen im SQL Datawarehouse](sql-data-warehouse-develop-transactions.md) | Tipps für die Implementierung von Transaktionen in Azure SQL Data Warehouse Lösungen. |
| 27 | [Benutzerdefinierte Schemas im SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md) | Tipps für die Verwendung von Schemas Transact-SQL Azure SQL Data Warehouse Lösungen. |
| 28 | [Zuweisen von Variablen im SQL Data Warehouse](sql-data-warehouse-develop-variable-assignment.md) | Tipps für das Zuweisen von Transact-SQL-Variablen in Azure SQL Data Warehouse Lösungen. |
| 29 | [Ansichten im SQL Datawarehouse](sql-data-warehouse-develop-views.md) | Tipps zur Verwendung von Transact-SQL-Ansichten in Azure SQL Data Warehouse Lösungen. |
| 30 | [Entwurf und Codierungstechniken für SQL Data Warehouse](sql-data-warehouse-overview-develop.md) | Die Begriffe der Webentwicklung, Designentscheidungen Recommendations und Codierungstechniken für SQL Data Warehouse. |



## <a name="manage"></a>Verwalten

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 31 | [Verwaltung Compute Azure SQL Data Warehouse (Übersicht)](sql-data-warehouse-manage-compute-overview.md) | Performance-Skalierung in Azure SQL Data Warehouse-Funktionen. Skalierung von DWUs anpassen oder anhalten und Fortsetzen von computeressourcen, um Kosten zu sparen. |
| 32 | [Verwaltung Compute Azure SQL Data Warehouse (Azure Portal)](sql-data-warehouse-manage-compute-portal.md) | Azure ausgeführte Aufgaben verwalten berechnen macht. Skalierung compute-Ressourcen durch DWUs anpassen. Anhalten oder Fortsetzen von computeressourcen, um Kosten zu sparen. |
| 33 | [Verwaltung Compute Azure SQL Data Warehouse (PowerShell)](sql-data-warehouse-manage-compute-powershell.md) | PowerShell Aufgaben berechnen macht. Skalierung compute-Ressourcen durch DWUs anpassen. Anhalten oder Fortsetzen von computeressourcen, um Kosten zu sparen. |
| 34 | [Verwaltung Compute in Azure SQL Data Warehouse (REST)](sql-data-warehouse-manage-compute-rest-api.md) | PowerShell Aufgaben berechnen macht. Skalierung compute-Ressourcen durch DWUs anpassen. Anhalten oder Fortsetzen von computeressourcen, um Kosten zu sparen. |
| 35 | [Verwaltung Compute Azure SQL Data Warehouse (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) | Transact-SQL (T-SQL) Aufgaben Scale-Out-Performance durch DWUs anpassen. Kosten Sie durch Skalierung in nicht-Spitzenzeiten. |
| 36 | [Überwachen Sie Ihre Arbeitslast mit DMVs](sql-data-warehouse-manage-monitor.md) | Erfahren Sie, wie Ihre Arbeitslast mit DMVs überwachen. |
| 37 | [Azure SQL Data Warehouse-Datenbanken verwalten](sql-data-warehouse-overview-manage.md) | Übersicht über SQL Data Warehouse-Datenbanken verwalten. Enthält Verwaltungstools, DWUs und Scale-Out-Performance troubleshooting Abfrageperformance gute Sicherheitsrichtlinien einrichten und Wiederherstellen einer Datenbank von Datenkorruption oder einen regionalen Ausfall. |
| 38 | [Überwachen von Benutzerabfragen Azure SQL Data Warehouse](sql-data-warehouse-overview-manage-user-queries.md) | Übersicht über die Aspekte der best Practices und Aufgaben zum Überwachen von Benutzerabfragen Azure SQL Data Warehouse |
| 39 | [SQL Data Warehouse wiederherstellen](sql-data-warehouse-restore-database-overview.md) | Übersicht der Datenbank Wiederherstellungsoptionen für die Wiederherstellung einer Datenbank in Azure SQL Data Warehouse. |
| 40 | [Wiederherstellen einer Azure SQL Datawarehouse (Portal)](sql-data-warehouse-restore-database-portal.md) | Azure ausgeführte Aufgaben für die Wiederherstellung einer Azure SQL Data Warehouse. |
| 41 | [Wiederherstellen einer Azure SQL Datawarehouse (PowerShell)](sql-data-warehouse-restore-database-powershell.md) | PowerShell-Aufgaben für die Wiederherstellung einer Azure SQL Data Warehouse. |
| 42 | [Wiederherstellen einer Azure SQL Datawarehouse (REST-API)](sql-data-warehouse-restore-database-rest-api.md) | REST-API Aufgaben für die Wiederherstellung einer Azure SQL Data Warehouse. |



## <a name="tables-and-indexes"></a>Tabellen und Indizes

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 43 | [Datentypen für Tabellen im SQL Data Warehouse](sql-data-warehouse-tables-data-types.md) | Erste Schritte mit Datentypen für Azure SQL Data Warehouse-Tabellen. |
| 44 | [Verteilen von Tabellen im SQL Data Warehouse](sql-data-warehouse-tables-distribute.md) | Erste Schritte mit Tabellen in Azure SQL Data Warehouse verteilen. |
| 45 | [Indizierung Tabellen im SQL Data Warehouse](sql-data-warehouse-tables-index.md) | Erste Schritte mit Tabelle Indizieren in Azure SQL Data Warehouse. |
| 46 | [Übersicht über Tabellen im SQL Data Warehouse](sql-data-warehouse-tables-overview.md) | Erste Schritte mit Azure SQL Data Warehouse-Tabellen. |
| 47 | [Partitionierung von Tabellen im SQL Data Warehouse](sql-data-warehouse-tables-partition.md) | Erste Schritte mit der Tabellenpartitionierung in Azure SQL Data Warehouse. |
| 48 | [Verwalten von Statistiken für Tabellen im SQL Data Warehouse](sql-data-warehouse-tables-statistics.md) | Erste Schritte mit Statistiken für Tabellen in Azure SQL Data Warehouse. |
| 49 | [Temporäre Tabellen im SQL Data Warehouse](sql-data-warehouse-tables-temporary.md) | Erste Schritte mit temporären Tabellen in Azure SQL Data Warehouse. |



## <a name="integrate"></a>Integrieren

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 50 | [Verwenden von Azure Data Factory mit SQL Datawarehouse](sql-data-warehouse-integrate-azure-data-factory.md) | Tipps zur Verwendung von Azure Data Factory (ADF) mit Azure SQL Data Warehouse Lösungen. |
| 51 | [Verwenden von Azure maschinelles Lernen mit SQL Datawarehouse](sql-data-warehouse-integrate-azure-machine-learning.md) | Lernprogramm für mit Azure Machine Learning Azure SQL Data Warehouse Lösungen. |
| 52 | [Verwenden von Azure Stream Analytics mit SQL Datawarehouse](sql-data-warehouse-integrate-azure-stream-analytics.md) | Tipps zur Verwendung von Azure Stream Analytics Azure SQL Data Warehouse Lösungen. |
| 53 | [Verwenden von Power BI mit SQL Datawarehouse](sql-data-warehouse-integrate-power-bi.md) | Tipps zur Verwendung von Power BI Azure SQL Data Warehouse Lösungen. |
| 54 | [Andere Dienste mit SQL Data Warehouse nutzen](sql-data-warehouse-overview-integrate.md) | Tools und Partnern Lösungen, die mit SQL Data Warehouse.  |



## <a name="load"></a>Laden

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 55 | [Laden von Daten aus Azure BLOB-Speicher in Azure SQL Data Warehouse (Azure Data Factory)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Informationen Sie zum Laden von Daten mit Azure Data Factory |
| 56 | [Laden von Daten aus Azure BLOB-Speicher in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Informationen Sie zum PolyBase verwenden, um Daten von Azure BLOB-Speicher in SQL Data Warehouse zu laden. Laden Sie einige Tabellen von Daten in Contoso Retail Data Warehouse-Schema. |
| 57 | [Laden von Daten aus SQL Server in Azure SQL Data Warehouse (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Verwendet Bcp Daten aus SQL Server, Flatfiles, AZCopy von Daten in Azure BLOB-Speicher und PolyBase zum Einlesen von Daten in Azure SQL Data Warehouse exportieren. |
| 58 | [Laden von Daten aus SQL Server in Azure SQL Data Warehouse (Flatfiles)](sql-data-warehouse-load-from-sql-server-with-bcp.md) | Für eine kleine Größe verwendet Bcp Daten aus SQL Server in Dateien exportieren und importieren Sie die Daten direkt in Azure SQL Data Warehouse. |
| 59 | [Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Veranschaulicht das Erstellen eines Pakets SQL Server Integration Services (SSIS), um Daten aus einer Vielzahl von Datenquellen in SQL Data Warehouse verschieben. |
| 60 | [Laden von Daten mit SQL Data Warehouse PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Verwendet Bcp Daten aus SQL Server, Flatfiles, AZCopy von Daten in Azure BLOB-Speicher und PolyBase zum Einlesen von Daten in Azure SQL Data Warehouse exportieren. |
| 61 | [Leitfaden für die Verwendung von PolyBase im SQL Data Warehouse](sql-data-warehouse-load-polybase-guide.md) | Richtlinien und empfohlene Vorgehensweisen zur Verwendung von PolyBase im SQL Data Warehouse Szenarien. |
| 62 | [Laden Sie Beispieldaten in SQL Data Warehouse](sql-data-warehouse-load-sample-databases.md) | Laden Sie Beispieldaten in SQL Data Warehouse |
| 63 | [Laden von Daten mit "bcp"](sql-data-warehouse-load-with-bcp.md) | Erfahren Sie, welche bcp und für Datawarehousing-Szenarien verwenden. |
| 64 | [Laden von Daten in Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md) | Die Szenarien in SQL Data Warehouse Daten enthält. Dazu gehören PolyBase, Azure BLOB-Speicher, Flatfiles und Datenträger-Versand verwenden. Sie können auch Tools von Drittanbietern verwenden. |



## <a name="migrate"></a>Migrieren

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 65 | [Migrieren des SQL-Codes zu SQL Data Warehouse](sql-data-warehouse-migrate-code.md) | Tipps für das Migrieren von Code SQL Azure SQL Data Warehouse Lösungen. |
| 66 | [Migrieren von Daten](sql-data-warehouse-migrate-data.md) | Tipps zum Migrieren der Daten in Azure SQL Data Warehouse Lösungen. |
| 67 | [Data Warehouse Migration Utility (Vorschau)](sql-data-warehouse-migrate-migration-utility.md) | Migrieren Sie zu SQL Datawarehouse. |
| 68 | [Das Schema mit SQL Data Warehouse migrieren](sql-data-warehouse-migrate-schema.md) | Tipps für die Migration des Schemas zu Azure SQL Data Warehouse Lösungen. |
| 69 | [Migrieren der Lösung zu SQL Data Warehouse](sql-data-warehouse-overview-migrate.md) | Hinweise zur Migration dafür Projektmappe Azure SQL Data Warehouse Plattform. |



## <a name="partners"></a>Partner

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 70 | [SQL Data Warehouse Business Intelligence Partner](sql-data-warehouse-partner-business-intelligence.md) | Listen von Drittanbietern Business Intelligence Partner Lösungen SQL Data Warehouse. |
| 71 | [Integrationspartner für SQL Data Warehouse-Daten](sql-data-warehouse-partner-data-integration.md) | Liste der Partner mit Data integrationslösungen Azure SQL Data Warehouse. |
| 72 | [SQL Data Warehouse Daten-Management-Partner](sql-data-warehouse-partner-data-management.md) | Listen mit Daten von Dritten Management Partner Lösungen SQL Data Warehouse. |



## <a name="reference"></a>Referenz

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 73 | [Referenzthemen für SQL Data Warehouse](sql-data-warehouse-overview-reference.md) | Verweis Inhaltslinks für SQL Data Warehouse. |
| 74 | [PowerShell-Cmdlets und REST-APIs für SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md) | Finden Sie die oberen PowerShell-Cmdlets für Azure SQL Data Warehouse zum Anhalten und Fortsetzen einer Datenbank. |
| 75 | [Sprachelemente](sql-data-warehouse-reference-tsql-language-elements.md) | Liste mit Links zu weiterführenden Inhalt für die Transact-SQL-Sprachelemente für SQL Data Warehouse verwendet. |
| 76 | [Transact-SQL-Themen](sql-data-warehouse-reference-tsql-statements.md) | Enthält Links zu weiterführenden Inhalt für die Transact-SQL-Themen von SQL Data Warehouse verwendet. |
| 77 | [Ansichten](sql-data-warehouse-reference-tsql-system-views.md) | Links zu System zeigt Inhalte für SQL Data Warehouse. |



## <a name="security"></a>Sicherheit

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 78 | [SQL Datawarehouse - Unterstützung für kompatible Clients für Überwachung und dynamische maskieren](sql-data-warehouse-auditing-downlevel-clients.md) | Erfahren Sie mehr über SQL Data Warehouse zur Unterstützung für kompatible Clients für die Überwachung von Daten |
| 79 | [Überwachung in Azure SQL Datawarehouse](sql-data-warehouse-auditing-overview.md) | Erste Schritte mit Azure SQL Data Warehouse Überwachung |
| 80 | [Erste Schritte mit Transparent Data Encryption (DSA) im SQL Data Warehouse](sql-data-warehouse-encryption-tde.md) | Transparente Verschlüsselung (DSA) im SQL Datawarehouse |
| 81 | [Erste Schritte mit Transparent Data Encryption (DSA)](sql-data-warehouse-encryption-tde-tsql.md) | Transparente Verschlüsselung (DSA) im SQL Datawarehouse (T-SQL) |
| 82 | [Sichern einer Datenbank im SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md) | Tipps zum Sichern einer Datenbank in Azure SQL Data Warehouse Lösungen. |



## <a name="miscellaneous"></a>Sonstige

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 83 | [Installieren Sie Visual Studio 2015 und SSDT für SQL Datawarehouse](sql-data-warehouse-install-visual-studio.md) | Installieren Sie Visual Studio und SQL Server-Entwicklungstools (SSDT) für SQL Azure Datawarehouse |
| 84 | [Migration zu Premium Speicherdetails](sql-data-warehouse-migrate-to-premium-storage.md) | Anleitung für die Migration ein vorhandenes SQL Data Warehouse zu Premium-Speicher |
| 85 | [Erste Schritte mit Erkennung](sql-data-warehouse-security-threat-detection.md) | Erste Schritte mit Erkennung |
| 86 | [SQL Data Warehouse Grenzen](sql-data-warehouse-service-capacity-limits.md) | Höchstwerte für Verbindungen, Datenbanken, Tabellen und Abfragen für SQL Data Warehouse. |
| 87 | [Problembehandlung bei Azure SQL Datawarehouse](sql-data-warehouse-troubleshoot.md) | Problembehandlung bei Azure SQL Datawarehouse. |

