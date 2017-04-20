<properties
   pageTitle="Verwaltung Compute in Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="Transact-SQL (T-SQL) Aufgaben Scale-Out-Performance durch DWUs anpassen. Kosten Sie durch Skalierung in nicht-Spitzenzeiten."
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Verwaltung Compute Azure SQL Data Warehouse (T-SQL)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Leistung durch dezentrales Skalieren berechnen Ressourcen und des Speichers ändern Ihre Arbeitslast gerecht. Kosten Sie durch Skalierung zurück Ressourcen Nebenzeiten oder Anhalten Compute insgesamt. 

Diese Auflistung von Tasks verwendet T-SQL auf:

- Aktuelle DWU Ansichtsoptionen
- Ändern von Datenverarbeitungsressourcen durch Anpassen von DWUs

Zum Anhalten oder Fortsetzen einer Datenbank, wählen Sie anderen Plattform-Optionen am Anfang dieses Artikels.

Informationen hierzu finden Sie unter [Verwalten berechnen Power (Übersicht)][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Aktuelle DWU Ansichtsoptionen

Anzeigen die aktuellen DWU für Datenbanken:

1. Öffnen Sie SQL Server-Objekt-Explorer in Visual Studio 2015.
2. Verbinden Sie mit der master-Datenbank mit logischen SQL-Datenbankserver verbunden sind.
2. Klicken Sie auf dynamische sys.database_service_objectives. Hier ist ein Beispiel: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skalierung compute

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

So ändern Sie die DWUs


1. Verbinden Sie mit der master-Datenbank die logischen SQL-Datenbankserver zugeordnet.
2. Verwenden Sie [ALTER DATABASE][] TSQL-Anweisung. Im folgende Beispiel wird das SLO auf DW1000 für die Datenbank MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte

Andere Verwaltungsaufgaben finden Sie unter [Überblick über][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Überblick]: ./sql-data-warehouse-overview-manage.md
[Verwalten von Compute Power (Übersicht)]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
