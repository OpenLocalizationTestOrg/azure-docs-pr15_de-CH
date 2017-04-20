<properties
   pageTitle="SQL Data Warehouse Grenzen | Microsoft Azure"
   description="Höchstwerte für Verbindungen, Datenbanken, Tabellen und Abfragen für SQL Data Warehouse."
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
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>SQL Data Warehouse Grenzen

Die folgenden Tabellen enthalten die zulässige Höchstwerte für verschiedene Komponenten der Azure SQL Data Warehouse.


## <a name="workload-management"></a>Arbeitslast-management

| Kategorie            | Beschreibung                                       | Maximale            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Data Warehouse-Einheiten (DWU)][]| Max. DWU für eine einzelne SQL Data Warehouse | 6000               |
| [Data Warehouse-Einheiten (DWU)][]| Maximale DWU für einen einzelnen SQL server         | 6000 standardmäßig<br/><br/> Standardmäßig hat jeder SQL Server (z.B. myserver.database.windows.net) DTU Kontingent von 45.000 bis 6000 DWU ermöglicht. Dieses Kontingent ist einfach eine Sicherheitsgrenze. Das Kontingent von [Support-Ticket erstellen][] und als Anfragetyp *Kontingent* auswählen können.  Berechnet die DTU muss die Summe der 7.5 Multiplizieren DWU benötigt. Sie können Ihre aktuellen DTU Verbrauch aus dem SQL Server-Blade im Portal anzeigen. Unterbrochen und die Ausführung Datenbanken DTU Kontingent gezählt. |
| Verbindung | Gleichzeitiger Sitzungen                          | 1024<br/><br/>Microsoft unterstützt maximal 1024 aktive Verbindung, die jeweils SQL Data Warehouse-Datenbank gleichzeitig Ersuchen kann. Beachten Sie, dass die Grenzen für die Anzahl der Abfragen, die eigentlich gleichzeitig ausgeführt werden können. Wenn die Parallelität überschritten wird sich die Anfrage in einer internen Warteschlange, Verarbeitung wartet.|
| Verbindung | Maximaler Speicher für vorbereitete Anweisung            | 20 MB              |
| [Arbeitslast-management][] | Maximale Anzahl gleichzeitiger Abfragen                    | 32<br/><br/> Standardmäßig können SQL Data Warehouse maximal 32 gleichzeitige Abfragen und Warteschlangen verbleibende Abfragen ausführen.<br/><br/>Der Grad an Parallelität kann sinken, wenn Benutzern zugewiesen sind, zu einer höheren Ressourcenklasse oder SQL Data Warehouse mit niedrigen DWU konfiguriert ist. Einige Abfragen wie DMV Abfragen darf immer ausgeführt.|
| [Tempdb][]          | Max. Größe der Tempdb                                | 399 GB pro DW100. Daher wird bei DWU1000 Tempdb 3,99 TB Größe angepasst |


## <a name="database-objects"></a>Datenbankobjekte

| Kategorie          | Beschreibung                                  | Maximale            |
| :---------------- | :------------------------------------------- | :----------------- |
| Datenbank          | Max. Größe                                     | 240 TB komprimiert auf Datenträger<br/><br/>Dieser Speicherplatz ist unabhängig von Tempdb Protokollspeicherplatz und daher hier mit permanenten Tabellen widmet.  Gruppierte Columnstore Komprimierung wird auf 5 X geschätzt.  Diese Komprimierung kann die Datenbank auf rund 1 PB bei allen gruppierten Columnstore (der Tabelle Standardtyp) sind.|
| Tabelle             | Max. Größe                                     | 60 TB komprimiert auf Datenträger   |
| Tabelle             | Tabellen pro Datenbank                          | 2 Mrd.          |
| Tabelle             | Spalten pro Tabelle                            | 1024 Spalten       |
| Tabelle             | Bytes pro Spalte                             | Abhängig vom [Datentyp][].  Beträgt 8000 für Char-Datentypen 4000 für Nvarchar oder 2 GB für MAX-Datentypen.|
| Tabelle             | Bytes pro Zeile definierte Größe                  | 8060 Byte<br/><br/>Die Anzahl der Bytes pro Zeile wird auf die gleiche Weise wie bei SQL Server mit Seite berechnet. Wie SQL Server unterstützt SQL Data Warehouse Overflow-Speicher ermöglicht **Spalten variabler Länge** aus der Zeile abgelegt werden. Variabler Länge Zeilen aus der Zeile gedrückt, ist nur 24 Byte Stamm im primären Datensatz gespeichert. Weitere Informationen finden Sie im MSDN-Artikel [Overflow-Daten überschreitet 8 KB][] .|
| Tabelle             | Partitionen pro Tabelle                    | 15.000<br/><br/>Hohe Leistung empfehlen wir für die Minimierung von Partitionen müssen zwar weiterhin unterstützen Ihre Bedürfnisse. Mit steigender Anzahl von Partitionen Aufwand (Datendefinitionssprache) und Data Manipulation Language (DML) wächst und wird langsamer.|
| Tabelle             | Zeichen pro Partition Grenzwert.| 4000 |
| Index             | Nicht gruppierte Indizes pro Tabelle.        | 999<br/><br/>Gilt für Rowstore-Tabellen.|
| Index             | Gruppierte Indizes pro Tabelle.            | 1<br><br/>Gilt für Tabellen Rowstore und Columnstore.|
| Index             | Index-Schlüsselgröße.                          | 900 Byte.<br/><br/>Gilt für Rowstore-Indizes.<br/><br/>Varchar-Spalten mit einer maximalen Größe von mehr als 900 Byte können erstellt werden, wenn die vorhandenen Daten in den Spalten 900 Byte nicht überschreitet, wenn der Index erstellt wird. Jedoch später einfügen oder UPDATE-Aktionen für die Spalten, die dazu führen, dass die Gesamtgröße 900 Byte überschreitet, schlägt fehl.|
| Index             | Spalten pro Index.                   | 16<br/><br/>Gilt für Rowstore-Indizes. Columnstore gruppierte Indizes enthalten alle Spalten.|
| Statistik        | Größe der kombinierten Werte.      | 900 Byte.         |
| Statistik        | Spalten pro Statistik.           | 32                 |
| Statistik        | Statistiken für Spalten pro Tabelle erstellt. | 30.000            |
| Gespeicherte Prozeduren | Maximalen Schachtelungsebenen.               | 8                 |
| Ansicht              | Spalten anzeigen                         | 1.024             |


## <a name="loads"></a>Lasten

| Kategorie          | Beschreibung                                  | Maximale            |
| :---------------- | :------------------------------------------- | :----------------- |
| Polybase laden    | Bytes pro Zeile                                | 32.768<br/><br/>Polybase lädt sind beide Zeilen kleiner als 32 KB geladen und auf VARCHR(MAX) 'nvarchar(max)' oder 'varbinary(max)' können nicht geladen werden.  Heute existiert dieses Limit wird entfernt relativ schnell.<br/><br/>


## <a name="queries"></a>Abfragen

| Kategorie          | Beschreibung                                  | Maximale            |
| :---------------- | :------------------------------------------- | :----------------- |
| Abfrage             | Warteschlange Abfragen auf Tabellen.               | 1000               |
| Abfrage             | Gleichzeitige Abfragen auf Ansichten.          | 100                |
| Abfrage             | Warteschlange Abfragen auf Ansichten               | 1000               |
| Abfrage             | Maximale Parameter                           | 2098               |
| Stapelverarbeitung             | Maximale Größe                                 | 65, 536 * 4096        |
| SELECT-Ergebnisse    | Spalten pro Zeile                              | 4096<br/><br/>Sie können nie mehr als 4096 Spalten pro Zeile im Ergebnis auswählen. Es gibt keine Garantie, dass Sie immer 4096. Wenn der Abfrageplan eine temporäre Tabelle erfordert, können 1024 Spalten pro Tabelle maximale anwenden|
| WÄHLEN SIE            | Verschachtelte Unterabfragen                            | 32<br/><br/>Sie können nicht mehr als 32 verschachtelte Unterabfragen in einer SELECT-Anweisung. Es gibt keine Garantie, dass Sie immer 32. Eine Verknüpfung kann beispielsweise eine Unterabfrage im Abfrageplan führen. Die Anzahl von Unterabfragen kann auch durch den verfügbaren Speicher begrenzt.|
| WÄHLEN SIE            | Spalten pro Verknüpfung                             | 1024 Spalten<br/><br/>Sie können nicht mehr als 1024 Spalten in der Verknüpfung. Es gibt keine Garantie, dass Sie immer 1024. JOIN-Plan eine temporäre Tabelle mit mehr Spalten als Ergebnis JOIN benötigt, gilt das Limit von 1024 in die temporäre Tabelle. |
| WÄHLEN SIE            | Bytes pro Gruppe von Spalten.                  | 8060<br/><br/>Die Spalten in der GROUP BY-Klausel können höchstens 8060 Byte.|
| WÄHLEN SIE            | Bytes pro ORDER BY-Spalten                   | 8060 Byte.<br/><br/>Die Spalten in der ORDER BY-Klausel können höchstens 8060 Byte.|
| Bezeichner und Konstanten pro Anweisung | Anzahl der referenzierten Bezeichnern und Konstanten. | 65.535<br/><br/>SQL Data Warehouse begrenzt die Anzahl der IDs und Konstanten, die in einem Ausdruck einer Abfrage enthalten sein können. Diese Grenze ist 65.535. Diese Zahl zu SQL Server Fehler 8632 überschreiten. Weitere Informationen finden Sie unter [Fehler: eine Expression Services wurde erreicht][].|


## <a name="metadata"></a>Metadaten

| System anzeigen                        | Maximale Zeilenanzahl |
| :--------------------------------- | :------------|
| Sys.dm_pdw_component_health_alerts | 10.000       |
| Sys.dm_pdw_dms_cores               | 100          |
| Sys.dm_pdw_dms_workers             | Gesamtzahl der DMS Arbeitskräfte für die aktuelle 1000 fordert SQL. |
| Sys.dm_pdw_errors                  | 10.000       |
| Sys.dm_pdw_exec_requests           | 10.000       |
| Sys.dm_pdw_exec_sessions           | 10.000       |
| Sys.dm_pdw_request_steps           | Gesamtzahl der Schritte für die letzten 1000 SQL Anfragen, die in sys.dm_pdw_exec_requests gespeichert. |
| Sys.dm_pdw_os_event_logs           | 10.000       |
| Sys.dm_pdw_sql_requests            | Die letzten 1000 SQL Anfragen, die in sys.dm_pdw_exec_requests gespeichert. |


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [SQL Data Warehouse Referenz (Übersicht)][].

<!--Image references-->

<!--Article references-->
[Data Warehouse-Einheiten (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[SQL Data Warehouse Referenz (Übersicht)]: ./sql-data-warehouse-overview-reference.md
[Arbeitslast-management]: ./sql-data-warehouse-develop-concurrency.md
[Tempdb]: ./sql-data-warehouse-tables-temporary.md
[Datentyp]: ./sql-data-warehouse-tables-data-types.md
[Support-Ticket erstellen]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Overflow-Daten über 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Interner Fehler: eine Expression Services wurde erreicht]: https://support.microsoft.com/kb/913050
