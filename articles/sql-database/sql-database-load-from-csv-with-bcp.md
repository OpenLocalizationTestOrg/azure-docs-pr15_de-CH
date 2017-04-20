<properties
   pageTitle="Laden von Daten aus CSV-Datei in Azure SQL-Verarbeitung (Bcp) | Microsoft Azure"
   description="Für eine kleine Größe verwendet Bcp zum Importieren von Daten in Azure SQL-Datenbank."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Laden von Daten aus CSV in Azure SQL Data Warehouse (Flatfiles)

Das Befehlszeilen-Dienstprogramm Bcp können Sie Daten aus einer CSV-Datei in Azure SQL-Datenbank importieren.

## <a name="before-you-begin"></a>Bevor Sie beginnen

### <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm durchgehen, müssen wie folgt vor:

- Eine logische Azure SQL-Datenbank-Server und Datenbank
- Bcp-Befehlszeilenprogramm installiert
- Das Befehlszeilenprogramm Sqlcmd installiert

Sie können die Dienstprogramme Bcp und Sqlcmd aus dem [Microsoft Download Center][]herunterladen.

### <a name="data-in-ascii-or-utf-16-format"></a>Daten in ASCII- oder UTF-16-format

Wenn Sie dieses Lernprogramm mit Ihren eigenen Daten versuchen, müssen Ihre Daten ASCII- oder UTF-16-Codierung verwendet seit Bcp UTF-8 nicht unterstützt. 

## <a name="1-create-a-destination-table"></a>1. erstellen Sie 1. eine Zieltabelle

Definieren einer Tabelle in SQL-Datenbank als die Zieltabelle. Die Daten in jeder Zeile der Datendatei entspricht Spalten in der Tabelle.

Um eine Tabelle zu erstellen, öffnen Sie ein Eingabeaufforderungsfenster und verwenden Sie sqlcmd.exe, den folgenden Befehl:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
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


## <a name="next-steps"></a>Nächste Schritte

Um eine SQL Server-Datenbank migrieren, finden Sie unter [Migration des SQL Server-Datenbank](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
