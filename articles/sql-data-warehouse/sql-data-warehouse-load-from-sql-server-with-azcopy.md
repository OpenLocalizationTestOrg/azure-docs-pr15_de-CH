<properties
   pageTitle="Laden von Daten aus SQL Server in Azure SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Verwendet Bcp Daten aus SQL Server, Flatfiles, AZCopy von Daten in Azure BLOB-Speicher und PolyBase zum Einlesen von Daten in Azure SQL Data Warehouse exportieren."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Laden von Daten aus SQL Server in Azure SQL Data Warehouse (AZCopy)

Verwenden Sie Bcp und AZCopy Befehlszeilenprogramme zum Laden von Daten aus SQL Server in Azure BLOB-Speicher. Verwenden Sie PolyBase oder Azure Data Factory, um die Daten in Azure SQL Data Warehouse zu laden. 


## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm durchgehen, müssen wie folgt vor:

- SQL Data Warehouse-Datenbank
- Das Dienstprogramm Bcp Befehlszeile installiert
- Das Befehlszeilenprogramm SQLCMD installiert

>[AZURE.NOTE] Sie können die Dienstprogramme Bcp und Sqlcmd aus dem [Microsoft Download Center][]herunterladen.

## <a name="import-data-into-sql-data-warehouse"></a>Importieren von Daten in SQL Data Warehouse

In diesem Lernprogramm wird eine Tabelle in Azure SQL Data Warehouse erstellen und Importieren von Daten in der Tabelle.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Schritt 1: Erstellen einer Tabelle in Azure SQL Data Warehouse

Sqlcmd mit der über eine Befehlszeile zum Erstellen einer Tabelle in der Instanz die folgende Abfrage ausführen:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

>[AZURE.NOTE] Weitere Informationen zum Erstellen einer Tabelle im SQL Data Warehouse und die Optionen in der WITH-Klausel finden Sie unter [Tabelle][] oder [CREATE TABLE-Syntax][] .

### <a name="step-2-create-a-source-data-file"></a>Schritt 2: Erstellen einer Quelldatei Daten

Öffnen Sie den Editor kopieren Sie die folgenden Zeilen der Daten in eine neue Textdatei und speichern Sie diese Datei in das lokale temporäre Verzeichnis C:\Temp\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [AZURE.NOTE] Es ist zu bedenken, dass bcp.exe die UTF-8-Codierung nicht unterstützt. Verwenden Sie ASCII- oder UTF-16-codierte Dateien Wenn bcp.exe verwenden.

### <a name="step-3-connect-and-import-the-data"></a>Schritt 3: Verbinden und importieren
Bcp können Sie verbinden und importieren Sie die Daten mit dem folgenden Befehl ersetzen die Werte bei Bedarf:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Sie können überprüfen, dass die Daten durch Ausführen der folgenden Abfrage mit Sqlcmd geladen wurde:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Diese sollten die folgenden Ergebnisse zurück:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Schritt 4: Erstellen Sie Statistiken neu geladene Daten

Azure SQL Data Warehouse ist noch Unterstützung automatisch erstellen oder auto Update Statistics. Um die Leistung von Abfragen zu erhalten, ist es wichtig, dass Statistiken für alle Spalten aller Tabellen nach dem ersten Laden erstellt werden oder wesentliche Änderungen in den Daten. Eine ausführliche Erläuterung von Statistiken finden Sie unter [Statistiken][] in die Themengruppe entwickeln. Es folgt ein Beispiel Statistiken für die Tabelle in diesem Beispiel laden erstellen

Führen Sie die folgende CREATE STATISTICS-Anweisung von Sqlcmd aufgefordert:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Exportieren von Daten aus SQL Data Warehouse
In diesem Lernprogramm erstellen Sie eine Datei aus einer Tabelle in SQL Data Warehouse. Wir werden weiter oben erstellten Daten exportieren, in eine neue Datei namens DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Schritt 1: Exportieren der Daten

Das Dienstprogramm Bcp können Sie verbinden und Exportieren von Daten mit dem folgenden Befehl ersetzen die Werte bei Bedarf:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Sie können überprüfen, dass die Daten einwandfrei exportiert wurde, die neue Datei. Die Daten in der Datei sollte den Text übereinstimmen:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

>[AZURE.NOTE] Aufgrund der verteilten Systemen Daten Reihenfolge gleich über SQL Data Warehouse-Datenbanken möglicherweise nicht. Eine weitere Option ist Funktion **Queryout** von Bcp Schreiben einer Abfrage extrahieren, anstatt die gesamte Tabelle zu exportieren.

## <a name="next-steps"></a>Nächste Schritte
Überblick Laden finden Sie unter [Laden von Daten in SQL Data Warehouse][].
Weitere Hinweise zur Entwicklung finden Sie unter [SQL Data Warehouse Development Overview][].

<!--Image references-->

<!--Article references-->

[Laden von Daten in SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse-Anwendungsentwicklung, Überblick]: ./sql-data-warehouse-overview-develop.md
[Tabelle]: ./sql-data-warehouse-tables-overview.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE-syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
