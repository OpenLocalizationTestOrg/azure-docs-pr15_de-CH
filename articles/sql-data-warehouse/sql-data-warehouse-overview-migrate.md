<properties
   pageTitle="Migrieren die Lösung zu SQL Data Warehouse | Microsoft Azure"
   description="Hinweise zur Migration dafür Projektmappe Azure SQL Data Warehouse Plattform."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Migrieren der Lösung zu SQL Data Warehouse

SQL Data Warehouse ist ein verteiltes Datenbanksystem, das Ihren Bedürfnissen elastisch skaliert. Um Leistung und Skalierung beizubehalten, werden nicht alle Funktionen von SQL Server im SQL Data Warehouse implementiert. Die folgenden Themen Migration Tippen Sie auf einige der wichtigsten Faktoren für die Migration der Projektmappe zu SQL Data Warehouse. Entwerfen von Datawarehouses Skalierung führt verschiedene Design Patterns und traditionelle Ansätze die beste nicht immer. Daher möglicherweise zur Anpassung der vorhandenen Projektmappe wird sichergestellt, dass Sie die verteilte Plattform SQL Data Warehouse nutzen.

Es ist auch wichtig SQL Data Warehouse eine in Microsoft Azure-Plattform ist. Daher kann die Migration auch gehört übertragen Ihrer Daten in der Cloud. Die Datenübertragung ist ein eigenes Thema und sorgfältig überdacht werden sollte; insbesondere beim steigern. Datenübertragung und Laden von Daten können Sie einzelne Themen.

## <a name="migration-guidance"></a>Hinweise zur Migration

Sicherstellen Sie, dass Sie diese Artikel um sicherzustellen, dass Sie einige Unterschiede und grundlegende Konzepte verstehen, vor der Migration gelesen haben.

- [Migrieren des Schemas][]
- [Migrieren von Daten][]
- [Migrieren von code][]

## <a name="next-steps"></a>Nächste Schritte

Katze (Customer Advisory Team) hat auch einige gute SQL Data Warehouse-Anleitung die sie Blogs veröffentlicht.  Betrachten Sie ihre Artikel [Migrieren von Daten in Azure SQL Data Warehouse in der Praxis][] zusätzliche Anleitung zur Migration.

<!--Image references-->

<!--Article references-->
[Migrieren des Schemas]: sql-data-warehouse-migrate-schema.md
[Migrieren von Daten]: sql-data-warehouse-migrate-data.md
[Migrieren von code]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Migrieren von Daten in Azure SQL Data Warehouse in der Praxis]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
