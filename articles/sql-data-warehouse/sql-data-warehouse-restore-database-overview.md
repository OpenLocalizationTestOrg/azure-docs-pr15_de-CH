<properties
   pageTitle="SQL Data Warehouse wiederherstellen | Microsoft Azure"
   description="Übersicht der Datenbank Wiederherstellungsoptionen für die Wiederherstellung einer Datenbank in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>SQL Data Warehouse wiederherstellen

> [AZURE.SELECTOR]
- [Übersicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

SQL Data Warehouse bietet lokalen und geografischen Wiederherstellung als Teil seiner Data Warehouse Disaster Recovery-Funktionen. Verwenden Sie Data Warehouse Backups Datawarehouse auf einen Wiederherstellungspunkt in der primären Region wiederherstellen, oder Geo überflüssige Sicherungskopien wiederherstellen einer anderen geographischen Region. Die Einzelheiten der Wiederherstellung ein Datawarehouse erläutert.

## <a name="what-is-a-data-warehouse-restore"></a>Was ist ein Data Warehouse wiederherstellen?

Data Warehouse Wiederherstellung ist eine neue Datawarehouse, die aus einer Sicherung eines vorhandenen erstellt oder gelöschten Datawarehouse. Das wiederhergestellten Datawarehouse erstellt gesicherte Datawarehouse zu einem bestimmten Zeitpunkt. Da SQL Data Warehouse ein verteiltes System, ist ein Data Warehouse Wiederherstellen von vielen Sicherungsdateien erstellt, die in Azure-Blobs gespeichert. 

Wiederherstellung der Datenbank ist Bestandteil jeder Business Continuity und Disaster Recovery-Strategie, da Ihre Daten nach versehentliche Beschädigung oder Löschung neu erstellt.

Weitere Informationen finden Sie unter:

-  [SQL Data Warehouse backups](sql-data-warehouse-backups.md)
-  [Business Continuity – Überblick](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Data Warehouse-Wiederherstellungspunkte

Azure Premium Speicher nutzen verwendet SQL Data Warehouse Azure Storage Blob Snapshots primären Datawarehouse sichern. Jeder Snapshot enthält einen Wiederherstellungspunkt für den Zeitpunkt der Snapshot gestartet. Um ein Datawarehouse wiederherzustellen, wählen Sie einen Wiederherstellungspunkt und Befehl wiederherstellen.  

SQL Data Warehouse wird immer die Sicherung eines neuen Datawarehouse wiederhergestellt. Halten die wiederhergestellten Datawarehouse und der aktuellen, oder löschen sie. Wenn Sie das aktuelle Datawarehouse mit dem Warehouse wiederhergestellten Daten ersetzen möchten, können Sie ihn umbenennen.

Benötigen Sie eine gelöschte oder angehaltenen Datawarehouse wiederherstellen, können Sie [eine Support-Ticket zu erstellen](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>Geo-redundant wiederherstellen

Bei Verwendung von Geo-redundanten Speicher können Sie das Datawarehouse mit dem [paarweisen Rechenzentrum](../best-practices-availability-paired-regions.md) in einer anderen geographischen Region wiederherstellen. Das Datawarehouse wird von der letzten täglichen Sicherung wiederhergestellt. 

## <a name="restore-timeline"></a>Zeitachse wiederherstellen

Sie können eine Datenbank innerhalb der letzten sieben Tage verfügbaren Wiederherstellungspunkt wiederherstellen. Snapshots alle vier bis acht Stunden und 7 Tage zur Verfügung. Wenn ein Snapshot älter als sieben Tage ist, Ablauf und der Wiederherstellungspunkt ist nicht mehr verfügbar.

## <a name="restore-costs"></a>Wiederherstellen von Kosten

Speicher kostenlos wiederhergestellten Datawarehouse ist Rate Azure Premium Speicher berechnet. 

Anhalten einer wiederhergestellten Datawarehouse werden Sie Speicher Rate Azure Premium Speicher berechnet. Der Vorteil der Pause ist nicht DWU Computerressourcen berechnet werden.

Weitere Informationen zu Preisen SQL Data Warehouse finden Sie unter [SQL Data Warehouse Preise](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Verwendet für die Wiederherstellung

Data Warehouse-Wiederherstellung wird primäre Daten Wiederherstellung nach Datenverlust oder Beschädigung.

Data Warehouse wiederherstellen können Sie eine Sicherung länger als sieben Tage beibehalten. Nach der Wiederherstellung Sie das Datawarehouse online und anhalten können unbegrenzt computekosten zu sparen. Angehaltene Datenbank entstehen Lagerkosten Rate Azure Premium-Speicher. 

## <a name="related-topics"></a>Verwandte Themen

### <a name="scenarios"></a>Szenarien

- Eine Übersicht über Business Continuity finden Sie unter [Business Continuity – Überblick](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

Um ein Data Warehouse Wiederherstellung wiederherstellen Sie mit:

- Azure-Portal finden Sie unter [Wiederherstellen einer Datawarehouse mithilfe des Azure-Portals](sql-data-warehouse-restore-database-portal.md)
- PowerShell-Cmdlets finden Sie unter [Wiederherstellung ein Datawarehouse mit PowerShell-cmdlets](sql-data-warehouse-restore-database-powershell.md)
- Halten Sie APIs, finden Sie unter [Wiederherstellung ein Datawarehouse mithilfe der REST-APIs](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
