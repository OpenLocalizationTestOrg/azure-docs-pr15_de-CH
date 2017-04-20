<properties
   pageTitle="Entwerfen von Beschlüssen und Codierungstechniken für SQL Data Warehouse-Entwicklung | Microsoft Azure"
   description="Die Begriffe der Webentwicklung, Designentscheidungen Recommendations und Codierungstechniken für SQL Data Warehouse."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Entwurf und Codierungstechniken für SQL Data Warehouse

Betrachten Sie diese Entwicklung Artikel wichtigsten Designentscheidungen, Vorschläge und Codierungstechniken für SQL Data Warehouse zu verstehen.

## <a name="key-design-decisions"></a>Wichtigsten Designentscheidungen
Die folgenden Artikel markieren einige Schlüsselkonzepte und Designentscheidungen für die Entwicklung der verteilten Datawarehouses mit SQL Data Warehouse verstehen müssen:

- [Anschlüsse][]
- [Parallelität][]
- [Transaktionen][]
- [Benutzerdefinierte schemas][]
- [tabellenverteilung][]
- [Indizes][]
- [Tabellenpartitionen][]
- [CTAS][]
- [Statistik][]

## <a name="development-recommendations-and-coding-techniques"></a>Entwicklung Empfehlung und Codierungstechniken
Diese Artikel hervorheben bestimmte Codierungstechniken, Tipps und Vorschläge für die Entwicklung von SQL Data Warehouse:

- [gespeicherte Prozeduren][]
- [Etiketten][]
- [Ansichten][]
- [temporäre Tabellen][]
- [Dynamisches SQL][]
- [Schleifen][]
- [Gruppe von Optionen][]
- [Zuweisen von Objektvariablen][]

## <a name="next-steps"></a>Nächste Schritte
Nach erfolgter Entwicklung Artikel betrachten Sie [Transact-SQL][] -Referenzseite für Weitere Informationen zur unterstützten Syntax für SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[Parallelität]: ./sql-data-warehouse-develop-concurrency.md
[Anschlüsse]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Dynamisches SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[Gruppe von Optionen]: ./sql-data-warehouse-develop-group-by-options.md
[Etiketten]: ./sql-data-warehouse-develop-label.md
[Schleifen]: ./sql-data-warehouse-develop-loops.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[gespeicherte Prozeduren]: ./sql-data-warehouse-develop-stored-procedures.md
[tabellenverteilung]: ./sql-data-warehouse-tables-distribute.md
[Indizes]: ./sql-data-warehouse-tables-index.md
[Tabellenpartitionen]: ./sql-data-warehouse-tables-partition.md
[temporäre Tabellen]: ./sql-data-warehouse-tables-temporary.md
[Transaktionen]: ./sql-data-warehouse-develop-transactions.md
[Benutzerdefinierte schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[Zuweisen von Objektvariablen]: ./sql-data-warehouse-develop-variable-assignment.md
[Ansichten]: ./sql-data-warehouse-develop-views.md
[Transact-SQL-Referenz]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
