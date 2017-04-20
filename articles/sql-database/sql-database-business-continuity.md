<properties
   pageTitle="Business Continuity Cloud - Datenbank-Recovery - SQL Datenbank | Microsoft Azure"
   description="Erfahren Sie, wie Azure SQL-Datenbank Cloud Business Continuity und Datenbank-Recovery unterstützt und hilft, unternehmenskritische Cloudanwendungen ausgeführt."
   keywords="Business Continuity Cloud Business Continuity datenbankwiederherstellung, Wiederherstellung"
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
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Übersicht über Business Continuity mit Azure SQL-Datenbank

Diese Übersicht beschreibt die Funktionen, die für Business Continuity und Disaster Recovery Azure SQL-Datenbank bereitstellt. Es bietet Optionen, Vorschläge und Lernprogramme zur Wiederherstellung nach Störungen, die können zu Datenverlust oder Ihre Datenbank und die Anwendung verfügbar. Was tun, wenn ein Benutzer die Diskussion enthält Anwendungsfehler wirkt sich auf die Datenintegrität, Azure-Region hat einen Ausfall oder Wartung der Anwendung. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL Datenbank-Features, mit denen Sie zu Business continuity

SQL-Datenbank bietet verschiedene Business Continuity-Funktionen einschließlich automatisierte Backups und optional die Replikation. Jeder hat unterschiedliche Eigenschaften für geschätzte Wiederherstellungszeit (Einfügen) und Datenverluste für aktuelle Transaktionen. Wenn Sie diese Optionen verstehen, können-auswählen und, in den meisten Szenarien zusammen verwenden, für unterschiedliche Szenarien. Business Continuity-Plan entwickeln, müssen Sie verstehen, maximale Zeitspanne, bevor die Anwendung vollständig, nachdem die störenden Ereignis wiederhergestellt-dies der Recovery Time Objective (RTO ist). Sie müssen auch den Hoechstbetrag der letzten Aktualisierung (Zeitintervall) verstehen die Anwendung tolerieren kann verloren gehen, wenn nach der störenden Ereignis - Recovery Point Objective (RPO) wiederherstellen. 

Die folgende Tabelle vergleicht einfügen und RPO für die drei häufigsten Szenarien.

| Funktion |  Grundlegende Ebene | Standard-Stufe  | Premium-Ebene |
|---|---|---|---|
| Zeitpunkt wiederherstellen aus einer Sicherung | Einen Wiederherstellungspunkt innerhalb von 7 Tagen   | Einen Wiederherstellungspunkt 35 Tagen  | Einen Wiederherstellungspunkt 35 Tagen |
Geo-Wiederherstellung von Backups Geo repliziert | ERT < 12h RPO < 1h   | ERT < 12h RPO < 1h   | ERT < 12h RPO < 1h |
|Aktive Geo-Replikation | ERT < 30 RPO < 5 s   | ERT < 30 RPO < 5 s | ERT < 30 RPO < 5 s |


### <a name="use-database-backups-to-recover-a-database"></a>Verwenden Sie Datenbank-Backups einer Datenbank

SQL-Datenbank führt automatisch eine Kombination von Datenbank-Backups wöchentlich differenzielle Datenbank stündlich Backups und Transaktionsprotokollen fünf Minuten zum Schutz Ihres Unternehmens vor Datenverlusten. Diese Backups werden in lokal redundanter Speicher 35 Tage für Datenbanken im Standard- und Premium-Dienstebenen und sieben Tage für Datenbanken in der Basisdienst - [Dienstebenen](sql-database-service-tiers.md) Einzelheiten auf Dienstebenen. Wenn die Beibehaltungsdauer für die Dienstebene Ihre Bedürfnisse nicht erfüllt, können Sie die Aufbewahrungsdauer durch [Ändern der Dienstebene](sql-database-scale-up.md)erhöhen. Die vollständige und differenzielle Datenbank Backups auch Schutz gegen einen Rechenzentrums hat - einen [paarweisen Rechenzentrum](../best-practices-availability-paired-regions.md) repliziert finden Sie unter [Automatische Datenbank-Backups](sql-database-automated-backups.md) für weitere Details.

Diese automatische Datenbank-Backups können zum Wiederherstellen einer Datenbank von verschiedenen Störungen im Rechenzentrum und einem anderen Rechenzentrum. Automatische Datenbank-Backups hängt, der ungefähre Zeitpunkt des Recovery Faktoren, z. B. Datenbanken wiederherstellen im selben Bereich gleichzeitig, die Datenbankgröße, Transaktionslog und Netzwerkbandbreite. In den meisten Fällen ist die Recovery-Zeit weniger als 12 Stunden. Beim Wiederherstellen einer anderen Datenbereich ist Datenverluste auf 1 Stunde von Geo redundante Speicherung stündlich differenzielle Datenbank-Backups. 

> [AZURE.IMPORTANT] Wiederherstellen mithilfe von automatischen Sicherungskopien muss Mitglied der SQL Server-Beitragender oder der Abonnementbesitzer - finden Sie unter [RBAC: integrierte Rollen](../active-directory/role-based-access-built-in-roles.md). Sie können mit der Azure-Portal, PowerShell oder die REST-API wiederherstellen. Sie können keine Transact-SQL.

Automatisierte Backups als Business Continuity und Recovery Mechanismus verwenden, wenn Ihre Anwendung:

- Mission kritische gilt nicht.
- Nicht haben eine Bindung SLA daher die Ausfallzeit von 24 Stunden oder länger nicht finanzielle Haftung führen.
- Hat eine niedrige ändern (niedrige Transaktionen pro Stunde) und bis zu verlieren Stunde Änderung ist ein akzeptabler Datenverlust. 
- Kosten reagiert. 

Verwenden Sie schnelleres Recovery benötigen, [Aktive Geo-Replikation](sql-database-geo-replication-overview.md) (weiter erläutert). Möchten Sie möglicherweise Daten von höchstens 35 Tagen ab, sollten Ihre Datenbank regelmäßig eine BACPAC Archivierung gespeichert (eine komprimierte Datei mit Ihrem Schema der Datenbank zugeordneten Daten) in Azure BLOB-Speicher oder an einem anderen Ort Ihrer Wahl. Weitere Informationen zum Erstellen einer transaktionell konsistenten Datenbankarchiv finden Sie [eine Datenbankkopie erstellen](sql-database-copy.md) und [Exportieren Sie die Datenbankkopie](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Aktive Geo-Replikation mit der Recovery-Zeit und Datenverlust eine Wiederherstellung zugeordnet

Neben Backups der Datenbank bei einer Unterbrechung des Geschäftsbetriebs verwenden, können Sie [Aktive Geo-Replikation](sql-database-geo-replication-overview.md) zum Konfigurieren einer Datenbank um bis zu vier lesbaren sekundären Datenbanken in den Bereichen Ihrer Wahl. Diese sekundären Datenbanken werden mit der primären Datenbank eine asynchrone Replikation Mechanismus synchronisiert. Diese Funktion dient zum Schutz des Geschäftsbetriebs einen Rechenzentrums hat oder bei einer anwendungsaktualisierung. Aktive Geo-Replikation kann auch verwendet werden, bessere Abfrageperformance für schreibgeschützte Abfragen geografisch verteilten Benutzern zur Verfügung.

Wenn die primäre Datenbank unerwartet offline geschaltet wird, Sie offline für Wartungsarbeiten müssen können Sie schnell eine sekundäre zu primären (auch als "Failover" bezeichnet) und Applikationen gestuften primäre Verbindung konfigurieren heraufstufen. Mit einem geplanten Failover ist kein Datenverlust. Mit einem ungeplanten Failover möglicherweise einige wenig Datenverlusten jüngsten Transaktionen aufgrund der asynchronen Replikation. Nach einem Failover können Sie später Failback - nach Plan oder wenn das Rechenzentrum wieder online ist. In allen Fällen Benutzer etwas Ausfallzeiten und wiederherstellen. 

> [AZURE.IMPORTANT] Um aktive Geo-Replikation verwenden, müssen Sie Ihrem Besitzer werden oder Administratorberechtigungen in SQL Server. Sie können und Failover mit Azure-Portal, PowerShell oder die REST-API Berechtigungen für das Abonnement oder Transact-SQL-Berechtigungen in SQL Server.

Verwenden Sie aktive Geo-Replikation, die Anwendung dieser Kriterien erfüllt:

- Mission ist kritisch.
- Hat eine Vereinbarung zum Servicelevel (SLA), die nicht mindestens 24 Stunden Ausfallzeit zulässt.
- Ausfallzeiten führen finanzielle Haftung.
- Ist Daten ändern hoch und Stunde Daten verlieren nicht akzeptabel ist.
- Die Kosten für aktive Geo-Replikation ist niedriger als der potenziellen finanziellen Haftung und zugeordnete Unternehmen.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Wiederherstellen einer Datenbank nach einem Benutzer oder einer Anwendung Fehler

* Niemand ist perfekt! Ein Benutzer könnte versehentlich Daten löschen, versehentlich löschen eine wichtige Tabelle oder sogar eine gesamte Datenbank löschen. Oder eine Anwendung kann Daten mit fehlerhaften Daten durch einen Anwendungsfehler versehentlich überschrieben. 

In diesem Szenario sind die Wiederherstellungsoptionen.

### <a name="perform-a-point-in-time-restore"></a>Führen Sie eine Point-in-Time-Wiederherstellung

Automatisierte Backups können Sie eine Kopie der Datenbank auf einen funktionsfähigen Zeitpunkt, wiederherzustellen, ist Zeit innerhalb der Aufbewahrungsdauer Datenbank. Nach dem Wiederherstellen der Datenbank die wiederhergestellte Datenbank die Originaldatenbank ersetzen oder Kopieren von Daten aus der wiederhergestellten Daten in die ursprüngliche Datenbank. Die Datenbank aktive Geo-Replikation verwendet, sollten die erforderlichen Daten aus der wiederhergestellten Kopie in die ursprüngliche Datenbank kopieren. Wenn Sie die ursprüngliche Datenbank mit der wiederhergestellten Datenbank ersetzen, müssen Sie konfigurieren und synchronisieren aktive Geo-Replikation (die einige bei einer großen Datenbank dauern kann). 

Weitere Informationen und ausführliche Anleitung zum Wiederherstellen einer Datenbank auf einem Azure-Portal oder PowerShell finden Sie [im Zeitpunkt wiederherstellen](sql-database-recovery-using-backups.md#point-in-time-restore). Mit Transact-SQL können nicht wiederhergestellt werden.

### <a name="restore-a-deleted-database"></a>Eine gelöschte Datenbank wiederherstellen

Wenn Datenbank gelöscht, aber nicht der logische Server gelöscht wurde, können Sie gelöschte Datenbank so wiederherstellen mit gelöscht wurde. Dadurch wird eine Sicherung mit demselben logischen SQL Server aus dem gelöschten wiederhergestellt. Sie können den ursprünglichen Namen wiederherzustellen oder einen neuen Namen oder die wiederhergestellte Datenbank.

Weitere Informationen sowie detaillierte Schritte zum Wiederherstellen einer gelöschten Azure-Portal oder PowerShell finden Sie [eine gelöschte Datenbank wiederherstellen](sql-database-recovery-using-backups.md#deleted-database-restore). Mit Transact-SQL können nicht wiederhergestellt werden.

> [AZURE.IMPORTANT] Wenn logische Server gelöscht wird, kann keine gelöschte Datenbank wiederherstellen. 

### <a name="import-from-a-database-archive"></a>Importieren Sie aus einer Datenbank archivieren

Der Daten-Verlust außerhalb der aktuellen Aufbewahrungsdauer für automatisierte Backups die Datenbank archivieren wurden haben, können Sie [Archivierte BACPAC Datei importieren](sql-database-import.md) in eine neue Datenbank. An diesem Punkt der importierten Datenbank die Originaldatenbank ersetzen oder die benötigten Daten aus den importierten Daten in die ursprüngliche Datenbank. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Wiederherstellen einer Datenbank in eine andere Region ein Azure regionalen Rechenzentrums hat

<!-- Explain this scenario -->

Selten haben eine Azure-Rechenzentrum ein Ausfalls. Bei einem Ausfall wird eine Geschäftsbetriebs, die zuletzt möglicherweise nur einige Minuten oder Stunden dauern kann. 

- Eine Möglichkeit ist die Datenbank wieder online geschaltet, wenn der Data Center Ausfall über warten. Dies gilt für Programme, die die Datenbank offline zu leisten können. Ein Projekt oder eine kostenlose Testversion müssen Sie beispielsweise ständig arbeiten. Wenn ein Rechenzentrum ein Ausfalls verfügt, wird nicht wissen Sie wie der Ausfall lange, damit diese Option nur, funktioniert Wenn die Datenbank eine Weile brauchen.
- Eine weitere Option ist entweder Failover auf einen anderen Datenbereich, wenn Sie aktive Geo-Replikation oder die Wiederherstellung mit Geo redundante Datenbank-Backups (Geo-Wiederherstellung) verwenden. Failover dauert nur wenige Sekunden während der Wiederherstellung von Sicherungskopien Stunden.

Bei der Aktion wiederherstellen Dauer und wie viel Datenverlust im Falle eines Rechenzentrums hat entstehen hängt wie Sie Business Continuity-Funktionen erläutert, die in Ihrer Anwendung verwenden möchten. Allerdings können Sie eine Kombination von Datenbank-Backups und aktive je nach anwendungsspezifischen Erfordernissen Geo-Replikation verwenden. Eine Erläuterung der Anwendung Entwurfsaspekte für eigenständige Datenbanken und elastische Pools mit Business Continuity-Funktionen finden Sie unter [Entwerfen einer Anwendung für die Cloud Disaster Recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md) und [elastischen Pool Disaster Recovery-Strategien](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Die folgenden Abschnitte enthalten eine Übersicht über die Schritte zum Wiederherstellen von Datenbank-Backups oder aktive Geo-Replikation. Einschließlich Planung Vorschriften Wiederherstellungsschritte buchen und Informationen zum Simulieren eines Ausfalls einer Disaster Recovery-Drilldown ausführen finden Sie [eine SQL-Datenbank nach einem Ausfall wiederherstellen](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Vorbereiten eines Ausfalls

Unabhängig von Business Continuity Feature Sie verwenden, müssen Sie:

- Identifizierung und Vorbereitung des Zielservers einschließlich Serverebene Firewallregeln, Benutzernamen und Berechtigungen der master-Datenbank.
- Wie Kunden und Clientanwendungen auf den neuen Server umleiten
- Dokumentieren Sie andere Abhängigkeiten wie Überwachung Einstellungen und Alarme 
 
Wenn Sie nicht planen und vorbereiten, bringt die Anwendung online nach einem Failover oder einer Wiederherstellung zusätzliche Zeit und wahrscheinlich benötigen Sie auch gleichzeitig Stress - eine schlechte Kombination Problembehandlung.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Failover zu einer sekundären Datenbank Geo repliziert 

Wenn Sie als Ihre Wiederherstellungsverfahren [erzwingen ein Failover auf einen sekundären Geo repliziert](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database)Active Geo-Replikation verwenden. Innerhalb des sekundären zur neuen primären heraufgestuft und kann neue Transaktionen und beantwortet Abfragen - mit nur wenigen Sekunden Datenverlust für die Daten, die noch nicht repliziert wurde. Informationen zum Automatisieren der Failover finden Sie unter [Entwerfen einer Anwendung für die Cloud Disaster Recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Wenn das Rechenzentrum wieder online ist, können Sie Failback auf den ursprünglichen (oder nicht).

### <a name="perform-a-geo-restore"></a>Geo-wiederherstellen 

Verwenden Sie automatisiert Backups mit Geo redundante Speicherreplikation Ihre Wiederherstellungsverfahren [Wiederherstellung einer Datenbank mit geografischen Wiederherstellung starten](sql-database-disaster-recovery.md#recover-using-geo-restore). Wiederherstellung erfolgt in der Regel innerhalb von 12 Stunden - Datenverlust eine Stunde bestimmt, wann die letzte stündlich differenzielle Sicherung genommen und replizierte. Bis zum Abschluss der Wiederherstellung kann die Datenbank Transaktionen oder Fragen beantworten. 

> [AZURE.NOTE] Wenn im Rechenzentrum wieder online ist, bevor Sie Ihre Anwendung die wiederhergestellte Datenbank zu wechseln, können Sie einfach die Wiederherstellung Abbrechen.  

### <a name="perform-post-failover--recovery-tasks"></a>Führen Sie die abschließenden Failover / Recovery-Vorgänge 

Nach Wiederherstellung von entweder Wiederherstellungsmechanismus führen Sie die folgenden zusätzlichen Aufgaben, bevor Benutzer und sind wieder einsatzbereit:

- Umleitung Clients und Clientanwendungen auf die neuen Server und Datenbank
- Sicherzustellen Sie, dass entsprechende Firewallregeln auf Serverebene für Benutzer verbinden (oder [Firewalls auf Datenbankebene](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Sicherstellen Sie, dass die entsprechenden Benutzernamen und Berechtigungen der master-Datenbank vorhanden sind (oder [enthaltenen Benutzer](https://msdn.microsoft.com/library/ff929188.aspx)verwenden)
- Konfigurieren der Überwachung gegebenenfalls
- Serveradministrator nach Bedarf

## <a name="upgrade-an-application-with-minimal-downtime"></a>Aktualisieren einer Anwendung mit minimalen Ausfallzeiten

In manchen Fällen benötigt eine Anwendung wegen Wartungsarbeiten wie eine anwendungsaktualisierung offline geschaltet werden. [Verwalten Anwendungsupgrades](sql-database-manage-application-rolling-upgrade.md) beschreibt mit aktive Geo-Replikation ermöglicht parallele Updates der Cloudanwendung zu Ausfallzeiten während Upgrades ein Wiederherstellungspfad falls etwas schief geht. In diesem Artikel zwei unterschiedliche Methoden der Aktualisierung orchestrieren untersucht und erläutert die vor- und Nachteile jeder Option.

## <a name="next-steps"></a>Nächste Schritte

Eine Erläuterung der Anwendung Entwurfsaspekte für eigenständige Datenbanken und elastische Pools finden Sie unter [Entwerfen einer Anwendung für die Cloud Disaster Recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md) und [elastischen Pool Disaster Recovery-Strategien](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).






