<properties
   pageTitle="Service Fabric Cluster Resource Manager - Platzierungsrichtlinien | Microsoft Azure"
   description="Übersicht über zusätzliche Platzierungsrichtlinien und Regeln für Fabric-Dienst"
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

# <a name="placement-policies-for-service-fabric-services"></a>Platzierungsrichtlinien für Fabric-Dienst
Gibt es viele verschiedene weitere Regeln, die Sie interessieren, wenn Cluster Service Fabric übergreifende Ende über geografische Entfernung sagen mehrere Rechenzentren oder Azure Regionen oder Ihrer Umgebung umfasst mehrere Bereiche geopolitische Steuerelement (oder einige andere Fall haben Sie interessieren rechtlichen oder politischen Grenzen die Distanzen wirken tatsächlichen Performance-Latenz). Die meisten Eigenschaften und Platzierung Einschränkungen konfiguriert werden, aber einige sind komplizierter. Um einfacher bieten wir diese zusätzlichen Befehlen. Wie können Platzierungsrichtlinien mit anderen Einschränkungen Platzierung für Dienst pro benannte Instanz einzeln konfiguriert werden.

## <a name="specifying-invalid-domains"></a>Ungültiger Domänen
Platzierungsrichtlinie InvalidDomain können Sie angeben, dass eine bestimmte Fehlerdomäne erfolgen ungültig ist. Diese Richtlinie gewährleistet, dass ein bestimmter Dienst in einem bestimmten Bereich beispielsweise für geopolitische oder Firmennetzwerk Gründen nicht ausgeführt wird. Mehrere ungültige Domänen können über eigene Richtlinien angegeben werden.

![Ungültige Domäne Beispiel][Image1]

Code:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Erforderlichen Domänen
Die erforderliche Platzierung Domänenrichtlinie muss statusbehaftete Replikate oder Staatenlose Dienstinstanzen für den Dienst in der angegebenen Domäne vorhanden sein. Über Richtlinien können mehrere Domänen erforderliche angegeben werden.

![Erforderliche Domäne Beispiel][Image2]

Code:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Festlegen einer bevorzugten Domäne für primäre Replikate
Die bevorzugte Primärdomäne ist eine interessante da an die Fehlerdomäne können in der primären platziert werden soll, wenn es möglich ist. Wenn alle fehlerfrei ist erhalten die primäre in dieser Domäne. Sollten die Domäne oder die primäre Replikat nicht oder aus irgendeinem Grund die primäre an einem anderen Speicherort migriert werden heruntergefahren werden. Wenn dieser Standort im Bereich bevorzugte ist, dann verschiebt möglichst Cluster Resource Manager es wieder bevorzugte Domäne. Diese Einstellung ist natürlich nur sinnvoll für statusbehaftete Dienste. Diese Richtlinie ist besonders in Clustern der Azure-Regionen oder mehrere Rechenzentren erstreckt. In diesen Fällen verwenden die Speicherorte für Redundanz, aber möchten, dass die primäre Replikate an einer bestimmten Position platziert werden, um geringere Latenz für Operationen bereitstellen, gehen die primäre (schreibt und auch standardmäßig alle Lesevorgänge von primären).

![Bevorzugter primärer Domänen und Failover][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Die Replikate an alle Domänen und dass Verpackung
Sie können eine andere Richtlinie ist Replikate immer verfügbar Fehlerdomänen verteilt werden. Dies wird in den meisten Fällen Cluster fehlerfrei ist, jedoch sind standardmäßig degenerierter Fall, Replikate für eine bestimmte Partition vorübergehend Ende, in einen einzelnen Fehler oder Domäne aktualisieren gepackt. Z. B. Angenommen, die zwar der Cluster 9 Knoten in 3 Fehlerdomänen (0, 1 und 2 hat) der Dienst 3 Replikate hat für diese Replikate Fehler Bereiche 1 und 2 verwendeten Knoten ausgefallen, und durch Kapazitätsprobleme kein Knoten in diesen Domänen gültig waren. Wenn Service Fabric Ersatz für diese Replikate erstellen, Cluster Resource Manager müssten sie in Fehlerdomäne 0 jedoch Situationen, in denen Fehlerdomäne Einschränkung verletzt wird, erstellt. Darüber hinaus die Möglichkeit, dass die gesamten Replikatgruppe verloren (wenn FD 0 Permananently verloren gegangen sein). (Weitere Informationen zu Integritätsregeln und Einschränkung Auschecken Prioritäten im allgemeinen [in diesem Thema](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Wenn Sie jemals einen Warnhinweis wie gesehen "Lastenausgleich hat die Verletzung einer Einschränkung für dieses Replikat: Fabric: /<some service name> sekundären Partition <some partition ID> die Einschränkung verletzt: FaultDomain" Sie haben diese Bedingung oder etwas erreicht. Situationen sind normalerweise vorübergehend (Knoten nicht lange bleiben oder wenn sie und wir müssen es sind andere Knoten im richtigen Fehlerdomänen gelten), aber einige Arbeitslasten, die eher Verfügbarkeit für das Risiko des Verlustes ihrer Replikate handeln würde. Hierzu können wir angeben die "RequireDomainDistribution"-Richtlinie, die garantiert, dass keine zwei Replikate von der gleichen Partition immer in der gleichen Domäne Fehler oder Aktualisierung zulässig sind.

Einige Arbeitslasten hätte lieber die vorgegebene Anzahl an Replikaten (Kopien des Zustands) jederzeit gegen total Domäne Fehler und wissen, dass sie normalerweise lokalen Zustand wiederherstellen können andere Ausfallzeiten lieber früher als die Richtigkeit und Datenverlust Probleme riskieren ergreifen würden. Da die meisten Produktionsarbeitslasten mit mehr als 3 ausführen, ist standardmäßig nicht Domäne Verteilung erfordern Lastausgleich und Failover normalerweise Fälle zu behandeln, auch wenn dies vorübergehend eine Domäne mehrere Replikate in es verpackt wurde.

Code:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nun wäre es möglich, diese Konfigurationen nicht geografisch übergreifende Dienste in einem Cluster verwenden? Sie könnten Sie! Aber gibt es kein guten Grund zu – besonders die erforderlichen ungültig und bevorzugte Domänenkonfigurationen vermieden werden, wenn Sie tatsächlich einen geografisch verteilte Cluster ausführen - keinen Sinn macht, erzwingen eine bestimmte Arbeitslast in einem einzigen Rack ausführen oder ein Segment des lokalen Cluster anderen bevorzugen, wenn es gibt verschiedene Arten von Hardware oder Arbeitslast Segmentierung auf , und Fälle über normale Platzierung Einschränkungen behandelt werden.

## <a name="next-steps"></a>Nächste Schritte
- Für Weitere Informationen zu anderen Optionen zur Konfiguration Auschecken Thema andere Cluster Resource Manager Konfigurationen services verfügbaren [Informationen zum Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
