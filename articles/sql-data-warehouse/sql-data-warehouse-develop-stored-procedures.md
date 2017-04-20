<properties
   pageTitle="Gespeicherte Prozeduren in SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Implementierung von gespeicherter Prozeduren in Azure SQL Data Warehouse Lösungen."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="stored-procedures-in-sql-data-warehouse"></a>Gespeicherte Prozeduren in SQL Data Warehouse

SQL Data Warehouse unterstützt viele Transact-SQL-Funktionen von SQL Server. Wichtiger sind dezentrales Besonderheiten, die wir zur Maximierung der Leistung Ihrer Lösung nutzen möchten.

Sind jedoch verwalten die Skalierung und Performance von SQL Data Warehouse gibt es auch einige Features und Funktionen, die Verhaltensunterschiede und andere nicht unterstützt werden.

Erläutert, wie gespeicherte Prozeduren in SQL Data Warehouse implementieren.

## <a name="introducing-stored-procedures"></a>Einführung in Prozeduren
Gespeicherte Prozeduren sind eine hervorragende Möglichkeit zum Kapseln des SQL-Codes. in der Nähe der Daten im Datawarehouse gespeichert. Kapselt den Code in verwaltbare Einheiten helfen gespeicherte Prozeduren Lösungsvorschläge modularisiert Entwickler; größere Wiederverwendung von Code erleichtern. Jede gespeicherte Prozedur kann auch flexibler machen Parameter akzeptieren.

SQL Data Warehouse stellt eine vereinfachte und optimierte gespeicherte Prozedur Implementierung. Der größte Unterschied im Vergleich zu SQL Server ist die gespeicherte Prozedur nicht bereits kompilierten Code. Datawarehouses sind wir in der Regel weniger um die Kompilierung. Es ist wichtiger, dass Code der gespeicherten Prozedur ordnungsgemäß optimiert ist, wenn mit großen Datenmengen. Das Ziel ist, Stunden, Minuten und Sekunden nicht Millisekunden zu speichern. Daher ist es hilfreicher, gespeicherte Prozeduren als Container für SQL-Logik vorstellen.     

Wenn SQL Data Warehouse gespeicherte Prozedur ausgeführt werden SQL-Anweisungen, übersetzten und optimierten Laufzeit analysiert. Dabei wird jede Anweisung in verteilten Abfragen konvertiert. Der SQL-Code, der die Daten tatsächlich ausgeführt wird unterscheidet die Abfrage gesendet.

## <a name="nesting-stored-procedures"></a>Verschachtelung gespeicherte Prozeduren
Wenn gespeicherte Prozeduren anderen gespeicherten Prozeduren oder dynamisches Sql ausführen, und der innere gespeicherte Prozedur oder Code Aufruf geschachtelt werden soll.

SQL Data Warehouse unterstützt maximal 8 Schachtelungsebenen. Dies ist etwas anders als SQL Server. Die Schachtelungsebene in SQL Server ist 32.

Aufruf der obersten Ebene gespeicherten Prozedur entspricht zum Verschachteln von Ebene 1

```sql
EXEC prc_nesting
```
Stellt die gespeicherte Prozedur auch anderen EXEC rufen steigt diese Schachtelungsebene 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Wenn die zweite Prozedur dann einige dynamisches Sql ausgeführt wird, das Verschachteln auf 3 erhöht
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Hinweis SQL Data Warehouse unterstützt derzeit keine @@NESTLEVEL. Sie müssen die Schachtelungsebene selbst verfolgen. Es ist unwahrscheinlich, 8 verschachteln Ebene Grenzwert erreichen, jedoch können Sie müssen Code arbeiten und "reduzieren", damit sie innerhalb dieses Zeitraums passt.

## <a name="insertexecute"></a>EINFÜGEN... AUSFÜHREN
SQL Data Warehouse erlaubt keine Ergebnismenge einer gespeicherten Prozedur mit einer INSERT-Anweisung verwenden. Es ist jedoch eine alternative Verwendung.

Finden Sie im folgenden Artikel auf [temporäre Tabellen] ein Beispiel dazu.

## <a name="limitations"></a>Grenzen

Es gibt einige Aspekte der gespeicherten Transact-SQL-Prozeduren, die nicht im SQL Data Warehouse implementiert werden.

Sie sind:

- temporäre gespeicherte Prozeduren
- nummerierte gespeicherte Prozeduren
- Erweiterte gespeicherte Prozeduren
- Gespeicherte CLR-Prozeduren
- Verschlüsselungsoption
- Replikation
- Tabellenwertparameter
- schreibgeschützte Parameter
- Standardparameter
- Ausführungskontexte
- return-Anweisung

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->

<!--Article references-->
[temporäre Tabellen]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Übersicht über die Anwendungsentwicklung]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
