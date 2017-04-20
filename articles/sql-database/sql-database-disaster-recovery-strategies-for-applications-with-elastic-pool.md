<properties 
   pageTitle="Cloud-Lösungen für Disaster Recovery mit SQL Geo-Replikation | Microsoft Azure"
   description="Informationen Sie zum Cloud-Lösung für Disaster Recovery mit der richtigen Failover-Muster entwerfen."
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
   ms.workload="NA" 
   ms.date="07/16/2016"
   ms.author="sashan"/>

# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pool"></a>Disaster Recovery-Strategien für Applikationen mit SQL Datenbank elastischen Pool 

Jahren haben wir gelernt, dass Cloud-Dienste nicht narrensicher und schwerwiegenden Vorfälle können und geschieht. SQL-Datenbank bietet eine Reihe von Funktionen für Business Continuity Ihrer Anwendung bereitzustellen, treten diese Ereignisse. [Elastische Pools](sql-database-elastic-pool.md) und eigenständige Datenbanken unterstützen dieselben Disaster Recovery-Funktionen. Dieser Artikel beschreibt mehrere Disaster Recovery-Strategien für elastischen pools nutzen diese SQL Datenbank Business Continuity-Funktionen.

Für die Zwecke dieses Artikels wird das kanonische SaaS ISV-Anwendungsmuster verwendet:

<i>Eine moderne Cloud basierten Web Application Vorschriften eine SQL-Datenbank für jeden Benutzer. ISV hat eine große Anzahl von Kunden und nutzt viele Datenbanken als Mieter Datenbanken bezeichnet. Da Datenbanken Mieter normalerweise unvorhersehbare Verhaltensmuster aufweisen, mithilfe der ISV elastischen Pool Kosten Datenbank zuverlässige über längere Zeit. Elastische Pool vereinfacht auch das Performance-Management bei der Benutzeraktivität Spitzen. Neben den Mieter Datenbanken verwendet die Anwendung auch mehrere Datenbanken verwalten von Benutzerprofilen, Sicherheit, Verwendungsmuster usw. sammeln. Verfügbarkeit der einzelnen Mandanten beeinträchtigt nicht die Verfügbarkeit als Ganzes. Allerdings ist die Verfügbarkeit und Leistung der Verwaltungsdatenbank für die Anwendung und Management Datenbanken offline die gesamte Anwendung ist offline.</i>  

Im Rest des Papiers besprechen wir die DR Strategien für Szenarien von Kosten vertrauliche Autostart-Programme, die mit hohen Verfügbarkeit.  

## <a name="scenario-1-cost-sensitive-startup"></a>Szenario 1. Kosten vertrauliche starten

<i>Ich bin ein Startup-Unternehmen und bin sehr vertrauliche Kosten.  Vereinfachung von Bereitstellung und Verwaltung der Anwendung möchte, und ich bin bereit, eine begrenzte SLA für einzelne Debitoren. Aber ich soll die Anwendung nie offline ist.</i>

Zum Erfüllen der Anforderung Einfachheit alle Mieter Datenbanken in einem elastischen Pool in der Azure-Region Ihrer Wahl bereitzustellen und Management Datenbanken als eigenständige Geo repliziert Datenbanken bereitstellen. Verwenden Sie für Wiederherstellung Mieter Geo-Wiederherstellung, ohne zusätzliche Kosten enthalten. Zur Sicherstellung der Verfügbarkeit von Management-Datenbanken sollten geografisch anderen Region (Schritt 1) repliziert sind. Die laufende Kosten von Disaster Recovery-Konfiguration in diesem Szenario entspricht die Gesamtkosten der sekundären Datenbank. Diese Konfiguration wird im folgenden Diagramm dargestellt.

![Abbildung 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Bei einem Ausfall der primären Region werden Wiederherstellungsschritte Online-Bewerbung zu nächsten Diagramm veranschaulicht.

- Sofort-Datenbanken Failover der Verwaltung (2) DR-Bereich. 
- Ändern der Verbindungszeichenfolge auf DR-Bereich der Anwendung. Alle neuen Konten und Mieter Datenbanken erstellt im Bereich Disaster Recovery. Bestehenden Kunden sehen ihre Daten vorübergehend nicht verfügbar.
- Elastischen Pool mit derselben Konfiguration wie das ursprüngliche Pool (3) erstellen. 
- Verwenden Sie Geo-Wiederherstellung zur Erstellung von Kopien der Mieter Datenbanken (4). Sie sollten Sie einzelne Wiederherstellung durch Endbenutzer Verbindungen auslösen oder einige andere Anwendung Priorität verwendet.

Die Anwendung ist wieder online im Bereich Disaster Recovery, aber einige Kunden werden Verzögerung beim Zugriff auf ihre Daten.

![Abbildung 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Der Ausfall temporäre war, ist möglich, dass die primäre Region von Azure vor der Wiederherstellung vollständig im Bereich Disaster Recovery wiederhergestellt werden. In diesem Fall sollten Sie orchestrieren, die Anwendung in den primären Bereich verschieben. Der Vorgang dauert die Schritte im nächsten Diagramm dargestellt.
 
- Alle ausstehenden Geo-Restore-Anfragen Abbrechen   
- Failover Management Datenbanken primäre Region (5). Hinweis: Nach der Region Wiederherstellung die alten Primärfarben automatisch sekundäre geworden. Jetzt werden sie Rollen aufgefordert. 
- Ändern der Verbindungszeichenfolge zurück auf den primären Bereich der Anwendung. Jetzt werden alle neuen Konten und Mieter Datenbanken im Bereich für primäre erstellt werden. Einige Kunden sehen ihre Daten vorübergehend nicht verfügbar.   
- Legen Sie alle Datenbanken im Pool DR schreibgeschützt, um sicherzustellen, dass sie im Bereich Disaster Recovery (6) geändert werden können. 
- Für jede Datenbank in der DR-Pool, der nach der Wiederherstellung geändert hat, umbenennen oder Löschen der entsprechenden Datenbanken im primären Pool (7). 
- Kopieren der aktualisierten Datenbanken aus dem DR primären Pool (8). 
- Löschen des DR-Pools (9)

Die Anwendung wird jetzt der primäre Region alle Mieter Datenbanken im primären Pool online sein.

![Abbildung 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Die wichtigsten **Vorteile** dieser Strategie ist laufenden kostengünstig Tier Datenredundanz. Backups werden automatisch von der SQL-Datenbankdienst keine Anwendung schreiben und ohne zusätzliche Kosten.  Die Kosten fallen nur bei elastischen Datenbanken wiederhergestellt werden. Der **Kompromiss** ist die vollständige Wiederherstellung aller Datenbanken Mieter erhebliche Zeit in Anspruch nehmen. Es hängt von der Summe der Wiederherstellung im Bereich Disaster Recovery initiieren und Gesamtgröße der Tenant-Datenbanken. Auch wenn einige Mieter stellt gegenüber anderen priorisieren werden alle Wiederherstellungsvorgänge konkurrieren, die im selben Bereich wie der Dienst vermitteln und minimieren die gesamte Auswirkung auf die vorhandenen Debitoren Datenbanken drosseln initiiert werden. Darüber hinaus darf nicht die Wiederherstellung von Datenbanken Mieter erstellt neue elastische Pool im Bereich Disaster Recovery beginnen.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Szenario 2. Ältere Anwendung mit tiered-service 

<i>Ich bin ein ausgereiftes SaaS-Anwendung tiered Service-Angebote mit unterschiedlichen SLAs für Testkunden und Kunden. Der Testkunden habe möglichst Kosten zu senken. Testkunden können Ausfallzeiten aber die Wahrscheinlichkeit reduziert werden soll. Zahlenden Kunden besteht Ausfallzeiten Flug. Also zu dieser Zahlen können Kunden immer auf ihre Daten zugreifen.</i> 

Zur Unterstützung dieses Szenarios muss die Testversion Mieter Inbetriebnahme separate elastische Pools bezahlte Mandanten eingehalten werden. Die Testkunden müsste niedriger eDTU Mieter und niedrigere SLA mit einer längere Wiederherstellungszeit. Zahlenden Kunden wäre in einem Pool mit höheren eDTU Mieter und eine höhere SLA. Gewährleistung die niedrigste Wiederherstellungszeit sollte zahlenden Kunden Mieter Datenbanken Geo repliziert. Diese Konfiguration wird im folgenden Diagramm dargestellt. 

![Abbildung 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Im ersten Szenario werden Management Datenbanken aktiv eine eigenständigen Geo replizierten Datenbank für (1) verwendet. Dadurch Prognostizierbare Performance für Abonnements von Neukunden, Profil Updates und andere Verwaltungsvorgänge. Bereich in dem primäre Management-Datenbanken befinden wird die primäre Region und Region in der sekundäre Management Datenbanken befinden DR-Bereich.

Die zahlenden Kunden Mieter Datenbanken haben aktive Datenbanken "bezahlt" Pool in der primären Region bereitgestellt. Sie sollten einen sekundären Pool mit demselben Namen im Bereich Disaster Recovery bereitstellen. Jeder würde Geo sekundären Pool (2) repliziert werden. Dies ermöglicht eine schnelle Wiederherstellung aller Mieter Datenbanken mit Failover. 

Ein Ausfall der primären Region werden Wiederherstellungsschritte zu Online-Bewerbung im nächsten Diagramm veranschaulicht:

![Abbildung 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

- Sofort ein Failover Management Datenbanken DR-Bereich (3).
- Ändern Sie Verbindungszeichenfolge der Anwendung auf den DR-Bereich. Jetzt werden alle neuen Konten und Mieter Datenbanken im Bereich Disaster Recovery erstellt werden. Vorhandenen Testkunden sehen ihre Daten vorübergehend nicht verfügbar.
- Failover bezahlte Mieter Datenbanken zum Pool im Bereich Disaster Recovery ihrer Verfügbarkeit (4) sofort wiederherstellen. Das Failover ist eine schnelle Metadatenänderung sollten Sie eine Optimierung, einzelne Failover bei Bedarf die benutzerverbindungen auslöst. 
- Wurde die sekundären eDTU Größe da sekundären Datenbanken nur die Kapazität benötigt zu Änderungsprotokolle sie sekundäre waren niedriger als die primäre, sollten Sie sofort die poolkapazität jetzt Auslastung aller Mandanten (5) erhöhen. 
- Erstellen Sie neuen elastischen Pool mit demselben Namen und derselben Konfiguration im Bereich Disaster Recovery der Testkunden Datenbanken (6). 
- Erstellte der Testkunden Pool verwenden Sie Geo-Wiederherstellung zur Wiederherstellung der einzelnen Test Mieter Datenbanken in den neuen Pool (7). Sie sollten Sie einzelne Wiederherstellung durch Endbenutzer Verbindungen auslösen oder einige andere Anwendung Priorität verwendet.

Zu diesem Zeitpunkt ist die Anwendung im Bereich Disaster Recovery wieder online. Alle zahlende Kunden haben Zugriff auf ihre Daten während die Testkunden beim Zugriff auf ihre Daten Verzögerung werden.

Wenn die primäre Region wiederhergestellt können von Azure *nach* wurden wiederhergestellt, die Anwendung im Bereich Disaster Recovery läuft in diesem Bereich fortgesetzt werden kann oder Sie die primäre Region nicht. Wiederhergestellte *vor* ist die primäre Region ist Failover-Prozess andernfalls zurück sofort sollten abgeschlossen. Das Failback wird in der nächsten Abbildung dargestellten Schritte ausführen: 
 
![Abbildung 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

- Alle ausstehenden Geo-Restore-Anfragen Abbrechen   
- Failover Management Datenbanken (8). Nachdem die Region Wiederherstellung des alten primären automatisch den sekundären geworden. Jetzt wird der primäre erneut.  
- Failover bezahlte Mieter Datenbanken (9). Nach Wiederherstellung der Region werden die alten Grundfarben entsprechend automatisch die sekundäre. Nun werden sie die Grundfarben erneut. 
- Legen Sie Testversionen wiederhergestellten Datenbanken, die in der Region DR schreibgeschützt (10) geändert wurden.
- Für jede Datenbank im Pool Testkunden DR, die nach der Wiederherstellung geändert, umbenennen oder Löschen der entsprechenden Datenbank Testkunden primären Pool (11). 
- Kopieren der aktualisierten Datenbanken aus dem DR primären Pool (12). 
- Löschen des DR-Pools (13) 

> [AZURE.NOTE] Das Failover ist asynchron. Minimierung die Recovery-Zeit ist es wichtig, die Mieter Datenbanken Failover Befehl Stapel von 20 Datenbanken ausführen. 

Die wichtigsten **Vorteile** dieser Strategie ist, dass es die höchste SLA für die zahlenden Kunden. Es wird sichergestellt, dass neue Versuche aufgehoben werden, als Testversion DR-Pool erstellt wird. Der **Kompromiss** ist Setup die Gesamtkosten der Tenant-Datenbanken des Einstandspreises des sekundären DR-Pools für Kunden. Darüber hinaus verfügt der sekundäre Pool eine andere Größe, treten zahlenden Kunden geringere Leistung nach einem Failover bis Abschluss der Aktualisierung Pool im Bereich Disaster Recovery. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Szenario 3. Geografisch verteilte Anwendung tiered Service

<i>Ich habe eine ausgereifte SaaS-Anwendung mit tiered Service-Angebote. Ich möchte meine Kunden sehr aggressive SLA anbieten und das Risiko der Auswirkung bei Ausfällen, da auch kurzer Unterbrechung Unzufriedenheit führen kann. Es ist wichtig, dass zahlenden Kunden stets auf ihre Daten zugreifen können. Die Versuche sind und eine SLA wird während des Testzeitraums nicht angeboten.</i> 

Zur Unterstützung dieses Szenarios müssen Sie drei separate elastische Pools. Zwei gleich große Pools mit hoher eDTUs pro Datenbank sollte in zwei verschiedenen Regionen bezahlte Kunden Mieter Datenbanken bereitgestellt werden. Dritte Pool mit der Testversion Mieter hätte eine niedrigere eDTUs pro Datenbank und in einer der beiden bereitgestellt werden.

Gewährleistung der niedrigsten Wiederherstellungszeit bei Ausfällen der zahlenden Debitoren Mieter sollte Datenbanken mit 50 % der primären Datenbank in jeder der beiden Regionen Geo repliziert. Ebenso hätte jeder Region 50 % der sekundären Datenbanken. So ist eine Region offline nur 50 % der bezahlten Kunden Datenbanken betroffen sein würden und Failover müssen. Die anderen Datenbanken würden unverändert. Diese Konfiguration wird im folgenden Diagramm dargestellt:

![Abbildung 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

In früheren Szenarien werden Management Datenbanken aktiv Sie als eigenständige Geo replizierten Datenbanken (1) konfiguriert werden soll. Dadurch vorhersagbare Performance neue Kundenabonnements Profil Updates und andere Verwaltungsvorgänge. Bereich A wäre die primäre Region für die Datenbank(en) Management und Region B für Recovery Management-Datenbanken verwendet werden.

Die zahlenden Kunden Mieter Datenbanken werden auch geografisch repliziert mit Primärfarben und sekundäre Region A und B (2) Bereich aufgeteilt. Auf diese Weise der Ausfall der primären Datenbank Mieter beeinflusst kann Failover auf den anderen Bereich und verfügbar. Die andere Hälfte der Tenant-Datenbanken wird überhaupt nicht beeinträchtigt. 

Im nächste Diagramm veranschaulicht die Schritte zur Wiederherstellung bei einem Ausfall im Bereich a

![Abbildung 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

- Sofort ein Failover Management Datenbanken auf Bereich B (3).
- Ändern Sie Verbindungszeichenfolge Datenbank(en) Management im Bereich b ändern auf Datenbanken neue Konten und Mieter Datenbanken im Bereich B erstellt und Mieter Datenbanken finden sich auch die Verwaltung der Anwendung. Vorhandenen Testkunden sehen ihre Daten vorübergehend nicht verfügbar.
- Failover Datenbanken bezahlte Mieter Pool 2 im Bereich B Verfügbarkeit (4) sofort wiederherstellen. Das Failover ist eine schnelle Metadatenänderung sollten Sie eine Optimierung, einzelne Failover bei Bedarf die benutzerverbindungen auslöst. 
- Pool 2 enthält jetzt nur primäre Datenbanken die gesamte Arbeitslast im Pool vergrößert wird, damit Sie sofort eDTU (5) vergrößern müssen. 
- Erstellen Sie neuen elastischen Pool mit demselben Namen und derselben Konfiguration im Bereich B der Testkunden Datenbanken (6). 
- Nach der Erstellung eines Pools mithilfe Geo-Wiederherstellung einzelner Test Mieter Datenbank im Pool (7) wiederherstellen. Sie sollten Sie einzelne Wiederherstellung durch Endbenutzer Verbindungen auslösen oder einige andere Anwendung Priorität verwendet.


> [AZURE.NOTE] Das Failover ist asynchron. Minimierung die Recovery-Zeit ist es wichtig, die Mieter Datenbanken Failover Befehl Stapel von 20 Datenbanken ausführen. 

Zu diesem Zeitpunkt ist die Anwendung im Bereich b wieder online Alle zahlende Kunden haben Zugriff auf ihre Daten während die Testkunden beim Zugriff auf ihre Daten Verzögerung werden.

Nach Region eine Wiederherstellung müssen Sie entscheiden, ob Sie mit Bereich B Testkunden oder Failback mit Testkunden Pool im Bereich a Ein Kriterium konnte % Testversion Mieter Datenbanken nach der Wiederherstellung geändert werden. Unabhängig von dieser Entscheidung müssen Sie bezahlt Mieter zwischen zwei Pools ausgleichen. Das folgende Diagramm veranschaulicht den Prozess, wenn die Testversion Mieter Datenbanken auf Bereich a nicht  
 
![Abbildung 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

- Alle ausstehenden Geo-Wiederherstellung Anfragen Testversion DR Pool aufheben   
- Failover-Verwaltungsdatenbank (8). Nach Wiederherstellung der Region wurde des alten primären automatisch der sekundären. Jetzt wird der primäre erneut.  
- Wählen Sie die bezahlte Mieter Datenbanken zum Pool 1 und initiieren Failover auf die sekundäre (9) fehl. Alle Datenbanken im Pool 1 wurde nach der Region Wiederherstellung automatisch sekundäre. Jetzt werden 50 % Primärfarben erneut. 
- Verkleinern Sie-Pool 2 zu den ursprünglichen eDTU (10).
- Alle wiederhergestellt Test-Datenbanken im Bereich B auf schreibgeschützt (11).
- Für jede Datenbank in der Testversion DR Pool, der nach der Wiederherstellung geändert hat umbenennen oder Löschen der entsprechenden Datenbank Testversion primären Pool (12). 
- Kopieren der aktualisierten Datenbanken aus dem DR primären Pool (13). 
- Löschen des DR-Pools (14) 

Die wichtigsten **Vorteile** dieser Strategie sind:

- Es unterstützt aggressivste SLA zahlenden Kunden, da sie sicherstellt, dass ein Ausfall nicht mehr als 50 % der Mieter Datenbanken beeinträchtigen kann. 
- Es garantiert, dass neue Versuche freigegeben als Trail DR Pool während der Wiederherstellung erstellt wird. 
- Es ermöglicht eine effizientere Nutzung der poolkapazität 50 % der sekundären Datenbanken in 1 und 2 garantiert weniger aktiv und der primären Datenbank.

Die wichtigsten **Nachteile** sind:

- CRUD-Vorgänge Management Datenbanken haben geringere Latenz für Endbenutzer verbunden Region A als Endbenutzer Bereich B verbunden, wie sie primäre Management-Datenbanken ausgeführt werden.
- Komplexeren Entwurf der Datenbank benötigt. Jeder Datensatz Mieter müsste beispielsweise ein Speicherorttag haben, während Failover und Failback geändert werden.  
- Zahlenden Kunden möglicherweise geringere Leistung als gewohnt bis zum Abschluss der Aktualisierung Pool im Bereich B. 

## <a name="summary"></a>Zusammenfassung

Dieser Artikel konzentriert sich auf Disaster Recovery-Strategien für die Datenbankebene eine SaaS ISV Multi-Tenant-Anwendung. Die gewählten Strategie muss die Anwendung wie das Geschäftsmodell SLA beruhen zu Ihren Kunden anbieten, budget Einschränkung usw... Jede beschriebene Strategie skizziert die vor- und Kompromiss, damit eine fundierte Entscheidung treffen kann. Außerdem wird Ihre Anwendung wahrscheinlich andere Azure Komponenten enthalten. So überprüfen die Business Continuity-Anleitung und koordinieren die Wiederherstellung der Datenbankebene mit. Um weitere Informationen zum Verwalten der Wiederherstellung in Azure-Datenbank Anwendung finden Sie in [Designing Cloudlösungen für Disaster Recovery](./sql-database-designing-cloud-solutions-for-disaster-recovery.md) .  


## <a name="next-steps"></a>Nächste Schritte

- Informationen zu Azure SQL-Datenbank automatisierte Backups Siehe [SQL Datenbank automatisierte backups](sql-database-automated-backups.md)
- Eine Übersicht über Business Continuity und Szenarien finden Sie unter [Business Continuity – Überblick](sql-database-business-continuity.md)
- Automatisierte Backups Recovery mit finden Sie unter [Wiederherstellen einer Datenbank aus den Sicherungskopien Dienst initiiert](sql-database-recovery-using-backups.md)
- Schnellere Recovery-Optionen finden Sie unter [Aktive geografische Replikation](sql-database-geo-replication-overview.md)  
- Archivierung mit automatischen Sicherungen finden Sie unter [Datenbank kopieren](sql-database-copy.md)
