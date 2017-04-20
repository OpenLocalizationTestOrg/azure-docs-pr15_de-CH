<properties
   pageTitle="Laden von Daten aus SQL Server in Azure SQL Data Warehouse (Bcp) | Microsoft Azure"
   description="Für eine kleine Größe verwendet Bcp Daten aus SQL Server in Dateien exportieren und importieren Sie die Daten direkt in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Laden von Daten aus SQL Server in Azure SQL Data Warehouse (Flatfiles)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Für kleine Datenmengen können Sie das Befehlszeilen-Dienstprogramm Bcp Daten aus SQL Server exportieren und dann direkt in Azure SQL Data Warehouse laden.

In diesem Lernprogramm verwenden Sie Bcp an:

- Exportieren einer Tabelle von SQL Server mit Bcp Befehl (oder erstellen Sie eine einfache Datei)
- Importieren Sie die Tabelle aus einer Textdatei in SQL Data Warehouse.
- Erstellen Sie Statistiken für die geladenen Daten.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Bevor Sie beginnen

### <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm durchgehen, müssen wie folgt vor:

- SQL Data Warehouse-Datenbank
- Bcp-Befehlszeilenprogramm installiert
- Das Befehlszeilenprogramm Sqlcmd installiert

Sie können die Dienstprogramme Bcp und Sqlcmd aus dem [Microsoft Download Center][]herunterladen.

### <a name="data-in-ascii-or-utf-16-format"></a>Daten in ASCII- oder UTF-16-format

Wenn Sie dieses Lernprogramm mit Ihren eigenen Daten versuchen, müssen Ihre Daten ASCII- oder UTF-16-Codierung verwendet seit Bcp UTF-8 nicht unterstützt. 

PolyBase UTF-8 unterstützt jedoch noch UTF-16 unterstützt. Beachten Sie, dass wenn Sie Bcp mit PolyBase kombinieren möchten müssen Sie Transformieren der Daten in UTF-8 nach dem Export aus SQL Server. 


## <a name="1-create-a-destination-table"></a>1. erstellen Sie 1. eine Zieltabelle

Definieren Sie eine Tabelle im SQL Data Warehouse, die die Zieltabelle für die Last. Die Daten in jeder Zeile der Datendatei entspricht Spalten in der Tabelle.

Um eine Tabelle zu erstellen, öffnen Sie ein Eingabeaufforderungsfenster und verwenden Sie sqlcmd.exe, den folgenden Befehl:


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


## <a name="2-create-a-source-data-file"></a>2. erstellen Sie eine Quelldatendatei

Öffnen Sie den Editor kopieren Sie die folgenden Zeilen der Daten in eine neue Textdatei und speichern Sie diese Datei in das lokale temporäre Verzeichnis C:\Temp\DimDate2.txt. Diese Daten werden im ASCII-Format.

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

(Optional) Um Ihre Daten aus einer SQL Server-Datenbank zu exportieren, öffnen Sie ein Eingabeaufforderungsfenster und führen Sie den folgenden Befehl. Ersetzen Sie Tabellenname, ServerName, Datenbankname, Benutzername und Kennwort durch eigene Informationen.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3. Laden Sie die Daten
Um die Daten zu laden, öffnen Sie ein Eingabeaufforderungsfenster, und führen Sie folgenden Befehl die Werte für Servername, Datenbank-Name, Benutzername und Kennwort durch eigene Informationen ersetzen.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Verwenden Sie diesen Befehl, um sicherzustellen, dass die Daten ordnungsgemäß geladen wurde

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Die Ergebnisse sollten wie folgt aussehen:

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

## <a name="4-create-statistics"></a>4. Erstellen von Statistiken

SQL Data Warehouse ist noch Unterstützung automatisch erstellen oder automatisch aktualisierte Statistiken. Um die abfrageleistung zu erhalten, ist es wichtig Statistiken für alle Spalten aller Tabellen erstellen, nach dem ersten Laden oder keine wesentlichen Änderungen in den Daten vorkommen. [Eine detaillierte Erklärung der Statistiken Statistiken.][] 

Führen Sie den folgenden Befehl zum Erstellen von Statistiken für die Tabelle neu geladen.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. exportieren Sie 5. Daten aus SQL Data Warehouse
Spaß können Sie die Daten exportieren, die nur aus SQL Data Warehouse geladen.  Der Befehl Exportieren ist genau identisch aus SQL Server exportieren.

Allerdings gibt es einen Unterschied zwischen den Ergebnissen. Da die Daten beim export von Daten in verteilten Standorten im SQL Data Warehouse gespeichert schreibt jeder Compute-Knoten sie Daten in die Ausgabedatei. Die Reihenfolge der Daten in der Ausgabedatei dürfte sich die Reihenfolge der Daten in der Eingabedatei.

### <a name="export-a-table-and-compare-exported-results"></a>Exportieren einer Tabelle und exportierte vergleichen

Die exportierten Daten, öffnen Sie ein Eingabeaufforderungsfenster und führen Sie diesen Befehl mit eigenen Parametern. ServerName ist der Name der Azure logischen SQL Server.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Sie können überprüfen, dass die Daten einwandfrei exportiert wurde, die neue Datei. Die Daten in der Datei sollte den Text übereinstimmen wahrscheinlich in einer anderen Reihenfolge sortiert:

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

### <a name="export-the-results-of-a-query"></a>Exportieren Sie die Ergebnisse einer Abfrage

Die Funktion **Queryout** Bcp können Sie die Ergebnisse einer Abfrage anstelle die gesamte Tabelle exportieren exportieren. 

## <a name="next-steps"></a>Nächste Schritte
Überblick Laden finden Sie unter [Laden von Daten in SQL Data Warehouse][].
Weitere Hinweise zur Entwicklung finden Sie unter [SQL Data Warehouse Development Overview][].
Weitere Informationen zum Erstellen einer Tabelle im SQL Data Warehouse finden Sie unter [Tabelle][] oder [CREATE TABLE-Syntax][] .

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
