<properties
   pageTitle="Problembehandlung bei Azure SQL Datawarehouse | Microsoft Azure"
   description="Problembehandlung bei Azure SQL Datawarehouse."
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
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Problembehandlung bei Azure SQL Datawarehouse

Dieses Thema listet einige der häufigsten Fragen zur Fehlerbehebung von unseren Kunden hören.

## <a name="connecting"></a>Herstellen einer Verbindung

| Problem                              | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Fehler bei der Anmeldung für den Benutzer 'NT-AUTORITÄT\ANONYMOUS-Anmeldung'. (Microsoft SQL Server, Fehler: 18456) | Dieser Fehler tritt auf, wenn ein AAD Benutzer versucht, eine Verbindung zur Masterdatenbank, aber keinen Benutzer im Master.  Zur Behebung dieses Problems entweder Geben Sie SQL Data Warehouse bei Verbindung herstellen oder die master-Datenbank den Benutzer hinzufügen möchten.  Siehe [Security][] Artikel Weitere Informationen.|
|Server ist nicht auf die Datenbank "Master" unter dem aktuellen Sicherheitskontext principal "MeinBenutzername". Standarddatenbank des Benutzers kann nicht geöffnet werden. Anmeldung fehlgeschlagen. Fehler bei der Anmeldung für den Benutzer 'MyUserName'. (Microsoft SQL Server, Fehler: 916) | Dieser Fehler tritt auf, wenn ein AAD Benutzer versucht, eine Verbindung zur Masterdatenbank, aber keinen Benutzer im Master.  Zur Behebung dieses Problems entweder Geben Sie SQL Data Warehouse bei Verbindung herstellen oder die master-Datenbank den Benutzer hinzufügen möchten.  Siehe [Security][] Artikel Weitere Informationen.|
| CTAIP-Fehler                        | Dieser Fehler kann auftreten, wenn ein Benutzername in der master-Datenbank von SQL Server, jedoch nicht im SQL Data Warehouse-Datenbank erstellt wurde.  Wenn dieser Fehler auftritt, sehen Sie sich Artikel [Security Overview][] .  Dieser Artikel erläutert das Erstellen einen Benutzernamen und Benutzer Master und dann zum Erstellen eines Benutzers im SQL Data Warehouse-Datenbank erstellen.|
| Von der Firewall blockiert                |Server und Datenbank auf Firewalls sichergestellt sind Azure SQL-Datenbanken geschützt nur bekannten IP-Adressen auf eine Datenbank zugreifen. Firewalls sind standardmäßig also explizit aktiviert werden müssen und IP-Adresse oder Adressbereich sicher, bevor Sie eine Verbindung herstellen können.  Konfigurieren Sie die Firewall auf die Schritte in der [Bereitstellung Informationen][] [Firewall konfigurieren-Serverzugriff für die Client-IP][] .|
| Verbindung mit Treiber oder kann nicht hergestellt werden. | SQL Data Warehouse empfiehlt [SSMS][] [SSDT für Visual Studio 2015][]oder [Sqlcmd][] Daten abgefragt. Weitere Informationen zu Treibern mit SQL Data Warehouse finden Sie [Treiber für Azure SQL Data Warehouse][] und [Azure SQL Data Warehouse mit][] Artikeln.|


## <a name="tools"></a>Tools

| Problem                              | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Visual Studio-Objekt-Explorer fehlt AAD Benutzer | Dies ist ein bekanntes Problem.  Um dieses Problem zu umgehen wird zeigen Sie der Benutzer in [Kataloganzeige an][].  Finden Sie weitere Informationen zum Data Warehouse SQL Azure Active Directory mit [Authentifizierung Azure SQL Data Warehouse][] .|

## <a name="performance"></a>Leistung

|  Problem                             | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Behandlung von Leistungsproblemen  | Wenn Sie eine bestimmte Abfrage zu beheben, starten Sie mit [Abfragen zu überwachen][].|
| Pläne und schlechte Leistung ist häufig aus Statistiken   | Die häufigste Ursache für eine geringe Leistung ist keine Statistiken für Tabellen.  Statistiken [Verwalten Tabelle] [ Statistics] Weitere Informationen zum Erstellen von Statistiken und warum sind entscheidend für die Leistung.|
| Niedrige Parallelität / Abfragen in der Warteschlange   | Understanding [Arbeitslast-Management][] ist wichtig wie Speicher reservieren und Parallelität abwägen.|
| Zum Implementieren von best practices    | Der beste Ort zum Lernen, Methoden zur Abfrage [SQL Data Warehouse bewährte][] Artikel.|
| Verbesserung der Leistung und Skalierung  | Manchmal ist die Lösung zur Verbesserung der Leistung, fügen Sie einfach weitere Abfragen durch [Skalierung SQL Data Warehouse][]zu berechnen.|
| Einer schlechten Abfrageperformance durch Index schlechter Qualität | Manchmal Abfragen können Verlangsamung aufgrund [schlechter Columnstore Index][].  Weitere Informationen finden Sie unter und Neuerstellen [Indizes Segment zu verbessern][].|

## <a name="system-management"></a>Systemmanagement

|  Problem                             | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Meldung 40847: Konnte den Vorgang nicht ausführen, da Server zulässige Transaktion Datenbankkomponententest Kontingent von 45000 überschreiten würde. | Reduzieren Sie die [DWU][] der Datenbank erstellen möchten oder [eine Erhöhung Kontingent anfordern][].|
| Untersuchen der Nutzung von Speicherplatz    | Siehe [Tabellen][] Speicherplatz Nutzung des Systems zu verstehen.|
| Hilfe bei der Verwaltung von Tabellen          | Die [Tabelle] [ Overview] Weitere Hilfe beim Verwalten von Tabellen.  Dieser Artikel enthält auch Links zu ausführlicheren Themen wie [Datentypen Tabelle][Data types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition], [Maintaining Tabellenstatistik] [ Statistics] [temporäre Tabellen]und[Temporary].|

## <a name="polybase"></a>Polybase

|  Problem                             | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| UTF-8-Fehler                        |  PolyBase unterstützt derzeit nur Datendateien, die UTF-8 kodiert wurden geladen.  Hinweise zur Umgehung dieses Problems finden Sie unter [PolyBase UTF-8-Anforderung umgehen][] .|
| Ladevorgang fehlschlägt, weil große Zeilen   | Große Zeile Unterstützung ist derzeit nicht für Polybase verfügbar.  Daher enthält die Tabelle 'varchar(max)', 'nvarchar(max)' oder 'varbinary(max)', externe Tabellen verwendet werden können, die Daten zu laden.  Für lange Zeilen ist derzeit nur über Azure Data Factory (mit BCP), Azure Stream Analytics, SSIS, BCP oder .NET SQLBulkCopy-Klasse unterstützt. PolyBase werden große Zeilen in einer zukünftigen Version unterstützt werden.|
| Bcp laden Tabelle mit MAX fehlschlägt | Es ist ein bekanntes Problem, wonach 'varchar(max)', 'nvarchar(max)' oder 'varbinary(max)' am Ende der Tabelle in einigen Szenarios platziert werden.  Versuchen Sie die maximale Anzahl von Spalten am Ende der Tabelle.|

## <a name="differences-from-sql-database"></a>Unterschiede zu SQL-Datenbank

|  Problem                             | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Nicht unterstützte SQL-Datenbankfunktionen  | [Nicht unterstützte Tabellenfeatures][]anzeigen|
| Nicht unterstützte SQL-Datentypen  | [Nicht unterstützte Datentypen][]anzeigen|
| DELETE und UPDATE-Grenzen      | Finden Sie unter [UPDATE Workarounds][] [Workarounds löschen][] und [CTAS verwenden, nicht unterstützte Syntax für UPDATE und DELETE umgehen][].  |
| MERGE-Anweisung nicht unterstützt   | Finden Sie unter [SERIENDRUCK Workarounds][].|
| Gespeicherte Prozedur Grenzen       | [Gespeicherte Prozedur Grenzen][] verständlich Einschränkungen gespeicherte Prozeduren anzeigen|
| UDFs unterstützt SELECT-Anweisung nicht | Dies ist eine aktuelle Beschränkung des unserer UDFs.  Unterstützten Syntax finden Sie unter [CREATE FUNCTION][] .   |

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie sind eine Lösung für Ihr Problem finden, Hier können weitere Quellen können versuchen.

- [Blogs]
- [Wünsche]
- [Videos]
- [CAT-Teamblogs]
- [Support-Ticket erstellen]
- [MSDN-forum]
- [Stack-Überlauf-forum]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Übersicht über die Sicherheit]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT für Visual Studio 2015]: ./sql-data-warehouse-install-visual-studio.md
[Treiber für Azure SQL Datawarehouse]: ./sql-data-warehouse-connection-strings.md
[Verbinden Sie mit SQL Azure Datawarehouse]: ./sql-data-warehouse-connect-overview.md
[Support-Ticket erstellen]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Skalieren von SQL Datawarehouse]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[eine Erhöhung des Kontingents anfordern]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Lernen, die Abfragen zu überwachen]: ./sql-data-warehouse-manage-monitor.md
[Bereitstellung von Informationen]: ./sql-data-warehouse-get-started-provision.md
[Für die Client-IP-Firewall Serverzugriff konfigurieren]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Bewährte SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Größen]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Nicht unterstützte Tabellenfunktionen]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Nicht unterstützte Datentypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Schlechte Columnstore Index Qualität]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild Indizes Segment zu verbessern]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Arbeitslast-management]: ./sql-data-warehouse-develop-concurrency.md
[Mithilfe von CTAS nicht unterstützte Syntax für UPDATE und DELETE umgehen]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[Abhilfen aktualisieren]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Abhilfen löschen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[SERIENDRUCK workarounds]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Gespeicherte Prozedur Grenzen]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentifizierung in SQL Azure Datawarehouse]: ./sql-data-warehouse-authentication.md
[Vermeiden von PolyBase UTF-8-Anforderung]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[Kataloganzeige]: https://msdn.microsoft.com/library/ms187328.aspx
[FUNKTION]: https://msdn.microsoft.com/library/mt203952.aspx
[Sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT-Teamblogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Wünsche]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stack-Überlauf-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

