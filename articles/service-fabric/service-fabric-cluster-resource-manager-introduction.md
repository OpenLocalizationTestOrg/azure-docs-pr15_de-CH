<properties
   pageTitle="Einführung in Service Fabric Cluster Resource Manager | Microsoft Azure"
   description="Eine Einführung in Service Fabric Cluster Resource Manager."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Einführung in Service Fabric Cluster Resource manager
Traditionell Verwalten von IT-Systemen oder eine Reihe von Diensten bedeutete wenigen physischen oder virtuellen Computern für diesen bestimmten Diensten oder Systemen vorgesehen. Viele wichtige Dienste wurden "Webebene" und "Daten" oder "Storage" Ebene, vielleicht einige andere spezielle Komponenten wie Cache aufgeschlüsselt. Andere Anwendungstypen müsste ein messaging-Schicht, Anfragen und geleiteten, angeschlossen an eine Arbeit für Analysen oder Transformation als Teil der messaging erforderlich. Jede dieser Aufgaben haben eine bestimmte Computer gewidmet: Datenbank haben einige Computer gewidmet, die Webserver ein Paar. Wenn eine bestimmte Art der Arbeitslast der Computer verursacht sollte auf heiß laufen und Sie weitere Computer mit der Arbeitslast, die darauf konfiguriert oder wenige Computer mit größeren ersetzt. Einfach. Wenn ein Computer nicht Lief, Teil der gesamten Anwendung Kapazität bis der Computer wiederhergestellt werden konnte. Noch einfach (wenn nicht unbedingt).

Nun, habe wir sagen Sie skalieren und Container bzw. Microservice Sprung haben müssen. Plötzlich finden Sie sich Dutzende, Hunderte oder sogar Tausende von Computern Dutzenden verschiedener Dienste möglicherweise Hunderte von anderen Instanzen dieser Dienste mit Instanzen oder Replikate für hohe Verfügbarkeit (HA).

Plötzlich Umwelt ist nicht so einfach wie das wenige Computern zum einzelnen Arten von Arbeitslasten verwalten. Die Server sind virtuell und müssen nicht mehr Namen (Sie *haben* Einstellung von [Haustieren, Rinder](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) nach allen gewechselt). Konfiguration ist über die Computer und über die Dienste selbst. Dedizierter Hardware ist eine der Vergangenheit und Services geworden kleine verteilte Systeme in kleinere Einheiten mit handelsüblichen Hardwarekomponenten.

Aufgrund Ihrer Anwendung früher monolithischen, tiered in separate Dienste auf Standardhardware unterteilt, können Sie viele weitere Kombinationen zu. Wer entscheidet, welche Arten von Arbeitslasten auf der Hardware ausgeführt werden können oder wie viele? Die Arbeitslasten auf derselben Hardware arbeiten und Widerspruch? Wenn ein Computer ausfällt. Was es auch ausgeführt wurde? Wer ist verantwortlich, sicherzustellen, dass Arbeit beginnt erneut ausführen? Abwarten Sie (virtuelle?) zurückkehren, oder führen Sie Ihre Arbeitslasten automatisch ein Failover zu anderen Computern weiterhin ausgeführt? Ist Eingreifen erforderlich? Was Upgrades in dieser Umgebung?

Als Entwickler und Betreiber in dieser Welt leben werden wir diese Komplexität Hilfe benötigen und Sie Einstellung Binge und versuchen, die Komplexität mit Papier ist nicht richtig.

Was ist zu tun?

## <a name="introducing-orchestrators"></a>Einführung von Bearbeitern
"Orchestrierung" wird als allgemeiner Begriff für eine Softwarekomponente, mit der Administratoren, die diese Art von Umgebung verwalten. Berufliche sind Komponenten, mit denen in Anfragen wie "5 Exemplare dieses Dienstes in meiner Umgebung ausführen möchte", true, stellen und (Versuch) so bleiben.

Berufliche (nicht Menschen) sind was Einsatz Wenn ein Computer ausfällt oder eine Arbeitslast Gründen unerwartet beendet. Die meisten Bearbeitern mehr bewältigen Fehler hilft Ihnen bei der Bereitstellung neuer Upgrades Behandlung und Umgang mit Ressourcenverbrauch, aber alle sind grundsätzlich zum Verwalten von einigen gewünschten Zustand der Konfiguration in der Umgebung. Sie möchten ein Orchestrator sagen, was Sie möchten und sie die Schwerarbeit. Chronos oder Mesos, Flotte, Schwarm, Kubernetes und Service Fabric Marathon sind Beispiele für berufliche oder Sie integriert. Mehr entstehen die Zeit als die Komplexität der Verwaltung der realen Welt Installationen in unterschiedlichen Umgebungen und wachsen und ändern.

## <a name="orchestration-as-a-service"></a>Orchestrierung als Dienst
Auftrag Orchestrator Service Fabric-Cluster erfolgt hauptsächlich durch die Cluster-Ressourcen-Manager. Service Fabric Cluster Resource Manager ist die Systemdienste in Service Fabric und wird innerhalb eines Clusters automatisch gestartet.  Im Allgemeinen ist der Cluster Ressourcen-Manager Job in drei Teile unterteilt:

1. Regeln erzwingen
2. Optimierung Ihrer Umgebung
3. Unterstützung in anderen Prozessen

### <a name="what-it-isnt"></a>Was es nicht ist
In herkömmlichen N-Tier-Web-apps war immer eine Vorstellung einer "Lastenausgleich" als ein Network Load Balancer (NLB) oder eine Anwendung Load Balancer (ALB) je nach, im Netzwerkstapel Sat. Einige Systeme zum Lastenausgleich sind hardwarebasiert wie F5 BigIP, andere Software, z.B. Microsoft NLB. In einer anderen Umgebung möglicherweise etwas HAProxy in dieser Rolle angezeigt werden. Diese Architektur ist die Aufgabe des Lastenausgleichs sicherstellen, dass alle anderen statusfreien front-End-Computer oder anderen Computern im Cluster dieselbe Arbeit (ungefähr) erhalten. Strategien für das variierte an Sitzung fixieren/Klebrigkeit aktuelle Schätzung und rufen Zuteilung auf Basis der Soll-Kosten und aktuellen Computer laden jeden anderen Aufruf auf einem anderen Server senden.

Beachten Sie, dass dies am besten ausgeglichen Mechanismus dafür, den ungefähr die Webebene blieb. Strategien für die Datenebene Lastenausgleich wurden vollständig unterschiedliche und Datenspeichermechanismus normalerweise um Daten Sharding zentrieren, caching, Datenbankansichten und gespeicherte Prozeduren, usw. von.

Einige dieser Strategien interessant sind, ist Service Fabric Cluster Resource Manager nichts wie ein Network Load Balancer oder einen Cache. Ein Netzwerk Lastenausgleich stellt sicher, dass front-Ends ausgeglichen sind, indem Sie Datenverkehr auf, in dem die Dienste ausgeführt werden, Service Fabric Cluster Resource Manager nimmt eine völlig andere Strategie – grundlegend, Service Fabric verschoben, wo sie am sinnvollsten *Services* (und Datenverkehr oder Last folgen). Diese können, z. B. Knoten derzeit kalt sind, weil die Dienste nicht viel Arbeit jetzt auch oder die gelöscht oder verschoben. Als weiteres Beispiel können Cluster Resource Manager Service vom Computer also aktualisiert werden oder durch einen Anstieg der Verbrauch Überladen von den Diensten verschieben, die auf ihm ausgeführt wurden. Da Cluster Ressourcen-Manager ist verantwortlich für das Verschieben von Dienstleistungen (nicht den Netzwerkverkehr zu Services bereits übermittelt), beträchtlich Feature in einem Lastenausgleich fände gegenüber enthält und beschäftigt grundlegend Strategien dafür, dass auch die Hardware-Ressourcen im Cluster verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
- Informationen über die Architektur und den Informationsfluss innerhalb der Cluster-Ressourcen-Manager finden Sie [in diesem Artikel](service-fabric-cluster-resource-manager-architecture.md)
- Der Cluster-Ressourcen-Manager hat viele Optionen für die Beschreibung des Clusters. Erfahren Sie lesen mehr darüber Sie diesen Artikel zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)
- Für Weitere Informationen zu anderen Optionen zur Konfiguration Auschecken Thema andere Cluster Resource Manager Konfigurationen services verfügbaren [Informationen zum Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)
- Metriken sind wie Service Fabric-Cluster Resource Manager Verbrauch und Kapazität im Cluster verwaltet. Erfahren Sie lesen mehr über sie und ihre Konfigurierung Sie [diesen Artikel](service-fabric-cluster-resource-manager-metrics.md)
- Cluster Resource Manager arbeitet mit Fabric Service Management-Funktionen. [Erfahren Sie mehr über die Integration, lesen](service-fabric-cluster-resource-manager-management-integration.md)
- Um herauszufinden, wie die Cluster-Ressourcen-Manager verwaltet und verteilt die Last im Cluster, Artikel auf [Last](service-fabric-cluster-resource-manager-balancing.md) Auschecken
