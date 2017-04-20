<properties
   pageTitle="SQL Datenbank-Wiederherstellung | Microsoft Azure"
   description="Informationen Sie zum Wiederherstellen einer Datenbank aus einer regionalen Datacenter Ausfall oder Azure SQL-Datenbank aktive Geo-Replikation und Geo-Wiederherstellungsfunktionen."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Wiederherstellen einer Azure SQL-Datenbank oder ein Failover auf die sekundäre

Azure SQL-Datenbank bietet die folgenden Funktionen zur Wiederherstellung nach einem Ausfall:

- [Aktive Geo-Replikation](sql-database-geo-replication-overview.md)
- [Geo-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

Business Continuity-Szenarien und die Funktionen dieser Szenarien finden Sie unter [Business Continuity](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Vorbereitung für den Fall eines Ausfalls

Für den Erfolg mit anderen Datenbereich mit aktiven Geo-Replikation oder Geo überflüssige Sicherungskopien Vorbereiten eines Servers müssen in einem anderen Rechenzentrum Ausfall der neue primäre Server muss auftreten, sowie auch Schritte dokumentiert und getestet, um eine reibungslose Wiederherstellung definiert haben. Diese vorbereitenden Schritte umfassen:

- Identifizieren Sie den logischen Server in einer anderen Region zu den neuen primären Server. Mit aktiven Geo-Replikation, werden das mindestens und vielleicht der sekundäre Server. Geo-Wiederherstellung werden dies im Allgemeinen ein Server in der [zugehörigen Region](../best-practices-availability-paired-regions.md) für die Region in dem sich die Datenbank befindet.
- Identifizieren und optional zur Benutzern die neue primäre Datenbank auf Serverebene Firewallregeln definieren.
- Bestimmen Sie, wie Sie Benutzer auf den neuen primären Server wie Verbindungszeichenfolgen ändern oder Ändern von DNS-Einträgen umgeleitet werden.
- Identifizieren und optional die Benutzernamen, die in der master-Datenbank auf dem neuen primären Server vorhanden, und sicherzustellen, dass diese Benutzernamen entsprechende Berechtigungen in der master-Datenbank ggf. erstellen. Weitere Informationen finden Sie unter [Sicherheit SQL Datenbank nach Wiederherstellung](sql-database-geo-replication-security-config.md)
- Identifizieren Sie Warnregeln, die aktualisiert werden, um die neue primäre Datenbank zuordnen.
- Dokumentieren Sie die Überwachungskonfiguration in der aktuellen primären Datenbank
- Führen Sie eine [Wiederherstellung Bohren](sql-database-disaster-recovery-drills.md). Zum Simulieren eines Ausfalls Geo-Wiederherstellung können Sie löschen oder umbenennen die Quelldatenbank zu Anwendung Verbindungsfehler. Zum Simulieren eines Ausfalls für aktive Geo-Replikation können Sie die Anwendung oder Datenbank oder Failover der Datenbank verbundenen virtuellen Connectity Anwendungsfehler deaktivieren.

## <a name="when-to-initiate-recovery"></a>Wenn Recovery zu starten

Recovery-Vorgang wirkt sich auf die Anwendung. Erfordert die SQL-Verbindungszeichenfolge oder Umleitung mit DNS ändern und dauerhaften Datenverlust führen. Daher, es erfolgt nur, wenn der Ausfall länger als die Anwendung Wiederherstellungszeit dürfte. Bei der Bereitstellung der Anwendungdes für die Produktion sollten Sie regelmäßig überwachen die Anwendungsintegrität und verwenden die folgenden Datenpunkte um zu bestätigen, dass die Wiederherstellung erforderlich ist:

1.  Permanente Verbindungsfehler auf der Anwendungsebene zur Datenbank.
2.  Azure-Portal zeigt eine Warnung über einen Vorfall mit großen Einfluss.
3.  Azure SQL-Datenbankserver wird als fehlerhaft markiert.

Abhängig von Ihrer Anwendung Toleranz Ausfallzeiten und mögliche Business Haftung können Sie folgenden Wiederherstellungsoptionen berücksichtigen.

Verwenden Sie die [Wiederhergestellt Datenbank](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) zu den neuesten Geo repliziert Wiederherstellungspunkt.

## <a name="wait-for-service-recovery"></a>Warten auf Recovery service

Azure Teams arbeiten sorgfältig Verfügbarkeit aber je nach Stamm schnell führen wiederherstellen kann Stunden oder Tage dauern.  Wenn Ihre Anwendung erhebliche Ausfallzeiten tolerieren kann, damit die Wiederherstellung abgeschlossen warten können. In diesem Fall ist keine Aktion Ihrerseits erforderlich. Sie sehen den aktuellen Servicestatus unsere [Azure Service Health Dashboard](https://azure.microsoft.com/status/). Nach der Wiederherstellung des Bereichs wird die Verfügbarkeit der Anwendung wiederhergestellt.

## <a name="failover-to-geo-replicated-secondary-database"></a>Failover auf sekundäre Datenbank Geo repliziert

Wenn die Anwendungsausfallzeiten Unternehmen führen kann sollten Sie Geo replizierten Datenbanken in der Anwendung verwenden. Sie können die Anwendung Verfügbarkeit in einer anderen Region bei einem Ausfall schnell wiederherstellen. Erfahren Sie, wie [Geo - Replikation](sql-database-geo-replication-portal.md).

Verfügbarkeit der Datenbanken wiederherstellen Sie Failover auf den sekundären Geo repliziert eine der unterstützten Methoden starten müssen.

Verwenden Sie eine der folgenden Leitlinien für Failover zu einer sekundären Datenbank Geo repliziert:

- [Failover auf eine sekundäre Geo repliziert mit Azure-Portal](sql-database-geo-replication-portal.md)
- [Failover auf sekundäre Geo repliziert mit PowerShell](sql-database-geo-replication-powershell.md)
- [Failover auf einen sekundären Geo repliziert mit T-SQL](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Wiederherstellen von Geo-Wiederherstellung

Wenn die Anwendungsausfallzeiten nicht Business Haftung führt können Geo-Wiederherstellung als Methode Datenbank(en) Anwendung wiederherstellen. Aus der aktuellen Geo redundantes Backup erstellt eine Kopie der Datenbank.

Mit einer der folgenden Handbüchern Geo-Wiederherstellung einer Datenbank in einen neuen Bereich:

- [Geo-Wiederherstellung einer Datenbank in einer neuen Region mit Azure-Portal](sql-database-geo-restore-portal.md)
- [Geo-Wiederherstellung einer Datenbank in einer neuen Region mit PowerShell](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>Datenbank nach Wiederherstellung konfigurieren

Verwenden Sie Geo-Replikation Failover oder geografische Wiederherstellung sind nach einem Ausfall wiederhergestellt, müssen Sie sicherstellen, dass die Konnektivität mit den neuen Datenbanken ordnungsgemäß konfiguriert ist, dass die Funktion der Anwendung fortgesetzt werden kann. Dies ist eine Checkliste für Aufgaben Produktion wiederhergestellte Datenbank bereit.

### <a name="update-connection-strings"></a>Aktualisieren der Verbindungszeichenfolgen

Da die wiederhergestellte Datenbank auf einem anderen Server befindet, müssen Sie Verbindungszeichenfolge Ihrer Anwendung auf dem Server aktualisieren.

Weitere Informationen zum Ändern von Verbindungszeichenfolgen finden Sie unter der entsprechenden Entwicklungssprache für Ihre [Verbindungsbibliothek](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Firewall-Regeln konfigurieren

Sie müssen sicherstellen, dass die Firewallregeln konfiguriert auf Server und der Datenbank übereinstimmen, die auf dem primären Server und primäre Datenbank konfiguriert wurden. Weitere Informationen finden Sie unter [How to: Configure Firewall Settings (Azure SQL-Datenbank)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Konfigurieren von Benutzernamen und Benutzern

Sie müssen sicherstellen, dass alle von der Anwendung verwendeten Benutzernamen auf dem Server vorhanden, die die wiederhergestellte Datenbank hostet. Weitere Informationen finden Sie unter [Sicherheitskonfiguration für Geo-Replikation](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] Konfigurieren und Testen Sie Ihre Firewallregeln Server und Benutzernamen (und deren Berechtigungen) während einer Bohrung Disaster Recovery. Diese Objekte auf Serverebene und deren Konfiguration können nicht während des Ausfalls verfügbar.

### <a name="setup-telemetry-alerts"></a>Telemetrie Alarme einrichten

Sie müssen darauf vorhandenen regeleinstellungen aktualisiert werden, um die wiederhergestellte Datenbank und anderen Server zuzuordnen.

Weitere Informationen zu Warnregeln Datenbank finden Sie unter [Warnung Benachrichtigungen erhalten](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) und [Track-Dienststatus](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Aktivieren der Überwachung

Überwachung erforderlich ist, auf die Datenbank zugreifen, müssen Sie nach der Wiederherstellung der Datenbank Auditing aktivieren. Indikator ist Überwachung erforderlich ist, dass Clientanwendungen sichere Verbindungszeichenfolgen in ein *. database.secure.windows.net. Weitere Informationen finden Sie unter [Erste Schritte mit SQL Datenbank überwachen](sql-database-auditing-get-started.md).


## <a name="next-steps"></a>Nächste Schritte

- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte backups](sql-database-automated-backups.md)
- Business Continuity Design und Recovery-Szenarien finden Sie unter [Continuity-Szenarien](sql-database-business-continuity.md)
- Automatisierte Backups Recovery mit finden Sie unter [Wiederherstellen einer Datenbank aus den Sicherungskopien Dienst initiiert](sql-database-recovery-using-backups.md)
