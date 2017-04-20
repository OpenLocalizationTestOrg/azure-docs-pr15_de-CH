<properties 
   pageTitle="SQL Datenbank Disaster Recovery Bohrer | Microsoft Azure" 
   description="Erfahren Sie Leitfäden und bewährte Methoden für die Verwendung von Azure SQL-Datenbank Disaster Recovery einen Drilldown ausführen, mit dem Ihre Mission kritische Business Applications sowie zu Fehlern und Ausfällen." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Disaster Recovery Drilldown ausführen

Es wird empfohlen, Validierung Anwendungsbereitschaft für Recovery-Workflow in regelmäßigen Abständen ausgeführt wird. Überprüfung von Anwendungsverhalten und folgen von Datenverlust oder Unterbrechung, Failover umfasst empfiehlt engineering. Es ist auch eine Anforderung durch die meisten Industriestandards als Teil des Business Continuity-Zertifizierung.

Durchführen einer Disaster Recovery-Bohrung besteht aus:

- Simulation Tier Datenausfall
- Wiederherstellen 
- Überprüfen der Integrität Post-Problem

Wie Sie [Ihre Anwendung für Business Continuity](sql-database-business-continuity.md), der Workflow der Drilldown ausführen variiert abhängig. Im folgenden beschreiben wir die best Practices Durchführen einer Disaster Recovery Bohrung im Kontext von Azure SQL-Datenbank. 

##<a name="geo-restore"></a>Geo-Wiederherstellung

Um Datenverluste zu verhindern, dass bei einem Drilldown Disaster Recovery empfehlen wir Drilldown durch eine Kopie dieser Umgebung erstellen und überprüfen die Anwendung Failover Workflow verwenden.
 
####<a name="outage-simulation"></a>Ausfall-simulation

Den Ausfall simulieren können Sie löschen oder umbenennen die Quelldatenbank. Dadurch werden Anwendungsfehler Konnektivität. 

####<a name="recovery"></a>Wiederherstellung

- Durchführen Sie Geo-Wiederherstellung der Datenbank auf einem anderen Server als Beschreibung [hier](sql-database-disaster-recovery.md) 
- Ändern Sie die Anwendungskonfiguration zu verbinden mit der wiederhergestellten Datenbanken führen [Konfigurieren einer Datenbank nach Wiederherstellung](sql-database-disaster-recovery.md) um die Wiederherstellung abzuschließen.

####<a name="validation"></a>Validierung

- Führen Sie den Bohrer überprüfen die Integrität Post Problem (Verbindungszeichenfolgen, Benutzernamen, grundlegende Funktionstests oder anderen Prüfung Teil standard Signoffs Verfahren).

##<a name="geo-replication"></a>Geo-Replikation

Für eine Datenbank mit Geo-Replikation geschützt wird Drilldown Übung Geplantes Failover auf die sekundäre Datenbank umfassen. Geplante Failover wird sichergestellt, dass der primäre und sekundären Datenbanken synchronisiert bleiben bei die Rollen. Im Gegensatz zu ungeplanten Failover wird dieser Vorgang nicht Datenverlust daher der Bohrer in dieser Umgebung ausgeführt werden kann. 

####<a name="outage-simulation"></a>Ausfall-simulation

Den Ausfall simuliert Anwendung oder virtuellen Computer mit der Datenbank verbunden zu deaktivieren. Dies führt zu der Verbindungsfehler für Web-Clients.

####<a name="recovery"></a>Wiederherstellung

- Sicherstellen Sie die Anwendungskonfiguration im Bereich Disaster Recovery des ehemaligen sekundären zeigt die vollständigen Zugriff auf neue primäre werden. 
- [Geplantes Failover](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) zu sekundären Datenbank eine neue primäre ausführen
- Führen Sie auf [eine Datenbank nach Wiederherstellung konfigurieren](sql-database-disaster-recovery.md) , um die Wiederherstellung abzuschließen.

####<a name="validation"></a>Validierung

- Führen Sie den Bohrer überprüfen die Integrität Post Problem (Verbindungszeichenfolgen, Benutzernamen, grundlegende Funktionstests oder anderen Prüfung Teil standard Signoffs Verfahren).


## <a name="next-steps"></a>Nächste Schritte

- Business Continuity Szenarien finden Sie unter [Continuity-Szenarien](sql-database-business-continuity.md)
- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte backups](sql-database-automated-backups.md)
- Automatisierte Backups Recovery mit finden Sie unter [Wiederherstellen einer Datenbank aus den Sicherungskopien Dienst initiiert](sql-database-recovery-using-backups.md)
- Schnellere Recovery-Optionen finden Sie unter [Aktive geografische Replikation](sql-database-geo-replication-overview.md)  
