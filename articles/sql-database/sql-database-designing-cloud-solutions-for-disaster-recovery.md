<properties
   pageTitle="Disaster Recovery-Lösung - SQL Datenbank aktive Geo-Replikation Cloud | Microsoft Azure"
   description="Erfahren Sie, wie Cloud Disaster Recovery-Lösung für Business Continuity-Planung mit Geo-Replikation für app-Daten-Backup mit Azure SQL-Datenbank."
   keywords="Cloud-Wiederherstellung, Disaster Recovery-Lösung, app-Daten-Backup, Geo-Replikation Business Continuity-Planung"
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
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Entwerfen Sie für Cloud Disaster Recovery über aktive Geo-Replikation in SQL-Datenbank


> [AZURE.NOTE] [Aktive Geo-Replikation](sql-database-geo-replication-overview.md) steht für alle Datenbanken auf allen Ebenen.



Enthält Informationen zum Entwurf ergeben sowie regionale Ausfälle zu katastrophalen Ausfällen [Aktive Geo-Replikation](sql-database-geo-replication-overview.md) in SQL-Datenbank verwenden. Business Continuity-Planung, sollten Sie die Anwendung Bereitstellungstopologie Sie abzielen, Servicelevel Datenverkehr Latenz und Kosten. In diesem Artikel wir allgemeine Anwendungsmuster betrachten und Erläutern Sie die vor- und Nachteile jeder Option. Informationen über aktive Geo-Replikation mit elastische finden Sie [elastischen Pool Disaster Recovery-Strategien](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Entwurfsmuster 1: Aktiv / Passiv-Bereitstellung Cloud Disaster Recovery mit einer zusammengestellten

Diese Option eignet sich am besten für Applikationen mit folgenden Merkmalen:

+ Aktive Instanz in einer Azure region
+ Abhängigkeit (RW) Schreibzugriff auf Daten
+ Cross-Region Konnektivität zwischen der Anwendungslogik und die Datenbank ist nicht akzeptabel aufgrund der Latenz und Datenverkehr    

In diesem Fall ist die Anwendung Bereitstellungstopologie für regionale Katastrophen behandeln, wenn alle Komponenten betroffen sind und Failover als Einheit optimiert. Geografische Redundanz der Anwendungslogik und die Datenbank in eine andere Region repliziert jedoch werden für die Arbeitslast der Anwendung unter normalen Umständen nicht verwendet. Die Anwendung in der sekundären sollte für eine SQL-Verbindungszeichenfolge für die sekundäre Datenbank konfiguriert werden. Traffic Manager soll [Failover Verteilungsmethode](../traffic-manager/traffic-manager-configure-failover-routing-method.md)verwenden.  

> [AZURE.NOTE] [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) wird in diesem Artikel nur zu Demonstrationszwecken verwendet. Sie können eine Lösung des Lastenausgleichs verwenden, die Routingmethode Failover unterstützt.    

Neben Instanzen Anwendung sollten Sie eine kleine [Worker-Rolle Anwendung](cloud-services-choose-me.md#tellmecs) , der die primäre Datenbank überwacht durch periodische T-SQL schreibgeschützt (RO) Befehle bereitstellen. Sie können Sie automatisch ein Failover auf Verwaltungskonsole der Anwendung Warnung oder beides ausgelöst. Um sicherzustellen, dass die Überwachung von regionalen Ausfälle nicht beeinflusst wird, bereitzustellen Überwachung Anwendungsinstanzen für jede Region und jede Instanz die primäre Datenbank im Bereich für primäre und sekundäre Datenbank in der Region verbunden. 

> [AZURE.NOTE] Sowohl Applikationen Überwachung sollte aktiv Prüfpunkt primäre und sekundäre Datenbanken. Diese dienen zum Fehler sekundären und Warnung erkennen, wenn die Anwendung nicht geschützt ist.     

Das folgende Diagramm zeigt diese Konfiguration vor Ausfällen.

![SQL Datenbank Geo-Replikationskonfiguration. Cloud-Wiederherstellung.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Nach einem Ausfall der primären Region erkennt Überwachung Anwendung, dass die primäre Datenbank ist nicht möglich und eine Benachrichtigung registriert. Je nach Ihrer SLA Anwendung können Sie entscheiden, wie viele fortlaufende Überwachung Prüfpunkte ausfällt, bevor ein Datenbankausfall deklariert. Um koordinierte Failover Endpunkt Anwendung und Datenbank zu erreichen, benötigen Sie monitoring-Anwendung die folgenden Schritte ausführen:

1. [Aktualisieren des Status der primäre Endpunkt](https://msdn.microsoft.com/library/hh758250.aspx) um Endpunkt Failover ausgelöst.
2. Rufen Sie die sekundäre Datenbank [datenbankfailover initiiert](sql-database-geo-replication-portal.md).

Nach einem Failover die Anwendung verarbeitet Benutzeranfragen im zweiten Bereich bleibt jedoch mit der Datenbank denn nun die primäre Datenbank im zweiten Bereich. Dieses Szenario ist im folgenden Diagramm dargestellt. In allen Diagrammen Linien anzuzeigen Partnerschaften, gepunktete Linie kennzeichnet unterbrochene Verbindung und schildern Aktion Trigger anzugeben.


![Geo-Replikation: Failover auf sekundären Datenbank. App-Daten-Backup.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Fall ein Ausfalls im zweiten Bereich Replikation Verknüpfung zwischen primären und sekundären Datenbank unterbrochen und Überwachung Anwendung registriert eine Warnung, dass die primäre Datenbank verfügbar gemacht wird. Die Leistung der Anwendung wird nicht beeinträchtigt, aber die Anwendung ausgesetzt ist und daher einem höheren Risiko bei beiden Bereichen fehl, nacheinander.

> [AZURE.NOTE] Wir empfehlen nur Bereitstellungskonfigurationen mit einem einzelnen DR-Bereich. Dies ist die meisten Azure Regionen zwei Bereiche haben. Diese Konfigurationen schützt Ihre Anwendung nicht vor Katastrophenfall beide Bereiche. In einer unwahrscheinlichen Fall eines Ausfalls können Sie Ihre Datenbanken [Geo - Wiederherstellungsvorgang](sql-database-disaster-recovery.md#recovery-using-geo-restore)dritten Gebiet wiederherstellen.

Sobald die Ausfallzeit verringert ist die sekundäre Datenbank automatisch mit der synchronisiert. Während der Synchronisierung konnte Leistung der primären etwas abhängig von der Datenmenge belastet, die synchronisiert werden soll. Das folgende Diagramm zeigt einen Ausfall in der sekundären Region.

![Sekundäre Datenbank synchronisiert mit. Cloud-Wiederherstellung.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


**Vorteile** dieses sind:

+ Die SQL-Verbindungszeichenfolge während der anwendungsbereitstellung in jeder Region festgelegt und ändert sich nach einem Failover.
+ Die Leistung der Anwendung nicht durch Failover der Anwendung beeinträchtigt und die Datenbank immer am selben Standort.

Wichtigste **Nachteil** ist, dass redundante Anwendungsinstanz in der sekundären Region nur für die Wiederherstellung verwendet wird.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Entwurfsmuster 2: aktive Bereitstellung Anwendung Lastenausgleich
Diese Cloud Disaster Recovery-Option eignet sich am besten für Applikationen mit folgenden Merkmalen:

+ Hohes Verhältnis von Lese-zu Schreibvorgängen Datenbank
+ Latenz beim Schreiben der Datenbank beeinträchtigt nicht den Endbenutzer  
+ Schreibgeschützte Logik kann schreibgeschützte Logik getrennt werden über eine andere Verbindungszeichenfolge
+ Schreibgeschützte Logik ist nicht vollständig mit den neuesten Updates synchronisiert Daten abhängig  

Wenn Ihre Anwendung diese Merkmale aufweisen, kann Lastenausgleich Endbenutzer Verbindungen zwischen mehreren Anwendungsinstanzen in unterschiedlichen Regionen Leistung und durch den Endbenutzer verbessern. Um einen Lastenausgleich zu implementieren, müssen jede Region eine aktive Instanz der Anwendung mit der verbunden mit der primären Datenbank in der primären schreibgeschützten (RW). Schreibgeschützt (RO) Logik sollte in eine sekundäre Datenbank im Bereich der Anwendungsinstanz verbunden sein. Traffic Manager sollte [Roundrobin - routing](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) oder [Leistung routing](../traffic-manager/traffic-manager-configure-performance-routing-method.md) [Endpunkt Überwachung](../traffic-manager/traffic-manager-monitoring.md) für jede Anwendungsinstanz aktiviert mit eingerichtet werden.

Wie Muster #1 sollten Sie eine ähnliche Überwachung Anwendung bereitstellen. Anders als Muster #1 Überwachung Anwendung wird jedoch nicht das Endpunkt Failover ausgelöst.

> [AZURE.NOTE] Während dieses Muster mehrere sekundäre Datenbank verwendet wird, würde nur einer sekundären Instanzen für Failover oben aufgeführten Gründen verwendet. Da dieses Muster nur-Lese-Zugriff auf die sekundäre erfordert, muss Active Geo-Replikation.

Traffic Manager sollte Leistung Zugriffe auf die Ihrem Standort am nächsten Anwendungsinstanz direkten routing konfiguriert werden. Das folgende Diagramm zeigt diese Konfiguration vor Ausfällen.
![Keine Ausfallzeiten: Leistung routing zum nächsten Anwendung. Geo-Replikation.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Wenn in der primären Datenbankausfall erkannt wird, initiiert Sie Failover der primären Datenbank eines sekundären Regionen Ändern des Speicherortes der primären Datenbank. Traffic Manager automatisch die routing-Tabelle der Offlinestatus verhindert, aber weiterhin routing des Datenverkehrs Endbenutzer an die verbleibenden Online-Instanzen. Da die primäre Datenbank jetzt in einem anderen Bereich ist, müssen alle Online-Instanzen ihre Schreib-/ SQL-Verbindungszeichenfolge für die Verbindung mit dem neuen primären ändern. Es ist wichtig, dass Sie diese Änderung vor das datenbankfailover initiiert. Schreibgeschützte SQL-Verbindungszeichenfolgen sollte unverändert, da sie immer mit der Datenbank in derselben Region zeigen. Die Failover-Schritte sind:  

1. Ändern Sie schreibgeschützte SQL-Verbindungszeichenfolgen auf neue primäre.
2. Rufen Sie die vorgesehene sekundäre Datenbank zu [initiieren datenbankfailover](sql-database-geo-replication-portal.md).

Das folgende Diagramm veranschaulicht die neue Konfiguration nach dem Failover.
![Konfiguration nach einem Failover. Cloud-Wiederherstellung.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

Bei einem Ausfall eines sekundären Regionen entfernt Traffic Manager automatisch den Offlinestatus in diesem Bereich aus der Routingtabelle. Replikation Kanal auf die sekundäre Datenbank in diesem Bereich ausgesetzt. Da die übrigen Bereiche zusätzliche Datenverkehr in diesem Szenario werden, wird die Leistung der Anwendung während des Ausfalls beeinträchtigt. Sobald die Ausfallzeit verringert ist die sekundäre Datenbank in der betroffenen Region unmittelbar mit der synchronisiert. Während der Synchronisierung konnte Leistung der primären etwas abhängig von der Datenmenge belastet, die synchronisiert werden soll. Das folgende Diagramm zeigt einen Ausfall eines sekundären Regionen.

![Ausfall im zweiten Bereich. Cloud-Disaster Recovery - Geo-Replikation.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Der größte **Vorteil** dieses Entwurfsmusters ist, dass Skalieren der Arbeitslast der Anwendung über mehrere sekundäre eine optimale endbenutzerleistung zu erzielen. Die **Nachteile** dieser Option sind:

+ Schreibgeschützte Verbindung zwischen Instanzen und Datenbank haben unterschiedliche Wartezeiten und Kosten
+ Während des Ausfalls wird Leistung der Anwendung beeinträchtigt.
+ Anwendungsinstanzen werden die SQL-Verbindungszeichenfolge nach datenbankfailover dynamisch ändern.  

> [AZURE.NOTE] Ein ähnlicher Ansatz kann verwendet werden, spezielle Arbeitslasten wie Aufträge, Business Intelligence-Tools oder Backups abnimmt. Diese Arbeitslasten beanspruchen normalerweise erhebliche Ressourcen wird daher empfohlen, eine sekundären Datenbanken für sie mit der Leistung die erwartete Arbeitslast zugeordnet bestimmen.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Entwurfsmuster 3: Aktiv / Passiv-Bereitstellung für die Beibehaltung der Daten  
Diese Option eignet sich am besten für Applikationen mit folgenden Merkmalen:

+ Datenverlust ist hohe geschäftliche Risiken. Datenbankfailover kann nur als letztes Mittel verwendet werden, wenn der Ausfall dauerhaft ist.
+ Die Anwendung kann in "schreibgeschützt" für einen Zeitraum ausgeführt werden.

In diesem Muster wechselt die Anwendung in schreibgeschützten Modus mit der sekundären Datenbank verbunden. Die Anwendungslogik in der primären Region ist mit der primären Datenbank und im schreibgeschützten Modus (RW). Die Anwendungslogik im zweiten Bereich wird mit der sekundären Datenbank und kann im schreibgeschützten Modus (RO).  Traffic Manager sollte [Failover routing](../traffic-manager/traffic-manager-configure-failover-routing-method.md) [Endpunkt Überwachung](../traffic-manager/traffic-manager-monitoring.md) aktiviert für beide Instanzen mit eingerichtet werden.

Das folgende Diagramm zeigt diese Konfiguration vor Ausfällen.
![Aktiv / Passiv-Bereitstellung vor dem Failover. Cloud-Wiederherstellung.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Traffic Manager ein Verbindungsfehler primäre Region erkennt, wird automatisch Benutzern für die Anwendungsinstanz im zweiten Bereich. Mit diesem Muster ist es wichtig, dass Sie **nicht** datenbankfailover initiiert, nachdem der Ausfall erkannt wird. Anwendung im zweiten Bereich wird aktiviert und wird im schreibgeschützten Modus der sekundären Datenbank. Dies wird im folgenden Diagramm dargestellt.

![Ausfall: Anwendung schreibgeschützt. Cloud-Wiederherstellung.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Nach die Ausfallzeit im Bereich für primäre verringert Traffic Manager erkennt die Wiederherstellung der Verbindung im Bereich für primäre und Benutzern für die Anwendungsinstanz primäre Region wechselt. Diese Anwendungsinstanz fortsetzt und arbeitet im Lese-/ Schreibmodus mit der primären Datenbank.

> [AZURE.NOTE] Da dieses Muster nur-Lese-Zugriff auf die sekundäre erfordert muss aktive Geo-Replikation.

Bei einem Ausfall in der sekundären Region Traffic Manager kennzeichnet Anwendung Endpunkt im Bereich für primäre herabgesetzt und Channel Replikation unterbrochen. Ausfall beeinflusst jedoch nicht die Leistung der Anwendung während des Ausfalls. Sobald die Ausfallzeit verringert ist die sekundäre Datenbank sofort mit der synchronisiert. Während der Synchronisierung konnte Leistung der primären etwas abhängig von der Datenmenge belastet, die synchronisiert werden soll.

![Ausfall: Sekundäre Datenbank. Cloud-Wiederherstellung.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Dieses hat mehrere **Vorteile**:

+ Verhindert Datenverlust bei temporären Ausfällen.
+ Es erfordert keine Überwachung Anwendung bereitstellen, als die Wiederherstellung von Traffic Manager ausgelöst wird.
+ Ausfallzeiten hängt wie schnell Traffic Manager der Verbindungsfehler erkennt konfiguriert ist.

Die **Nachteile** sind:

+ Anwendung muss im schreibgeschützten Modus ausgeführt werden können.
+ Es ist erforderlich, dynamisch zu wechseln, wenn sie mit einer schreibgeschützten Datenbank verbunden ist.
+ Wiederaufnahme der Lese-/ Schreibvorgänge erfordert Wiederherstellung des primären Region, d. Datenzugriff h. für Stunden oder sogar Tage möglicherweise deaktiviert.

> [AZURE.NOTE] Bei einer dauerhaften Dienstausfall im Bereich müssen manuell datenbankfailover zu aktivieren und den Datenverlust akzeptieren. Die Anwendung wird im zweiten Bereich mit Lese-/ Schreibzugriff auf die Datenbank funktionsfähig.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Business Continuity-Planung: Wählen Sie ein Anwendungsdesign Cloud Disaster Recovery

Bestimmte Cloud-Wiederherstellungsstrategie kann kombiniert oder erweitert diese Entwurfsmuster am besten Bedürfnisse Ihrer Anwendung.  Wie bereits erwähnt, basiert die gewählten Strategie auf SLA Kunden und die Bereitstellungstopologie Anwendung anbieten möchten. Um die Entscheidung, in der folgenden Tabelle vergleicht Auswahlmöglichkeiten basierend auf der erwarteten Datenverlust oder Recovery point Objective (RPO) und geschätzte Wiederherstellungszeit (Einfügen).

| Muster | RPO | EINFÜGEN
| :--- |:--- | :---
| Aktiv / Passiv-Bereitstellung für Disaster Recovery mit zusammengestellten Datenbankzugriff | Lese-/ Schreibzugriff < 5 Sek. | Fehler-Erkennungszeit + Failover-API Aufrufen anwendungsüberprüfung testen
| Aktive Bereitstellung Anwendung Lastenausgleich | Lese-/ Schreibzugriff < 5 Sek. | Fehler-Erkennungszeit Failover-API-Aufruf SQL-Verbindungszeichenfolge ändern + anwendungsüberprüfung testen
| Aktiv / Passiv-Bereitstellung für die Beibehaltung der Daten | Nur-Lese-Zugriff < 5 Sek. Schreibzugriff = null | Lesezugriff = Konnektivität Fehler Erkennungszeit + Anwendung Überprüfungstests <br>Lese-/ Schreibzugriff = Zeit, um die Ausfallzeit zu minimieren

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte backups](sql-database-automated-backups.md)
- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
- Automatisierte Backups Recovery mit finden Sie unter [Wiederherstellen einer Datenbank aus den Sicherungskopien Dienst initiiert](sql-database-recovery-using-backups.md)
- Schnellere Recovery-Optionen finden Sie unter [Aktive geografische Replikation](sql-database-geo-replication-overview.md)  
- Archivierung mit automatischen Sicherungen finden Sie unter [Datenbank kopieren](sql-database-copy.md)
- Informationen über aktive Geo-Replikation mit elastische finden Sie [elastischen Pool Disaster Recovery-Strategien](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
