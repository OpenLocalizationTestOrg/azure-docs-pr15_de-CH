<properties
   pageTitle="Datentypen für Tabellen im SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit Datentypen für Azure SQL Data Warehouse-Tabellen."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Datentypen für Tabellen im SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

SQL Data Warehouse unterstützt verwendeten die am häufigsten Datentypen.  Es folgt eine Liste der von SQL Data Warehouse unterstützten Datentypen.  Weitere Informationen zu Datentyp unterstützt, finden Sie unter [Erstellen][].

|**Unterstützte Datentypen**|||
|---|---|---|
[bigint][]|[Dezimal][]|[smallint][]|
[Binär][]|[float][]|[smallmoney][]|
[Bit][]|[int][]|[sysname][]|
[Char][]|[Money][]|[Zeit][]|
[Datum][]|[NCHAR][]|[tinyint][]|
[DateTime][]|[nvarchar][]|[uniqueidentifier][]|
[datetime2][]|[Real][]|[varbinary][]|
[DateTimeOffset][]|[smalldatetime][]|[varchar][]|


## <a name="data-type-best-practices"></a>Best Practices-Datentyp

 Wenn die Spaltentypen definieren, wird der kleinste Datentyp Unterstützung für Ihre Daten Abfrageperformance verbessern. Dies ist besonders wichtig für CHAR und VARCHAR-Spalten. Ist der längste Wert in einer Spalte 25 Zeichen, definieren Sie die Spalte als VARCHAR(25). Vermeiden Sie alle Zeichenspalten an eine große Standardlänge definieren. Darüber hinaus definieren Sie Spalten als VARCHAR Wenn dies alles ist, die benötigt wird, anstatt [NVARCHAR][]verwenden.  Anstelle NVARCHAR(4000) oder vom Datentyp VARCHAR(8000) möglichst NVARCHAR(MAX) oder VARCHAR(MAX).

## <a name="polybase-limitation"></a>Polybase Einschränkung

Wenn Sie Polybase verwenden, um Tabellen laden, definieren Sie Tabellen, sodass mögliche Länge, einschließlich die Gesamtlänge aller Spalten variabler Länge 32.767 Byte nicht überschreiten.  Während eine Zeile mit variabler Länge definieren können, die über diese Breite und Zeilen mit BCP kann können nicht mit Polybase Daten laden Sie.  Polybase wird Breite Zeilen bald unterstützt werden.

## <a name="unsupported-data-types"></a>Nicht unterstützte Datentypen

Wenn Sie die Datenbank von einer anderen Plattform von SQL Azure SQL-Datenbank, migrieren Sie migrieren, treffen Sie einige Datentypen, die auf SQL Data Warehouse unterstützt werden.  Im folgenden sind nicht unterstützte Datentypen sowie alternativen anstelle von nicht unterstützten Datentypen verwenden.

|Datentyp|Abhilfe|
|---|---|
|[Geometrie][]|[varbinary][]|
|[Geografie][]|[varbinary][]|
|[hierarchyid][]|[nvarchar][] (4000)|
|[Bild][ntext,text,image]|[varbinary][]|
|[Text][ntext,text,image]|[varchar][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[sql_variant][]|Spalte in mehrere stark typisierten Spalten aufteilen.|
|[Tabelle][]|Temporäre Tabellen konvertieren.|
|[Zeitstempel][]|Überarbeiten von Code [datetime2][] verwendet und `CURRENT_TIMESTAMP` Funktion.  Nur Konstanten werden als Standard daher Current_timestamp kann nicht definiert werden als Default-Einschränkung. Benötigen Sie eine Timestampspalte typisierte Versionswerte migrieren verwenden Sie [BINARY][](8) oder [VARBINARY][BINARY](8) für nicht NULL oder NULL-Werten der Zeile.|
|[XML][]|[varchar][]|
|[Benutzerdefinierte Typen][]|möglichst an ihren systemeigenen Typen konvertieren|
|Standardwerte|Standardwerte unterstützt Literalen und Konstanten nur.  Nicht-deterministische Ausdrücke oder -Funktionen, wie `GETDATE()` oder `CURRENT_TIMESTAMP`, werden nicht unterstützt.|

Die folgenden SQL kann Ihre ausgeführt werden SQL-Datenbank zu Spalten sind nicht von Azure SQL Data Warehouse unterstützt:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Artikel auf [Tabelle][Übersicht], [eine Tabelle Verteilung][verteilen] [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition], [Tabellenstatistik Verwalten von][Statistiken] und [Temporärtabellen][temporäre].  Weitere Informationen über bewährte Methoden finden Sie unter [SQL Data Warehouse Best Practices][].

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

<!--MSDN references-->

<!--Other Web references-->
[Tabelle erstellen]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[Binär]: https://msdn.microsoft.com/library/ms188362.aspx
[Bit]: https://msdn.microsoft.com/library/ms177603.aspx
[Char]: https://msdn.microsoft.com/library/ms176089.aspx
[Datum]: https://msdn.microsoft.com/library/bb630352.aspx
[DateTime]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[DateTimeOffset]: https://msdn.microsoft.com/library/bb630289.aspx
[Dezimal]: https://msdn.microsoft.com/library/ms187746.aspx
[float]: https://msdn.microsoft.com/library/ms173773.aspx
[Geometrie]: https://msdn.microsoft.com/library/cc280487.aspx
[Geografie]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[Money]: https://msdn.microsoft.com/library/ms179882.aspx
[NCHAR]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[Real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[Tabelle]: https://msdn.microsoft.com/library/ms175010.aspx
[Zeit]: https://msdn.microsoft.com/library/bb677243.aspx
[Zeitstempel]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[Benutzerdefinierte Typen]: https://msdn.microsoft.com/library/ms131694.aspx
