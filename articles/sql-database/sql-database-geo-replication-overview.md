<properties
    pageTitle="Aktive Geo-Replikation für SQL Azure-Datenbank"
    description="Aktive Geo-Replikation können Sie 4 Replikaten der Datenbank eines Azure Rechenzentren einrichten."
    services="sql-database"
    documentationCenter="na"
    authors="stevestein"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/26/2016"
    ms.author="sstein" />

# <a name="overview-sql-database-active-geo-replication"></a>Übersicht: SQL Datenbank aktive Geo-Replikation

Aktive Geo-Replikation können Sie bis zu vier lesbare sekundäre Datenbanken in der gleichen oder in verschiedenen Datenzentren (Bereiche) konfigurieren. Sekundäre Datenbanken stehen für Abfragen und Failover eines Rechenzentrums hat oder nicht mit der primären Datenbank verbinden.

>[AZURE.NOTE] Aktive Geo-Replikation (lesbare sekundäre) steht jetzt für alle Datenbanken in alle Dienstebenen. Im April 2017 nicht lesbaren sekundären Typs zurückgezogen und Datenbanken nicht lesbar, lesbare sekundäre werden automatisch aktualisiert.

 Sie können aktive Geo-Replikation mit [Azure-Portal](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md)oder [REST API - erstellen oder Update](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Konfigurieren: Azure-portal](sql-database-geo-replication-portal.md)
- [Konfigurieren: PowerShell](sql-database-geo-replication-powershell.md)
- [Konfigurieren: T-SQL](sql-database-geo-replication-transact-sql.md)

Bei Grund die primäre Datenbank fehlschlägt oder einfach offline geschaltet werden muss, können Sie *Failover* auf sekundäre Datenbanken. Beim Failover zu einem sekundären Datenbanken aktiviert ist, werden andere sekundäre automatisch mit dem neuen primären verknüpft.

Sie können Failover auf einen sekundären [Azure-Portal](sql-database-geo-replication-failover-portal.md), [PowerShell](sql-database-geo-replication-failover-powershell.md), [Transact-SQL](sql-database-geo-replication-failover-transact-sql.md), die [REST API - Failover geplant](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)oder in [REST API - ungeplanten Failover](https://msdn.microsoft.com/library/azure/mt582027.aspx).


> [AZURE.SELECTOR]
- [Failover: Azure-portal](sql-database-geo-replication-failover-portal.md)
- [Failover: PowerShell](sql-database-geo-replication-failover-powershell.md)
- [Failover: T-SQL](sql-database-geo-replication-failover-transact-sql.md)

Nach einem Failover sicher, dass die Authentifizierung an Ihre Server und die Datenbank auf dem neuen Primärserver konfiguriert sind. Details finden Sie im [Security SQL Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md).


Die aktive Geo-Replikationsfunktion implementiert einen Mechanismus zur Datenbank Redundanz innerhalb derselben Microsoft Azure-Region oder in verschiedenen Regionen (Geo-Redundanz). Aktive Geo-Replikation repliziert asynchron Transaktionen aus einer Datenbank bis zu vier Kopien der Datenbank auf verschiedenen Servern mit Commit Snapshotisolation (RCSI) für die Isolation lesen. Wenn aktive Geo-Replikation konfiguriert ist, wird eine sekundäre Datenbank auf dem angegebenen Server erstellt. Die Originaldatenbank wird der primären Datenbank. Die Primärdatenbank repliziert asynchron Transaktionen auf sekundäre Datenbanken. Nur vollständige Transaktionen werden repliziert. Zu jedem Zeitpunkt die sekundäre Datenbank möglicherweise etwas hinter die primäre Datenbank, die sekundären Daten garantiert nie Teiltransaktionen. Die RPO Daten finden unter [Übersicht über Business Continuity](sql-database-business-continuity.md).

Einer der Hauptvorteile der aktive Geo-Replikation werden niedriger Wiederherstellungszeit eine Datenbankebene Disaster Recovery-Lösung bietet. Beim Platzieren der sekundären Datenbank auf einem Server in einer anderen Region hinzufügen Sie maximale Stabilität Ihrer Anwendung. Cross-Region Redundanz ermöglicht Applikationen wieder aus einem dauerhaften Verlust der gesamten Rechenzentrum oder Teile eines Datencenters durch Naturkatastrophen, schweren menschlichem Versagen oder böswillige Aktionen verursacht. Die folgende Abbildung zeigt ein Beispiel der aktive Geo-Replikation mit einem in der nördlichen zentralen USA und sekundären in der südlichen zentralen USA konfiguriert.

![Geo-Replikations-Beziehung](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Ein weiterer wichtiger Vorteil ist, dass sekundären Datenbanken lesbar und auslagern schreibgeschützt Arbeitslasten wie Aufträge verwendet werden. Wenn Sie die sekundäre Datenbank für den Lastenausgleich verwenden möchten, können Sie es im Bereich der primären erstellen. Erstellen sekundärer in derselben Region erhöht nicht die Anwendung gegenüber katastrophalen Ausfällen.  

Andere Szenarien aktive Geo-Replikation Verwendungsnachweis gehören:

- **Datenbankmigration**: können aktive Geo-Replikation zum Migrieren einer Datenbank von einem Server auf einen anderen online bei minimalen Ausfallzeiten.
- **Anwendungsupgrades**: können zusätzliche sekundäre Kopie Fehler zurück während des Upgrades der Anwendung.

Um echte Business Continuity zu erreichen, ist die Datenbank Redundanz zwischen Rechenzentren nur einen Teil der Lösung. Wiederherstellen einer Anwendung (Service) End-to-End nach einem Systemausfall Recovery aller Komponenten, die den Dienst bilden und alle abhängigen Dienste erfordert. Diese Komponenten gehören Clientsoftware (z. B. einen Browser mit einer benutzerdefinierten JavaScript), Web-front-Ends, Speicher und DNS. Es ist wichtig, alle Komponenten sind auf den gleichen Fehler in Recovery Time Objective (RTO) der Anwendung verfügbar. Daher müssen Sie alle abhängigen Dienste identifizieren und die Garantien und Funktionsumfang. Dann müssen Sie Schritte ausreichend um sicherzustellen, dass Ihre Servicefunktionen während des Failovers Dienste von denen es abhängt. Weitere Informationen zum Entwerfen von Lösungen für die Wiederherstellung finden Sie unter [Entwerfen Cloudlösungen für Disaster Recovery verwenden aktive Geo-Replikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Aktive Geo-Replikationsfunktionen
Die aktive Geo-Replikation-Funktion bietet folgenden wichtigen Funktionen:

- **Automatische asynchrone Replikation**: Sie können nur eine sekundäre Datenbank durch Hinzufügen zu einer vorhandenen Datenbank. Die sekundäre kann nur in einem anderen Azure SQL-Datenbank erstellt werden. Nach dem Erstellen wird die sekundäre Datenbank mit den Daten aus der primären Datenbank kopiert aufgefüllt. Dieser Vorgang wird als seeding bezeichnet. Erstellt und ausgesät sekundären Datenbank werden die Updates mit der primären Datenbank asynchron automatisch auf die sekundäre Datenbank repliziert. Asynchroner Replikation bedeutet, dass Transaktionen in der primären Datenbank festgeschrieben wurden, bevor sie die sekundäre Datenbank repliziert werden. 

- **Sekundäre Datenbanken**: zwei oder mehr sekundäre Datenbanken Redundanz und Schutz für die primäre Datenbank und Anwendung erhöhen. Wenn mehrere sekundäre Datenbanken vorhanden sind, bleibt die Anwendung Ausfall eines sekundären Datenbanken geschützt. Nur eine sekundäre Datenbank vorhanden ist und es nicht ist die Anwendung höhere Risiko ausgesetzt, bis eine neue sekundäre Datenbank erstellt wird.

- **Lesbare sekundäre Datenbanken**: eine Anwendung kann eine sekundäre Datenbank für schreibgeschützte Operationen über die gleichen oder einem anderen Sicherheitsprinzipale verwendet für den Zugriff auf die primäre Datenbank zugreifen. Sekundären Datenbanken arbeiten im Isolationsmodus Snapshot, um sicherzustellen, dass nicht verzögert Replikation von Updates der primäre (Wiedergabe) von Abfragen auf dem sekundären Server ausgeführt.

>[AZURE.NOTE] Die Wiedergabe ist auf die sekundäre Datenbank verzögert Schema Updates, die vom primären erhält, die eine Schemasperre für die sekundäre Datenbank benötigen.

- **Aktive Geo-Replikation elastischen Pool Datenbanken**: aktive Geo-Replikation kann für jede Datenbank in jeder Pool elastische Datenbanken konfiguriert werden. Die sekundäre Datenbank kann in einem anderen Datenbankpool elastische. Für reguläre Datenbanken kann die sekundäre eine elastische Datenbank Pool und umgekehrt, wie die Dienstebenen identisch sind. 

- **Konfigurierbare Performance auf der sekundären Datenbank**: eine sekundäre Datenbank mit niedriger Leistung als primären erstellt werden. Primäre und sekundäre Datenbanken müssen die gleichen Service-Ebene haben. Diese Option für Applikationen mit hoher Datenbank schreiben abgeraten, da die erhöhte Replikation Verzögerung erheblichen Datenverlust nach einem Failover steigt. Darüber hinaus wird nach einem Failover Leistung der Anwendung beeinträchtigt bis neue primärer Leistung höher aktualisiert wird. Das Protokoll IO Prozentsatz Diagramm Azure-Portal bietet eine gute Möglichkeit, die minimalen Leistungsstufe der sekundären schätzen, die erforderlich ist, um die Replikationslast erhalten. Beispielsweise ist die primäre Datenbank P6 (1000 DTU) und das Protokoll i/o-Prozent der sekundären mindestens muss 50 % P4 (500 DTU). Sie können auch die e/a-Daten mit [resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) oder [sys.dm_db_resource_stats]( https://msdn.microsoft.com/library/dn800981.aspx) Ansichten abrufen.  Weitere Informationen zu der SQL-Datenbank-Performance finden Sie unter [SQL-Datenbank und Leistung](sql-database-service-tiers.md). 

- **Benutzergesteuerter Failover und Failback**: eine sekundäre Datenbank explizit läßt die primäre Rolle jederzeit von der Anwendung oder der Benutzer. Bei realen Ausfällen sollte die Option "nicht geplante" verwendet werden sofort eine sekundäre fördert zu primären. Ausgefallene Primärlaufwerk wiederhergestellt, ist wieder automatisch wiederhergestellte primär als sekundärer markiert und neuer primärer Stand bringen. Aufgrund der asynchronen Replikation kann eine kleine Datenmenge verloren bei ungeplanten Failover schlägt eine primäre vor der letzten Änderung auf der sekundären Datenbank repliziert. Beim Failover Primär mit mehrere sekundäre automatisch neu Beziehungen Replikation und verbleibende sekundäre ohne Eingriff des Benutzers auf den gestuften verknüpft. Nachdem Ausfälle, die verursacht das Failover verringert kann die Anwendung die primäre Region wünschenswert. Dazu sollte der Failover-Befehl mit der Option "geplanten" aufgerufen werden. 

- **Anmeldeinformationen und Firewallregeln synchron halten**: Wir empfehlen mit [Datenbank-Firewall-Regeln](sql-database-firewall-configure.md) für diese Regeln Geo replizierten Datenbanken werden kann um sicherzustellen, dass alle sekundäre Datenbanken dieselbe Firewallregeln als primäre Datenbank repliziert werden. Dadurch entfällt manuell konfigurieren und pflegen der Firewallregeln auf Server sowohl die primären und sekundären Datenbanken. Ebenso Datenzugriff sichert primäre und sekundäre Datenbanken immer derselben Benutzer mit [enthaltenen Benutzer](sql-database-manage-logins.md) Anmeldeinformationen so bei einem Failover ist es ohne Unterbrechung durch Konflikte mit Benutzernamen und Kennwörtern. Mit [Active Directory Azure](../active-directory/active-directory-whatis.md)können Kunden Benutzerzugriff auf primären und sekundären Datenbanken und überflüssig für die Verwaltung von Anmeldeinformationen in Datenbanken vollständig verwalten.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Aktualisieren oder Herabstufen einer Primärdatenbank
Sie aktualisieren oder herabstufen eine primäre Datenbank auf unterschiedliche Performance (innerhalb der gleichen Service-Ebene) ohne sekundären Datenbanken trennen. Beim Aktualisieren wird empfohlen, dass Sie die sekundäre Datenbank zuerst, und aktualisieren Sie die primäre. Beim Herunterstufen, Reihenfolge: zunächst die primäre herabstufen und Herabstufen des sekundären. 

Die sekundäre Datenbank muss in der gleichen Service als den primären Migrieren der primären Datenbank auf einen anderen Dienst erfordert Link Geo-Replikation beenden und die sekundäre Datenbank umbenennen oder einfach löschen. Migrieren Sie der primären auf neue Service-Tier und konfigurieren Sie Geo-Replikation. Der neuen sekundären wird standardmäßig automatisch mit der gleichen Performance als primäre erstellt.

## <a name="preventing-the-loss-of-critical-data"></a>Verhindern den Verlust wichtiger Daten
Durch hohe Latenz der WANs verwendet fortlaufende Kopie einer asynchronen Replikationsmechanismus. Asynchroner Replikation macht Datenverlust unvermeidlich, tritt ein Fehler auf. Einige Programme erfordern allerdings keine Daten verloren. Um diese wichtigen Updates zu schützen, kann Anwendungsentwickler die Systemprozedur [Sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) unmittelbar nach dem Commit der Transaktions aufrufen. Aufruf von der aufrufende Thread **Sp_wait_for_database_copy_sync** blockiert, bis die letzte festgeschriebene Transaktion auf die sekundäre Datenbank repliziert wurden. Die Prozedur wartet, bis alle Transaktionen in Warteschlangen die sekundäre Datenbank anerkannt haben. **Sp_wait_for_database_copy_sync** ist eine kontinuierliche Kopie Verknüpfung beschränkt. Jeder Benutzer mit der Verbindung mit der primären Datenbank kann diese Prozedur aufrufen.

>[AZURE.NOTE] Die Verzögerung durch ein **Sp_wait_for_database_copy_sync** Prozeduraufruf kann beträchtlich sein. Die Verzögerung hängt von der Größe der Transaktion Protokolllänge derzeit und dieser Aufruf kehrt erst das gesamte Protokoll repliziert wird. Vermeiden Sie diese Prozedur aufrufen, wenn unbedingt notwendig.

## <a name="programmatically-managing-active-geo-replication"></a>Aktive Geo-Replikation verwalten programmgesteuert

Wie bereits erwähnt, kann aktive Geo-Replikation auch programmgesteuert mit Azure PowerShell die REST-API verwaltet werden. Die folgenden Tabellen beschreiben die verfügbaren Befehle.

- **Azure-Ressourcen-Manager-API und rollenbasierte Sicherheit**: aktive Geo-Replikation umfasst eine Reihe von [Azure-Ressourcen-Manager-APIs]( https://msdn.microsoft.com/library/azure/mt163571.aspx) für Management, einschließlich [Azure-Ressourcen-Manager-basierten PowerShell-Cmdlets](sql-database-geo-replication-powershell.md). Diese APIs erfordern die Verwendung von Ressourcengruppen und rollenbasierte Sicherheit (RBAC) unterstützen. Weitere Informationen zum Implementieren von Access finden Sie unter Rollen [Azure_Role-Based Access Control](../active-directory/role-based-access-control-configure.md).

>[AZURE.NOTE] Viele neue Features aktive Geo-Replikation werden nur unterstützt mit [Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md) basiert [Azure SQL-REST-API](https://msdn.microsoft.com/library/azure/mt163571.aspx) und [Azure SQL-Datenbank-PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx). Die REST API (classic)] (https://msdn.microsoft.com/library/azure/dn505719.aspx) und [Azure SQL-Datenbank (classic) Cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) werden unterstützt aus Kompatibilitätsgründen mithilfe der Azure-Ressourcen-Manager-basierten APIs werden empfohlen. 


### <a name="transact-sql"></a>Transact-SQL

|Befehl|Beschreibung|
|-------|-----------|
|[ALTER Datenbank SQL Azure](https://msdn.microsoft.com/library/mt574871.aspx)|Verwenden Sie sekundäre auf SERVER hinzufügen Argument eine sekundäre Datenbank für eine vorhandene Datenbank und startet die Datenreplikation erstellen|
|[ALTER Datenbank SQL Azure](https://msdn.microsoft.com/library/mt574871.aspx)|Verwenden Sie FAILOVER oder FORCE_FAILOVER_ALLOW_DATA_LOSS, um eine sekundäre Datenbank primären Failover initiieren zu wechseln
|[ALTER Datenbank SQL Azure](https://msdn.microsoft.com/library/mt574871.aspx)|Verwenden Sie sekundäre auf SERVER entfernen Datenreplikation zwischen einer SQL-Datenbank und die angegebene sekundäre Datenbank beendet.|
|[Sys.geo_replication_links (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575501.aspx)|Gibt Informationen über alle vorhandenen Replikationslinks für jede Datenbank auf dem logischen Server Azure SQL-Datenbank.|
|[Sys.dm_geo_replication_link_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575504.aspx)|Ruft den Zeitpunkt der letzten Replikation, Lag der letzten Replikation und andere Informationen zur Replikation Verknüpfung für eine bestimmte SQL-Datenbank.|
|[Sys.dm_operation_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn270022.aspx)|Zeigt den Status für alle Datenbankvorgänge einschließlich des Status der Replikationslinks.|
|[Sp_wait_for_database_copy_sync (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn467644.aspx)|die Anwendung wartet, bis alle festgeschriebenen Transaktionen repliziert und aktive sekundäre Datenbank anerkannt wird.|
||||

### <a name="powershell"></a>PowerShell

|Cmdlets|Beschreibung|
|------|-----------|
|[AzureRmSqlDatabase abrufen](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Ruft eine oder mehrere Datenbanken.|
|[Neue AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Erstellt eine sekundäre Datenbank für eine vorhandene Datenbank und startet die Datenreplikation.|
|[AzureRmSqlDatabaseSecondary festlegen](https://msdn.microsoft.com/en-us/library/mt619393.aspx)|Wechselt eine sekundäre Datenbank zu primären Failover initiiert.|
|[AzureRmSqlDatabaseSecondary entfernen](https://msdn.microsoft.com/en-us/library/mt603457.aspx)|Datenreplikation zwischen einer SQL-Datenbank und die angegebene sekundäre Datenbank beendet.|
|[AzureRmSqlDatabaseReplicationLink abrufen](https://msdn.microsoft.com/library/mt619330.aspx)|Ruft die Links Geo-Replikation zwischen einer Azure SQL-Datenbank und einer Ressourcengruppe oder SQL Server.|
||||

### <a name="rest-api"></a>REST-API

|API|Beschreibung|
|---|-----------|
|[Erstellen oder aktualisieren (CreateMode = wiederherstellen)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Erstellt, aktualisiert und stellt eine primäre oder eine sekundäre Datenbank.|
|[Erstellen oder Aktualisieren des Datenbankstatus](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Gibt den Status bei einem Erstellungsvorgang.|
|[Festlegen Sie sekundäre Datenbank als primäre (geplanten Failover)](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Eine sekundäre Datenbank partnerschaftlich Geo-Replikation der neuen primären Datenbank zu fördern.|
|[Festlegen Sie sekundäre Datenbank als primäre (ungeplanten Failover)](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Erzwingen Sie ein Failover auf die sekundäre Datenbank und den zweiten als primäre.|
|[Replikationslinks zu erhalten](https://msdn.microsoft.com/library/azure/mt600929.aspx)|Ruft alle Replikationslinks für eine bestimmte SQL-Datenbank in eine Partnerschaft Geo-Replikation. Die Informationen in der Katalogansicht sys.geo_replication_links abgerufen.|
|[Replikation Verknüpfung](https://msdn.microsoft.com/library/azure/mt600778.aspx)|Ruft eine spezielle Replikations-Verknüpfung für eine bestimmte SQL-Datenbank in eine Partnerschaft Geo-Replikation. Die Informationen in der Katalogansicht sys.geo_replication_links abgerufen.|
||||



## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte Backups](sql-database-automated-backups.md).
- Automatisierte Backups Recovery mit finden Sie unter [Wiederherstellen einer Datenbank aus den Sicherungskopien Dienst initiiert](sql-database-recovery-using-backups.md).
- Archivierung mit automatischen Sicherungen finden Sie unter [Datenbank kopieren](sql-database-copy.md).
- Zur Authentifizierung an einem neuen primären Server und einer Datenbank finden Sie unter [Sicherheit der SQL-Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md).
