<properties
   pageTitle="Dynamisches SQL im SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von dynamic SQL Azure SQL Data Warehouse Lösungen."
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

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamisches SQL im SQL Datawarehouse
Bei der Entwicklung von Anwendungscode für SQL Data Warehouse müssen Sie dynamisches Sql verwenden, um flexibel und modular generische Lösungen. SQL Data Warehouse unterstützt die Blob-Datentypen zu diesem Zeitpunkt nicht. Dies kann die Größe der Zeichenfolgen beschränken, als Blob varchar(max) bzw. nvarchar(max) Typen gehören. Wenn Sie diese Typen im Anwendungscode verwendet haben, als sehr große Zeichenfolgen erstellen, müssen Sie den Code in Blöcken und die EXEC-Anweisung verwenden.

Ein einfaches Beispiel:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Wenn die Zeichenfolge kurzer können Sie [Sp_executesql][] Normal.

> [AZURE.NOTE] Als dynamisches SQL Statements werden weiterhin alle TSQL Validierungsregeln.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Anwendungsentwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
