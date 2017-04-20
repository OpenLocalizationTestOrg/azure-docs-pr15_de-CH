<properties
   pageTitle="Skalierbar von Service Fabric-Services | Microsoft Azure"
   description="Beschreibt, wie Service Fabric-services"
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

# <a name="scaling-service-fabric-applications"></a>Service Fabric-Anwendung skalieren
Azure Service Fabric erleichtert die skalierbare Anwendung Load balancing Services Partitionen und Replikate auf allen Knoten in einem Cluster erstellen. Dies ermöglicht maximale Ressourcenverwendung.

Hohe Maßstab für Service Fabric-Anwendung kann auf zwei Arten erreicht werden:

1. Auf Partitionsebene skalieren

2. Skalierung auf Dienstebene name

## <a name="scaling-at-the-partition-level"></a>Auf Partitionsebene skalieren
Fabric Service unterstützt einen einzelnen Dienst in mehrere kleinere Partitionen zu partitionieren. [Partitionierung Übersicht](service-fabric-concepts-partitioning.md) enthält Informationen zu den unterstützten Partitionierungsschemas. Die Knoten in einem Cluster sind Replikate der einzelnen Partitionen verteilt. Sollten Sie einen Dienst, der reichte Partitionierungsschema mit einem niedrigen Schlüssel 0, hohe Schlüssel 99 und vier Partitionen verwendet. In einem Cluster mit drei Knoten kann der Dienst mit vier festgelegt werden, die die Ressourcen auf jedem Knoten verwenden, wie hier gezeigt:

![Partitionslayout mit drei Knoten](./media/service-fabric-concepts-scalability/layout-three-nodes.png)

Die Zahl der Knoten ermöglicht Service Fabric Ressourcen auf den neuen Knoten nutzen, indem Sie einige Replikate auf leeren Knoten. Durch Erhöhen der Anzahl von Knoten 4 hat den Dienst jetzt drei Replikationen auf jedem Knoten (verschiedene Partitionen) für besseren Nutzung der Ressourcen und der Leistung.

![Partitionslayout mit vier Knoten](./media/service-fabric-concepts-scalability/layout-four-nodes.png)

## <a name="scaling-at-the-service-name-level"></a>Skalierung auf Dienstebene name
Eine Instanz ist eine bestimmte Instanz der Anwendungsname und ein (siehe [Service Fabric-Lebenszyklus](service-fabric-application-lifecycle.md)). Beim Erstellen eines Dienstes geben Sie Partition scheme (siehe [Partitionierung Service Fabric-Services](service-fabric-concepts-partitioning.md)) verwendet werden.

Die erste Ebene der Skalierung ist nach Dienstnamen. Sie können neue Instanzen eines Diensts mit unterschiedlichen Partitionierung als Ihre älteren beschäftigt Dienstinstanzen erstellen. Dadurch können neue Dienstconsumer weniger ausgelastet Dienstinstanzen, anstatt ausgelasteten verwenden.

Eine Option zur Erhöhung der Kapazität sowie aufsteigende oder Partition zählt werden eine neue Dienstinstanz mit ein neues Partitionsschema erstellt. Dadurch Komplexität, als verwendeten Clients müssen wissen, wann und wie Sie unterschiedlich benannten Dienst.

### <a name="example-scenario-embedded-dates"></a>Beispielszenario: eingebettete Daten
Ein mögliches Szenario wäre Informationen als Teil des Dienstnamens. Beispielsweise können Sie eine Dienstinstanz mit einem bestimmten Namen für alle Kunden an 2013 und einen anderen Namen für Kunden im 2014. Dieses Benennungsschema ermöglicht programmgesteuert erhöhen die Namen je nach (als 2014 Ansätze die Dienstinstanz 2014 kann erstellt bei Bedarf).

Dieser Ansatz basiert jedoch auf den Clients mit anwendungsspezifischen Namensinformationen, die außerhalb des Service Fabric wissen.

- *Unter Verwendung einer Benennungskonvention*: 2013, wenn Ihre Anwendung geht, erstellen Sie einen Dienst namens Fabric: service2013/app. In zweiten Quartal 2013 einen anderen Dienst namens Fabric erstellen: service2014/app. Beide Dienste sind vom gleichen Diensttyp. Bei diesem Ansatz müssen Ihren Client Logik basierten auf dem Dienstnamen erstellt beschäftigen.

- *Verwenden Sie einen Suchdienst*: ein weiteres Muster ist eine sekundäre Suchdienst, den Namen des Dienstes für die gewünschte Taste angeben können. Neuen Dienstinstanzen können dann durch den Suchdienst erstellt werden. Der Suchdienst selbst beibehalten keine Daten nur Daten zu den Namen, das erstellt wird. So würde Jahr basierende obigen Beispiel Client wenden zuerst den Namen verarbeiten Daten für ein bestimmtes Jahr zu suchen und verwenden Sie diesen Service-Namen für den aktuellen Vorgang. Das Ergebnis der ersten Suche kann zwischengespeichert werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Service Fabric-Konzepte finden Sie unter:

- [Verfügbarkeit von Service Fabric-services](service-fabric-availability-services.md)

- [Partitionierung Service Fabric-services](service-fabric-concepts-partitioning.md)

- [Definieren und Verwalten von Status](service-fabric-concepts-state.md)
