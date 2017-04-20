<properties
   pageTitle="Service Fabric Cluster Resource Manager: Bewegung Kosten | Microsoft Azure"
   description="Übersicht der bewegungskosten für Service Fabric-services"
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

# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Bewegung Servicekosten beeinflussen Cluster Ressourcen-Manager-Optionen
Wichtig Wenn Sie versuchen, zu bestimmen, welche Änderungen zu einem Cluster und Bewertung einer Lösung werden die Gesamtkosten einer Lösung.

Verschieben Sie mindestens Service Instanzen oder Replikate Kosten CPU und Bandbreite. Statusbehaftete Dienste kostet auch den Speicherplatz auf dem Datenträger Sie eine Kopie des Status vor dem Herunterfahren alte Replikate erstellen müssen. Klar möchten Sie die Kosten einer Lösung zu minimieren, die Azure Service Fabric Cluster Resource Manager kommt mit. Aber auch nicht Solutions ignorieren möchten, die die Zuordnung von Ressourcen im Cluster erheblich verbessern.

Cluster-Ressourcen-Manager hat zwei verschiedene computing Kosten und beschränken, selbst wenn versucht wird, zum Verwalten des Clusters seine Ziele. Die erste ist, dass Cluster-Ressourcen-Manager ist ein neues Layout für den Cluster Planung jeder gezählt, die sie machen. In einem einfachen Fall man zwei Lösungen mit diesem Saldo insgesamt (Bewertung) am Ende und nehmen die mit den niedrigsten Kosten (Gesamtanzahl verschoben).

Dies funktioniert gut. Aber wie bei Standard oder statische Lasten dürfte in einem komplexen System alle verschoben wird. Einige sind wahrscheinlich teurer.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>Ändern des Replikats verschieben Kosten und Faktoren
Wie können mit Load (ein weiteres Feature von Cluster-Ressourcen-Manager), Sie dem Dienst selbst Reporting wie teuer der Dienst ist zu einem bestimmten Zeitpunkt.

Code:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost hat vier Ebenen: 0 (null), Niedrig, Mittel und hoch. Diese hängen, außer Null. NULL bedeutet, dass ein Replikat ist kostenlos und sollte nicht für die Bewertung der Lösung. Verschieben Kosten hoch ist *keine* Garantie, die nicht nur verschoben wird nicht, es sei denn, es ein guter Grund gibt das Replikat verschoben.

![Verschieben von Kosten als Faktor bei Replikate für Bewegung][Image1]

MoveCost können Sie die Lösungen, die geringste Beeinträchtigung insgesamt und einfachste und entsprechende Saldo noch angekommen sind. Ein Dienst Konzept Kosten kann relativ viel. Die häufigsten bei verschieben Kosten sind:

- Der Betrag oder Daten, die der Dienst verschieben.
- Die Kosten der Trennung der Clientverbindung. Ein primäres Replikat verschieben kostet normalerweise höher als sekundäres Replikat verschieben.
- Die Kosten für ein Flugzeug Vorgang unterbrechen. Einige Vorgänge an Daten speichern Ebene oder Operationen auf ein Clientaufruf sind kostspielig. Nach einem bestimmten Punkt möchten Sie haben Sie auf Beenden. So bump für die Dauer des Vorgangs Sie die Kosten für die Wahrscheinlichkeit zu verringern, die dem dienstreplikat oder verschieben möchten. Wenn der Vorgang abgeschlossen ist, setzen Sie es wieder.

## <a name="next-steps"></a>Nächste Schritte
- Service Fabric-Cluster Resource Manager verwendet Metriken Verbrauch und Kapazität im Cluster verwalten. Um weitere Informationen zu Messgrößen und ihre Konfigurierung checken Sie [Ressourcenverbrauch verwalten und Laden in Service Fabric mit Metriken aus](service-fabric-cluster-resource-manager-metrics.md).
- Informationen zum Cluster Resource Manager verwaltet und verteilt die Last im Cluster anzeigen Sie [Balancing Cluster Service Fabric](service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
