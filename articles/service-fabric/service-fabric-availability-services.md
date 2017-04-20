<properties
   pageTitle="Verfügbarkeit von Service Fabric-Services | Microsoft Azure"
   description="Beschreibt die Fehlersuche, Failover und Recovery für Dienste"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="availability-of-service-fabric-services"></a>Verfügbarkeit von Service Fabric-services
Azure Service Fabric-Dienste können statusbehaftet oder statusfrei sein. Dieser Artikel gibt einen Überblick über die wie Service Fabric Verfügbarkeit eines Dienstes bei Ausfällen verwaltet.

## <a name="availability-of-service-fabric-stateless-services"></a>Verfügbarkeit von Service Fabric zustandsloser Dienste
Ein zustandsloser Dienst ist ein Anwendungsdienst, der nicht [lokalen Zustand](service-fabric-concepts-state.md).

Erstellen statusfreien Service erfordert definieren eine Instanz Count die Anzahl der Instanzen des Dienstes statusfreie im Cluster ausgeführt werden soll. Dies ist die Anzahl der Kopien der Anwendungslogik im Cluster instanziiert werden. Die Zahl der Instanzen wird empfohlen eine statusfreie Service Skalierung.

Wenn ein in einer Instanz einer statusfreien Service Fehler wird auf einem anderen können Knoten im Cluster eine neue Instanz erstellt.

## <a name="availability-of-service-fabric-stateful-services"></a>Verfügbarkeit der statusbehaftete Dienstfabric-services
Statusbehafteter Dienst hat einige Status zugeordnet. In Service Fabric ist ein statusbehafteter Dienst wie Replikate modelliert. Jedes Replikat ist eine Instanz des Codes des Dienstes, der eine des Staates Kopie. Lese-und Schreibvorgänge erfolgt ein Replikat (die primäre genannt). Ändern von Status schreiben Operationen auf mehreren Replikaten (aktive sekundäre bezeichnet) *repliziert* werden. Die Kombination aus primären und aktive sekundäre Replikate ist der Replikatsatz des Dienstes.

Es kann nur eine primäre Replikat Service Lese- und Schreibanfragen können, aber es mehrere aktive sekundäre Kopien. Die Anzahl der aktiven sekundären Replikate ist konfigurierbar und mehr Replikate kann mehr parallele Software und Hardware-Ausfälle tolerieren.

Im Fall eines Fehlers (bei primäre Replikat), Fabric Service gehört die aktive sekundäre Replikate neues primäres Replikat. Diese aktiven sekundären Kopie hat bereits die aktualisierte Version des Zustands (durch *Replikation*) und es weiter verarbeitet weiter lesen und schreiben.

Dieses Konzept – ein Replikat als Primär oder Active Secondary - Rolle Replikat bezeichnet.

### <a name="replica-roles"></a>Replikat Rollen
Die Rolle eines Replikats zum Verwalten des Lebenszyklus von diesem Replikat Zustand. Ein Replikat, dessen Funktion Hauptdienste ist, Leseanfragen. Schreibanfragen Dienste auch den Zustand und die Änderungen auf aktive sekundäre in der Replikatgruppe repliziert. Die aktive sekundäre soll Zustandsänderungen primäre Replikat repliziert wurde empfangen und zum Aktualisieren der Ansicht des Status.

>[AZURE.NOTE] Übergeordnete Programmiermodelle wie [zuverlässige Akteure Framework](service-fabric-reliable-actors-introduction.md) Abstraktion das Konzept der replikatrolle vom Entwickler.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Service Fabric-Konzepte finden Sie unter:

- [Skalierbar von Service Fabric-services](service-fabric-concepts-scalability.md)

- [Partitionierung Service Fabric-services](service-fabric-concepts-partitioning.md)

- [Definieren und Verwalten von Status](service-fabric-concepts-state.md)
