<properties
   pageTitle="Zuverlässige Architektur | Microsoft Azure"
   description="Übersicht über die Architektur zuverlässig statusbehaftete und statusfreie"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Architektur für statusbehaftete und statusfreie zuverlässige Dienste

Ein Azure Fabric zuverlässigen Dienst möglicherweise statusbehaftet oder statusfrei. Jede dieser Service wird innerhalb einer bestimmten Architektur ausgeführt. Dieser Architekturen werden in diesem Artikel beschrieben.
[Zuverlässige Übersicht über](service-fabric-reliable-services-introduction.md) Informationen zu den Unterschieden zwischen statusbehaftete und statusfreie anzeigen

## <a name="stateful-reliable-services"></a>Statusbehaftete zuverlässige Dienste

### <a name="architecture-of-a-stateful-service"></a>Architektur der statusbehaftete Dienst
![Architekturdiagramm statusbehaftete Dienst](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Statusbehaftete zuverlässig

Statusbehaftete Serviceangebot können StatefulService oder StatefulServiceBase-Klasse abgeleitet werden. Beide dieser Klassen werden von Service Fabric bereitgestellt. Sie bieten verschiedene Ebenen und Abstraktion für statusbehaftete Dienst mit Service - Schnittstelle und als Dienst im Cluster Service Fabric.

StatefulService wird von StatefulServiceBase abgeleitet. StatefulServiceBase Services bietet mehr Flexibilität, erfordert jedoch weitere Kenntnisse Internals Service Fabric.
Weitere Informationen zu den Besonderheiten von Services mithilfe der Klassen StatefulService und StatefulServiceBase schreiben finden Sie [zuverlässige Service-Überblick](service-fabric-reliable-services-introduction.md) und [zuverlässig die fortgeschrittene Verwendung](service-fabric-reliable-services-advanced-usage.md) .

Beide Klassen verwalten die Lebensdauer und die Service-Implementierung. Service-Implementierung kann virtuelle Methoden einer Basisklasse überschreiben, wenn die Service-Implementierung Arbeit an diesen Punkten im Lebenszyklus Implementierung Service - oder um eine Kommunikation Listener-Objekt erstellen. Beachten Sie, dass zwar eine Service-Implementierung eigener Listener Kommunikationsobjekt Verfügbarmachen ICommunicationListener, in dem Diagramm oben implementieren kann kommunikationslistener durch Service - ist die Service-Implementierung kommunikationslistener verwendet, der durch Service implementiert.

Statusbehaftete zuverlässig verwendet den zuverlässige zuverlässige Sammlungen nutzen. Zuverlässige Sammlungen sind lokale Datenstrukturen, die Service - hoch verfügbar ist, sind immer verfügbar, unabhängig von Service-Failover. Jede dieser zuverlässigen Auflistung wird von einem zuverlässigen zustandsanbieter implementiert.
Weitere Informationen zu reliable Sammlungen finden Sie unter [Übersicht über zuverlässige Sammlungen](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Zuverlässige Status-Manager und Status-Anbieter

Zuverlässige Status-Manager ist das Objekt, das zuverlässigen Zustand Anbieter verwaltet. Es verfügt über Funktionen zum Erstellen, löschen, auflisten und zuverlässigen Zustand Anbieter beibehaltenen und sind hochgradig verfügbar. Eine Instanz des zuverlässigen Zustand stellt eine Instanz einer dauerhaften und hochverfügbare Datenstruktur eines Wörterbuchs oder einer Warteschlange.

Jeder Provider zuverlässigen Zustand stellt eine Schnittstelle, die Interaktion mit der zuverlässigen zustandsanbieter statusbehaftete Dienst verwendet wird. IReliableDictionary ist z. B. mit zuverlässigen Wörterbuch und IReliableQueue mit zuverlässigen Warteschlange. Alle zuverlässigen Zustand Anbieter implementieren die Schnittstelle IReliableState.

Zuverlässige Status-Manager hat eine Schnittstelle namens IReliableStateManager, die Zugriff von einem statusbehaftete Dienst ermöglicht. Schnittstellen für zuverlässigen Zustand Anbieter werden durch IReliableStateManager zurückgegeben.

Zuverlässige Status-Manager verwendet eine Plug-in-Architektur, sodass neue Auflistungstypen zuverlässige dynamisch angeschlossen werden können.

Die Implementierung eines leistungsstarken, Version differenzielle sind zuverlässige Wörterbuch und zuverlässige Warteschlange aufgebaut.

### <a name="transactional-replicator"></a>Transaktionale replicator

Transaktionale Replicator-Komponente ist dafür verantwortlich, dass der Status des Dienstes (Status-Manager zuverlässigen Zustand und zuverlässigen Sammlungen) in allen Replikaten mit dem Dienst entspricht. Es wird sichergestellt, dass der Status im Protokoll beibehalten wird. Zuverlässigen Zustand Manager Schnittstellen mit Transaktions-Replicator über einen privaten Mechanismus.

Transaktionale Replicator verwendet ein Netzwerkprotokoll Zustand mit anderen Replikaten der Dienstinstanz kommunizieren, so dass alle Replikate aktuell Zustandsinformationen.

Transaktionale Replicator verwendet ein Protokoll Informationen beibehalten werden, damit die Zustandsinformationen Prozess überlebt oder Knoten stürzt ab. Die Schnittstelle für das Protokoll erfolgt über einen privaten Mechanismus.

### <a name="log"></a>Protokoll

Die Protokollierungskomponente bietet einen leistungsfähige permanenten Speicher, der für Schreibvorgänge optimiert werden Spinnerei oder Solid-State-Festplatten.  Der Entwurf des Protokolls wird zum permanenten Speichern (z.B. Festplatten) zu lokalen Knoten den statusbehafteten Dienst ausgeführt werden. Dies ermöglicht niedrige Latenz und hohem Durchsatz als permanente Remotespeicher ist nicht lokal auf dem Knoten.

Die Protokollierungskomponente verwendet mehrere Protokolldateien. Es ist eine Knoten Wide freigegebene Protokolldatei, die alle Replikate verwenden, wie es die niedrigste Latenz und höchsten Durchsatz kann zum Speichern von Daten. Standardmäßig freigegebene Protokolldatei im Arbeitsverzeichnis von Service Fabric Knoten platziert, aber möglicherweise auch an anderer Stelle im Idealfall auf einem Datenträger für die gemeinsamen Protokoll platziert werden konfiguriert werden. Jedes Replikat für den Dienst auch eine dedizierte Protokolldatei und dedizierte Protokoll befindet sich im Arbeitsverzeichnis des Dienstes. Es gibt keinen Mechanismus dedizierte Protokoll an einer anderen Stelle platziert werden sollen.

Freigegebene Protokoll ist Übergangsbereich für das Replikat Zustandsinformationen dedizierte Protokolldatei Endziel, beibehalten. In diesem Entwurf die Zustandsinformationen zuerst freigegebene Protokolldatei geschrieben und verzögert dedizierte Protokolldatei in den Hintergrund verschoben. Auf diese Weise hätte schreiben freigegebenen Protokoll die niedrigste Latenz und höchsten Durchsatz ermöglicht den Dienst schneller vorankommen.

Liest und schreibt in das freigegebene Protokoll über direkte IO, vorab Speicherplatz auf dem Datenträger für die freigegebene Datei. Um eine optimale Verwendung von Speicherplatz auf dem Laufwerk mit dedizierten wird als NTFS Datendichte dedizierte Protokolldatei erstellt. Beachten Sie, dass dadurch wird Festplattenspeicher Bereitstellung überproportional vieler Betriebssystem zeigen die dedizierte Protokolldateien mehr Speicherplatz als tatsächlich verwendet wird.

Das Protokoll steht neben eine minimale Benutzermodus-Schnittstelle in das Protokoll als Kernelmodus-Treiber. Durch Ausführung als Kernelmodus-Treiber bieten Protokoll die höchste Leistung auf alle Dienste, die sie verwenden.

Weitere Informationen zum Konfigurieren des Protokolls finden Sie unter [Statusbehaftete zuverlässige Dienste konfigurieren](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Statusfreie zuverlässig

### <a name="architecture-of-a-stateless-service"></a>Architektur der statusfreien service
![Architekturdiagramm statusfreie Service](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Statusfreie zuverlässig

Statusfreie Service-Implementierung von StatelessService oder StatelessServiceBase-Klasse abgeleitet werden. Die StatelessServiceBase-Klasse ermöglicht mehr Flexibilität als die StatelessService-Klasse.
Beide Klassen verwalten die Lebensdauer und die Rolle eines Dienstes.

Service-Implementierung kann virtuelle Methoden einer Basisklasse überschreiben, wenn der Dienst Arbeit an diesen Punkten des Service-Lebenszyklus hat oder will ein Listener Kommunikationsobjekt erstellen. Beachten Sie, dass auch eigene Kommunikation Listener-Objekt verfügbar machen im obigen Diagramm ICommunicationListener, die Implementierung des Service kann kommunikationslistener von Service Fabric ist dieser Service-Implementierung kommunikationslistener verwendet, der durch Service implementiert wird.

Finden Sie weitere Informationen zu den Besonderheiten von Services mithilfe der StatelessService und StatelessServiceBase schreiben [zuverlässigen Service Overview](service-fabric-reliable-services-introduction.md) und [zuverlässig die fortgeschrittene Verwendung](service-fabric-reliable-services-advanced-usage.md) .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Service Fabric finden Sie unter:

[Zuverlässiger Service-Überblick](service-fabric-reliable-services-introduction.md)

[Schnellstart](service-fabric-reliable-services-quick-start.md)

[Übersicht über zuverlässige Sammlungen](service-fabric-reliable-services-reliable-collections.md)

[Die fortgeschrittene Verwendung zuverlässig](service-fabric-reliable-services-advanced-usage.md)

[Zuverlässige Konfiguration](service-fabric-reliable-services-configuration.md)  
