<properties
   pageTitle="Leitfaden für die Verwendung von PolyBase im SQL Data Warehouse | Microsoft Azure"
   description="Richtlinien und empfohlene Vorgehensweisen zur Verwendung von PolyBase im SQL Data Warehouse Szenarien."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Leitfaden für die Verwendung von PolyBase im SQL Data Warehouse

Dieses Handbuch bietet Informationen zur Verwendung von PolyBase im SQL Data Warehouse.

Zum Einstieg finden Sie unter Lernprogramm [Daten mit PolyBase laden][] .


## <a name="rotating-storage-keys"></a>Rotierende Speicherschlüssel

Von Zeit zu Zeit sollten Sie die Zugriffstaste dem BLOB-Speicher aus Sicherheitsgründen ändern.

Elegante Möglichkeit für diese Aufgabe ist ein Prozess als "Drehen die Schlüssel" bezeichnet. Möglicherweise haben Sie bemerkt haben Sie zwei Speicherschlüssel für das Speicherkonto Blob. So hat Sie übergehen können

Drehen Ihre Azure-Speicher kontoschlüssel ist einfach drei Schritten

1. Erstellen Sie zweite Datenbank begrenzt Anmeldeinformationen basierend auf Sekundärspeicher Zugriffstaste
2. Erstellen Sie zweite externe Datenquelle basierend auf diesem neuen Zertifikat
3. Löschen Sie und erstellen Sie externen Tabellen auf die externe Datenquelle

Wenn Sie migriert alle externen Tabellen an die externe Datenquelle, dann können bereinigen Aufgaben:

1. Erste externe Datenquelle löschen
2. Drop erste Datenbank begrenzt Anmeldeinformationen anhand der Zugriffstaste Primärspeicher
3. Azure melden Sie an und regenerieren Sie die primäre Taste zur nächsten

## <a name="query-azure-blob-storage-data"></a>Abfragedaten Azure Blob-Speicher
Abfragen für externe Tabellen verwenden einfach den Tabellennamen, als wäre es eine relationale Tabelle.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Eine Abfrage auf eine externe Tabelle kann mit *"Abfrage abgebrochen - die maximale ablehnen Schwellenwert beim Lesen aus einer externen Quelle erreichte"*Fehler fehl. Dies bedeutet, dass Ihre externen *geänderter* Datensätze enthält. Datensatz ist "geändert" betrachtet, wenn die tatsächlichen Daten Typen/Anzahl der Spalten nicht die Definitionen der externen Tabelle übereinstimmen oder angegebenen externen Dateiformats die Daten entsprechen nicht. Um dieses Problem zu beheben, sicherstellen der externen Tabelle und externen Definitionen für Dateiformate und die externen Daten entspricht diese Definitionen. Eine Teilmenge der externen Datensätze werden geändert, können Sie diese Einträge für Abfragen mithilfe der Optionen ablehnen in EXTERNEN Tabelle DDL erstellen ablehnen.


## <a name="load-data-from-azure-blob-storage"></a>Daten von Azure BLOB-Speicher laden
In diesem Beispiel lädt Daten aus Azure BLOB-Speicher in SQL Data Warehouse-Datenbank.

Speichern Daten direkt entfernt die Übertragungsdauer Daten für Abfragen. Speichern von Daten mit einem Columnstore-Index verbessert die Abfrageperformance für Analyesabfragen bis zu 10 X.

Dieses Beispiel verwendet die CREATE TABLE AS SELECT-Anweisung zum Laden von Daten. Die neue Tabelle erbt die Spalten in der Abfrage. Es erbt die Datentypen der Spalten aus der externen Tabelle.

CREATE TABLE AS SELECT ist sehr effektiv Transact-SQL-Anweisung, die Daten parallel zu den Compute-Knoten des SQL Data Warehouse lädt.  Es wurde ursprünglich für die parallele Verarbeitung (MPP) Engine in Analytics-Plattform entwickelt und ist jetzt im SQL Data Warehouse.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Finden Sie unter [CREATE TABLE AS SELECT (Transact-SQL)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Statistiken neu geladene Daten erstellen

Azure SQL Data Warehouse ist noch Unterstützung automatisch erstellen oder auto Update Statistics.  Um die Leistung von Abfragen zu erhalten, ist es wichtig, dass Statistiken für alle Spalten aller Tabellen nach dem ersten Laden erstellt werden oder wesentliche Änderungen in den Daten.  Eine ausführliche Erläuterung von Statistiken finden Sie unter [Statistiken][] in die Themengruppe entwickeln.  Es folgt ein Beispiel Statistiken für die Tabelle in diesem Beispiel laden erstellen.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Exportieren von Daten in Azure BLOB-Speicher
Dieser Abschnitt zeigt, wie Daten aus SQL Data Warehouse in Azure BLOB-Speicher exportieren. Dieses Beispiel erstellen Sie externe Tabelle AS SELECT ist sehr effektiv Transact-SQL-Anweisung die Daten aus den Compute-Knoten zu exportieren.

Das folgende Beispiel erstellt eine externe Tabelle Weblogs2014 Definitionen und Daten von Dbo. Weblogs Tabelle. Definition der externen Tabelle im SQL Data Warehouse gespeichert, und die Ergebnisse der SELECT-Anweisung in das Verzeichnis "/ archive/log2014" unter der BLOB-Container von der Datenquelle exportiert. Die Daten werden in die angegebene Datei im Textformat exportiert.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Vermeiden von PolyBase UTF-8-Anforderung
Bei vorhanden PolyBase unterstützt das Laden von Datendateien, die UTF-8 kodiert wurden. UTF-8 verwendet den gleichen Zeichensatz als ASCII-PolyBase wird auch Unterstützung beim Laden von Daten ASCII codiert ist. Allerdings PolyBase unterstützt keine anderen Zeichensatz wie UTF-16 / Unicode oder ASCII-Zeichen. Erweiterte ASCII umfasst Zeichen mit Akzenten wie die deutsche Umlaut.

Die beste Antwort ist diese Anforderung umgehen, UTF-8-Kodierung neu schreiben.

Es gibt verschiedene Methoden zur Verfügung. Unten sind zwei Ansätzen Powershell:

### <a name="simple-example-for-small-files"></a>Beispiel für kleine Dateien

Es folgt eine einfache zeilenweise Powershell-Skript, das die Datei erstellt.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Dies ist eine einfache Möglichkeit, die Daten erneut codieren ist es jedoch nicht die. E/a-Stream Beispiel ist sehr viel schneller und erzielt dasselbe Ergebnis.

### <a name="io-streaming-example-for-larger-files"></a>E/a-Streaming wird bei großen Dateien

Das folgende Codebeispiel ist komplexer aber wie Datenzeilen aus Quelle Ziel es gibt viel effizienter. Verwenden Sie diesen Ansatz für größere Dateien.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Nächste Schritte
Um weitere Informationen zum Verschieben von Daten in SQL Data Warehouse finden Sie unter [Übersicht über die Datenmigration][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Laden von Daten mit PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Übersicht über die Datenmigration]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Erstellen externes DATEIFORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026) .aspx [Erstellen einer EXTERNEN Tabelle (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[Erstellen der Tabelle AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
