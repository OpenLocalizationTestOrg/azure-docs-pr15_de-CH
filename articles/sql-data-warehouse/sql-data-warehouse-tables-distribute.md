<properties
   pageTitle="Verteilen Tabellen im SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit Tabellen in Azure SQL Data Warehouse verteilen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>Verteilen von Tabellen im SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

SQL Data Warehouse ist eine parallele Verarbeitung (MPP) Datenbank verteilt.  Daten und Verarbeitungskapazität auf mehreren Knoten bieten SQL Data Warehouse große skalierbar - weit über jedes einzelne System.  Wie die Daten im SQL Data Warehouse ist einer der wichtigsten Faktoren, um optimale Leistung zu erreichen.   Der Schlüssel zum optimalen Daten minimiert und wiederum ist der Schlüssel zur Minimierung von Daten die richtigen Distributionsstrategie auswählen.

## <a name="understanding-data-movement"></a>Grundlegendes zum Verschieben von Daten

MPP-System die Daten aus jeder Tabelle in zugrunde liegenden Datenbanken aufgeteilt.  Optimierten Abfragen auf einem MPP-System können einfach durch Ausführen der einzelnen verteilten Datenbanken ohne Interaktion zwischen den Datenbanken übergeben werden.  Angenommen haben Sie eine Datenbank mit Verkaufsdaten enthält zwei Tabellen, Verkauf und Debitoren.  Wenn Sie eine Abfrage, die das sales-Tabelle in der Customer-Tabelle verknüpfen und Sie verteilen Ihre Vertriebs- und Kundentabellen, Kundennummer, jeden Kunden in einer separaten Datenbank, können Fragen die Verkaufs- und Kundendaten beitreten innerhalb jeder Datenbank ohne Wissen der anderen Datenbanken gelöst werden.  Dagegen Wenn Ihre Verkaufsdaten nach Bestellnummer und Ihre Daten nach Kundennummer unterteilt dann angegebene Datenbank müssen die entsprechenden Daten für jeden Kunden und möchten Ihre Verkaufsdaten Kundendaten beitreten, müsste zum Abrufen der Daten für jeden Kunden aus den Datenbanken.  In diesem zweiten Beispiel müssten die Daten, um die Kundendaten in die Verkaufsdaten zu verschieben, so dass die beiden Tabellen verknüpft werden können.  

Daten nicht immer schlecht, manchmal ist es erforderlich, eine Abfrage zu lösen.  Aber wenn dieser zusätzliche Schritt vermieden, natürlich die Abfrage schneller.  Daten entsteht am häufigsten auf, wenn Tabellen verknüpft oder Aggregationen durchgeführt.  Häufig möchten, müssen zwar für ein Szenario, wie eine Verknüpfung optimieren können Sie weiterhin Datentransfer zu einem anderen Szenario wie eine Aggregation lösen.  Der Trick ist herauszufinden, die weniger Arbeit.  In den meisten Fällen ist das Verteilen einer Regel verknüpften Spalte große Faktentabellen die effektivste Methode zur Verringerung des meisten Datentransfers.  Verteilen von Daten auf Verknüpfungsspalten wird ein viel zu Datentransfer als Daten für Spalten in einer Aggregation verteilt.

## <a name="select-distribution-method"></a>Wählen Sie Verteilungsmethode

Hinter den Kulissen teilt SQL Data Warehouse Daten in 60 Datenbanken.  Jeder einzelnen Datenbank wird als **Verteilerliste**bezeichnet.  Beim Laden von Daten in jeder Tabelle hat SQL Data Warehouse wissen, wie Sie Ihre Daten über diesen 60 Distributionen unterteilen.  

Die Verteilungsmethode auf Tabellenebene definiert, und zurzeit gibt es zwei Optionen:

1. **Round-Robin** die Daten nach dem Zufallsprinzip aber gleichmäßig verteilen.
2. **Distributed Hash** der Daten basierend auf Werte aus einer einzelnen Spalte hashing verteilt

Standardmäßig Wenn Sie keine Verteilungsmethode Daten definieren wird die Tabelle verteilt Methode für **Round-Robin** -Verteilung.  Wie Sie in Ihrer Implementierung immer raffinierter, und sollten Sie in Erwägung **distributed Hash** Tabellen um wiederum Abfrageperformance optimieren Daten zu minimieren.

### <a name="round-robin-tables"></a>Round-Robin-Tabellen

Mit der Round-Robin-Methode verteilt Daten ist sehr wie klingt.  Die Daten geladen werden, wird jede Zeile einfach der nächsten Verteilung gesendet.  Diese Methode zum Verteilen von Daten verteilt immer zufällig Daten sehr gleichmäßig über alle Distributionen.  Es also kein während der Round-Robin-Prozess, der die Daten sortieren.  Round-Robin-Verteilung wird einen zufälligen Hash deshalb bezeichnet.  Mit verteilten Round Robin besteht keine Notwendigkeit, die Daten.  Aus diesem Grund stellen Roundrobin-Tabellen häufig gute laden Ziele.

Standardmäßig wird keine Verteilungsmethode gewählt, Round-Robin-Verteilungsmethode dienen.  Beim Round-Robin-Tabellen einfach sind Daten zufällig System verteilt werden, bedeutet, dass das System die Verteilung garantieren kann jedoch jede Zeile auf.  Daher muss das System manchmal Aufrufen eine Datenoperation verschieben Ihre Daten besser zu organisieren, bevor eine Abfrage auflösen können.  Dieser zusätzliche Schritt kann Abfragen verlangsamen.

Berücksichtigen Sie beim Round-Robin-Verteilung für die Tabelle in den folgenden Szenarien:

- Zu Beginn als einfache Ausgangspunkt
- Wenn es keine offensichtlichen beitretenden Schlüssel
- Wenn es nicht gut Spalte Hash die Tabelle verteilen
- Wenn die Tabelle einen gemeinsame Verbindungsschlüssel nicht mit anderen Tabellen gemeinsam
- Wenn die Verbindung als andere Joins in der Abfrage
- Wenn die Tabelle eine temporäre Stagingtabelle ist

In beiden Beispielen werden eine Roundrobin-Tabelle erstellen:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] Beim Round-Robin explizit in der DDL Art der Standard empfohlen ist sind die Absichten der das Tabellenlayout für andere.

### <a name="hash-distributed-tables"></a>Hash verteilte Tabellen

Mithilfe eines Algorithmus **distributed Hash** Tabellen verteilen kann für viele Szenarien Leistungssteigerung durch Reduzierung der Daten zur Abfragezeit.  Verteilte Hashtabellen sind Tabellen unterteilt werden zwischen den verteilten Datenbanken mit einem Hashalgorithmus für eine einzelne Spalte auswählen.  Verteilungsspalte bestimmt wie die Daten in verteilten Datenbanken aufgeteilt werden.  Die Hash-Funktion verwendet die verteilungsspalte Distributionen Zeilen zuweisen.  Der Hashalgorithmus und resultierende Verteilung ist deterministisch.  Hat der gleiche Wert mit demselben Datentyp wird immer der gleiche Verteilung.    

Dieses Beispiel erstellt eine Tabelle ID verteilt:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Wählen Sie verteilungsspalte

Beim Verteilen **Hash** eine Tabelle auswählen, müssen Sie einen Distributionsordner Spalte.  Wenn eine verteilungsspalte sind drei wichtige Faktoren zu berücksichtigen.  

Wählen Sie eine einzelne Spalte die:

1. Nicht aktualisiert
2. Gleichmäßiges Verteilen von Daten, Daten neigen vermeiden
3. Minimieren von Daten

### <a name="select-distribution-column-which-will-not-be-updated"></a>Wählen Sie verteilungsspalte nicht aktualisiert

Verteilung Spalten sind daher nicht aktualisierbar, wählen Sie eine Spalte mit statischen Werten.  Wenn eine Spalte aktualisiert werden muss, ist es im Allgemeinen nicht gute Kandidaten.  Ist ein Fall, in dem eine verteilungsspalte aktualisieren müssen, können dies durch die Zeile zuerst gelöscht und eine neue Zeile einfügen.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Wählen Sie verteilungsspalte der Daten gleichmäßig verteilt

Da nur so schnell wie die langsamste Verteilung ein verteiltes System führt ist wichtig, die gleichmäßig über die Verteilung verteilen Erreichung ausgewogene Umsetzung im System.  Unterteilt die Arbeit in einem verteilten System basiert auf die Daten für jede Verteilung lebt.  Dadurch sehr wichtig, wählen Sie die verteilungsspalte rechts für die Verteilung von Daten, damit jede Verteilung gleich arbeiten hat und gleichzeitig seinen Teil der Arbeit.  Wenn das System auch Arbeit unterteilt ist, ist die Distributionen Daten verteilt.  Wenn Daten nicht gleichmäßig ausgeglichen, rufen wir diese **Daten neigen**.  

Um Daten gleichmäßig aufzuteilen und Daten neigen, berücksichtigen Sie bei die Distribution-Spalte auswählen:

1. Wählen Sie eine Spalte enthält zahlreiche unterschiedliche Werte.
2. Verteilen von Daten auf unterschiedliche Werte zu vermeiden. 
3. Verteilen von Daten für Spalten mit einer hohen Frequenz von NULL zu vermeiden.
4. Vermeiden Sie Verteilen von Daten auf Datumsspalten.

Da jeder Wert 1 60 Distributionen gehasht wird, um gleichmäßige Verteilung zu erreichen möchten Sie eine Spalte auswählen, die einzigartigen und 60 eindeutige Werte enthalten.  Zur Veranschaulichung nehmen wir einmal eine Spalte nur 40 eindeutige Werte hat.  Wenn diese Spalte als Verteilungsschlüssel ausgewählt wurde, würden die Daten für diese Tabelle 40 Distributionen, angespielt werden 20 Distributionen keine verarbeiten müssen und keine Daten verlassen.  Dagegen müsste die 40 Distributionen mehr Arbeit, wenn die Daten über 60 Verteilung gleichmäßig verteilt wurde.  Dieses Szenario ist ein Beispiel neigen.

Alle Distributionen ihren Anteil an dieser Arbeit abgeschlossen MPP System wartet jedem Abfrage.  Wenn einem mehr als andere Arbeit, dann die Ressource die Distributionen sind im Wesentlichen verschwendet beschäftigt Verteilung warten.  Wenn Arbeit alle Distributionen nicht gleichmäßig verteilt ist, rufen wir diese **Verzerrung verarbeiten**.  Verarbeitung Neigung verursachen Abfragen langsamer als kann Wenn die Arbeitslast gleichmäßig auf die Distributionen verteilt werden.  Daten neigen führt Verarbeitung neigen.

Vermeiden Sie Verteilung für eine Spalte stark als null-Werte auf der gleichen landet. Verteilung für eine Spalte kann auch Verarbeitung Neigung denn alle Daten für ein bestimmtes Datum auf der gleichen land. Wenn mehrere Abfragen ausführen werden alle Filter auf gleichen und nur 1 60 Verteilung aller seit einem bestimmten Datum ausführen wird nur eine Verteilung. In diesem Szenario wird die Wahrscheinlichkeit Abfragen 60 Mal langsamer, wenn die Daten gleichmäßig über alle Distributionen verteilten. 

Wenn keine guten Kandidaten Spalten vorhanden sind, sollten Sie Round-Robin als Verteilungsmethode verwenden.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Wählen Sie verteilungsspalte der Daten minimieren

Minimieren Daten durch Auswählen der Spalte rechts Verteilung ist eine der wichtigsten Strategien zum Optimieren der Leistung des SQL Data Warehouse.  Daten entsteht am häufigsten auf, wenn Tabellen verknüpft oder Aggregationen durchgeführt.  Spalten im `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` und `HAVING` für **gute machen** Klauseln hash Verteilung geeignet. 

Die anderen Spalten in der `WHERE` Klausel **nicht** gut Hash Spalte Kandidaten da sie beschränken die Abfrage Distributionen Teilnahme neigen Verarbeitung verursacht.  Ein gutes Beispiel für eine Spalte, die möglicherweise auf verteilen verlockend, aber kann dazu führen, dass diese Verarbeitung neigen wird eine Datumsspalte.

Im Allgemeinen haben Sie zwei große Faktentabellen häufig Beteiligten in einer Verknüpfung, erhalten die meisten Leistung Sie beide Tabellen auf die Join-Spalten verteilt.  Haben Sie eine Tabelle, die nicht mit einem anderen großen Faktentabelle angehört, suchen Sie auf Spalten, die häufig in der `GROUP BY` Klausel.

Es gibt einige wichtige Kriterien, die erfüllt sein müssen, während eine Verknüpfung zu Daten:

1. Die Tabellen in der Verknüpfung muss auf **eine** der Spalten in der Verknüpfung distributed Hash.
2. Beide Tabellen müssen die Datentypen der Join-Spalten überein.
3. Die Spalten müssen mit Equals-Operator verbunden werden.
4. Die Art der Verknüpfung möglicherweise kein `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Fehlerbehandlung neigen

Wenn Tabellendaten verwenden der Hash besteht die Möglichkeit, dass einige Distributionen unverhältnismäßig mehr Daten als andere verzerrt werden. Eine zu große Datenmenge Neigung kann Leistung auswirken, da das Endergebnis einer verteilten Abfrage warten muss, bis die längste Verteilung fertig stellen. Je nach Neigung Daten müssen Sie adressieren.

### <a name="identifying-skew"></a>Identifizieren der Neigung

Eine einfache Möglichkeit, eine Tabelle identifizieren geneigt ist mit `DBCC PDW_SHOWSPACEUSED`.  Dies ist eine sehr schnelle und einfache Möglichkeit Anzahl Zeilen anzeigen, die in jeweils 60 Verteilung der Datenbank gespeichert sind.  Beachten Sie, dass die Zeilen in der Tabelle verteilte ausgewogene Leistung gleichmäßig über alle Distributionen verteilt werden soll.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Wenn Sie die dynamische Verwaltungsansichten (DMV) von Azure SQL Data Warehouse Abfragen können Sie eine ausführlichere Analyse ausführen.  Um zu erstellen Sie die [dbo.vTableSizes][] Ansicht mit SQL aus der[ [Tabelle]Artikel]  Erstellte Ansicht dieser Abfrage zu bestimmen, welche Tabellen haben mehr als 10 % Daten Neigung.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Auflösen von Daten neigen

Reicht nicht alle Neigung um eine Korrektur zu rechtfertigen.  In einigen Fällen kann die Leistung in einigen Abfragen einer Tabelle Schaden Daten neigen zunichte machen.  Entscheidung, ob Daten aufgelöst werden muss sollten Neigung in einer Tabelle Sie möglichst viele Daten-Volumes und Abfragen Ihrer Arbeit verstehen.   Eine Möglichkeit der Auswirkungen der Neigung wird anhand der Schritte in Artikel [Abfrage Überwachung][] überwachen die Auswirkung der Neigung Leistung und insbesondere die Auswirkung wie lange Abfragen dauern auf einzelnen Distributionen.

Verteilen von Daten wird finden Sie das richtige Gleichgewicht zwischen Daten Neigung minimieren und Daten minimiert. Diese können gegen Ziele und manchmal neigen Daten um Daten reduziert werden soll. Beispielsweise wird die verteilungsspalte häufig freigegebenen Spalte in Joins und Aggregationen, werden Sie Daten minimiert werden. Die Vorteile des minimale Datentransfers kann die Auswirkung der Daten neigen überwiegen. 

Gewohnt zu Daten Neigung ist die Tabelle mit einer Spalte verschiedene Verteilerlisten neu erstellen. Da es keine Möglichkeit die verteilungsspalte einer vorhandenen Tabelle, um die Verteilung einer Tabelle ändern ändern sie mit einem [[CTAS]] erstellen.  Hier sind zwei Beispiele für Daten neigen beheben:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Beispiel 1: Neuerstellen der Tabelle eine neue Spalte Verteilung

Dieses Beispiel verwendet eine Tabelle mit einer Spalte des Hashwertes Verteilung neu erstellen [CTAS] []. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Beispiel 2: Erstellen der Tabelle mit Round-Robin-Verteilung

Dieses Beispiel verwendet [CTAS] [] Tabelle Round-Robin statt hashverteilung neu erstellen. Diese Änderung wird auch Datendistribution Kosten mehr Daten erzeugt. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über den Entwurf von Tabellen finden Sie unter [verteilen][], [Index][], [Partition][], [Datentypen][], [Statistiken][] und [Temporärtabellen][temporäre] Artikel.

Eine Übersicht über die Vorgehensweisen finden Sie unter [SQL Data Warehouse Best Practices][].


<!--Image references-->

<!--Article references-->
[Übersicht]: ./sql-data-warehouse-tables-overview.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Temporäre]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Überwachen der Abfrage]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
