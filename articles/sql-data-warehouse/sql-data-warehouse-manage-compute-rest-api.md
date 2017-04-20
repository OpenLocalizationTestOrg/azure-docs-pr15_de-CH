<properties
   pageTitle="Verwaltung Compute in Azure SQL Data Warehouse (REST) | Microsoft Azure"
   description="PowerShell Aufgaben berechnen macht. Skalierung compute-Ressourcen durch DWUs anpassen. Anhalten oder Fortsetzen von computeressourcen, um Kosten zu sparen."
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

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Verwaltung Compute in Azure SQL Data Warehouse (REST)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Leistung durch dezentrales Skalieren berechnen Ressourcen und des Speichers ändern Ihre Arbeitslast gerecht. Kosten Sie durch Skalierung zurück Ressourcen Nebenzeiten oder Anhalten Compute insgesamt. 

Diese Auflistung von Tasks verwendet im Azure-Portal:

- Skalierung compute
- Pause compute
- Resume compute

Informationen hierzu finden Sie unter [Verwalten berechnen Overview][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Rechenleistung skalieren

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Um die DWUs zu ändern, verwenden Sie die REST API [Erstellen oder aktualisieren][] . Im folgende Beispiel wird das SLO auf DW1000 für die Datenbank MySQLDW, die auf Server MyServer befindet. Der Server ist ein Azure Ressourcengruppe mit dem Namen ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pause compute

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Verwenden Sie zum Anhalten einer Datenbank die [Datenbank angehalten][] REST-API. Im folgende Beispiel wird eine Datenbank namens Database02 mit dem Namen Server01 gehostet angehalten. Der Server ist ein Azure Ressourcengruppe mit dem Namen ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Resume compute

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Verwenden Sie zum Starten einer [Bewerberdatenbank][] REST-API. Im folgende Beispiel wird eine Datenbank namens Database02 mit dem Namen Server01 gehostet. Der Server ist ein Azure Ressourcengruppe mit dem Namen ResourceGroup1. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte

Andere Verwaltungsaufgaben finden Sie unter [Überblick über][].

<!--Image references-->

<!--Article references-->
[Überblick]: ./sql-data-warehouse-overview-manage.md
[Verwalten von Compute-Übersicht]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Pause-Datenbank]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Lebenslauf-Datenbank]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Erstellen oder Aktualisieren von Datenbank]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
