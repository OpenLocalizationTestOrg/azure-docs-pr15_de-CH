<properties 
    pageTitle="Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit PowerShell | Microsoft Azure" 
    description="Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit PowerShell



> [AZURE.SELECTOR]
- [Azure-portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


Dieser Artikel veranschaulicht die SQL-Datenbank mit PowerShell geplanten oder ungeplanten Failover initiieren. Konfigurieren Sie Geo-Replikation finden Sie unter [Geo-Replikation für Azure SQL-Datenbank konfigurieren](sql-database-geo-replication-powershell.md).



## <a name="initiate-a-planned-failover"></a>Geplantes Failover initiieren

Verwenden Sie das Cmdlet " **Set-AzureRmSqlDatabaseSecondary** " mit dem Parameter **- Failover** zu einer sekundären Datenbank zu neuen primären Datenbank vorhandenen primärer zu sekundärer herabstufen. Diese Funktionen wie bei einem geplanten Failover bei Disaster Recovery Werkzeuge entwickelt und erfordert, dass die primäre Datenbank verfügbar.

Der Befehl führt den folgenden Workflow:

1. Vorübergehend wechseln Sie Replikation zu synchronen Modus Dadurch werden alle ausstehende Transaktionen auf die sekundäre geleert.

2. Wechseln der Rollen der beiden Datenbanken partnerschaftlich Geo-Replikation.  

Diese Reihenfolge gewährleistet, dass die beiden Datenbanken synchronisiert werden, bevor die Rollen wechseln und daher keine Daten verloren. Gibt eine kurze während der beiden Datenbanken (Reihenfolge 0, 25 Sekunden) fehlen während Rollen gewechselt werden. Der gesamte Vorgang dauert weniger als eine Minute unter normalen Umständen. Weitere Informationen finden Sie unter [Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx).




Dieses Cmdlet wird zurückgegeben, wenn die sekundäre Datenbank zur primären wechseln abgeschlossen ist.

Der folgende Befehl wechselt die Rollen der Datenbank mit dem Namen "Mydb" auf dem Server "srv2" unter der Ressource Gruppe "rg2" primäre. Die ursprüngliche primäre, "db2" mit, wechselt zu sekundären nach zwei Datenbanken vollständig synchronisiert sind.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] In seltenen Fällen ist es möglich, dass der Vorgang nicht abgeschlossen und nicht mehr reagiert scheint. In diesem Fall kann Benutzer Kraft Failover-Befehl (ungeplanten Failover) aufgerufen und Datenverlust akzeptieren.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Initiieren eines ungeplanten Failovers aus der primären Datenbank in der sekundären Datenbank


Sie können **Set-AzureRmSqlDatabaseSecondary** -Cmdlet **-Failover** und **AllowDataLoss -** Parameter heraufstufen eine sekundäre Datenbank zu der neuen primären Datenbank auf ungeplante Weise erzwingen die Herabstufung eines vorhandenen primärer zu sekundärer gleichzeitig als primäre Datenbank nicht verfügbar ist.

Diese Funktion dient für Disaster Recovery bei Verfügbarkeit der Datenbank wiederherstellen, wichtig ist und Datenverlust akzeptabel ist. Beim erzwungenen Failover aufgerufen wird, wird die primäre Datenbank angegebene sekundäre Datenbank sofort und akzeptieren Schreibtransaktionen beginnt. Als die ursprüngliche primäre Datenbank nach der erzwungenen Failover-Vorgang mit dieser neuen primären Datenbank verbinden kann, wird eine inkrementelle Sicherung auf das ursprüngliche primäre Datenbank und alte primäre Datenbank eine sekundäre Datenbank für die neue primäre Datenbank vorgenommen wird. Daher ist es nur ein Replikat der neuen primären.

Aber da In Zeit Wiederherstellungspunkt nicht auf sekundären Datenbanken unterstützt wird, möchten Sie Daten verpflichtet die alte primäre Datenbank nicht mit der neuen primären Datenbank repliziert wurde, Sie sollten CSS eine Datenbank der bekannten Sicherung wiederherzustellen.

> [AZURE.NOTE] Wenn der Befehl ausgeführt wird, wenn die primären und sekundären sind online alten primären werden neue sekundäre ohne Datensynchronisation. Ist die primäre auftreten verpflichtet Transaktionen bei der Befehl Daten verloren.


Wenn die primäre Datenbank mehrere sekundäre ist der Befehl teilweise erfolgreich. Werden primäre des sekundären, auf dem der Befehl ausgeführt wurde. Alte primäre bleiben jedoch primäre, also zwei Primärfarben werden in inkonsistent und unterbrochene Replikation Link verbunden. Der Benutzer muss diese Konfiguration mithilfe einer API "sekundäre entfernen" auf beiden primären Datenbanken manuell zu reparieren.


Der folgende Befehl wechselt die Rollen der Datenbank mit dem Namen "Mydb" primäre beim primären nicht verfügbar ist. Die ursprüngliche primäre, "Mydb" mit, wechselt zu sekundären wieder online ist. An diesem Punkt kann die Synchronisierung Datenverlust führen.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Nächste Schritte   

- Nach einem Failover sicher, dass die Authentifizierung an Ihre Server und die Datenbank auf dem neuen Primärserver konfiguriert sind. Details finden Sie im [Security SQL Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md).
- Zu Wiederherstellung nach einem Notfall aktive Geo-Replikation, einschließlich vor und Schritte nach der Wiederherstellung eine Disaster Recovery-Drilldown ausführen [Disaster Recovery Bohrer](sql-database-disaster-recovery.md) angezeigt
- Sasha Nosov Blogbeiträgen über aktive Geo-Replikation finden Sie in [Spotlight auf neue Geo - Replikationsfunktionen](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informationen zum Entwickeln von Cloudanwendungen aktive Geo-Replikation verwenden finden Sie unter [Entwerfen von Cloudanwendungen für Business Continuity Geo - Replikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informationen über aktive Geo-Replikation mit elastische Datenbank finden Sie unter [elastischen Pool Disaster Recovery-Strategien](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Eine Übersicht über Business Continurity finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
