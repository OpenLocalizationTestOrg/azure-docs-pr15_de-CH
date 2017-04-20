<properties
    pageTitle="Compatibility level Bewertung | Microsoft Azure"
    description="Schritte und Tools zur Bestimmung der Kompatibilitätsgrad für die Datenbank in Azure SQL-Datenbank oder Microsoft SQL Server"
    services="sql-database"
    documentationCenter=""
    authors="alainlissoir"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.devlang="NA"
    ms.tgt_pltfrm="NA"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="alainl"/>


# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Verbesserte Leistung Kompatibilität auf 130 in Azure SQL-Datenbank


Azure SQL-Datenbank läuft transparent Hunderte oder Tausende von Datenbanken an vielen verschiedenen Kompatibilitätsgraden, beibehalten und gewährleistet die Abwärtskompatibilität mit der entsprechenden Version von Microsoft SQL Server für seine Kunden!

Daher hindert nichts Kunden profitieren von neuen Abfrageoptimierer und Abfrage Prozessorfunktionen vorhandenen Datenbanken auf die aktuelle Kompatibilität ändern. An Verlauf Ausrichtung SQL Versionen standardmäßig Kompatibilitätsgraden lauten wie folgt:

- 100: in SQL Server 2008 und SQL Azure-Datenbank V11.
- 110: in SQL Server 2012 und Azure SQL-Datenbank V11.
- 120: in SQL Server 2014 und Azure SQL-Datenbank V12.
- 130: in SQL Server V12 2016 und Azure SQL-Datenbank.


> [AZURE.IMPORTANT] Ab **Mitte 2016**in Azure SQL-Datenbank werden der Standard-Kompatibilitätsgrad 130 statt 120 für **neu erstellte** Datenbanken.
> 
> Mitte 2016 erstellte Datenbanken werden *nicht* betroffen und behalten ihre aktuelle Kompatibilitätsgrad (100, 110 oder 120). Datenbanken migrieren von Azure SQL-Datenbank Version haben V11, V12 nicht deren Kompatibilitätsgrad geändert.


In diesem Artikel untersucht die Vorteile der Kompatibilitätsgrad 130 und wie Sie diese Vorteile nutzen. Wir adressieren Nebenwirkungen auf die Abfrageperformance für vorhandenen SQL-Applikationen.


## <a name="about-compatibility-level-130"></a>Über 130-Kompatibilitätsgrad


Wenn den aktuelle Kompatibilitätsgrad der Datenbank möchten, führen Sie zunächst die folgende Transact-SQL-Anweisung.


```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Vor dieser Änderung auf 130 für **neu** erstellte Datenbanken, wir prüfen diese Änderung durch einige sehr einfache Beispiele für geht, und wie jeder profitieren kann.

Verarbeitung in relationalen Datenbanken kann sehr komplex sein und viele Informatik und Mathematik verbundenen Designoptionen und Verhalten führen. In diesem Dokument wurde Inhalt absichtlich vereinfacht damit jeder Benutzer mit minimalen technischen Hintergrund der Auswirkungen der Änderung der Kompatibilität und bestimmen, wie sie Programme profitieren.

Werfen Sie einen Blick auf der Kompatibilitätsgrad 130 am Tisch bringt.  Finden Sie weitere Informationen unter [ALTER Kompatibilitätsgrad der Datenbank (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), aber hier ist eine kurze Zusammenfassung:

- Einfügen einer Insert Select-Anweisung kann mit mehreren Threads oder haben einen parallelen Plan vor diesem Vorgang Singlethread-while.
- Speicher optimiert Tabelle und Variablen Abfragen können nun parallele Pläne while, bevor dieser Vorgang auch Singlethreaded wurde.
- Statistik für Tabelle Speicher optimiert jetzt genießen und werden automatisch aktualisiert. Siehe [Datenbankmodul Neuheiten: im Arbeitsspeicher OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) Weitere Informationen.
- Batch-Modus V/s ändert Zeile Spalte Speicher Indizes
  - Für eine Tabelle mit einer Spalte Speicher sind jetzt im Batch-Modus.
  - Windowing-Aggregate arbeiten nun im Batch-Modus wie TSQL Verzögerung/führen.
  - Abfragen auf Tabellen mit mehreren unterschiedlichen Klauseln Spalte Speicher im Batch-Modus ausgeführt werden.
  - Abfragen unter DOP = 1 oder mit einem seriellen Plan auch im Batch-Modus.
- Kardinalität Vorkalkulation Verbesserung kommen tatsächlich mit Kompatibilitätsgrad 120, aber für diejenigen mit niedrigeren Kompatibilitätsgrad (d. h. 100 oder 110), die Verschiebung auf die Kompatibilität auf 130 bringt diese Verbesserung und diese profitieren auch die Abfrageperformance Anwendung.


## <a name="practicing-compatibility-level-130"></a>Kompatibilitätsgrad 130 üben


Erste erfahren Sie einige Tabellen, Indizes und Zufallsdaten erstellt, um diese neuen Funktionen zu üben. Beispiele für TSQL-Skripts können unter SQL Server 2016 oder Azure SQL-Datenbank ausgeführt werden. Stellen Sie jedoch beim Erstellen einer Azure SQL-Datenbank mindestens eine P2-Datenbank wählen Sie brauchen Sie über mindestens zwei Kerne Multi-threading und daher von diesen Merkmalen profitieren.


```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Nun werfen wir einen Blick auf einige Merkmale der Verarbeitung der Abfrage mit Kompatibilitätsgrad 130.


## <a name="parallel-insert"></a>Parallele einfügen


Ausführen der TSQL-Anweisung unten führt den Einfügevorgang unter Kompatibilitätsgrad 120 und 130, die bzw. den Einfügevorgang in einem einzelnen Thread-Modell (120) und ein Multi-Thread-Modell (130) ausgeführt wird.


```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Anfordern der Abfrageplan auf der Grafik oder des XML-Inhalts können Sie die Kardinalität Vorkalkulation bestimmen Funktion spielen. Betrachtet die Pläne Seite für Seite in Abbildung 1 deutlich sehen wir, dass die Ausführung Spalte Shop Einfügen von 120 seriellen Parallel 130 geht. Beachten Sie, dass das Symbol Iterator 130 Plan mit zwei parallele Pfeile veranschaulicht die Tatsache, dass jetzt der Iterator die Ausführung tatsächlich parallel ist. Große Einfügevorgänge abgeschlossen haben, wird die parallele Ausführung an die zentrale für die Datenbank zur Verfügung haben verknüpft besser; Abhängig von Ihrer Situation, bis zu 100 Mal schneller!


*Abbildung 1: Einfügevorgang ändert sich von serielle, parallele Kompatibilität Ebene 130.*


![Abbildung 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)


## <a name="serial-batch-mode"></a>SERIELLE Batchmodus


Bei der Verarbeitung von Datenzeilen Kompatibilität auf 130 verschieben kann ebenso Stapelverarbeitung Modus. Zunächst stehen Batchoperationen Modus nur wenn Sie einen Spaltenindex Speicher verfügen. Zweitens ein Stapel steht normalerweise ca. 900 Zeilen ein Code Logik für multicore-Prozessor und höheren Durchsatz optimiert und direkt nutzt die komprimierten Daten möglichst Speichertyp Spalte. Unter diesen Umständen SQL Server 2016 verarbeiten ~ 900 Zeilen auf einmal statt 1 Zeile zur Zeit und folglich insgesamt Gemeinkosten des Vorgangs jetzt freigegeben ist der gesamte Batch zeilenweise Gesamtkosten verringern. Dieser freigegebenen Operationen im Wesentlichen mit der Spalte Speicher kombiniert reduziert der Latenz an einem Betrieb aktivieren. Finden weitere Informationen über den Spalte-Speicher und Stapelverarbeitungsmodus [Columnstore Indizes](https://msdn.microsoft.com/library/gg492088.aspx)Guide.


```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

– The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Wie unten wird anhand die Abfrage plant Side-by-Side in Abbildung 2, können wir sehen, dass der Verarbeitungsmodus mit dem Kompatibilitätsgrad geändert wurde und daher beim Ausführen von Abfragen in beiden Kompatibilitätsgrad insgesamt, wir, dass die meisten der Verarbeitungszeit sehen verbracht Modus (86 %) gegenüber den Batchmodus (14 %), 2 Batches verarbeitet wurden. Das Dataset erhöhen, erhöhen wird.


*Abbildung 2: Wählen Sie Vorgang ändert serielle aus, Batch-Modus mit Kompatibilitätsgrad 130.*


![Abbildung 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)


## <a name="batch-mode-on-sort-execution"></a>Im Batchmodus sortieren Ausführung


Ähnlich wie oben, aber für einen Sortiervorgang Übergang Modus (Kompatibilitätsgrad 120), Batch-Modus (Kompatibilitätsgrad 130) verbessert die Leistung des Sortiervorgangs aus denselben Gründen.


```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Sichtbar-nebeneinander in Abbildung 3, sehen Sie, dass der Sortiervorgang Modus repräsentiert 81 % der Kosten im Batchmodus nur 19 % der Kosten (bzw. 81 % und 56 % auf die selbst).


*Abbildung 3: Sortiervorgang wird von Zeile zu Batch-Modus mit Kompatibilitätsgrad 130.*


![Abbildung 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)


Natürlich enthalten diese Beispiele nur Zehntausende von Zeilen nichts bei den Daten in den meisten SQL Server diese Tage. Stattdessen nur Projekt gegen Millionen von Zeilen und das Übersetzen in wenigen Minuten Ausführung täglich bis Ihre Arbeitslast gespart.


## <a name="cardinality-estimation-ce-improvements"></a>Verbesserung der Kardinalität Schätzung (CE)


Mit SQL Server 2014 eingeführt, eine Datenbank mit dem Kompatibilitätsgrad 120 oder über Nutzen der neuen Kardinalität Abschätzung Funktionalität. Kardinalität Schätzung ist die Logik bestimmt, wie SQL Server eine Abfrage auf Grundlage der geschätzten Kosten führen. Die Schätzung wird berechnet Input aus Objekten Abfrage beteiligt. Praktisch auf hoher Kardinalität Vorkalkulation Funktionen handelt Zeile Anzahl zusammen mit Informationen über die Verteilung der Werte eindeutigen Wert zählt und doppelte Zahlen in Tabellen und Objekte in der Abfrage verwiesen wird. Falsche Schätzung der Kosten kann unnötige Festplatte aufgrund unzureichender Arbeitsspeicher gewährt (d. h. TempDB Überläufe) oder an einem seriellen Plan Ausführung einer parallelen Ausführung führen zu nennen. Abschließend führt zu einer allgemeinen Leistungsabfall der Abfrage unzutreffende Schätzungen. Auf der anderen Seite bessere Schätzwerte genauere Schätzung führt zu besseren abfrageausführungen!

Wie bereits erwähnt, Abfrage Optimierungen und Vorkalkulationen sind komplex, aber wenn Sie Abfragepläne und Estimator Kardinalität erfahren möchten, finden Sie das Dokument zur [Optimierung Ihrer Abfrageplänen mit SQL Server 2014 Kardinalität Gutachter](https://msdn.microsoft.com/library/dn673537.aspx) eingehender.


## <a name="which-cardinality-estimation-do-you-currently-use"></a>Die Kardinalität Schätzung verwenden Sie zurzeit?


Bestimmt unter die Kardinalität-Schätzung Abfragen ausgeführt, wir verwenden einfach die Abfrage Beispiele unten. Beachten Sie, dass dieses erste Beispiel unter Kompatibilitätsgrad 110, dass alte Kardinalität Vorkalkulation Funktionen ausgeführt werden.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Nach Abschluss der Ausführung klicken Sie auf XML und die Eigenschaften des ersten Iterators betrachten Sie, wie unten dargestellt. Beachten Sie den Namen der Eigenschaft namens CardinalityEstimationModelVersion auf 70 festgelegt. Es bedeutet nicht, dass der Kompatibilitätsgrad der Datenbank auf die SQL Server 7.0-Version (110 in der TSQL-Anweisung oben als sichtbar festgelegt) festgelegt ist, sondern der Wert 70 einfach legacy Kardinalität Vorkalkulation Funktionen seit SQL Server 7.0, die keine bedeutende Revisionen bis SQL Server 2014 (kommt mit dem Kompatibilitätsgrad von 120).


*Abbildung 4: Das CardinalityEstimationModelVersion wird auf 70 festgelegt, Kompatibilitätsgrad 110 oder unter Verwendung.*


![Abbildung 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)


Alternativ können Sie den Kompatibilitätsgrad auf 130 ändern und Deaktivieren der neuen Kardinalität Vorkalkulation Funktion mit LEGACY_CARDINALITY_ESTIMATION mit [ALTER Bereich DATENBANKKONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx)festgelegt. Dadurch werden genau mit 110 Gesichtspunkt eine Kardinalität Vorkalkulation Funktion, mit der Verarbeitung Kompatibilitätsgrad der aktuellen Abfrage. Damit profitieren von der neuen Abfrage-Verarbeitung Funktionen mit der aktuellen Kompatibilitätsgrad (d. h. im Batchmodus) können die alten Kardinalität Vorkalkulation Funktionen ggf. noch abhängig.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Verlagerung des Kompatibilitätsgrads 120 oder 130 ermöglicht die neue Kardinalität Schätzung. In diesem Fall standardmäßig CardinalityEstimationModelVersion festgelegt entsprechend 120 oder 130 wie unten angezeigt.


```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Abbildung 5: Die CardinalityEstimationModelVersion ist 130 fest Verwendung einen Kompatibilitätsgrad von 130.*


![Abbildung 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)


## <a name="witnessing-the-cardinality-estimation-differences"></a>Erleben die Kardinalität Vorkalkulation Unterschiede


Nun führen wir etwas komplexer Abfragen mit einem INNER JOIN mit einer WHERE-Klausel mit einigen Prädikaten und betrachten geschätzte Anzahl Zeilen aus der alten Kardinalität Vorkalkulation Funktion zuerst.


```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Abfrage effektiv 200,704 Zeilen zurückgibt, während Zeile Vorkalkulation mit alten Kardinalität Vorkalkulation Funktionen 194,284 Zeilen vorgibt. Offensichtlich, wie gesagt, diese Ergebnisse der Zeilenzählung hängt auch wie oft die vorhergehenden Beispiele ausführen die Beispieltabellen immer wieder bei jedem Lauf füllt. Natürlich werden die Prädikate in Ihrer Abfrage auch Einfluss auf die aktuelle Vorkalkulation neben Tabellenform Dateninhalt, und wie diese Daten tatsächlich miteinander korrelieren.


*Abbildung 6: Geschätzte Anzahl Zeilen ist 194,284 oder 6000 Zeilen 200,704 Zeilen erwartet.*


![Abbildung 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)


Auf gleiche Weise wir jetzt ausführen derselben Abfrage mit der neuen Funktionalität Kardinalität Schätzung.


```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Auf der unten jetzt sehen wir, dass die Zeile Schätzung 202,877, oder viel näher und höher als die alte Kardinalität Vorkalkulation.

*Abbildung 7: Geschätzte Anzahl Zeilen ist jetzt 202,877 statt 194,284.*


![Abbildung 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)


In Wirklichkeit das Ergebnis ist 200,704 Zeilen (aller hängt ab, wie oft Abfragen der vorherigen Beispiele ausgeführt, aber wichtiger TSQL RAND() Anweisung verwendet die tatsächlichen Werte zurückgegeben variieren aus einem zum nächsten). In diesem Beispiel nicht die neue Kardinalität Vorkalkulation daher besser zur Schätzung der Anzahl der Zeilen ist 202,877 viel näher, 200704, 194,284! Schließlich ändern Sie die WHERE-Klausel auf Prädikate (statt ">" zum Beispiel), dies könnte die Vorkalkulationen zwischen der alten und neuen Kardinalität Funktion noch je nach Anzahl entspricht kann.

Natürlich stellt in diesem Fall ~ 6000 Zeilen aus aus Zählung wird keine große Datenmengen in einigen Situationen dar. Austauschen, das Millionen von Zeilen in mehrere Tabellen und komplexere Abfragen und manchmal die Schätzung aus Millionen von Zeilen und daher von der falschen Ausführungsplan aufnehmen oder Speichermangel Zuschüsse zu TempDB verschüttete und mehr e/a-Anforderung sind wesentlich höher.

Haben Sie die Möglichkeit, Übung dieser Vergleich mit die häufigste Abfragen Datasets, und überzeugen Sie sich selbst von wie viel einige der alten und neuen Schätzungen sind betroffen, während einige nur mehr aus der Wirklichkeit oder einige andere nur einfach näher auf die aktuelle Zeile werden konnte in den Resultsets tatsächlich zurückgegebene zählt. Alles hängt die Form der Abfragen, SQL Azure-Datenbank Merkmale, die Art und Größe der Datasets und Statistiken über sie. Wenn Sie die Instanz SQL Azure erstellen, müssen der Abfrageoptimierer wissen völlig anstelle der Wiederverwendung der vorherigen Abfrage führt Statistiken erstellen. Die Schätzwerte sind sehr kontextuellen und fast jeder Situation Server- und entsprechend. Es ist wichtig zu beachten.


## <a name="some-considerations-to-take-into-account"></a>Einige Aspekte berücksichtigen


Obwohl die meisten Arbeitslasten von der Kompatibilitätsgrad 130 vor Annahme den Kompatibilitätsgrad für Ihre produktionsumgebung profitieren haben Sie im Grunde 3 Optionen:

1. Kompatibilität auf 130 verschoben und sehen, wie Dinge. Beachten Sie einige Regressionen, Sie einfach die Kompatibilität auf die ursprüngliche Ebene festlegen, oder 130 und nur reverse Kardinalität Schätzung zurück, des legacy-Modus (wie bereits dargelegt, dies allein lösen das Problem).
2. Testen die vorhandene Anwendung unter ähnlichen Arbeitslast, optimieren und Überprüfen der Leistung vor Produktion. Bei Problemen wie oben, Sie können immer wieder auf den ursprünglichen Kompatibilitätsgrad gehen oder einfach stornieren Kardinalität Schätzung auf legacy-Modus.
3. Als letzte Möglichkeit und letzten können diese Fragen ist der Abfrage Speicher. Das ist heute empfohlen! Zur Unterstützung die Analyse der Abfragen-Kompatibilität 120 Ebene oder unterhalb und 130, wir können nicht empfehlen genug Abfrage verwendet. Query Store ist mit der neuesten Version von Azure SQL-Datenbank V12 verfügbar, und es soll Ihnen bei der Behandlung von Leistungsproblemen helfen. Abfrage-Speicher als flugdatenschreiber für Ihre Datenbank sammeln und Präsentieren von Informationen alle Abfragen historisch vorstellen. Dies vereinfacht Leistung Forensics gesenkt diagnostizieren und beheben. Finden Sie weitere Informationen unter [Abfrage Store: flugdatenschreiber für die Datenbank](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).


AT der allgemeinen Wenn Sie bereits eine Reihe von Datenbanken Kompatibilitätsgrad 120 oder unter, und einige 130, oder da die Arbeitslast automatisch neue Datenbanken werden bereitgestellt werden demnächst auf 130, beachten Sie Folgendes:

- Aktivieren Sie auf Kompatibilität neuer Produktion ändern, Query Store. Sie können weitere Informationen zu [Ändern](https://msdn.microsoft.com/library/bb895281.aspx) , der Datenbank-Kompatibilitätsmodus verwenden die Abfrage verweisen.
- Testen Sie danach alle kritische Arbeitslasten mit repräsentativen Daten und Abfragen einer produktionsähnlichen Umgebung und vergleichen die Leistung erfahren und laut Query Store. Wenn einige Regressionen auftreten, identifizieren die zurückgesetzten Abfragen mit der Abfrage und mithilfe des Planes Query Store Option erzwingen (aka plan fixieren). In diesem Fall Sie endgültig Kompatibilitätsgrad 130 bleiben, und den ehemalige Abfrageplan als von Abfrage laden.
- Wenn Sie neue Features und Funktionen von Azure SQL-Datenbank (SQL Server 2016 läuft) nutzen möchten, aber sind Änderungen der Kompatibilitätsgrad 130, als letztes Mittel sollten Sie erzwingen der Kompatibilitätsgrad wieder auf, die Ihre Arbeit mithilfe von ALTER DATABASE-Anweisung entspricht. Aber zunächst Query Store Plan festhalten Option die beste Option ist, denn nicht mit 130 grundsätzlich auf der Funktionsebene einer älteren Version von SQL Server ist.
- Haben mandantenfähige Applikationen mehrere Datenbanken möglicherweise aktualisieren Bereitstellung Logik Datenbanken konsistent Kompatibilitätsgrad in allen Datenbanken sichergestellt werden; ALT und neu bereitgestellt sind. Die Leistung Ihrer Anwendung Workload konnte sein, dass einige Datenbanken auf verschiedenen Kompatibilitätsgraden ausgeführt werden und daher möglicherweise Kompatibilität auf Konsistenz einer Datenbank erforderlich, um auf Ganzer Linie für Ihre Kunden bereitstellen. Beachten Sie, dass kein Mandat, es hängt wirklich wie der Kompatibilitätsgrad die Anwendung auswirken.
- Vorkalkulation Kardinalität und wie ändern den Kompatibilitätsgrad zunächst in Produktion empfiehlt es, testen die Arbeitslast unter Umständen neuen feststellen, ob Kardinalität Vorkalkulation Verbesserung Ihrer Anwendung profitiert.


## <a name="conclusion"></a>Abschluss


Mithilfe von Azure SQL-Datenbank alle SQL Server 2016 Weiterentwicklungen profitieren kann abfrageausführungen deutlich verbessern. Wie-ist! Natürlich wie jede neue Funktion eine ordnungsgemäße Auswertung erforderlich, um die genauen Vorschriften bestimmen unter dem die Arbeitslast der beste. Die Erfahrung zeigt, dass die meisten Aufgaben werden voraussichtlich mindestens transparent unter Kompatibilitätsgrad 130, und nutzen neue Funktionen sowie neue Kardinalität Vorkalkulation Abfrage. Die so realistisch sind immer Ausnahmen und richtige Fälligkeitsdatum tun Sorgfalt beurteilt wichtig festzustellen, wie viel profitieren diese Optimierungen. Und wieder Abfrage Laden dieser Arbeit eine große Hilfe!

Weiterentwicklung von SQL Azure erwarten Sie zukünftig Kompatibilitätsgrad 140. Wenn Zeit ist, beginnen wir was diese zukünftige Kompatibilitätsgrad 140 bringen, reden, wie wir kurz erörtert welche Kompatibilitätsgrad 130 heute bringt.

Jetzt nicht vergessen, ab Juni 2016 Azure SQL-Datenbank ändert sich den Standard-Kompatibilitätsgrad von 120 130 für neu erstellte Datenbanken. Denken!


## <a name="references"></a>Referenzen


- [Neuigkeiten im Datenbankmodul](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)

- [Blog: Abfrage Store: flugdatenschreiber für Ihre Datenbank von Borko Novakovic, Juni 8 2016](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)

- [ALTER der Kompatibilitätsgrad (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)

- [BEWERTETE DATENBANKKONFIGURATION ÄNDERN](https://msdn.microsoft.com/library/mt629158.aspx)

- [Kompatibilitätsgrad 130 für SQL Azure-Datenbank V12](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)

- [Optimieren der Abfrage-Pläne mit SQL Server 2014 Kardinalität Gutachter](https://msdn.microsoft.com/library/dn673537.aspx)

- [Columnstore Indizes Guide](https://msdn.microsoft.com/library/gg492088.aspx)

- [Blog: Verbesserte Leistung von Abfragen mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank von Alain Lissoir Mai 6 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)



<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
