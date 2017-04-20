<properties
   pageTitle="Defragmentierung Metriken in Azure Service Fabric | Microsoft Azure"
   description="Eine Übersicht über die Defragmentierung oder Verpackung als Strategie für Metriken in Service Fabric"
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

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentierung von Metriken und Fabric Service
Service Fabric Cluster Resource Manager ist hauptsächlich mit in die Last verteilen – sicherstellen, dass alle Knoten im Cluster ebenso verwendet werden. Dies ist normalerweise die sicherste und smartest Layout in Fehler überleben, da sie sicherstellt, dass bestimmte Fehler, einige Großteil einer bestimmten Arbeitslast nicht. Service Fabric Cluster Resource Manager unterstützt auch eine andere Strategie ist die Defragmentierung. Defragmentierung bedeutet im Allgemeinen, anstatt die Nutzung einer Metrik im Cluster verteilen wir tatsächlich sollten sie konsolidieren. Dies ist Glück Umkehrung der normalen Strategie – statt Minimierung der durchschnittliche Standardabweichung der metrischen Auslastung für eine bestimmte Metrik Cluster optimieren, zunächst erhöht Abweichung optimieren. Aber warum dieser Strategie?

Auch wenn Sie die Last gleichmäßig auf alle Knoten im Cluster verteilt haben haben dann Sie einige Ressourcen gegessen, die die Knoten an. Normalerweise kein Problem, aber gelegentlich einige Arbeitslasten Dienste sind sehr groß und verbraucht den meisten Knoten erstellen – z. B. 75 % bis 95 % des Knotens Ressourcen Ende für eine einzelne Instanz oder Replikat. Dieses Problem nicht, Cluster Resource Manager erkennt zum Zeitpunkt der Erstellung Service, die reorganize Cluster Platz für diese große Arbeitslast und zur Ausführung benötigt, aber in der Zwischenzeit, Arbeitslast warten im Cluster geplant werden muss.

Da die Planung neuer Arbeitslasten in der Regel zumindest ein wenig Wartezeit vertraulich ist brauchen wir nichts anders können wir manchmal Schlag rechts diesen SLAs ist viel Zustand zu bewegen, besonders wenn Arbeitslasten im Cluster normalerweise groß sind (und daher dauert länger im Cluster verschieben). Wir Erstellungszeiten Simulationen auf echten Clusters gemessen, sahen wir, dass Dienste groß genug und Cluster wurde relativ verwendet, dass die große Dienstleistungen verlangsamt würde und wir dies durch die Einführung der Richtlinie Defragmentierung Metriken verbessern können.

Wie Datei erstellen oder Zugriff kann verlangsamt erhalten eine Festplatte fragmentiert wurde, konnte das Laufwerk defragmentiert, damit größere zusammenhängende Blöcke zur Verfügung standen beschleunigt konfigurierbaren Defragmentierung Metriken zu der Cluster Ressourcen-Manager proaktiv versuchen, die Last der Dienste in weniger Knoten verkleinern, damit Platz für große Services (fast) immer vorhanden ist , um schnell erstellt werden. Meistens wird nicht benötigt, da Dienste sollten in der Regel gering und ist daher nicht schwer, Platz finden, aber wenn Sie große Service und sie schnell erstellt (und akzeptieren Nachteile wie höhere Impactfulness von Fehlern und einige Ressourcen bleiben ungenutzt, warten sie Arbeitslasten geplant werden) wird die Defragmentierung Strategie für Sie.

Im folgenden Diagramm bietet eine visuelle Darstellung der zwei verschiedene Cluster, der defragmentiert wird und die nicht. Berücksichtigen Sie bei balanced verbringen die wäre eines der größten Serviceobjekte muss eine neue erstellt, defragmentierten Cluster sofort auf Knoten 4 oder 5 Platzierung konnte gegenüber.

![Ausgewogene und defragmentiert Cluster vergleichen][Image1]

## <a name="defragmentation-pros-and-cons"></a>Defragmentierung vor- und Nachteile
Was sind die grundlegende Kompromisse? Umfassende Messung der Arbeitslasten vor Defragmentierung Metriken empfohlen. Hier ist eine kurze Tabelle überlegen:

| Defragmentierung Pros  | Defragmentierung Nachteile |
|----------------------|----------------------|
|Ermöglicht schnellere Erstellung von großen services | Konzentriert sich auf weniger Knoten erhöhen Konflikte laden
|Geringere Daten während der Erstellung    | Fehler können beeinträchtigt Weitere Dienste und weitere Abwanderung
|Ermöglicht umfassende Beschreibung an und die Rückgewinnung von Speicherplatz | Komplexere Gesamtkonfiguration Ressourcenmanagement

Defragmentierte und normale Metriken in demselben Cluster mischen und der Ressourcen-Manager tun sollten um sicherzustellen, dass Sie eine Layout, die Teil der Defragmentierung Metriken konsolidiert, wie er versucht, sich den Rest kann. Die genauen Ergebnisse erhalten Sie hängt die Anzahl der Netzwerklastenausgleich Metriken gegenüber der Defragmentierung Metriken und Gewichte von aktuellen lädt, etc..

## <a name="configuring-defragmentation-metrics"></a>Konfigurieren der Defragmentierung Metriken
Konfigurieren der Defragmentierung Metriken ist eine globale Entscheidung im Cluster und einzelne Metriken für die Defragmentierung ausgewählt werden können:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Nächste Schritte
- Der Cluster-Ressourcen-Manager hat viele Optionen für die Beschreibung des Clusters. Erfahren Sie lesen mehr darüber Sie diesen Artikel zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)
- Metriken sind wie Service Fabric-Cluster Resource Manager Verbrauch und Kapazität im Cluster verwaltet. Erfahren Sie lesen mehr über sie und ihre Konfigurierung Sie [diesen Artikel](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
