<properties
   pageTitle="Disaster Recovery für Azure Applications | Microsoft Azure"
   description="Technische Übersicht und ausführliche Informationen über das Entwerfen von Applikationen für Disaster Recovery für Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="disaster-recovery-for-applications-built-on-microsoft-azure"></a>Disaster Recovery bei Microsoft Azure

Hoher Verfügbarkeit zur Verwaltung von temporären Fehlers ist ist Disaster Recovery (DR) zu einem katastrophalen Verlust der Funktionalität. Betrachten Sie beispielsweise das Szenario, in dem eine Region ausfällt. In diesem Fall müssen Sie einen Plan zum Ausführen der Anwendung oder Ihre Daten außerhalb der Azure-Region. Die Ausführung dieses Plans umfasst Personen, Prozesse und unterstützende Programme, mit denen das System funktioniert. Die geschäftlichen und technologischen Besitzer das System Betriebsmodus für einen Notfall definieren bestimmen auch die Funktionalität für den Dienst im Katastrophenfall. Die Ebene der Funktionalität kann einige Formen annehmen: vollständig verfügbar, teilweise verfügbar Funktionen beeinträchtigt oder verzögert verarbeitet, oder vollständig verfügbar.

##<a name="azure-disaster-recovery-features"></a>Azure Disaster Recovery-Funktionen

Verfügbarkeit Aspekte ist Azure [Resiliency technische Anleitung](./resiliency-technical-guidance.md) , die um Disaster Recovery zu unterstützen. Gibt es auch eine Beziehung zwischen der Verfügbarkeit von Azure und Disaster Recovery. Die Verwaltung der verschiedenen Fehlerdomänen erhöht beispielsweise die Verfügbarkeit einer Anwendung. Ohne dieses Management wird eine nicht behandelte Hardwarefehler "Fehlerszenario". Daher ist die Anwendung von Verfügbarkeit und Strategien Bestandteil Disaster proofing Ihrer Anwendung. Allerdings geht diesem Artikel allgemeine Verfügbarkeitsprobleme zu schweren (und seltener) Katastrophe.

##<a name="multiple-datacenter-regions"></a>Mehrere Datacenter-Bereiche

Azure verwaltet Rechenzentren in vielen Regionen der Welt. Diese Infrastruktur unterstützt mehrere Disaster Recovery-Szenarien wie die vom System bereitgestellten Geo-Replikation der Azure-Speicher zum sekundären Regionen. Dies bedeutet auch, dass Sie einfach und kostengünstig einen Cloud-Dienst an mehreren Standorten auf der ganzen Welt bereitstellen können. Vergleichen Sie dies mit den Kosten und Probleme in mehreren Regionen eigene Rechenzentren ausgeführt. Bereitstellung von Daten und Diensten in mehreren Regionen schützt Ihre Anwendung bei großen Ausfällen in einer Region.

##<a name="azure-traffic-manager"></a>Azure Traffic Manager

Tritt ein Fehler regionsspezifische müssen Sie Dienste oder Bereitstellung in anderen Datenverkehr umleiten. Führen Sie diesen Arbeitsplan manuell, aber es ist effizienter, automatisiert. Azure Traffic Manager ist für diese Aufgabe entwickelt. Sie können um das Failover von Benutzern in einer anderen Region bei Ausfall die primäre Region automatisch zu verwalten. Ist Verkehrsmanagement Bestandteil der Gesamtstrategie, ist es wichtig, die Grundlagen von Traffic Manager.

Im folgenden Diagramm Verbindung mit einer URL, die für Traffic Manager (__http://myATMURL.trafficmanager.net__) und diese Auszüge aus der aktuellen Website-URLs (__http://app1URL.cloudapp.net__ und __http://app2URL.cloudapp.net__) angegeben wurde. Basierend auf die Konfiguration der Kriterien für an Benutzer weiterleiten, werden Benutzern auf die richtige aktuelle Website gesendet, wenn die Richtlinie schreibt vor. Die Optionen sind Round Robin, Leistung oder Failover. Für diesen Artikel werden wir nur die Option Failover kümmern.

![Routing über Azure Traffic Manager](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Wenn Sie Traffic Manager konfigurieren, geben Sie ein neues Traffic Manager DNS-Präfix. Dies ist das URL-Präfix, die Sie bieten den Benutzern auf Ihren Dienst zugreifen. Traffic Manager abstrahiert jetzt Lastenausgleich eine Ebene nach oben und nicht auf der Regionsebene. Traffic Manager DNS ordnet CNAME für alle verwalteten Installationen.

In Traffic Manager geben die Priorität der Benutzer weitergeleitet werden, tritt Fehler Bereitstellungen. Traffic Manager überwacht die Endpunkte der Bereitstellung und Notizen beim primäre Bereitstellung fehlschlägt. Bei einem Fehler Traffic Manager analysiert priorisierte Liste der Bereitstellung und leitet Benutzer zur nächsten in der Liste.

Obwohl Traffic Manager, wo ein Failover entscheidet, können Sie entscheiden, ob Ihre Domäne Failover ist inaktiv oder aktiv, während Sie nicht im Failover-Modus. Diese Funktion hat nichts mit Azure Traffic Manager. Traffic Manager erkennt Fehler am primären Standort und Failover-Standort übernommen. Traffic Manager wird unabhängig davon, ob diese Site derzeit Benutzer davon dient.

Weitere Informationen zur Funktionsweise von Azure Traffic Manager finden Sie in:

 * [Traffic Manager (Übersicht)](../traffic-manager/traffic-manager-overview.md)
 * [Konfigurieren Sie Failover routing](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##<a name="azure-disaster-scenarios"></a>Azure Notfällen

Den folgenden Abschnitten werden verschiedene Arten von Notfällen. Flächendeckende Ausfällen verursachen nicht nur Anwendung Fehler. Schlechter Entwurf oder Verwaltung Fehler können auch zu Ausfällen führen. Es ist wichtig, die möglichen Ursachen für Fehler beim Entwerfen und Testphasen Wiederherstellungsplans. Ein guter Plan nutzt Azure Funktionen und mit anwendungsspezifischen erweitert. Die ausgewählte Antwort richtet sich nach der Wichtigkeit der Anwendung Recovery Point Objective (RPO) und Recovery Time Objective (RTO).

###<a name="application-failure"></a>Anwendungsfehler

Azure Traffic Manager verwaltet automatisch Fehler, die aus der zugrunde liegenden Hardware- oder Software in virtuellen Hostcomputer. Azure erstellt eine neue Instanz der Rolle auf einem funktionstüchtigen Server und Lastenausgleich Drehung hinzugefügt. Die Anzahl der Instanzen größer als 1 ist, verschiebt Azure Verarbeitung den ausgeführten Rolleninstanzen beim Ersetzen des ausgefallenen Knotens.

Es gibt schwerwiegenden Anwendungsfehlern, die unabhängig von Hardware und Betriebssystem Fehler auftreten. Die Anwendung kann aufgrund von fehlerhaften Logik oder Datenintegritätsprobleme verursachten schwerwiegenden Ausnahmen nicht. Damit ein Überwachungssystem Fehler erkennen und einen benachrichtigen kann, müssen Sie genügend Telemetrie in den Code integrieren. Ein Administrator mit Kenntnis der Disaster Recovery-Prozesse kann Failover Prozessaufrufe entscheiden. Administrator akzeptiert auch einfach Ausfalls Verfügbarkeit um schwerwiegende Fehler zu beheben.

###<a name="data-corruption"></a>Beschädigte Daten

Azure speichert automatisch Ihre Azure SQL-Datenbank und Azure Storage dreimal redundant in anderen Fehlerdomänen im selben Bereich. Verwenden Sie Geo-Replikation werden die Daten drei weitere Male in einer anderen Region gespeichert. Wenn der Benutzer oder die Anwendung die Daten in die primäre Kopie beschädigt, repliziert die Daten schnell der Kopien. Leider führt dies drei Kopien der Daten beschädigt werden.

Um mögliche Beschädigung von Daten zu verwalten, haben Sie zwei Optionen. Erstens können Sie eine benutzerdefinierte Sicherungsstrategie verwalten. Sie können Ihre Backups in Azure oder lokal, je nach Unternehmen und Governance-Regeln speichern. Eine weitere Option ist die neue Point-in-Time-Wiederherstellung Option zum Wiederherstellen einer SQL-Datenbank. Weitere Informationen finden Sie im Abschnitt [Daten](#data-strategies-for-disaster-recovery)Strategien für die Wiederherstellung.

###<a name="network-outage"></a>Netzwerkausfall

Wenn Teile der Azure-Netzwerk zugegriffen werden kann Sie zu der Anwendung oder Daten möglicherweise nicht. Wenn eine oder mehrere Instanzen aufgrund von Netzwerkproblemen nicht verfügbar sind, verwendet Azure verbleibenden verfügbaren Instanzen der Anwendung. Wenn die Anwendung die Daten aufgrund einer Azure Netzwerkausfall zugreifen kann, können Sie möglicherweise in einem eingeschränkten Modus ausführen, mithilfe von zwischengespeicherten Daten lokal. Sie müssen die Disaster Recovery-Strategie für die Ausführung in einem eingeschränkten Modus in Ihrer Anwendung entwerfen. Bei einigen Applikationen praktische möglicherweise nicht.

Eine weitere Option ist Daten an einem alternativen Speicherort gespeichert, bis die Verbindung wiederhergestellt ist. Sparmodus nicht möglich ist, sind die übrigen Optionen Ausfallzeiten oder ein Failover auf einen anderen Bereich. Der Entwurf einer Anwendung in einem eingeschränkten Modus ist ebenso eine Entscheidung als technische. Dies wird beschrieben im Abschnitt [Funktionalität beeinträchtigt](#degraded-application-functionality).

###<a name="failure-of-a-dependent-service"></a>Ausfall eines abhängigen Dienstes

Azure bietet zahlreiche Dienste, die regelmäßigen Ausfallzeiten auftreten können. [Azure Redis Cache](https://azure.microsoft.com/services/cache/) angenommen. Dieser Multi-Tenant-Service bietet Zwischenspeicherfunktionen zur Anwendung. Es ist wichtig, was in der Anwendung, wenn der abhängige Dienst nicht verfügbar ist. In vielerlei Hinsicht ähnelt diesem Szenario den Ausfall. Jedoch jeden Dienst einzeln führt mögliche Verbesserung der Gesamtplan berücksichtigen.

Azure Redis Cache bietet Zwischenspeichern der Anwendung in der Cloud Service-Bereitstellung, die Disaster Recovery-Vorteile bietet. Zuerst wird der Dienst jetzt auf die Bereitstellung sind Rollen ausgeführt. Daher können Sie besser überwachen und verwalten Sie den Status des Caches als Teil der allgemeinen Verwaltungsprozesse für den Clouddienst. Diese Art des Zwischenspeicherns stellt neue Features. Eines der neuen Features ist eine hohe Verfügbarkeit für zwischengespeicherte Daten. Dadurch werden zwischengespeicherte Daten beibehalten, wenn ein Knoten ausfällt, indem doppelte Kopien auf anderen Knoten.

Beachten Sie, dass hohe Verfügbarkeit sinkt der Durchsatz und Wartezeit durch das Aktualisieren der sekundären Kopie für Schreibvorgänge erhöht. Auch verdoppelt die Größe des Arbeitsspeichers, der für jedes Element verwendet wird, also dafür. Dieses Beispiel veranschaulicht, dass jeder abhängiger Dienst Funktionen haben, die die allgemeine Verfügbarkeit und gegen schwer wiegenden Fehlern verbessern.

Mit jeder abhängigen Dienst sollten Sie die Implikationen einer Service-Unterbrechung verstehen. Im Cache wird möglicherweise auf die Daten direkt aus einer Datenbank, bis den Cache wiederherstellen. Dies würde Sparmodus Leistung, sondern vollständige Funktionalität Daten.

###<a name="region-wide-service-disruption"></a>Flächendeckende serviceunterbrechung

Der vorherige Fehler wurden hauptsächlich Fehler, die innerhalb desselben Bereichs Azure verwaltet werden können. Allerdings müssen Sie auch die Möglichkeit vorbereiten, der gesamten Region Dienst unterbrochen wird. Tritt eine langwierige flächendeckende sind lokal redundanten Datenkopien nicht verfügbar. Wenn Sie Geo-Replikation aktiviert haben, sind drei Kopien der Blobs und Tabellen in einer anderen Region. Microsoft verlorene Region erklärt, ordnet Azure alle DNS-Einträge in den Bereich Geo repliziert.

>[AZURE.NOTE] Beachten Sie, dass keine Kontrolle über diesen Prozess, und nur bei regionalen langwierige tritt. Deshalb müssen Sie auf andere anwendungsspezifische Sicherungsstrategien höchste Verfügbarkeit zu verlassen. Weitere Informationen finden Sie im Abschnitt [Daten](#data-strategies-for-disaster-recovery)Strategien für die Wiederherstellung.

###<a name="azure-wide-service-disruption"></a>Azure-weite Service-Unterbrechung

Im Notfall zu planen, müssen Sie den gesamten Bereich der möglichen Katastrophen berücksichtigen. Einer der schwerwiegendsten Ausfällen beinhaltet alle Azure Bereiche gleichzeitig. Wie bei anderen Ausfällen können Sie temporäre Ausfallrisiko in diesem Fall nehmen. Umfangreiche Ausfällen, die Bereiche umfassen sollte viel seltener als isolierte Ausfällen, die abhängigen Dienste oder einzelne Bereiche.

Jedoch bei einigen geschäftskritischen Applikationen empfiehlt, dass ein backup-Plan für dieses Szenario auch sein muss. Plan für dieses Ereignis enthalten möglicherweise in einer [anderen Wolke](#alternative-cloud) oder eine [hybride lokalen und cloud-Lösung](#hybrid-on-premises-and-cloud-solution)Failover.

###<a name="degraded-application-functionality"></a>Funktionalität beeinträchtigt

Eine gut entworfene Anwendung verwendet normalerweise eine Auflistung von Modulen, die über die Implementierung von lose gekoppelten Informationsaustausch Muster kommunizieren. Eine DR-Anwendung erfordert die Trennung von Aufgaben auf Modulebene. Dies ist die Unterbrechung eines abhängigen Dienstes verhindern Sie die gesamte Anwendung. Angenommen Sie, einer Web Commerce-Anwendung für Unternehmen. Folgende Module können die Anwendung zusammensetzt:

 * __Produktkatalog__ können Benutzer Produkte.
 * __Warenkorb__ können Benutzer hinzufügen und Entfernen von Produkten in ihrem Warenkorb.
 * __Bestellstatus__ zeigt den Versandstatus von Aufträgen für Benutzer.
 * __Übermittlung der Bestellung__ schließt die Einkaufszeit der Bestellung mit Zahlung.
 * __Die Reihenfolge für die Datenintegrität überprüft und führt eine Menge überprüfen.__

Wenn abhängig von einem Modul in dieser Anwendung ausfällt, wie funktioniert das Modul bis Teil wiederhergestellt? Eine gut entwickelte System implementiert Isolationsgrenzen hinweg durch Trennung von Aufgaben zur Entwurfszeit und zur Laufzeit. Sie können alle Fehler wiederherstellbaren und nicht wiederherstellbaren kategorisieren. Nicht behebbare Fehler nehmen Sie das Modul, aber einen behebbarer Fehler über Alternativen verringern. Wie beschrieben unter hoher Verfügbarkeit können Sie Probleme von Benutzern ausblenden, Fehlerbehandlung und andere Aktionen. Bei schwerwiegenderen Service-Ausfällen kann die Anwendung vollständig verfügbar. Eine dritte Option ist jedoch weiterhin bedienen Benutzer im heruntergestuften Modus.

Beispielsweise fällt die Datenbank zum Hosten von Aufträgen, verliert das Modul Auftragsabwicklung die Möglichkeit zum Verarbeiten von Transaktionen. Je nach Architektur kann es schwierig oder unmöglich für die Übermittlung der Bestellung und Auftragsabwicklung Teile der Anwendung sein. Wenn die Anwendung nicht mit diesem Szenario, kann die gesamte Anwendung offline gehen.

In dieses Szenario ist es jedoch möglich, dass die Daten an einem anderen Ort gespeichert ist. In diesem Fall das Produktkatalog Modul noch dienen zum Anzeigen von Produkten. Im eingeschränkten Modus weiterhin die Anwendung für Benutzer verfügbaren Funktionen wie den Produktkatalog anzeigen. Andere Teile der Anwendung sind jedoch nicht verfügbar, z. B. Sortierung oder Lagerbestand Abfragen.

Eine andere Variante der Sparmodus zentriert auf Leistung und Funktionen. Betrachten Sie beispielsweise ein Szenario, in dem Produktkatalog durch Azure Redis Cache zwischengespeichert wird. Zwischenspeichern verfügbar, vorkommen die Anwendung direkt an Serverspeicher Katalog Produktinformationen abzurufen. Dieser Zugriff kann jedoch langsamer als die zwischengespeicherte Version. Deshalb wird die Leistung der Anwendung beeinträchtigt bis Cache-Dienst wiederhergestellt ist.

Entscheiden, wie viel von einer Anwendung weiterhin im heruntergestuften Modus ist sowohl eine Entscheidung eine technische Entscheidung. Die Anwendung muss auch entscheiden, wie temporäre Probleme informieren. In diesem Beispiel können die Anwendung Produkte anzeigen und sogar einem Einkaufswagen hinzufügen. Wenn der Benutzer versucht, einen Kauf tätigen, benachrichtigt die Anwendung dem Benutzer ist Vertriebsmoduls vorübergehend nicht möglich. Ist ideal für Kunden, aber eine langwierige Anwendung verhindert.

##<a name="data-strategies-for-disaster-recovery"></a>Daten-Strategien für die Wiederherstellung

Umgang mit Daten korrekt ist der schwierigste richtig in Disaster Recovery-Plan. Wiederherstellen von Daten ist auch Teil des Wiederherstellungsprozesses dauert die Zeit. Auswahlmöglichkeiten in einem Leistungsabfall führen auftretenden Problemen für Daten-Recovery von Fehler und Konsistenz nach Fehler.

Einer der Faktoren muss wiederherstellen oder eine Kopie der Daten. Diese Daten verwenden Sie Referenz und transaktionale Zwecke an einem sekundären Standort. Eine lokale Einstellung erfordert eine teure und lange Planung eine Region mehrere Disaster Recovery-Strategie implementieren. Die cloud-Anbieter, einschließlich Azure, leicht ermöglichen die Bereitstellung von Applikationen auf mehrere Bereiche. Diese Bereiche sind so geografisch verteilt, Region mehrere langwierige selten sein sollte. Die Strategie für die Behandlung von Daten in Regionen ist einer der Faktoren für den Erfolg der Disaster Recovery-Plan.

Die folgenden Abschnitte erläutern Disaster Recovery Techniken Daten und Referenzdaten Transaktionsdaten.

###<a name="backup-and-restore"></a>Sicherung und Wiederherstellung

Sichern Sie Daten regelmäßig unterstützen einige Disaster Recovery-Szenarien. Verschiedene Ressourcen erfordern unterschiedliche Techniken.

Für den Stufen Basic, Standard und Premium SQL Datenbank können Sie Point-in-Time-Wiederherstellung zum Wiederherstellen der Datenbank nutzen. Weitere Informationen finden Sie unter [Übersicht: Cloud Business Continuity und Datenbank-Wiederherstellung mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md). Eine weitere Option ist die aktive Geo-Replikation für SQL-Datenbank verwenden. Dies repliziert Änderungen automatisch auf sekundäre Datenbanken in derselben Azure-Region oder sogar in einer anderen Azure-Region. Dies ist eine mögliche Alternative einiger manuellen Daten Synchronisation vorgestellten Techniken in diesem Artikel. Weitere Informationen finden Sie unter [Übersicht: SQL Datenbank aktive Geo-Replikation](../sql-database/sql-database-geo-replication-overview.md).

Sie können auch ein eher manuelles Verfahren für die Sicherung verwenden und wiederherstellen. Befehl Kopie der Datenbank eine Kopie der Datenbank erstellen. Diesen Befehl verwenden, eine Sicherung mit Transaktionskonsistenz. Sie können auch den Import-/Export-Dienst Azure SQL-Datenbank. Ausführende Datenbanken BACPAC Dateien in Azure BLOB-Speicher unterstützt.

Die integrierte Redundanz der Azure-Speicher erstellt zwei Replikate der Sicherungsdatei im selben Bereich. Die Häufigkeit der Ausführung des backup-Vorgangs bestimmt jedoch die RPO ist die Datenmenge, die in Notfällen verlieren. Angenommen Sie, Sie Sicherung am Anfang der Stunde, einem Notfall zwei Minuten vor der vollen Stunde. Verlieren Sie Daten, die nach der letzten Sicherung stattgefunden 58 Minuten. Außerdem sollten Sie zum Schutz vor einem regionalen langwierige BACPAC Dateien auf einen anderen Bereich kopieren. Sie können dann die Backups im anderen Bereich wiederherstellen. Weitere Informationen finden Sie unter [Übersicht: Cloud Business Continuity und Datenbank-Wiederherstellung mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md).

Azure Storage können Sie entwickeln eigener benutzerdefinierter Sicherungsvorgang oder gehen viele backup-Tools von Drittanbietern. Beachten Sie, dass die meisten Anwendungsentwürfen zusätzliche Komplexität, Speicherressourcen verweisen aufeinander. Angenommen Sie, eine SQL-Datenbank, die eine Spalte mit einem Link zu einem Blob in Azure Storage verfügt. Wenn die Sicherungskopien nicht gleichzeitig geschehen, möglicherweise die Datenbank auf ein Blob nicht vor Auftreten des Fehlers gesichert wurde. Anwendung oder Wiederherstellungsplan müssen Prozesse diese Inkonsistenz nach einer Behandlung.

###<a name="reference-data-pattern-for-disaster-recovery"></a>Verweis Datenmuster für Disaster recovery

Referenzdaten sind schreibgeschützte Daten, die Anwendung unterstützt. Es wird in der Regel nicht häufig geändert. Sicherung und Wiederherstellung ist eine Methode zum Behandeln von regionalen Ausfällen das RTO relativ lange. Beim Bereitstellen der Anwendung auf einen zweiten Bereich verbessern Strategien RTO für Daten.

Da Daten verweisen, können Sie das RTO verbessern, indem eine permanente Kopie der Referenzdaten in der sekundären. Dadurch wird die Zeit zur Wiederherstellung von Sicherungskopien im Katastrophenfall. Um die Region mehrere Disaster Recovery erfüllen müssen Sie die Anwendung und die Referenzdaten, die in mehreren Regionen zusammen bereitstellen. Wie erwähnt im [Verweis Datenmuster für hohe Verfügbarkeit](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability)können Sie die Rolle, externe Speicher oder eine Kombination beider Daten bereitstellen.

Verweis Bereitstellung Datenmodell in Compute-Knoten entspricht implizit Disaster Recovery an. Verweis Daten Bereitstellung SQL Datenbank erfordert, dass Sie eine Kopie der Daten für jede Region bereitstellen. Dieselbe Strategie gilt für Azure-Speicher. Primäre und sekundäre Regionen in Azure Storage gespeichert ist muss eine Kopie der Verweisdaten bereitgestellt werden.

![Daten-Publikation primäre und sekundäre Referenz](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

Implementieren Sie eine eigene anwendungsspezifische Sicherungsroutinen für alle Daten, einschließlich Daten. Geo-replizierten Kopien Regionen werden nur in regionalen serviceunterbrechung verwendet. Um längere Ausfallzeiten zu vermeiden, Bereitstellung wichtiger Teil der Anwendungsdaten sekundäre Region. Ein Beispiel für diese Topologie finden Sie unter [Aktiv / Passiv-Modell](#active-passive).

###<a name="transactional-data-pattern-for-disaster-recovery"></a>Transaktionsdaten Muster für Disaster recovery

Umsetzung einer voll funktionstüchtige Disaster-Modus erfordert asynchrone Replikation von Transaktionsdaten in den zweiten Bereich. In dem die Replikation können praktisch Zeitfenster bestimmt RPO-Merkmalen der Anwendung. Sie können die Daten wiederherstellen, die von der primären Region Zeitfenster Replikation. Sie können später mit dem zweiten Bereich Zusammenführen auch.

Die folgenden Architektur Beispiele für einige Ideen auf verschiedene Arten mit Transaktionsdaten in ein Failover-Szenario. Es ist wichtig zu beachten, dass diese Beispiele nicht vollständig sind. Z. B. möglicherweise temporären Speicherorte wie Warteschlangen mit Azure SQL-Datenbank ersetzt. Die Warteschlangen selbst möglicherweise Azure Storage oder Azure Service Bus Warteschlangen (siehe [Azure und Service Bus-Warteschlangen - verglichen und gegenübergestellt](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Server Storage Ziele auch variieren, wie Azure Tabellen statt SQL Datenbank. Darüber hinaus können Worker-Rollen als Vermittler in verschiedenen Schritten eingefügt werden. Das wichtigste ist nicht genau dieser Architekturen emulieren, sondern verschiedene alternativen bei der Wiederherstellung von Transaktionsdaten und zugehörige Module.

####<a name="replication-of-transactional-data-in-preparation-for-disaster-recovery"></a>Replikation von Transaktionsdaten in Vorbereitung für Disaster recovery

Eine Anwendung, Azure Storage Warteschlangen Transaktionsdaten zu berücksichtigen. Dadurch können Transaktionsdaten in der Serverdatenbank entkoppelte Architektur verarbeiten Worker-Rollen. Dies erfordert die Transaktionen verwenden einige temporäre Zwischenspeichern die Front-End-Rollen die sofortige Abfrage von Daten benötigen. Je nach Datenverlust Toleranz können Sie die Warteschlangen, die Datenbank oder alle der Speicherressourcen replizieren. Mit nur Datenbankreplikation, geht die primäre Region können Sie wiederherstellen die Daten in die Warteschlangen bei der primäre Bereich zurück.

Das folgende Diagramm zeigt eine Architektur, in die Serverdatenbank Regionen synchronisiert ist.

![Replikation von Transaktionsdaten in Vorbereitung für Disaster recovery](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

Die größte Herausforderung für die Implementierung dieser Architektur ist die Strategie für die Replikation zwischen. Azure SQL Data Sync-Dienst ermöglicht diese Art der Replikation. Allerdings wird der Dienst ist noch in der Vorschau und sollte nicht für die Produktion. Weitere Informationen finden Sie unter [Übersicht: Cloud Business Continuity und Datenbank-Wiederherstellung mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md). Für Produktion Applikationen müssen in eine Lösung investieren oder eigene Replikation Logik in Code erstellen. Je nach Architektur möglicherweise die Replikation bidirektional ist auch komplexer.

Einer möglichen Implementierung möglicherweise temporären Warteschlange im vorherigen Beispiel verwenden. Worker-Rolle, die die Daten zum letzten Ziel möglicherweise im Bereich für primäre und sekundäre Region ändern. Diese sind nicht trivialen Aufgaben und vollständige Anleitung zur replikationscode ist nicht Gegenstand dieses Artikels. Wichtig ist, dass viel Zeit und Tests sollte der Schwerpunkt auf wie die Daten an die sekundäre Region repliziert. Zusätzliche Verarbeitung und Tests können sicherstellen, dass das Failover und Recovery-Prozesse ordnungsgemäß möglichen Inkonsistenzen oder doppelte Transaktionen verarbeiten.

>[AZURE.NOTE] Die meisten der in diesem Dokument konzentriert sich auf Plattform als Service (PaaS). Replikation und Verfügbarkeit Zusatzoptionen für Hybrid Applications verwenden jedoch Azure Virtual Machines. Diese Hybrid Applications verwenden Infrastruktur als Service (IaaS) auf virtuellen Computern in Azure SQL Server hosten. Dadurch werden Verfügbarkeit Herkömmliche Ansätze in SQL Server AlwaysOn Availability Gruppen oder Protokollversand. Einige Techniken wie AlwaysOn, funktionieren nur zwischen lokalen SQL Server-Instanzen und Azure virtuelle Computer. Weitere Informationen finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

####<a name="degraded-application-mode-for-transaction-capture"></a>Nicht mehr redundantes Anwendungsmodus für Transaktion erfassen

Sollten Sie eine zweite Architektur arbeitet in einem eingeschränkten Modus. Anwendung auf die sekundäre Region deaktiviert alle Funktionen wie reporting Business Intelligence (BI) oder Warteschlangen ausgeglichen. Er akzeptiert nur die wichtigsten Arten von transactional Workflows festgelegten Anforderungen. Das System zeichnet die Transaktionen und schreibt sie in Warteschlangen. Das System kann der Datenverarbeitung in der Anfangsphase der serviceunterbrechung verschieben. Wenn das System auf die primäre Region erwartet Zeitfenster reaktiviert wird, können Worker-Rollen in der primären die Warteschlangen abarbeiten. Dadurch entfällt die Datenbank zusammenführen. Wenn die primäre Region langwierige Fenster tolerable überschreitet, kann die Anwendung gestartet Warteschlangen verarbeiten.

In diesem Szenario enthält die Datenbank auf dem sekundären Server inkrementelle Transaktionsdaten zusammengeführt werden müssen, nachdem die primäre reaktiviert wird. Das folgende Diagramm zeigt diese Transaktionsdaten vorübergehend gespeichert, bis die primäre Region wiederhergestellt.

![Nicht mehr redundantes Anwendungsmodus für Transaktion erfassen](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Weitere Erläuterung Datenmanagement-Techniken für robustes Azure Applications finden Sie unter [Failsafe: Hinweise zur Cloud-Architekturen sowie](https://channel9.msdn.com/Series/FailSafe).

##<a name="deployment-topologies-for-disaster-recovery"></a>Bereitstellungstopologien für Disaster recovery

Sie müssen geschäftskritischen Applikationen die Möglichkeit einer regionalen Service-Unterbrechung. Dazu Einbinden mehrerer Region Bereitstellungsstrategie in die operative Planung.

Region mehrere Bereitstellungen möglicherweise IT Pro Anwendung und Verweis Daten an die sekundäre Region nach einem Notfall veröffentlichen Prozesse umfassen. Erfordert die Anwendung sofort Failover möglicherweise Verfahren eine Aktiv/Passiv-Installation oder eine aktiv/aktiv-Installation umfassen. Diese Art der Bereitstellung hat vorhandene Instanzen der Anwendung im Bereich Alternative. Werkzeug wie Azure Traffic Manager bietet Dienstleistungen auf DNS-Lastenausgleich. Es erkennt Ausfällen und Benutzer in verschiedene Bereiche bei Bedarf weiterleiten.

Teil eines erfolgreichen Azure Disaster Recovery ist die Recovery der Lösung ab Architektur. Die Cloud bietet zusätzliche Optionen zur Wiederherstellung nach Fehlern bei einem Notfall in herkömmlichen Hostinganbieter nicht verfügbar sind. Insbesondere können Sie dynamisch und schnell zu einem anderen Bereich Ressourcen zuweisen. Daher wird nicht viel Leerlauf Ressourcen Bezahlung Wartezeit für einen Fehler.

Den folgenden Abschnitten werden verschiedene Bereitstellungstopologien für Disaster Recovery. Normalerweise besteht ein Kompromiss höhere Kosten und Komplexität für zusätzliche Verfügbarkeit.

###<a name="single-region-deployment"></a>Bereitstellung für einzelne region

Eine einzige Region Bereitstellung ist nicht wirklich ein Disaster Recovery-Topologie aber soll Vergleiche mit anderen Architekturen. Bereitstellung für einzelne Region gelten für Applikationen in Azure. Jedoch ist diese Art der Bereitstellung nicht schwerwiegenden Anwärter für eine Disaster Recovery-Plan.

Das folgende Diagramm zeigt eine Anwendung mit einer einzelnen Azure-Region. Azure Traffic Manager und die Verwendung der Fehlertoleranz und Aktualisierung Verfügbarkeit der Anwendung innerhalb des Bereichs.

![Bereitstellung für einzelne region](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

Hier ergibt sich, dass die Datenbank eine einzelne Fehlerquelle. Obwohl Azure Daten über verschiedene Fehlerdomänen auf interne Replikate repliziert, tritt diese alle im selben Bereich. Die Anwendung kann keinem Katastrophenfall standhalten. Fällt die Region, gehen alle Fehlerdomänen – einschließlich aller Instanzen und Speicherressourcen.

Für alle mindestens kritischen Applikationen müssen Sie einen Plan zum Bereitstellen der Anwendung in mehreren Regionen entwickeln. Auch sollten Sie RTO und Nebenbedingungen berücksichtigen die Bereitstellungstopologie mit Kosten.

Schauen Sie auf bestimmte Muster Failover in verschiedenen Regionen unterstützt. Alle diese Beispiele verwenden zwei Bereiche beschrieben.

###<a name="redeployment-to-a-secondary-azure-region"></a>Erneute Bereitstellung einer sekundären Azure Region

Muster für die erneute Bereitstellung auf einen zweiten Bereich hat die primäre Region Applikationen und Datenbanken. Die sekundäre Region ist nicht für ein automatisches Failover festgelegt. Wenn ein Notfall eintritt, müssen Sie so alle Teile des Dienstes in der neuen Region drehen. Dies schließt einen Cloud-Dienst in Azure Cloud-Dienst bereitstellen, Wiederherstellung und Änderung DNS zum Umleiten des Datenverkehrs hochladen.

Das hat günstigste Region mehrere Optionen, aber schlimmsten RTO-Eigenschaften. In diesem Modell werden die Service-Paket und Datenbank Sicherungskopien gespeichert entweder lokal oder in Azure BLOB-Speicherinstanz des zweiten Bereichs. Sie müssen einen neuen Dienst bereitstellen und die Daten wiederherstellen, bevor Vorgang fortgesetzt wird. Auch wenn Datenübertragung von backup-Speicher vollautomatisierte verbraucht Einrichten der neuen datenbankumgebung viel Zeit. Verschieben von Daten von den backup-Speicherplatz auf leere Datenbank auf der sekundären Region ist die teuerste Teil des Wiederherstellungsvorgangs. Sie müssen jedoch dazu die neue Datenbank repliziert ist nicht betriebsbereit angezeigt.

Am besten werden Servicepakete im BLOB-Speicher in den sekundären Region gespeichert. Dadurch muss das Paket auf Azure hochladen ist was passiert, wenn Sie von einem lokalen Entwicklungscomputer bereitstellen. Mithilfe von PowerShell-Skripts können Sie die Service-Pakete von BLOB-Speicher schnell einen neuen Cloud-Dienst bereitstellen.

Diese Option ist nur für nicht kritische Anwendung, die eine hohe RTO tolerieren können. Beispielsweise funktioniert dies für eine Anwendung, die Sie stundenlang kann aber sollte innerhalb von 24 Stunden erneut ausgeführt werden.

![Erneute Bereitstellung einer sekundären Azure Region](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###<a name="active-passive"></a>Aktiv / Passiv

Aktiv / Passiv-Muster ist die Wahl, die viele Unternehmen bevorzugen. Dieses Muster stellt eine Verbesserung gegenüber der erneute Muster, das RTO mit geringen Kosten.
In diesem Szenario ist eine primäre und eine sekundäre Azure Region. Den gesamten Datenverkehr weiter aktiv Bereitstellung die primäre Region. Sekundäre Region ist besser für Disaster Recovery vorbereitet, da die Datenbank auf beide Bereiche ausgeführt wird. Außerdem ist ein Synchronisierungsmechanismus zwischen ihnen. Dadurch standby betreffen zwei Varianten: einen Datenbank oder Bereitstellung vollständig im zweiten Bereich.

####<a name="database-only"></a>Datenbank

In der ersten Variante des Aktiv / Passiv-Muster hat die primäre Region eine bereitgestellte Cloud-Dienst-Anwendung. Beide Bereiche werden jedoch anders als Muster für die erneute Bereitstellung mit dem Inhalt der Datenbank synchronisiert. (Weitere Informationen finden Sie auf [Transaktionsdaten für Disaster Recovery](#transactional-data-pattern-for-disaster-recovery)Abschnitt.) Wenn ein Notfall eintritt, sind weniger Aktivierung Anforderungen. Starten Sie die Anwendung in der sekundären Region, Verbindungszeichenfolgen in die neue Datenbank ändern und ändern Sie die DNS-Einträge zum Datenverkehr umleiten.

Muster für die erneute Bereitstellung sollten Sie bereits Service-Pakete in Azure BLOB-Speicher in den sekundären Region für schnellere Bereitstellung gespeichert haben. Anders als das Muster für die erneute Bereitstellung entstehen nicht den größten Teil des Verwaltungsaufwands, die Wiederherstellung der Datenbank erforderlich sind. Die Datenbank ist bereit. Dies spart viel Zeit, damit eine kostengünstige DR-Muster. Es ist auch das beliebteste DR-Muster.

![Aktiv-Passiv-Datenbank](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####<a name="full-replica"></a>Vollständiges Replikat

In der zweiten Variante des Aktiv / Passiv-Muster haben Bereich für primäre und sekundäre Region eine vollständige Implementierung. Diese Bereitstellung enthält die Clouddienste und eine synchronisierte Datenbank. Allerdings ist die primäre Region Netzwerkanfragen Benutzer aktiv behandeln. Die sekundäre Region aktiv nur, wenn die primäre Region eine serviceunterbrechung auftritt. In diesem Fall weiterleiten alle neuen Netzwerkanfragen an sekundären Region. Azure Traffic Manager können diese Failover automatisch verwalten.

Failover rührt schneller als Variante Datenbank nur die Dienste bereits bereitgestellt wurden. Dieses Muster bietet eine sehr niedrige RTO. Sekundärer Failover-Region muss sofort nach Ausfall der primären Region gehen bereit.

Mit schneller Reaktionszeit hat dieses Muster den Vorteil von Speicher und backup-Services bereitstellen. Sie müssen einen Bereich ohne neue Instanzen im Katastrophenfall Zuzuweisender Speicherplatz sorgen. Dies ist wichtig, wenn die sekundäre Azure Region Kapazität nähert. Es gibt keine Garantie (Service Level Agreement), dass Sie sofort eine Anzahl neuer Clouddienste in jeder Region bereitstellen können.

Schnellste Reaktionszeit bei diesem Modell müssen Größenordnung (Anzahl der Rolleninstanzen) in primäre und sekundäre. Trotz der Vorteile für nicht verwendete Serverinstanzen Zahlen ist teuer und möglicherweise nicht die klügste finanzielle Wahl. Dadurch wird eine leicht verkleinerte Version der Cloud-Services auf der sekundären Region verwenden. Sie können schnell ein Failover und sekundäre Bereitstellung bei Bedarf skalieren. Damit nach die primäre Region zugegriffen werden kann, je nach Belastung zusätzliche Instanzen aktivieren, sollten Sie den Failoverprozess automatisieren. Dies kann eine automatische Skalierung Mechanismus wie [virtuellen Skalierung festgelegt](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Die folgende Abbildung zeigt das Modell, die primäre und sekundäre Bereiche vollständig bereitgestellte Cloud-Dienst in einer Aktiv / Passiv-Muster enthalten.

![Aktiv-Passiv, vollständigen Replikat](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###<a name="active-active"></a>Aktive

Nun, Sie sind wahrscheinlich herauszufinden, die Entwicklung der Muster: Verringern des RTOs erhöht Kosten und Komplexität. Die aktive Projektmappe tatsächlich bricht diese Tendenz hinsichtlich Kosten.

Eine aktive Muster werden die Cloud-Services und die Datenbank vollständig in beiden Regionen bereitgestellt. Im Gegensatz zur Aktiv / Passiv-Modell empfangen beide Bereiche Benutzerdatenverkehr. Diese Option liefert die schnellste Wiederherstellungszeit. Dienste werden bereits skaliert, um einen Teil der Last in der jeweiligen Region zu behandeln. DNS ist bereits aktiviert, die sekundäre Region verwenden. Gibt zusätzlicher Komplexität bestimmen, wie Benutzer auf die entsprechende Region weitergeleitet. Round-Robin-scheduling kann erreicht werden. Es ist wahrscheinlicher, dass bestimmte Benutzer eine bestimmte Region verwenden, befindet sich die primäre Kopie der Daten.

Bei einem Failover einfach deaktivieren Sie DNS in den primären Bereich. Dieser leitet den Datenverkehr aller sekundären Region.

Auch in diesem Modell gibt es einige Varianten. Das folgende Diagramm zeigt z. B. ein Modell, in dem die primäre Region die Masterkopie der Datenbank besitzt. Die Clouddienste in beiden Regionen Schreiben mit der primären Datenbank. Sekundäre Bereitstellung kann die primäre oder replizierten Datenbank gelesen werden. Replikation in diesem Beispiel erfolgt eine Möglichkeit.

![Aktive](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

Gibt es ein Nachteil für die aktive Architektur im obigen Diagramm. Zweite Bereich muss die Datenbank aus dem ersten Bereich zugreifen, da die Masterkopie wohnt. Deutlich abfällt Performance beim Zugriff auf Daten außerhalb eines Bereichs. Im Cross-Region Datenbankaufrufe sollten Sie eine Art von Strategie zur Verbesserung der Leistung dieser Aufrufe Batchverarbeitung. Weitere Informationen finden Sie unter [Batchverarbeitung zur Verbesserung der Anwendungsperformance SQL-Datenbank verwenden](../sql-database/sql-database-use-batching-to-improve-performance.md).

Eine alternative Architektur kann jede Region eine eigene Datenbank direkt auf umfassen. Bei diesem Modell muss ein bidirektionale Replikation zum Synchronisieren der Datenbanken in jeder Region.

Aktive Muster benötigen Sie möglicherweise nicht so viele Instanzen auf den primären Bereich wie das aktiv / passiv-Muster. Haben Sie 10 Instanzen auf den primären Bereich in einer Aktiv / Passiv-Architektur benötigen Sie nur 5 in jeder Region in einer aktiv / aktiv-Architektur. Beide Bereiche teilen nun die Last. Möglicherweise eine Kostenersparnis über Aktiv / Passiv-Muster Wenn Sie einen warmen standby auf passiven Bereich mit 10 Instanzen Failover warten.

Erkennen Sie, bis wieder die primäre Region sekundäre Region einen plötzlichen Anstieg der neuen Benutzer empfangen kann. Wenn 10.000 Benutzer auf jedem Server bei der primäre Bereich eine langwierige, muss die sekundäre Region plötzlich 20.000 Benutzer. Überwachungsregeln für den zweiten Bereich muss diese und doppelte Instanzen in der sekundären Region erkannt werden. Weitere Informationen hierzu finden Sie im Abschnitt auf die [Erkennung von Fehlern](#failure-detection).

##<a name="hybrid-on-premises-and-cloud-solution"></a>Hybrid-lokal und cloud-Lösung

Eine weitere Strategie für Disaster Recovery ist eine hybridanwendung entwerfen, die lokal ausgeführt wird und in der Cloud. Je möglicherweise die primäre Region einer Position. Betrachten Sie die vorherigen Architekturen und vorstellen Sie die primäre oder sekundäre Region einen lokalen Speicherort.

Diese hybridarchitekturen umfasst eine Herausforderung dar. Erstens Großteil dieses Artikels befasst PaaS-Architekturmuster. Normale PaaS-Anwendung in Azure basieren auf Azure-spezifische Konstrukte wie Rollen, Cloud-Dienste und Traffic Manager. Eine lokale Lösung für diesen PaaS-Anwendung erfordert eine deutlich andere Architektur. Dies kann nicht aus einem Management oder Kosten sein.

Eine gemischte Lösung für Disaster Recovery hat jedoch weniger Probleme für herkömmliche Architekturen, die einfach in die Cloud verschoben wurden. Dies gilt für Architekturen, die IaaS verwenden. IaaS Clientanwendungen verwenden VMs in der Cloud, die lokalen direkte Entsprechung haben. Virtuelle Netzwerke können Sie Computer in die Cloud mit lokalen Netzwerkressourcen herstellen. Dies Möglichkeiten, die nicht nur PaaS-Anwendung möglich sind. Beispielsweise können SQL Server AlwaysOn Availability Gruppen und später Disaster Recovery-Lösung nutzen. Details finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure virtuellen Maschinen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

IaaS-Lösung bietet auch einen einfacherer Weg lokale Anwendung Azure als Failover-Option verwenden. Sie müssen eine voll funktionsfähige Anwendung in einen vorhandenen lokalen Bereich. Allerdings wenn Sie Ressourcen zu einer geografischen Region für Failover? Sie können virtuelle Maschinen und virtuelle Netzwerke verwenden, um Ihre Anwendung in Azure. In diesem Fall definieren Sie Prozesse, die Daten in der Cloud zu synchronisieren. Dann die Azure-Bereitstellung wird die sekundäre Region für Failover verwenden. Die primäre Region bleibt der lokalen Anwendung. Weitere Informationen IaaS-Architekturen und Funktionen finden Sie in der [Dokumentation zu virtuellen Maschinen](https://azure.microsoft.com/documentation/services/virtual-machines/).

##<a name="alternative-cloud"></a>Alternative cloud

Es gibt Situationen auch die Stabilität der Microsoft Cloud nicht treffen kann interne Compliance-Regeln oder Richtlinien, die Ihre Organisation benötigt. Selbst die beste Vorbereitung und Design Sicherungssysteme Notfall implementieren kurz ist eine globale Service-Ausfällen eines Dienstanbieters Cloud.

Werden Kosten und Komplexität der Verfügbarkeit Anforderungen vergleichen möchten. Eine Risikoanalyse und RTO und RPO für die Projektmappe definieren. Wenn Ihre Anwendung Ausfallzeiten tolerieren kann, könnte es für eine andere Cloudlösung in Erwägung sinnvoll. Wenn das gesamte Internet ausfällt, kann eine andere Cloudlösung möglicherweise weiterhin verfügbar, wenn Azure weltweit zugänglich.

Mit Hybrid-Szenario kann Failover Bereitstellung vorherige Disaster Recovery-Architektur auch in eine andere Cloudlösung. Alternative Cloud DR Websites sollte nur für Projektmappen verwendet werden, deren RTO wenig ggf. Ausfallzeiten kann. Beachten Sie, dass eine Lösung, die DR-Standort außerhalb Azure zu konfigurieren, entwickeln, bereitstellen und verwalten muss. Es ist auch schwieriger zu implementieren best Practices in einer Cross-Cloud-Architektur. Zwar Cloud-Plattformen wie allgemeine Konzepte, unterscheiden sich die APIs und Architekturen.

Möchten Sie die DR auf anderen Plattformen Teilen, sinnvoll Architekten Abstraktionsschichten im Entwurf der Lösung. Wenn Sie dies tun, müssen Sie entwickeln und zwei unterschiedliche Versionen derselben Anwendung für andere Cloudplattformen Notfall. Als Hybrid-Szenario die Verwendung von Azure Virtual Machines oder Azure Container Service einfacher in diesen Fällen die Verwendung von Cloud-spezifischen PaaS-Designs möglicherweise.

##<a name="automation"></a>Automatisierung

Gerade erörterten Muster erfordern schnelle Aktivierung offline-Installationen sowie Wiederherstellung von bestimmten Teilen eines Systems. Automatisierung oder Skripts unterstützt die Ressourcen bei Bedarf aktivieren und Projektmappen schnell bereitstellen. In diesem Artikel DR-bezogene Automatisierung [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)gleichgesetzt, aber die [Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) ist auch.

Entwickeln von Skripts unterstützt zu Azure nicht transparent behandelt die Teile des DR. Dies hat den Vorteil, konsistente Ergebnisse jeweils die minimiert die Wahrscheinlichkeit von Benutzerfehlern. DR-Skripten haben vordefinierte verkürzt auch ein System und seine Bestandteile in einem Notfall wiederherstellen. Sie möchten manuell herauszufinden, wie Ihre Website wiederherzustellen, während es nach unten und verlieren Geld pro Minute ist.

Nachdem Sie die Skripts erstellen, testen sie wiederholt von Anfang bis Ende. Nach der Überprüfung ihrer grundlegenden Funktionen unbedingt testen im [Notfall Simulation](#disaster-simulation). Dadurch werden Fehler in Skripts oder Prozesse aufdecken.

Empfohlen durch Automatisierung ist ein Repository von PowerShell-Skripts oder Befehlszeilenschnittstelle (CLI) Skripts für Azure Disaster Recovery erstellen. Eindeutig kennzeichnen und für einfache Suche kategorisieren. Festlegen einer Person Repository und Versionskontrolle Skripts verwalten. Dokumentieren sie gut mit Parametern und Beispielen Skript verwenden. Außerdem sicherstellen Sie, dass Sie diese Dokumentation mit der Azure-Bereitstellung synchronisieren. Dies Unterstreicht den Zweck einer Person für alle Teile des Repository.

##<a name="failure-detection"></a>Erkennung von Fehlern

Ordnungsgemäß behandeln Sie Probleme mit der Verfügbarkeit und Disaster Recovery müssen Sie möglicherweise erkennen und Analysieren von Fehlern. Führen Sie erweiterte Server- und Bereitstellung überwachen, also schnell können bei einem System oder seinen Teilen Sie sind. Überwachungstools, die auf den Gesamtzustand der Cloud-Dienst und die zugehörigen Dateien kann Teil dieser Arbeit ausführen. Ein Microsoft-Tool ist [System Center 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/). Drittanbieter-Tools bieten auch Überwachungsfunktionen. Die meisten überwachungslösungen verfolgen wichtige Leistungsindikatoren und Verfügbarkeit.

Obwohl diese Tools wichtig sind, tauschen sie Fehlersuche und reporting in einem Clouddienst planen müssen. Sie müssen ordnungsgemäß Azure-Diagnose verwenden möchten. Benutzerdefinierte Leistungsindikatoren oder Einträge im Ereignisprotokoll kann auch Teil der Gesamtstrategie. Dadurch werden mehr Daten bei Ausfällen schnell diagnostizieren des Problems und Wiederherstellungsfunktionen vollständige. Darüber hinaus zusätzliche Kriterien, mit denen die Überwachungstools Anwendungszustands bestimmt. Weitere Informationen finden Sie unter [Aktivieren von Azure Diagnostics in Azure Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md). Der insgesamt "Health Model" Planung, siehe [Failsafe: Hinweise zur Cloud-Architekturen sowie](https://channel9.msdn.com/Series/FailSafe).

##<a name="disaster-simulation"></a>Disaster-simulation

Testen der Simulation umfasst das Erstellen von kleiner Situationen am Arbeitsplatz zu beobachten, wie reagieren die Teammitglieder. Simulationen zeigen außerdem wie effektiv Lösungen im Wiederherstellungsplan. Führen Sie Simulationen so, dass erstellten Szenarien selbst bei realen Situationen weiterhin Gefühl stören nicht.

Sollten Sie die Architektur ein "Übersicht" in der Anwendung manuell Verfügbarkeitsprobleme simulieren. Beispielsweise durch ein soft Switching lösen Sie Datenbankausnahmen für eine Bestellung Modul Zugriff durch zu Fehlfunktionen verursacht aus. Sie können ähnliche leicht für andere Module auf dem Netzwerk Ansätzen.

Die Simulation Zeigt Probleme behoben wurden. Simulierten Szenarien müssen vollständig steuerbar sein. Dies bedeutet, dass auch wenn der Wiederherstellungsplan beschädigt, die Situation wieder wiederherstellen können ohne erheblichen Schaden. Außerdem ist es wichtig, dass informieren übergeordneten Management wann und wie die Übungen ausgeführt werden. Dieser Plan sollte enthalten Informationen über die Zeit oder Ressourcen unproduktiv werden während der Simulationstest ausgeführt wird. Wenn Sie Ihren Wiederherstellungsplan Test unterziehen, unbedingt auch definieren, wie der Erfolg gemessen wird.

Es gibt verschiedene Techniken, mit denen Sie Testen von Disaster Recovery-Pläne. Die meisten sind jedoch nur geänderte Versionen dieser grundlegenden Techniken. Das Hauptmotiv hinter dieser Test ist wie möglich und wie möglich die Wiederherstellung erfolgt. Disaster Recovery-Tests konzentriert sich auf die Details zu Lücken in der Wiederherstellungsplan muss.

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe von Artikeln, [Disaster Recovery und hohe Verfügbarkeit bei Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md)konzentriert. Der frühere Artikel dieser Reihe ist eine [hohe Verfügbarkeit bei Microsoft Azure](./resiliency-high-availability-azure-applications.md).
