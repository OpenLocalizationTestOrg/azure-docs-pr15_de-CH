<properties
    pageTitle="Wiederherstellen einer gelöschte Azure SQL-Datenbank (Azure Portal) | Microsoft Azure"
    description="Wiederherstellen einer gelöschten Azure SQL-Datenbank (Azure-Portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Wiederherstellen einer gelöschten Azure SQL-Datenbank mit der Azure-Portal

> [AZURE.SELECTOR]
- [Übersicht](sql-database-recovery-using-backups.md)
- [**Wiederherstellen gelöschter DB: Portal**](sql-database-restore-deleted-database-portal.md)
- [Wiederherstellen gelöschter DB: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Wählen Sie die Datenbank wiederherstellen 

Eine gelöschte Datenbank in Azure-Portal wiederherstellen:

1.  Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **Weitere Dienste** > **SQL Server**.
3.  Wählen Sie den Server, der die Datenbank enthalten, die Sie wiederherstellen möchten.
4.  Abschnitt **Operationen** der Blade-Server Blättern und wählen Sie **Gelöschte Datenbanken**: ![eine Azure SQL-Datenbank wiederherstellen](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Wählen Sie die Datenbank wiederherstellen möchten.
6.  Datenbanknamen Sie einen, und klicken Sie auf **OK**:

    ![Wiederherstellen einer SQL Azure-Datenbank](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte backups](sql-database-automated-backups.md)
- Automatisierte Backups Recovery mit finden Sie unter [Wiederherstellen einer Datenbank aus den Sicherungskopien Dienst initiiert](sql-database-recovery-using-backups.md)
- Schnellere Recovery-Optionen finden Sie unter [Aktive geografische Replikation](sql-database-geo-replication-overview.md)  
- Archivierung mit automatischen Sicherungen finden Sie unter [Datenbank kopieren](sql-database-copy.md)
