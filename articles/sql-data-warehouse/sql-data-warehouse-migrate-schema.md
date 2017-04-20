<properties
   pageTitle="Migration des Schemas zu SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Migration des Schemas zu Azure SQL Data Warehouse Lösungen."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Das Schema mit SQL Data Warehouse migrieren#

Die folgenden Übersichten können Sie die Unterschiede zwischen SQL Server und SQL Data Warehouse die Datenbank migrieren.

## <a name="table-migration"></a>Tabelle-Migration

Tabellen migrieren, sollten Sie die Tabellenfeatures von SQL Data Warehouse-Tabellen kennen.  Die [Tabelle][] ist ein hervorragender Ausgangspunkt.  In diesem Artikel werden die wichtigsten Aspekte beim Erstellen einer Tabelle wie Tabellenstatistik, Verteilung, Partitionierung und Indizierung.  Es behandelt auch einige [nicht unterstützte Tabellenfunktionen][] und Abhilfen.

SQL Data Warehouse unterstützt allgemeine Business-Datentypen.  Finden Sie eine Liste der [Datentypen][] unterstützte und [nicht unterstützte Datentypen][].  [Datentypen][] Artikel enthält außerdem eine Abfrage [nicht unterstützten Datentypen][]zu ermitteln.  Beim Konvertieren von Datentypen müssen Sie [Daten vom optimalen][]betrachten.

## <a name="next-steps"></a>Nächste Schritte
Nachdem das Datenbankschema SQL Data Warehouse erfolgreich migriert haben, fahren Sie mit einem der folgenden Artikel:

- [Migrieren von Daten][]
- [Migrieren von code][]

Weitere Informationen zu bewährten SQL Data Warehouse finden Sie auf [best Practices][] .

<!--Image references-->

<!--Article references-->
[Migrieren von code]: ./sql-data-warehouse-migrate-code.md
[Migrieren von Daten]: ./sql-data-warehouse-migrate-data.md
[Bewährte Methoden]: ./sql-data-warehouse-best-practices.md
[Tabelle]: ./sql-data-warehouse-tables-overview.md
[nicht unterstützte Tabellenfunktionen]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[nicht unterstützte Datentypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[best Practices-Datentyp]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->
