<properties
    pageTitle="Beispiele für häufige Verwendungsmuster Stream Analytics Abfrage | Microsoft Azure"
    description="Allgemeine Azure Stream Analytics Abfragemuster "
    keywords="Beispiele für"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Beispiele für häufige Verwendungsmuster Stream Analytics Abfragen

## <a name="introduction"></a>Einführung

Abfragen in Azure Stream Analytics werden in einer SQL-ähnlichen Abfragesprache ausgedrückt im [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) Guide dokumentiert.  Dieser Artikel beschreibt Projektmappen auf mehrere allgemeine Abfrage basierend auf realen Szenarien.  Es ist In Arbeit und weiterhin mit neuen laufend aktualisiert.

## <a name="query-example-data-type-conversions"></a>Beispielabfrage: Datentypen
**Beschreibung**: Definieren Sie die Typen der Eigenschaften im Eingabestream.
Z. B. Gewicht im Eingabestream kommt als Zeichenfolgen und muss INT auszuführenden konvertiert ZUSAMMENGEFASST.

**Eingabe**:

| Stellen | Zeit | Gewicht |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | '1000' |
| Honda | 2015-01-01T00:00:02.0000000Z | "2000" |

**Ausgabe**:

| Stellen | Gewicht |
| --- | --- |
| Honda | 3000 |

**Lösung**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Erklärung**: die Anweisung eine Umwandlung im Feld Gewicht zur Angabe des Typs (Liste der unterstützten Datentypen [hier](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Beispielabfrage: wie nicht wie auf übereinstimmende Muster
**Beschreibung**: Überprüfen Sie, dass ein Feldwert für das Ereignis ein bestimmtes Muster z. B. return Kennzeichen entspricht, die mit A beginnen und enden mit 9

**Eingabe**:

| Stellen | Nummernschild | Zeit |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Toyota | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Ausgabe**:

| Stellen | Nummernschild | Zeit |
| --- | --- | --- |
| Toyota | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Lösung**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Erklärung**: die Anweisung wie überprüfen, ob der Feldwert Nummernschild mit beginnt dann ist eine Zeichenfolge aus null oder mehr Zeichen und mit 9 endet. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Beispiel für eine Abfrage: Geben Logik für verschiedene Fälle/Werte (CASE-Anweisungen)
**Beschreibung**: andere Berechnung für ein Feld anhand einiger Kriterien angeben.
Beispielsweise bieten Sie eine beschreibende Zeichenfolge für wie viele Autos eines bestimmten Herstellers mit besonderen Fall 1 übergeben.

**Eingabe**:

| Stellen | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Ausgabe**:

| CarsPassed | Zeit |
| --- | --- | --- |
| 1 Honda | 2015-01-01T00:00:10.0000000Z |
| 2 Toyota | 2015-01-01T00:00:10.0000000Z |

**Lösung**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Erklärung**: die CASE-Klausel können zu einer anderen Berechnung anhand einiger Kriterien (in unserem Fall die Anzahl von Autos im Fenster aggregate).

## <a name="query-example-send-data-to-multiple-outputs"></a>Beispielabfrage: Senden von Daten an mehrere Ausgaben
**Beschreibung**: Senden von Daten an mehrere Ziele in einem Auftrag.
Z. B. Datenanalyse für eine Warnung Schwellenwerten basieren und Archivieren aller Ereignisse BLOB-Speicher

**Eingabe**:

| Stellen | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output1**:

| Stellen | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Ausgang2**:

| Stellen | Zeit | Anzahl |
| --- | --- | --- |
| Toyota | 2015-01-01T00:00:10.0000000Z | 3 |

**Lösung**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Erklärung**: die INTO-Klausel weist Stream Analytics der Ausgaben dieser Anweisung geschrieben.
Die erste Abfrage ist eine Pass-Through-Daten, die wir Ausgabe erhalten wir ArchiveOutput benannt.
Die zweite Abfrage enthält einige einfache Aggregation und Filtern und sendet die Ergebnisse einer nachgelagerten Warnsystem.
*Hinweis*: Sie können auch Ergebnisse von CTEs (d. h. mit der Anweisung) in mehrere ausgabeanweisungen wiederverwenden – Dies hat den Vorteil, weniger Leser mit der Datenquelle zu öffnen.
Zum Beispiel 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Beispielabfrage: eindeutige Werte zählen
**Beschreibung**: Anzahl eindeutiger Feldwerte im Stream in einem Zeitfenster angezeigt.
Z. B. wie viele eindeutige stellen Autos Mautstation in einem zweiten Fenster 2 durchlaufen?

**Eingabe**:

| Stellen | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Ausgabe:**

| Anzahl | Zeit |
| --- | --- |
| 2 | 2015-01-01T00:00:02.000Z |
| 1 | 2015-01-01T00:00:04.000Z |

**Lösung:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Beschreibung:** Wir führen eine anfängliche Aggregation zu eindeutig macht mit ihren über das Fenster.
Dann machen wir eine Aggregation wir alle eindeutigen Werte in einem Fenster erhalten denselben Zeitstempel dann das zweite Aggregation Fenster muss minimal 2 Windows zunächst nicht zusammenfassen gegeben haben – wie viele macht.

## <a name="query-example-determine-if-a-value-has-changed"></a>Abfragebeispiel: ermitteln, ob ein Wert geändert wurde#
**Beschreibung**: Betrachten eines vorherigen Werts, ob anders ist als der aktuelle Wert z. B. vorherige Auto auf der Straße gebührenfrei zu aktuellen Auto?

**Eingabe**:

| Stellen | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Ausgabe**:

| Stellen | Zeit |
| --- | --- |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Lösung**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Erläuterung**: Verzögerung einen Blick in die Eingabe stream ein Ereignis zurück und stellen nutzen. Auf das aktuelle Ereignis vergleichen Sie und geben Sie das Ereignis aus, wenn sie unterschiedlich sind.

## <a name="query-example-find-first-event-in-a-window"></a>Beispielabfrage: erste Ereignis in einem Fenster Suchen
**Beschreibung**: alle 10 Minuten erste Auto suchen?

**Eingabe**:

| Nummernschild | Stellen | Zeit |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Ausgabe**:

| Nummernschild | Stellen | Zeit |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |

**Lösung**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Nachdem wir das Problem und finden erste Auto bestimmten ändern in alle 10 Minuten.

| Nummernschild | Stellen | Zeit |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Lösung**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Beispielabfrage: Letztes Ereignis in einem Fenster Suchen
**Beschreibung**: letzte Auto suchen in alle 10 Minuten.

**Eingabe**:

| Nummernschild | Stellen | Zeit |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Ausgabe**:

| Nummernschild | Stellen | Zeit |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Lösung**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Erläuterung**: zwei Schritte in der Abfrage – zuerst sucht neuesten Zeitstempel in Windows 10 Minuten. Im zweite Schritt verknüpft die Ergebnisse der ersten Abfrage mit ursprünglichen Ereignisse entsprechen letzten Zeitstempel in jedem Fenster finden. 

## <a name="query-example-detect-the-absence-of-events"></a>Beispielabfrage: keine Ereignisse erkennen
**Beschreibung**: Überprüfen Sie, ob ein Stream kein Wert, die bestimmten Kriterien entspricht.
Beispielsweise haben 2 aufeinanderfolgende Autos aus der gleichen gebührenpflichtige Straßen innerhalb von 90 Sekunden eingegeben?

**Eingabe**:

| Stellen | Nummernschild | Zeit |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Honda | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Toyota | DEF-987 | 2015-01-01T00:00:03.0000000Z |
| Honda | GHI 345 | 2015-01-01T00:00:04.0000000Z |

**Ausgabe**:

| Stellen | Zeit | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01-01T00:00:02.0000000Z | AAA-999 | ABC-123 | 2015-01-01T00:00:01.0000000Z |

**Lösung**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Erläuterung**: Verzögerung einen Blick in die Eingabe stream ein Ereignis zurück und stellen nutzen. Dann vergleichen sie auf das aktuelle Ereignis und das Ereignis gleich und LAG zum Abrufen von Daten über das vorherige Auto verwenden.

## <a name="query-example-detect-duration-between-events"></a>Beispielabfrage: Dauer zwischen Ereignissen erkennen
**Beschreibung**: die Dauer eines bestimmten Ereignisses finden. Bestimmen Sie beispielsweise angegebene Web Clickstream für eine Funktion aufgewendete Zeit.

**Eingabe**:  
  
| Benutzer | Funktion | Ereignis | Zeit |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Starten | 2015-01-01T00:00:01.0000000Z |
| user@location.com | RightMenu | Ende | 2015-01-01T00:00:08.0000000Z |
  
**Ausgabe**:  
  
| Benutzer | Funktion | Dauer |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Lösung**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Erläuterung**: letzte Funktion Verwendung beim Typ wurde 'Start' letzter Zeitwert abgerufen. Beachten Sie, dass Funktion kennzeichnet PARTITION BY [Benutzer] Ergebnis pro eindeutigen Benutzer berechnet werden.  Die Abfrage eine Stunde Obergrenze Zeitdifferenz zwischen "Start" und "Stop" aber konfigurierbar als erforderlichen (LIMIT DURATION(hour, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Beispielabfrage: Dauer einer Bedingung erkennen
**Beschreibung**: herauszufinden, wie lange für eine Bedingung aufgetreten ist.
Angenommen, die einen Fehler, der alle Autos mit einer falschen Gewicht geführt haben (über 20.000 Pfund) – wir die Dauer des Fehlers berechnen möchten.

**Eingabe**:

| Stellen | Zeit | Gewicht |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | 2000 |
| Toyota | 2015-01-01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:08.0000000Z | 2000 |

**Ausgabe**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z | 2015-01-01T00:00:07.000Z |

**Lösung**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Beschreibung**: mit Verzögerung zu Eingabestream 24 Stunden suchen Instanzen, StartFault und StopFault durch übergreifende, Gewicht < 20000.

## <a name="query-example-fill-missing-values"></a>Beispielabfrage: fehlende Werte ausfüllen
**Beschreibung**: für den Stream von Ereignissen, die fehlende Werte erzeugen einen Stream von Ereignissen mit.
Beispielsweise generieren Sie Ereignis alle 5 Sekunden, die vor kurzem gesehen Datenpunkt melden.

**Eingabe**:

| t | Wert |
|--------------------------|-------|
| "2014-01-01T06:01:00" | 1 |
| "2014-01-01T06:01:05" | 2 |
| "2014-01-01T06:01:10" | 3 |
| "2014-01-01T06:01:15" | 4 |
| "2014-01-01T06:01:30" | 5 |
| "2014-01-01T06:01:35" | 6 |

**Ausgabe (die ersten 10 Zeilen)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014-01-01T14:01:00.000Z | 2014-01-01T14:01:00.000Z | 1 |
| 2014-01-01T14:01:05.000Z | 2014-01-01T14:01:05.000Z | 2 |
| 2014-01-01T14:01:10.000Z | 2014-01-01T14:01:10.000Z | 3 |
| 2014-01-01T14:01:15.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:20.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:25.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:30.000Z | 2014-01-01T14:01:30.000Z | 5 |
| 2014-01-01T14:01:35.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:40.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:45.000Z | 2014-01-01T14:01:35.000Z | 6 |

    
**Lösung**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Erklärung**: Diese Abfrage generiert Ereignisse alle 5 Sekunden und gibt das letzte Ereignis, das bereits empfangen wurde. [Hopping Fenster] (https://msdn.microsoft.com/library/dn835041.aspx "Hopping Fenster - Azure Stream Analytics") Dauer bestimmt, wie weit die Abfrage suchen wird das neueste Ereignis (300 Sekunden in diesem Beispiel).


## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
