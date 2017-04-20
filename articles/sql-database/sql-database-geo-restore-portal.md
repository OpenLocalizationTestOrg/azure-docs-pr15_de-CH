<properties
    pageTitle="Wiederherstellen eine SQL Azure-Datenbank von einer automatischen Sicherung (Azure Portal) | Microsoft Azure"
    description="Wiederherstellen einer SQL Azure-Datenbank aus einer automatischen Sicherung (Azure-Portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Wiederherstellen einer SQL Azure-Datenbank eine automatische Wiederherstellung des Azure-Portals


> [AZURE.SELECTOR]
- [Übersicht](sql-database-recovery-using-backups.md#geo-restore)
- [Geo-Wiederherstellung: PowerShell](sql-database-geo-restore-powershell.md)

Dieser Artikel beschreibt, wie die Datenbank eine [Automatische Sicherung](sql-database-automated-backups.md) auf einen neuen Server mit [Geo-Wiederherstellung](sql-database-recovery-using-backups/.md#geo-restore) Wiederherstellen mit Azure-Portal.

## <a name="select-a-database-to-restore"></a>Wählen Sie eine Datenbank wiederherstellen

Zum Wiederherstellen einer Datenbank im Azure-Portal gehen Sie folgendermaßen vor:

1.  Zum [Azure-Portal](https://portal.azure.com).
2.  Wählen Sie auf der linken Seite des Bildschirms **und neue** > **Datenbanken** > **SQL-Datenbank**:

    ![Wiederherstellen einer SQL Azure-Datenbank](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Wählen Sie **Sicherung** als Quelle, und wählen Sie die Sicherungskopie wiederherstellen möchten. Datenbanknamen Sie einen, einem Server in die Datenbank wiederherstellen möchten, und klicken Sie auf **Erstellen**:
  
    ![Wiederherstellen einer SQL Azure-Datenbank](./media/sql-database-geo-restore-portal/geo-restore.png)

Überwachen Sie den Status des Wiederherstellungsvorgangs durch Klicken auf das Symbol oben rechts auf der Seite. 


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte backups](sql-database-automated-backups.md)
- Automatisierte Backups Recovery mit finden Sie unter [Wiederherstellen einer Datenbank aus den Sicherungskopien Dienst initiiert](sql-database-recovery-using-backups.md)
- Schnellere Recovery-Optionen finden Sie unter [Aktive geografische Replikation](sql-database-geo-replication-overview.md)  
- Archivierung mit automatischen Sicherungen finden Sie unter [Datenbank kopieren](sql-database-copy.md)
