<properties
   pageTitle="Laden Sie Beispieldaten in SQL Data Warehouse | Microsoft Azure"
   description="Laden Sie Beispieldaten in SQL Data Warehouse"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Laden Sie Beispieldaten in SQL Data Warehouse

Folgen Sie diesen Schritten und Abfragen der Beispieldatenbank von Adventure Works. Diese Skripts verwenden Sqlcmd zuerst auszuführende SQL die Tabellen und Ansichten erstellen. Angelegten Tabellen werden die Skripts Bcp verwenden, um Daten zu laden.  Wenn Sie bereits Sqlcmd und Bcp installiert haben, folgen Sie diesen Links [Bcp installieren][] und [Sqlcmd zu installieren][].

##<a name="load-sample-data"></a>Laden von Beispieldaten

1. [Adventure Works Beispielskripts für SQL Data Warehouse][] Zip-Datei herunterladen.

2. Extrahieren Sie die Dateien in ein Verzeichnis auf Ihrem lokalen Computer aus heruntergeladenen Zip.

3. Bearbeiten Sie der extrahierten Datei aw_create.bat, und legen Sie die folgenden Variablen am Anfang der Datei.  Lassen Sie keine Leerzeichen zwischen dem "=" und der Parameter.  Im folgenden sind Beispiele für die Bearbeitung wie aussehen.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. Eine Windows Cmd Prompt führen Sie bearbeitete aw_create.bat aus.  Achten Sie im Verzeichnis befinden, in dem die bearbeitete Version des aw_create.bat gespeichert.
Dieses Skript wird...
    * Löschen von Adventure Works Tabellen oder Ansichten in der Datenbank bereits vorhanden
    * Adventure Works Tabellen und Ansichten erstellen
    * Laden Sie jede mit Bcp Adventure Works-Tabelle
    * Überprüfen Sie die Zeilenanzahl für jede Tabelle Adventure Works
    * Erfassen von Statistiken für jede Spalte für jede Tabelle Adventure Works


##<a name="query-sample-data"></a>Abfragen von Beispieldaten

Wenn Sie Beispieldaten in SQL Data Warehouse geladen haben, können Sie schnell einige Abfragen ausführen.  Zum Ausführen einer Abfrage schließen Sie, die neu erstellte Adventure Works-Datenbank in Azure SQL DW mit Visual Studio und SSDT, Dokument [Abfragen mit Visual Studio][] .

Beispiel für einfache select-Anweisung alle Informationen der Mitarbeiter:

```sql
SELECT * FROM DimEmployee;
```

Beispiel für eine komplexe Abfrage mithilfe Konstrukte wie GRUPPIEREN nach Gesamtbetrag für alle Verkäufe für jeden Tag:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Beispiel für eine SELECT-Anweisung mit einer WHERE-Klausel Aufträge vor einem bestimmten Datum filtern:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL Data Warehouse unterstützt fast alle T-SQL-Konstrukten SQL Server unterstützt.  Unterschiede sind in der Dokumentation zum [Migrieren von Code][] dokumentiert.

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie nun, Gelegenheit hatte, einige Abfragen mit Beispieldaten [entwickeln][], [Laden][]oder [Migration][] zu SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[Migrieren]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md
[Laden]: sql-data-warehouse-overview-load.md
[Abfrage mit Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Migrieren von code]: sql-data-warehouse-migrate-code.md
[Bcp installieren]: sql-data-warehouse-load-with-bcp.md
[Sqlcmd installieren]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Beispielskripts für SQL Datawarehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
