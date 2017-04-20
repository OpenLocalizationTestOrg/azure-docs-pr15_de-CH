<properties 
    pageTitle="Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit Transact-SQL | Microsoft Azure" 
    description="Starten einer geplanten oder ungeplanten Failover mit Transact-SQL Azure SQL-Datenbank" 
    services="sql-database" 
    documentationCenter="" 
    authors="CarlRabeler" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management"
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Starten einer geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit Transact-SQL


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


Dieser Artikel zeigt Ihnen wie Failover zu einer sekundären SQL-Datenbank mit Transact-SQL. Konfigurieren Sie Geo-Replikation finden Sie unter [Geo-Replikation für Azure SQL-Datenbank konfigurieren](sql-database-geo-replication-transact-sql.md).



Um Failover initiieren, benötigen Sie Folgendes:

- Ein Benutzername, der auf dem primären DBManager ist haben Db_ownership Geo-Replikation wird die lokale Datenbank und DBManager Partner Server, Sie Geo-Replikation konfigurieren.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Heraufstufen einer sekundären Datenbank zur neuen primären Geplantes Failover initiieren

**ALTER DATABASE** -Anweisung können Sie um eine sekundäre Datenbank der neuen primären Datenbank geplante Weise zu fördern bestehende primärer zu sekundärer herabstufen. Dies erfolgt in der Masterdatenbank auf dem logischen Server Azure SQL-Datenbank in der sekundäre Datenbank in geografisch repliziert, die heraufgestuft wird befindet. Diese Funktion dient für geplanten Failover wie während DR-Werkzeuge und erfordert, dass die primäre Datenbank verfügbar.

Der Befehl führt den folgenden Workflow:

1. Vorübergehend wechselt Replikation zu synchronen Modus verursacht aller ausstehende Transaktionen auf die sekundäre geleert und alle neue Transaktionen blockieren.

2. Wechselt die Rollen der beiden Datenbanken partnerschaftlich Geo-Replikation.  

Diese Reihenfolge gewährleistet, dass die beiden Datenbanken synchronisiert werden, bevor die Rollen wechseln und daher keine Daten verloren. Gibt eine kurze während der beiden Datenbanken (Reihenfolge 0, 25 Sekunden) fehlen während Rollen gewechselt werden. Hat die primäre Datenbank mehrere sekundäre Datenbanken, wird der Befehl automatisch sekundäre Verbindung mit neuer primärer konfiguriert.  Der gesamte Vorgang dauert weniger als eine Minute unter normalen Umständen. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Dienstebenen](sql-database-service-tiers.md).


Gehen Sie folgendermaßen vor, um ein geplantes Failover initiieren.

1. In Management Studio eine Verbindung mit den logischen Azure SQL-Datenbank-Server in dem geografisch repliziert sekundäre Datenbank befindet.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **master**und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie **ALTER DATABASE** -Anweisung, die sekundäre Datenbank die primäre Rolle wechseln.

        ALTER DATABASE <MyDB> FAILOVER;

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

>[AZURE.NOTE] In seltenen Fällen ist es möglich, dass der Vorgang nicht abgeschlossen und fest scheinen. In diesem Fall kann der Benutzer Befehl zum Erzwingen von Failover und Datenverlust akzeptieren.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Initiieren eines ungeplanten Failovers aus der primären Datenbank in der sekundären Datenbank

Die Anweisung **ALTER DATABASE** können Sie heraufstufen eine sekundäre Datenbank zu der neuen primären Datenbank auf ungeplante Weise erzwingen die Herabstufung eines vorhandenen primärer zu sekundärer gleichzeitig als primäre Datenbank nicht verfügbar ist. Dies erfolgt in der Masterdatenbank auf dem logischen Server Azure SQL-Datenbank in der sekundäre Datenbank in geografisch repliziert, die heraufgestuft wird befindet.

Diese Funktion dient für Disaster Recovery bei Verfügbarkeit der Datenbank wiederherstellen, wichtig ist und Datenverlust akzeptabel ist. Beim erzwungenen Failover aufgerufen wird, wird die primäre Datenbank angegebene sekundäre Datenbank sofort und akzeptieren Schreibtransaktionen beginnt. Als die ursprüngliche primäre Datenbank mit neuen primären Datenbank wiederherstellen kann, wird eine inkrementelle Sicherung auf das ursprüngliche primäre Datenbank und alte primäre Datenbank eine sekundäre Datenbank für die neue primäre Datenbank vorgenommen wird. Daher ist es nur eine Synchronisierung Replikat der neuen primären.

Da In Zeit Wiederherstellungspunkt nicht auf sekundären Datenbanken unterstützt wird, will der Benutzer Daten der alten primären Datenbank, die nicht in der neuen primären Datenbank repliziert wurden hatte vor dem erzwungenen Failover verpflichtet, muss der Benutzer zu unterstützen diese verlorene Daten wiederherstellen.

Hat die primäre Datenbank mehrere sekundäre Datenbanken, wird der Befehl automatisch sekundäre Verbindung mit neuer primärer konfiguriert.

Gehen Sie folgendermaßen vor ungeplanten Failover initiiert.

1. In Management Studio eine Verbindung mit den logischen Azure SQL-Datenbank-Server in dem geografisch repliziert sekundäre Datenbank befindet.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **Datenbanken** mit der rechten Maustaste auf **master**und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie **ALTER DATABASE** -Anweisung, die sekundäre Datenbank die primäre Rolle wechseln.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

>[AZURE.NOTE] Wenn der Befehl ausgeführt wird, wenn die primären und sekundären sind online alten primären werden neue sekundäre ohne Datensynchronisation. Ist die primäre auftreten verpflichtet Transaktionen bei der Befehl Daten verloren.



## <a name="next-steps"></a>Nächste Schritte   

- Nach einem Failover sicher, dass die Authentifizierung an Ihre Server und die Datenbank auf dem neuen Primärserver konfiguriert sind. Details finden Sie im [Security SQL Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md).
- Zu Wiederherstellung nach einem Notfall über aktive Geo-Replikation vor einschließlich Informationen und Schritte nach der Wiederherstellung durchführen einer Disaster Recovery Bohrung [Disaster Recovery](sql-database-disaster-recovery.md)
- Sasha Nosov Blogbeiträgen über aktive Geo-Replikation finden Sie in [Spotlight auf neue Geo - Replikationsfunktionen](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informationen zum Entwickeln von Cloudanwendungen aktive Geo-Replikation verwenden finden Sie unter [Entwerfen von Cloudanwendungen für Business Continuity Geo - Replikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informationen über aktive Geo-Replikation mit elastische Datenbank finden Sie unter [elastischen Pool Disaster Recovery-Strategien](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Eine Übersicht über Business Continurity finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
