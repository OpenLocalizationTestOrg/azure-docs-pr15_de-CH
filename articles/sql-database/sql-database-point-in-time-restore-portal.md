<properties
    pageTitle="Wiederherstellen eine SQL Azure-Datenbank zu einem früheren Zeitpunkt in Zeit (Azure Portal) | Microsoft Azure"
    description="Wiederherstellen einer SQL Azure-Datenbank zu einem früheren Zeitpunkt Zeit."
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


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Wiederherstellen einer SQL Azure-Datenbank zu einem früheren Zeitpunkt mit der Azure-portal


> [AZURE.SELECTOR]
- [Übersicht](sql-database-recovery-using-backups.md)
- [Point-In-Time-Wiederherstellung: PowerShell](sql-database-point-in-time-restore-powershell.md)

Dieser Artikel beschreibt, wie die Datenbank zu einem früheren Zeitpunkt aus [SQL-Datenbank automatisierte Backups](sql-database-automated-backups.md) Wiederherstellen mit Azure-Portal.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>Wiederherstellen einer SQL-Datenbank zu einem früheren Zeitpunkt in Zeit

Wählen Sie eine Datenbank im Azure-Portal wiederherstellen:

1.  [Azure-Portal](https://portal.azure.com)zu öffnen.
2.  Wählen Sie auf der linken Seite des Bildschirms **Weitere Dienste** > **SQL-Datenbanken**.
3.  Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
4.  Wählen Sie am oberen Rand der Datenbank **Wiederherstellen**:

    ![Wiederherstellen einer SQL Azure-Datenbank](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  Wählen Sie auf der Seite **Wiederherstellen** Datum und Uhrzeit (in UTC-Zeit) die Datenbank wiederherstellen, und klicken Sie auf **OK**:

    ![Wiederherstellen einer SQL Azure-Datenbank](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>Die Wiederherstellung überwachen

1. Nach dem Klicken auf **OK** im vorherigen Schritt klicken Sie auf das Symbol rechts oben auf der Seite und klicken Sie auf die Benachrichtigung **Wiederherstellen SQL Datenbank** Details.

    ![Wiederherstellen einer SQL Azure-Datenbank](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. Das Wiederherstellen von SQL-Datenbank wird geöffnet mit Informationen zum Status der Wiederherstellung. Klicken Sie auf den Zeilenposten für weitere Details:

    ![Wiederherstellen einer SQL Azure-Datenbank](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte backups](sql-database-automated-backups.md)
- Automatisierte Backups Recovery mit finden Sie unter [Wiederherstellen einer Datenbank aus den Sicherungskopien Dienst initiiert](sql-database-recovery-using-backups.md)
- Schnellere Recovery-Optionen finden Sie unter [Aktive geografische Replikation](sql-database-geo-replication-overview.md)  
- Archivierung mit automatischen Sicherungen finden Sie unter [Datenbank kopieren](sql-database-copy.md)
