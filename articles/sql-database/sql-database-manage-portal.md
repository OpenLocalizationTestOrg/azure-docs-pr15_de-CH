<properties
    pageTitle="Azure SQL-Datenbank mithilfe der Azure-Portal verwalten | Microsoft Azure"
    description="Informationen Sie zum Azure-Portal verwenden, um eine relationale Datenbank in der Cloud Azure-Portal verwalten."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Verwalten von Azure SQL-Datenbanken mithilfe von Azure-portal


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

[Azure-Portal](https://portal.azure.com/) können Sie erstellen, überwachen und Verwalten von Azure SQL-Datenbanken und Server. Dieser Artikel enthält eine kurze Beschreibung und Links zu den Details der häufigeren Aufgaben.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Anzeigen der SQL Azure-Datenbanken, Servern und pools

Um die verfügbaren SQL Datenbank Dienste anzuzeigen, klicken Sie auf **Weitere Dienste**und geben Sie **SQL** in das Suchfeld ein:

![SQL-Datenbank](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Wie erstellen oder Anzeigen von SQL Azure-Datenbanken?

Öffnen Sie das Blade **SQL-Datenbanken** klicken Sie auf **SQL-Datenbanken**und dann die Datenbank arbeiten möchten, auf oder **+ Hinzufügen** zum Erstellen einer SQL-Datenbank. Details finden Sie in der [SQL-Datenbank in Minuten mithilfe des Azure-Portals erstellen](sql-database-get-started.md).


![SQL-Datenbanken](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Wie erstellen oder anzeigen SQL Azure-Server?

Öffnen Sie das **SQL Server** -Blade klicken Sie auf **SQL Server**, dann klicken Sie auf dem Server arbeiten möchten oder klicken Sie auf **+ Hinzufügen** zum Erstellen eines SQL-Servers. Details finden Sie in der [SQL-Datenbank in Minuten mithilfe des Azure-Portals erstellen](sql-database-get-started.md).

![SQL Server](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Wie erstellen oder anzeigen SQL elastische Pools?

Öffnen Sie **SQL elastische Pools** Blade **SQL elastische Pools**klicken Sie und dann Pool arbeiten möchten, auf oder **+ Hinzufügen** zum Erstellen eines Pools. Details finden Sie unter [Erstellen einer elastischen Datenbankpool mit Azure-Portal](sql-database-elastic-pool-create-portal.md).

![Elastische SQL-pools](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Wie aktualisieren oder Einstellungen SQL-Datenbank?

Anzeigen oder aktualisieren Sie Ihre datenbankeinstellungen, klicken Sie auf die gewünschte Einstellung auf die SQL-Datenbank:


![SQL Datenbank settings](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Finden SQL Datenbanken vollqualifizierte Servernamen

Zum Anzeigen der Servername Datenbanken auf **Übersicht** auf die **SQL-Datenbank** und notieren Sie den Server:


![SQL Datenbank settings](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Verwaltung Firewall-Regeln zur Steuerung des Zugriffs auf meine SQL Server und Datenbank

Zum Anzeigen, erstellen oder Aktualisieren von Firewall-Regeln, klicken Sie auf **Server-Firewall** auf die **SQL-Datenbank** . Weitere Informationen finden Sie unter [Konfigurieren einer Azure SQL-Datenbank auf Serverebene mithilfe Firewallregel Azure-Portal](sql-database-configure-firewall-settings.md).


![Firewall-Regeln](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Wie ändere ich meine SQL-Datenbank Tier oder Leistung Servicelevel?


Aktualisieren Sie die Dienstebene Tier oder Leistung einer SQL-Datenbank klicken Sie auf **Preisstufe (Skalieren DTUs)** auf die **SQL-Datenbank** . Details finden Sie in der [Ebene und Leistung Dienstebene (Tarif) einer SQL-Datenbank ändern](sql-database-scale-up.md).


![Preise von Ebenen](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Wie konfigurieren Sie Überwachung und Bedrohung Erkennung für eine SQL-Datenbank

Konfigurieren Sie Überwachung und Bedrohung Erkennung für eine SQL-Datenbank klicken Sie auf die **SQL-Datenbank** auf **Überwachung und Bedrohung erkennen** . Details finden Sie unter [Erste Schritte mit SQL Datenbank-auditing](sql-database-auditing-get-started.md)und [Erste Schritte mit SQL Datenbank Erkennung](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Wie konfigurieren Sie dynamische Daten masking für eine SQL-Datenbank

Konfigurieren Sie dynamische Daten masking für eine SQL-Datenbank klicken Sie auf **dynamische Daten masking** auf die **SQL-Datenbank** . Details finden Sie unter [Erste Schritte mit SQL Datenbank dynamische Daten maskieren](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Konfiguration transparente Verschlüsselung (DSA) für eine SQL-Datenbank

Um transparente Verschlüsselung für eine SQL-Datenbank zu konfigurieren, klicken Sie auf **transparente Verschlüsselung** auf dem **SQL-Datenbank** . Näheres [TDE auf eine Datenbank über das Portal aktivieren](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Wie anzeigen oder ändern Sie die maximale Größe einer SQL-Datenbank?

Anzeigen oder Ändern der Größe einer SQL-Datenbank, klicken Sie auf **Datenbank** auf dem **SQL-Datenbank** . Aktualisieren Sie die maximale Größe einer Datenbank durch die Service-Tier oder Leistung ändern. Details finden Sie in der [Ebene und Leistung Dienstebene (Tarif) einer SQL-Datenbank ändern](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Wie überwachen und Verbessern der Leistung einer SQL-Datenbank?

Zum Überwachen und Verbessern der Performance-Merkmale einer SQL-Datenbank, klicken Sie auf die **SQL-Datenbank** auf **Performance Overview** . Details finden Sie in der [SQL-Datenbank-Performance-Sicherung](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Konfiguration Geo-Replikation

Geo-Replikation für eine SQL-Datenbank klicken Sie **Geo-Replikation** auf der **SQL-Datenbank** . Details finden Sie unter [Geo-Replikation für SQL Azure-Datenbank mit der Azure-Portal konfigurieren](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Vorgehensweise beim Failover zu einer SQL-Datenbank Geo repliziert?

Failover auf sekundäre Geo repliziert **Geo-Replikation** auf der **SQL-Datenbank** klicken **Failover**. Details finden Sie unter [Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit der Azure-Portal](sql-database-geo-replication-failover-portal.md).


## <a name="how-do-i-copy-a-sql-database"></a>Wie wird kopieren eine SQL-Datenbank?

Klicken Sie zum Kopieren einer SQL-Datenbank auf dem **SQL-Datenbank** **Kopieren** . Details finden Sie unter [kopieren eine SQL Azure-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md).


![SQL Datenbank settings](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Wie archivieren Sie eine SQL Azure-Datenbank in eine Datei BACPAC

Um eine BACPAC einer SQL-Datenbank zu erstellen, klicken Sie auf die **SQL-Datenbank** **Exportieren** . Details finden Sie im [Archiv eine SQL Azure-Datenbank eine BACPAC Datei Azure-Portal](sql-database-export.md).


![SQL-Datenbank exportieren](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Wie kann ich eine SQL-Datenbank zu einem früheren Zeitpunkt Zeitpunkt wiederherstellen?

Klicken Sie zum Wiederherstellen einer SQL-Datenbank auf die **SQL-Datenbank** **Wiederherstellen** . Details finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit Azure-Portal](sql-database-point-in-time-restore-portal.md).


![SQL Datenbank settings](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Wie erstelle ich eine SQL Azure-Datenbank aus einer Datei BACPAC?

Zum Erstellen einer SQL-Datenbank aus einer BACPAC-Datei klicken Sie auf das **SQL Server** -Blade **-Datenbank importieren** . Details finden Sie in der [Datei BACPAC zum Erstellen einer SQL Azure-Datenbank importieren](sql-database-import.md).


![SQL server](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Wiederherstellen eine gelöschte SQL-Datenbank

Klicken Sie zum Wiederherstellen einer gelöschten SQL-Datenbank auf **SQL Server** -Blade (SQL Server, die die Datenbank enthalten, die gelöscht wurde) **Datenbanken gelöscht** . Weitere Informationen finden Sie unter [Wiederherstellen einer gelöschte Azure SQL-Datenbank mit der Azure-Portal](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Wie wird löschen eine SQL-Datenbank?

Klicken Sie zum Löschen einer SQL-Datenbank auf dem **SQL-Datenbank** **Löschen** . 

![SQL Datenbank settings](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Datenbank](sql-database-technical-overview.md)
- [Überwachen Sie und verwalten Sie einer elastischen Datenbank mit Azure-portal](sql-database-elastic-pool-manage-portal.md)
