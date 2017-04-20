<properties
   pageTitle="Wiederherstellen einer Azure SQL Datawarehouse (REST-API) | Microsoft Azure"
   description="REST-API Aufgaben für die Wiederherstellung einer Azure SQL Data Warehouse."
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
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Wiederherstellen einer Azure SQL Datawarehouse (REST-API)

> [AZURE.SELECTOR]
- [Übersicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In diesem Artikel lernen Sie das Wiederherstellen einer Azure SQL Data Warehouse mithilfe der REST-API.

## <a name="before-you-begin"></a>Bevor Sie beginnen

**Überprüfen Sie die Kapazität Ihres DTU.** Jede SQL Data Warehouse wird von SQL Server (z.B. myserver.database.windows.net) gehostet hat ein Kontingent DTU.  Überprüfen Sie vor dem Wiederherstellen einer SQL Data Warehouse, die Ihren SQL Server hat genügend verbleibenden DTU Kontingent für die Datenbank wiederhergestellt wird. DTU Bedarf berechnen und weitere DTU Informationen finden Sie unter [DTU Kontingent Änderung anfordern][].

## <a name="restore-an-active-or-paused-database"></a>Wiederherstellen einer Datenbank aktiven oder angehalten

Zum Wiederherstellen einer Datenbank:

1. Ruft die Liste der Wiederherstellungspunkte Datenbank mithilfe des Datenbank-Wiederherstellungspunkte Get.
2. Beginnen Sie die Wiederherstellung mit der Operation [erstellen Datenbank wiederherstellen Anforderung][] .
3. Den Status der Wiederherstellung mit der [Status des Datenbank][] -Operation.

>[AZURE.NOTE] Nach Abschluss die Wiederherstellung können Sie die wiederhergestellte Datenbank folgende [Datenbank nach Wiederherstellung konfigurieren][].

## <a name="restore-a-deleted-database"></a>Eine gelöschte Datenbank wiederherstellen

Eine gelöschte Datenbank wiederherstellen:

1.  Listen Sie aller gelöschten wiederherstellbaren Datenbanken mit der Operation [wiederherstellbare gelöschte Datenbanken auflisten auf][]
2.  Informieren Sie die gelöschte Datenbank mit der Operation [wiederherstellbare gelöschte Datenbank][] wiederherstellen möchten.
3.  Beginnen Sie die Wiederherstellung mit der Operation [erstellen Datenbank wiederherstellen Anforderung][] .
4.  Den Status der Wiederherstellung mit der [Status des Datenbank][] -Operation.

>[AZURE.NOTE] Konfigurieren die Datenbank nach der Wiederherstellung finden Sie in der [Datenbank nach Wiederherstellung konfigurieren][]. 


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Business Continuity-Funktionen von Azure SQL Database Edition lesen Sie [Azure SQL-Datenbank Business Continuity – Überblick][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank Business Continuity – Überblick]: ./sql-database-business-continuity.md
[DTU Kontingent Änderung anfordern]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Datenbank nach Wiederherstellung konfigurieren]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Datenbank wiederherstellen-Anforderung erstellen]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Datenbank Vorgangsstatus]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Wiederherstellbare gelöschte Datenbank]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Liste der wiederherstellbaren Datenbanken gelöscht]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
