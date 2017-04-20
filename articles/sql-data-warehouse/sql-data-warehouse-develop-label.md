<properties
   pageTitle="Verwenden Sie Etiketten Instrument Abfragen im SQL Data Warehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Etiketten Instrument Abfragen in Azure SQL Data Warehouse Lösungen."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Beschriftung Instrument Abfragen im SQL Data Warehouse
SQL Data Warehouse unterstützt das Konzept Abfrage Etiketten. Betrachten Sie vor Tiefe wir ein Beispiel:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Diese letzte Zeile markiert die Zeichenfolge "Meine Abfrage Bezeichnung" der Abfrage. Dies ist besonders hilfreich, da die Bezeichnung Abfrage über die DMVs ist. Dies bietet ein Mechanismus zum Erkennen von problematischen Abfragen und Fortschritt einer ETL-Ausführung zu identifizieren.

Eine gute Namenskonvention hilft hier. Z. B. etwas wie "Projekt: Verfahren: Anweisung: Kommentar ' würde zur Identifizierung der Abfrage unter den Code im Datenquellen-Steuerelement.

Bezeichnung suchen können Sie die folgende Abfrage, die die dynamische Verwaltungsansichten verwendet:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] Es ist wichtig, dass Sie Klammern oder Anführungszeichen Bezeichnung Word umbrochen, wenn Abfragen. Bezeichnung ein reserviertes Wort und hat einen Fehler verursacht, wenn es nicht getrennt wurde.


## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Anwendungsentwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
