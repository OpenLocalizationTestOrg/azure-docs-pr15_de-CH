<properties 
    pageTitle="Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit Azure-Portal | Microsoft Azure" 
    description="Starten einer geplanten oder ungeplanten Failover für Azure SQL Azure-portal" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit Azure-portal


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


Dieser Artikel veranschaulicht das Failover zu einer sekundären Datenbank SQL [Azure-Portal](http://portal.azure.com)zu initiieren. Konfigurieren Sie Geo-Replikation finden Sie unter [Geo-Replikation für Azure SQL-Datenbank konfigurieren](sql-database-geo-replication-portal.md).


## <a name="initiate-a-failover"></a>Ein Failover initiiert

Die sekundäre Datenbank kann zur primären gewechselt werden.  

1. [Azure-Portal](http://portal.azure.com) suchen mit der primären Datenbank partnerschaftlich Geo-Replikation.
2. Auf der SQL-Datenbank, wählen Sie **Alle** > **Geo-Replikation**.
3. Wählen Sie in der Liste **sekundäre** der Datenbank werden der neuen primären und **Failover**auf.

    ![Failover][2]

4. Klicken Sie auf **Ja,** um das Failover zu beginnen.

Der Befehl wechselt die sekundäre Datenbank sofort in die primäre Rolle. 

Gibt eine kurze während der beiden Datenbanken (Reihenfolge 0, 25 Sekunden) fehlen während Rollen gewechselt werden. Hat die primäre Datenbank mehrere sekundäre Datenbanken, wird der Befehl automatisch sekundäre Verbindung mit neuer primärer konfiguriert. Der gesamte Vorgang dauert weniger als eine Minute unter normalen Umständen. 

>[AZURE.NOTE] Wenn die primäre online und übergeben der Transaktionen bei der Befehl Datenverlust auftreten.


## <a name="next-steps"></a>Nächste Schritte   

- Nach einem Failover sicher, dass die Authentifizierung an Ihre Server und die Datenbank auf dem neuen Primärserver konfiguriert sind. Details finden Sie im [Security SQL Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md).
- Zu Wiederherstellung nach einem Notfall aktive Geo-Replikation, einschließlich vor und Schritte nach der Wiederherstellung eine Disaster Recovery-Drilldown ausführen [Disaster Recovery Bohrer](sql-database-disaster-recovery.md) angezeigt
- Sasha Nosov Blogbeiträgen über aktive Geo-Replikation finden Sie in [Spotlight auf neue Geo - Replikationsfunktionen](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informationen zum Entwickeln von Cloudanwendungen aktive Geo-Replikation verwenden finden Sie unter [Entwerfen von Cloudanwendungen für Business Continuity Geo - Replikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informationen über aktive Geo-Replikation mit elastische Datenbank finden Sie unter [elastischen Pool Disaster Recovery-Strategien](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Eine Übersicht über Business Continurity finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png
