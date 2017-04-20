<properties
   pageTitle="Wiederherstellen einer Azure SQL Datawarehouse (Portal) | Microsoft Azure"
   description="Azure ausgeführte Aufgaben für die Wiederherstellung einer Azure SQL Data Warehouse."
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

# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Wiederherstellen einer Azure SQL Datawarehouse (Portal)

> [AZURE.SELECTOR]
- [Übersicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In diesem Artikel erfahren Sie, wie einer Azure SQL Data Warehouse mithilfe der Azure-Portal wiederherstellen.

## <a name="before-you-begin"></a>Bevor Sie beginnen

**Überprüfen Sie die Kapazität Ihres DTU.** Jede SQL Data Warehouse wird von SQL Server (z.B. myserver.database.windows.net) gehostet hat ein Kontingent DTU.  Überprüfen Sie vor dem Wiederherstellen einer SQL Data Warehouse, die Ihren SQL Server hat genügend verbleibenden DTU Kontingent für die Datenbank wiederhergestellt wird. DTU Bedarf berechnen und weitere DTU Informationen finden Sie unter [DTU Kontingent Änderung anfordern][].


## <a name="restore-an-active-or-paused-database"></a>Wiederherstellen einer Datenbank aktiven oder angehalten

Zum Wiederherstellen einer Datenbank:

1. [Azure-Portal][] anmelden
2. Wählen Sie auf der linken Seite des Bildschirms **Durchsuchen** , und wählen Sie dann **SQL Server**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Navigieren Sie zu dem Server und wählen Sie
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Suchen Sie aus und wählen sie den gewünschten SQL Data Warehouse
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Klicken Sie am oberen Rand der Data Warehouse-Blade auf **Wiederherstellen**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Sie **Name der Datenbank**
7. Wählen Sie den aktuellsten **Wiederherstellungspunkt**
    1. Stellen Sie sicher, dass den aktuellsten Wiederherstellungspunkt auswählen.  Da Wiederherstellungspunkte in UTC angezeigt werden, ist manchmal die Standardoption angezeigt nicht den aktuellsten Wiederherstellungspunkt an.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Klicken Sie auf **OK**
9. Datenbank-Wiederherstellungsvorgang beginnt und mithilfe von **BENACHRICHTIGUNGEN** überwacht werden

>[AZURE.NOTE] Nach Abschluss die Wiederherstellung können Sie die wiederhergestellte Datenbank folgende [Datenbank nach Wiederherstellung konfigurieren][].


## <a name="restore-a-deleted-database"></a>Eine gelöschte Datenbank wiederherstellen

Eine gelöschte Datenbank wiederherstellen:

1. [Azure-Portal][] anmelden
2. Wählen Sie auf der linken Seite des Bildschirms **Durchsuchen** , und wählen Sie dann **SQL Server**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Navigieren Sie zu dem Server und wählen Sie
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Abschnitt mit den Vorgängen auf dem Server Scrollen
5. Klicken Sie auf die Kachel **Datenbanken gelöscht**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Wählen Sie die gelöschte Datenbank wiederherstellen
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Sie **Name der Datenbank**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Klicken Sie auf **OK**
9. Datenbank-Wiederherstellungsvorgang beginnt und mithilfe von **BENACHRICHTIGUNGEN** überwacht werden

>[AZURE.NOTE] Konfigurieren die Datenbank nach der Wiederherstellung finden Sie in der [Datenbank nach Wiederherstellung konfigurieren][]. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Business Continuity-Funktionen von Azure SQL Database Edition lesen Sie [Azure SQL-Datenbank Business Continuity – Überblick][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank Business Continuity – Überblick]: ./sql-database-business-continuity.md
[Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Datenbank nach Wiederherstellung konfigurieren]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[DTU Kontingent Änderung anfordern]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure-portal]: https://portal.azure.com/
