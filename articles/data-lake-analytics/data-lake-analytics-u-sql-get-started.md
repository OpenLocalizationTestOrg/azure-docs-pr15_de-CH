<properties
   pageTitle="U-SQL-Skripts mit Daten See Tools für Visual Studio entwickeln | Azure"
   description="Informationen Sie zum Installieren Daten See Tools für Visual Studio entwickeln und U-SQL-Skripts. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Lernprogramm: Erste Schritte mit Azure Data Lake Analytics U-SQL-Sprache

U-SQL ist eine Sprache, die die Vorteile von SQL mit Ausdruckskraft eigenen Code alle Daten jeder Ebene vereinheitlicht. U-SQL ermöglicht skalierbare verteilte Abfrage effiziente Analyse der Daten im Speicher und relationalen Speicher wie Azure SQL-Datenbank.  Sie können den Prozess unstrukturierte Daten mittels Schema auf lesen, Einfügen benutzerdefinierter Logik und des UDF und enthält Erweiterbarkeit feinkörnige Kontrolle Ebene ausführen aktivieren. Erfahren Sie mehr über die Designphilosophie U-SQL finden Sie diese [Visual Studio Blogbeitrag](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Es gibt einige Unterschiede von ANSI SQL oder T-SQL. Beispielsweise müssen die Schlüsselwörter wie SELECT in Großbuchstaben.

Es ist Typ System und Expression-Sprache in select-Klauseln Prädikate usw. in sind C#.
Dadurch werden die C#-Typen und die Datentypen verwendet C# NULL-Semantik und Vergleichsoperationen innerhalb eines Prädikats folgen C#-Syntax (z. B. ein == "Foo").  Dies bedeutet auch, dass die Werte vollständige .NET Objekte können Sie problemlos alle gehen auf das Objekt (z.B. "f o o o". Split(' ')).

Weitere Informationen finden Sie unter [U-SQL-Referenz](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen [Tutorial: Entwickeln U-SQL-Skripts mit Daten See Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

Im Lernprogramm haben Sie einen Auftrag See Datenanalyse mit folgenden U-SQL-Skript ausgeführt:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Dieses Skript muss keine Schritte Transformation. Liest aus der Quelldatei namens **SearchLog.tsv**, schematizes er und gibt das Rowset in einer Datei namens **SearchLog-First-u-sql.csv**.

Beachten Sie das Fragezeichen neben das Feld Dauer. Das bedeutet, dass das Feld Dauer null sein könnte.

Einige Konzepte und Schlüsselwörter im Skript gefunden:

- **Rowset Variablen**: jedes Abfrageausdrucks, ein Rowset erzeugt, kann einer Variablen zugewiesen. U-SQL folgt das T-SQL-Variable Benennungsmuster **@searchlog** im Skript.
    Beachten Sie, dass die Zuordnung nicht Ausführung erzwingen. Nur den Namen des Ausdrucks und ermöglicht den Aufbau komplexer Ausdrücke.
- **EXTRAHIEREN** können Sie beim Lesen ein Schemas definieren. Das Schema wird durch einen Spaltennamen und C# Typ Name pro Spalte angegeben. Es verwendet eine so genannte **Extractor**, z. B. **Extractors.Tsv()** Tsv-Dateien extrahieren. Sie können benutzerdefinierte Extraktionsprogramme entwickeln.
- **Nimmt ein Rowset und serialisiert.** Die Outputters.Csv() Ausgabe eine CSV-Datei an der angegebenen Position. Sie können auch benutzerdefinierte Outputters entwickeln.
- Beachten Sie die beiden Pfade sind relative Pfade. Sie können auch absolute Pfade.  Zum Beispiel

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Zugriff auf Dateien der verknüpften Speicherkonten müssen Sie absoluten Pfad verwenden.  Die Syntax für Dateien verknüpfte Azure Storage-Konto lautet:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure BLOB-Container mit öffentlichen Blobs oder öffentlichen Container Berechtigungen werden derzeit nicht unterstützt.

## <a name="use-scalar-variables"></a>Skalare Variablen

Skalare Variablen können sowie Sie um Ihr Skript-Wartung zu vereinfachen. Das vorherige U-SQL-Skript kann auch wie folgt geschrieben werden:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Transformieren von rowsets

Verwenden Sie Rowsets transformieren **auswählen** :

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Die WHERE-Klausel verwendet [C# boolescher Ausdruck](https://msdn.microsoft.com/library/6a71f45d.aspx). Die Sprache C# Ausdruck können Sie eigene Ausdrücke und Funktionen. Sie können auch Filterung komplexer mit logischen Konjunktionen (und) und disjunktionen (oder).

Das folgende Skript verwendet die DateTime.Parse()-Methode und eine Verbindung.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Beachten Sie, dass die zweite Abfrage mit dem Ergebnis des ersten Rowsets arbeitet und so das Ergebnis eine Kombination der beiden Filter ist. Sie können auch einen Variablennamen und die Namen lexikalisch begrenzt.

## <a name="aggregate-rowsets"></a>Aggregate rowsets

U-SQL bietet die **ORDER BY**, **GROUP BY** und Aggregationen.

Die folgende Abfrage ermittelt die Gesamtdauer pro Region und gibt dann oben 5 Dauer in Reihenfolge.

U-SQL Rowsets beibehalten die Reihenfolge für die nächste Abfrage nicht. Daher müssen eine Ausgabe zu bestellen Reihenfolge der OUTPUT-Anweisung wie folgt hinzufügen:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U-SQL ORDER BY-Klausel hat mit der FETCH-Klausel in einer SELECT-Ausdruck.

U-SQL HAVING-Klausel kann verwendet werden, um die Ausgabe zu Gruppen einzuschränken, die HAVING-Bedingung erfüllen:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Verknüpfen von Daten

U-SQL bietet allgemeine Verknüpfungsoperatoren INNER-JOIN links/rechts/FULL OUTER JOIN, SEMI JOIN nur Tabellen sondern Rowsets (auch diejenigen, die aus Dateien) an.

Das folgende Skript Searchlog mit einer Ankündigung Eindruck verknüpft und gibt uns die anzeigen für die Abfragezeichenfolge für ein bestimmtes Datum.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U-SQL unterstützt nur kompatible ANSI-Verknüpfungssyntax: Prädikat Rowset1 JOIN Rowset2 auf. Die alte Syntax von Rowset1 dem Rowset2-Prädikat wird nicht unterstützt.
Das Prädikat in einer Verknüpfung muss eine Verknüpfung Gleichheit und kein Ausdruck. Wenn Sie einen Ausdruck verwenden möchten, Hinzufügen eines vorherigen Rowsets select-Klausel. Möchten Sie einen anderen Vergleich, können Sie es in der WHERE-Klausel verschieben.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Erstellen Sie Datenbanken, Tabellenwertfunktionen, Ansichten und Tabellen

U-SQL können Sie Daten im Kontext einer Datenbank und Schema verwenden. Sie müssen also immer lesen oder Schreiben von Dateien.

Jedes U-SQL-Skript führt eine Standarddatenbank (Master) und das Standardschema (DBO) als die Standardkontext. Sie können eine eigene Datenbank und Schema erstellen. Ändern Sie den Kontext Anweisung **verwenden,** um den Kontext zu ändern.


### <a name="create-a-table-valued-function-tvf"></a>Erstellen Sie eine Tabellenwertfunktion (TVF)

Im vorherigen U-SQL-Skript wiederholt Sie EXTRAHIEREN beim Lesen in der gleichen Quelldatei verwenden. U-SQL Tabellenwertfunktion können Sie Daten für eine spätere Verwendung zu kapseln.   

Das folgende Skript erstellt eine Tabellenwertfunktion namens *Searchlog()* in der Standarddatenbank und Schema:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Das folgende Skript veranschaulicht die Tabellenwertfunktion definiert im Skript verwenden:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Ansichten erstellen

Haben Sie nur einen Abfrageausdruck, der abstrakten möchten und sie parametrisieren möchten, können Sie eine Ansicht anstelle einer Tabellenwertfunktion erstellen.

Das folgende Skript erstellt eine Ansicht namens *SearchlogView* in der Standarddatenbank und Schema:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Das folgende Skript veranschaulicht die Verwendung der definierten Ansicht:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Erstellen von Tabellen

Wie relationale Datenbanktabelle kann U-SQL eine Tabelle mit einem vordefinierten Schema erstellen oder eine Tabelle erstellen und das Schema aus der Abfrage, die die Tabelle (auch bekannt als CREATE TABLE AS SELECT oder CTAS) füllt.

Das folgende Skript erstellt eine Datenbank und zwei Tabellen:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Abfragetabellen

Sie können Tabellen (im Skript erstellt) genauso wie Sie eine Abfrage über Dateien Abfragen. Anstatt ein Rowset mit EXTRAHIEREN, können Sie jetzt nur die Namen verweisen.

Transform-Skript zuvor verwendeten wird geändert, um aus den Tabellen lesen:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Beachten Sie, dass Sie derzeit SELECT auf einer Tabelle in einem Skript das Skript ausführen können, in dem die Tabelle erstellen.


##<a name="conclusion"></a>Abschluss

Das Lernprogramm behandelten ist nur ein kleiner Teil der U-SQL. Aufgrund des Umfangs dieses Lernprogramm kann nicht es, beinhalten:

- Mit CROSS APPLY, Zeichenfolgen, Arrays und Karten in Zeilen zu entpacken.
- Partitionierte Datensätze (Dateigruppen und partitionierten Tabellen) arbeiten.
- Entwickeln Sie benutzerdefinierte Operatoren wie Extraktionsprogramme Outputters Prozessoren, benutzerdefinierte Aggregatoren in C#.
- Verwenden Sie U-SQL Windowing-Funktionen.
- Verwalten Sie U-SQL-Code mit Ansichten, Tabellenwertfunktionen und gespeicherte Prozeduren.
- Auf Knoten Verarbeitung beliebigen benutzerdefinierten Code auszuführen.
- Verbinden Sie mit Azure SQL-Datenbanken und vereinheitlichen Abfragen über sie und Ihre U-SQL und Azure Data Lake.

## <a name="see-also"></a>Siehe auch

- [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
- [Entwickeln Sie U-SQL-Skripts mit Daten See Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Fensterfunktionen U-SQL verwenden Azure Data Lake Analytics Aufträge](data-lake-analytics-use-window-functions.md)
- [Überwachung und Problembehandlung von Azure Data Lake Analytics Aufträge mithilfe von Azure-Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Teilen Sie uns Ihre Meinung

- [Feature anfordern](http://aka.ms/adlafeedback)
- [Hilfe in den Foren](http://aka.ms/adlaforums)
- [Feedback zu U-SQL](http://aka.ms/usqldiscuss)
