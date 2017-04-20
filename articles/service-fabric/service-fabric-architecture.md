<properties
   pageTitle="Service Fabric-Architektur | Microsoft Azure"
   description="Service Fabric ist eine skalierbare, zuverlässige und einfache Verwaltung Anwendung für die Cloud zur verteilten Systemplattform. Dieser Artikel beschreibt die Architektur von Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="rishirsinha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/09/2016"
   ms.author="rsinha"/>

# <a name="service-fabric-architecture"></a>Service Fabric-Architektur

Service Fabric ist mit Ebenen Subsysteme erstellt. Diese Subsysteme können Sie Applikationen schreiben:

* Hohe Verfügbarkeit
* Skalierbare
* Verwaltbar
* Getestet werden

Das folgende Diagramm zeigt die wichtigsten Subsysteme von Service Fabric.

![Diagramm der Service Fabric-Architektur](media/service-fabric-architecture/service-fabric-architecture.png)

In einem verteilten System unbedingt die Möglichkeit zur sicheren Kommunikation zwischen Knoten in einem Cluster. Der Stapel ist das Transportsubsystem die sichere Kommunikation zwischen den Knoten. Über den Transport liegt Subsystem das Föderation-Subsystem, das die verschiedenen Knoten damit Service Fabric können Fehler erkennen Hinweislinie Wahl auszuführen und konsistente Nachrichtenrouting in einer einzelnen Entität (bezeichnet als Cluster) Cluster. Zuverlässigkeit Subsystem, Subsystem Föderation überlagern ist verantwortlich für die Zuverlässigkeit der Service Fabric-Services durch Mechanismen wie Replikation, Ressourcenmanagement und Failover. Föderation Subsystem unterliegt außerdem Subsystem hosten und Aktivierung, das Lebenszyklus einer Anwendung auf einem einzelnen Knoten verwaltet. Management-Subsystem managt den Lebenszyklus von Programmen und Diensten. Subsystem Prüfbarkeit kann Entwickler ihre Dienste über simulierter Fehler vor und nach der Bereitstellung von Applikationen und Diensten sich testen. Service Fabric ermöglicht Standorten über seine Kommunikation Subsystem zu beheben. Die Anwendung Programmiermodelle Entwickler ausgesetzt sind über diese Subsysteme mit Anwendungsmodell Werkzeuge aktivieren gelegt.

## <a name="transport-subsystem"></a>Transportsubsystem
Das Transportsubsystem implementiert einen Punkt Datagramm Kommunikationskanal. Dieser Kanal wird für in Service Fabric-Cluster und Kommunikation zwischen Service Fabric-Cluster und Clients verwendet. Unterstützt unidirektionale und Anforderung-Antwort-Kommunikation die Grundlage für die Implementierung von Broadcast- und Multicast in der Föderation Ebene. Das Transportsubsystem sichert die Kommunikation mit X509 Zertifikate oder Windows-Sicherheit. Dieses Subsystem von Service Fabric intern verwendet und kann nicht direkt für Entwickler für Programmierung.

## <a name="federation-subsystem"></a>Föderation subsystem
Um Grund über eine Gruppe von Knoten in einem verteilten System müssen Sie eine konsistente Ansicht des Systems. Teilsystems Föderation verwendet Kommunikation primitive Transportsubsystem bereitgestellt und Stiche verschiedenen Knoten in einem einheitlichen Cluster, dem über Grund. Es bietet verteilten Systemen primitive anderen Teilsysteme - Erkennung von Fehlern projektleiterwahl und konsistente routing erforderlich. Verbundsubsystem basiert auf verteilten Hashtabellen mit 128-Bit-token. Das Subsystem erstellt eine Ringtopologie Knoten, wobei jeder Knoten in den Teil der token Platz für den Besitz zugewiesen. Für die Erkennung von Fehlern verwendet die Schicht einen leasing-Mechanismus basierend auf Herz und vermitteln. Föderation Subsystem außerdem über komplizierte Join und Abreise Protokolle, die nur ein einzelner Besitzer eines Tokens jederzeit vorhanden ist. Diese bieten Hinweislinie Wahl und konsistente routing gewährleistet.

## <a name="reliability-subsystem"></a>Zuverlässigkeit subsystem
Teilsystems Zuverlässigkeit bietet den Mechanismus der Service Fabric-Dienst _Replicator_, _Failover-Manager_und _Resource Balancer_hoch verfügbar zu machen.

* Der Replikator sorgt Zustandsänderungen im primären dienstreplikat automatisch auf sekundäre Replikate repliziert Konsistenz zwischen primären und sekundären Replikate in einer Replikatgruppe Service. Der Replicator ist für die Quorumressource Verwaltung zwischen den Replikaten in der Replikatgruppe. Interagiert mit dem Failover zu der Liste der zu replizieren und Neukonfiguration Agent wird die Konfiguration des Replikatsatzes. Diese Konfiguration gibt an, welche Replikate Vorgänge repliziert werden müssen. Fabric Service bietet eine standardmäßige Replicator aufgerufen Fabric Replicator programming Model-API verwendet werden, um den Dienststatus hoch verfügbarer und zuverlässiger machen.
* Der Failover-Manager stellt sicher, dass wenn Knoten hinzugefügt oder aus dem Cluster entfernt werden, wird die Last automatisch auf die verfügbaren Knoten verteilt. Wenn ein Knoten im Cluster ausfällt, wird Cluster Service Replikate um Verfügbarkeit automatisch konfiguriert.
* Der Ressourcen-Manager Replikate über Fehler Domäne im Cluster platziert und stellt sicher, dass alle Failover betriebsbereit sind. Der Ressourcen-Manager gleicht Ressourcen auch auf zugrunde liegenden gemeinsamen Pool von Clusterknoten zu optimalen einheitliche Verteilung.

## <a name="management-subsystem"></a>Management-subsystem
Management-Subsystem bietet End-to-End und Anwendungslebenszyklus-Verwaltung. PowerShell-Cmdlets und administrative APIs können Sie bereitstellen, Bereitstellung, patch, aktualisieren und de-Applikationen ohne Verlust der Verfügbarkeit bereitstellen. Management-Subsystem führt dies durch die folgenden Dienste.

* **Cluster-Manager**: diese den primären Dienst interagiert mit der Failover-Manager von Zuverlässigkeit Anträge auf Grundlage Integritätsregeln Platzierung Service Knoten platziert wird. Der Ressourcen-Manager im Failover-Subsystem gewährleistet, dass die Integritätsregeln nicht unterbrochen werden. Die Clusterverwaltung verwaltet den Lebenszyklus der Anwendung aus Bereitstellung aufgehoben. Integriert mit den Diagnose-Manager sicherstellen, dass die Verfügbarkeit während des Upgrades nicht aus Sicht semantische Zustand verloren geht.
* **Diagnose-Manager**: mit diesem Service können Gesundheitsberichterstattung Applikationen, Services und clusterentitäten. Clusterentitäten (z. B. Knoten Servicepartitionen und Replikate) können Gesundheitsdaten melden-Speicher zentrale Gesundheit aggregiert. Diese Gesundheitsinformationen bietet einen allgemeine Point-in-Time Health Snapshot und Knoten verteilt über mehrere Knoten im Cluster erforderlichen Maßnahmen zu aktivieren. Health Query APIs können Sie die Systemereignisse Abfragen Health-Subsystem gemeldet. APIs raw Health Daten im Informationsspeicher Gesundheit oder der aggregierten zurückgeben Health Abfrage interpretiert Daten für einen bestimmten Cluster Entität.
* **Abbildspeicher**: dieser Service bietet, Lagerung und Verteilung von Binärdateien der Anwendung. Dieser Service bietet einen einfache verteilte Dateispeicher, wo die Anwendung hochgeladen und heruntergeladen.


## <a name="hosting-subsystem"></a>Hosting-subsystem
Die Clusterverwaltung informiert das hosting Subsystem (ausgeführt auf jedem Knoten) welche Dienste für einen bestimmten Knoten verwalten muss. Hosting-Subsystem verwaltet den Lebenszyklus der Anwendung auf diesen Knoten. Es interagiert mit den Zuverlässigkeit und Gesundheit Replikate richtig platziert und unbeschädigt sind.

## <a name="communication-subsystem"></a>Kommunikation-subsystem
Dieses Teilsystem bietet zuverlässiges messaging innerhalb der Cluster und Service Discovery Naming Service. Naming Service löst Namen im Cluster an und ermöglicht die Verwaltung von Namen und Eigenschaften. Naming Service verwenden, können Clients sicher kommunizieren mit jedem beliebigen Knoten im Cluster Service-Namen auflösen und Abrufen von Metadaten. Einen einfachen Naming-Client-API können Benutzer Service Fabric Services und Kunden können die aktuellen Netzwerkstandort trotz Knoten Dynamik oder die Größe des Clusters entwickeln.

## <a name="testability-subsystem"></a>Prüfbarkeit subsystem
Prüfbarkeit ist eine Suite von Tools, die speziell für die Prüfung auf Service erstellt. Die Tools können Entwickler leicht sinnvolle Faults auslösen und Durchführen von Testszenarien zu Übung zahlreiche Zustände und Übergänge, die ein Dienst während seiner Lebensdauer in kontrolliert und sicher erfahren. Prüfbarkeit bietet ferner einen Mechanismus zum Ausführen von mehr Tests durchlaufen können durch verschiedene mögliche Fehler ohne Verfügbarkeit. Dies bietet einen Test Produktionsumfeld.
