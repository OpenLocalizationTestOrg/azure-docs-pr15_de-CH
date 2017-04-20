<properties
   pageTitle="Planen der Kapazität Cluster Service Fabric | Microsoft Azure"
   description="Service Fabric-Cluster Kapazität Planung. NodeTypes, Haltbarkeit und Zuverlässigkeit Ebenen"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric-Cluster Kapazität Planung

Für jede produktionsbereitstellung ist Planung ein wichtiger Schritt aus. Hier sind einige der Elemente, die als Teil dieses Prozesses zu berücksichtigen.

- Die Anzahl der benötigten Cluster mit Knotentypen
- Die Eigenschaften der einzelnen Knotentyp (Größe, primäre Internetzugriff VMs usw.)
- Zuverlässigkeit und Haltbarkeit Merkmale des Clusters

Lassen Sie uns kurz jedes dieser Elemente.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Die Anzahl der benötigten Cluster mit Knotentypen

Zunächst müssen Sie herausfinden, was der Cluster erstellen für verwendet werden und welche Programme Sie in diesem Cluster Bereitstellung planen. Wenn Sie nicht auf den Zweck des Clusters deaktiviert sind, Sie wahrscheinlich noch nicht bereit, dem Kapazitätsplanungsprozess eingeben.

Richten Sie die Anzahl der benötigten Cluster mit Knotentypen ein.  Jeder Knotentyp ist Virtual Machine Skalierung festlegen zugeordnet. Jeden Knotentyp kann dann skaliert werden oder unabhängig haben unterschiedliche Ports öffnen und haben unterschiedliche Kapazitäten Metriken. Daher kommt die Entscheidung die Anzahl der Knoten im Wesentlichen auf Folgendes:

- Verfügt die Anwendung über mehrere Dienste und müssen diese öffentlichen Internetzugriff sein? Einsatzbeispiele enthalten ein Front-End-Gatewaydienst, der Eingaben von einem Client empfängt und eine oder mehrere Back-End-Dienste, die Kommunikation mit Front-End-Services. So Ende bei mindestens zwei Knoten aufweisen.

- Haben Ihre Dienste (die die Anwendung bilden) Infrastruktur benötigt mehr Arbeitsspeicher oder zusätzliche CPU-Zyklen? Lassen Sie uns beispielsweise die bereitzustellende Anwendung enthält ein Front-End- und Back-End-Service. Front-End-Dienst kann auf kleinere VMs (VM-Größen wie D2) ausgeführt, die mit dem Internet geöffneten Ports.  Back-End-Dienst steht jedoch Berechnung Intensive und muss auf größere VMs (mit VM wie D4 D6 D15) ausführen, die nicht Internet.

 In diesem Beispiel auch entscheiden können, alle Dienste auf einem Knotentyp sollten Sie in einem Cluster mit zwei Knoten platzieren.  Dadurch wird für jeden Knoten unterschiedliche Eigenschaften virtueller Speicher oder Internet-Verbindung haben. Die Anzahl der VMs kann unabhängig voneinander, skaliert werden.  

- Da die Zukunft vorhersagen können, mit Fakten, denen Sie kennen und Anzahl der Knotentypen, die Anwendung zu. Sie können immer später hinzufügen oder entfernen Knotentypen. Service Fabric-Cluster muss mindestens ein Knoten sein.

## <a name="the-properties-of-each-node-type"></a>Die Eigenschaften jedes Knotens

Der **Knotentyp** sehen als Rollen im Cloud-Dienste. Knotentypen definieren VM-Größe, Anzahl der VMs und deren Eigenschaften. Alle Knotentypen in einem Cluster Service Fabric definiert ist als separaten virtuellen Skalierung Set eingerichtet. VM sind Skala ein Azure compute-Ressource, die Sie zum Bereitstellen und Verwalten einer Auflistung von virtuellen Computern als Gruppe verwenden können. Definiert als unterschiedliche VM Maßstab legt fest jeden Knotentyp kann dann skaliert werden, haben unterschiedliche Ports öffnen, oder haben andere Speicherkapazität Metriken.

Cluster kann mehrere Knotentyp aber Hauptknotentyp (den ersten, die im Portal definieren) müssen mindestens fünf VMs für Produktionsarbeitslasten zur Cluster (oder mindestens drei VMs für Test-Cluster). Wenn Sie den Cluster mithilfe einer Ressourcen-Manager-Vorlage erstellen, finden ein Attribut **ist primäre** unter Knoten Typdefinition Sie. Der Hauptknotentyp ist der Knotentyp der Systemdienste Service Fabric platziert sind.  

### <a name="primary-node-type"></a>Primäre Knoten
Für einen Cluster mit mehreren Knoten müssen Sie eine primäre zu wählen. Hier sind die Merkmale des primären Knotentyp:

- Haltbarkeit Ebene wählen Sie die Mindestgröße VMs für den primären Knotentyp bestimmt. Der Standardwert für die Dauerhaftigkeit Tier ist Bronze. Scrollen Sie für Details auf Haltbarkeit Tier und die Werte können.  

- Zuverlässigkeit Ebene wählen Sie die minimale Anzahl von VMs für den primären Knotentyp bestimmt. Der Standardwert für Zuverlässigkeit Stufe ist Silber. Scrollen Sie für Details auf die Ebene Zuverlässigkeit und Werte können.

- Service Fabric-Systemdienste (z. B. Cluster-Manager-Dienst oder Image Store Service) werden auf dem Primärknoten und so die Zuverlässigkeit und Haltbarkeit des Clusters wird anhand der Zuverlässigkeit Tier und Haltbarkeit Tier-Wert für den primären Knotentyp wählen.

![Screenshot eines Clusters, die zwei Knotentypen ][SystemServices]


### <a name="non-primary-node-type"></a>Nicht-primäre Knoten
Für einen Cluster mit mehreren Knoten gibt einen Hauptknotentyp und die restlichen nicht primär. Hier sind die Merkmale eines Knotentyps nicht primäre:

- Haltbarkeit Ebene wählen Sie die Mindestgröße der VMs für diesen Knotentyp bestimmt. Der Standardwert für die Dauerhaftigkeit Tier ist Bronze. Scrollen Sie für Details auf Haltbarkeit Tier und die Werte können.  

- Die minimale Anzahl von VMs für diesen Knotentyp möglich. Wählen Sie jedoch diese Zahl die Anzahl der Replikationen/Anwendungsdienste, die in diesem Knoten ausgeführt werden soll. Die Anzahl der VMs in einem Knoten kann erhöht werden, nachdem Sie den Cluster bereitgestellt haben.


## <a name="the-durability-characteristics-of-the-cluster"></a>Die Haltbarkeit Merkmale des Clusters

Haltbarkeit Tier wird das System die Berechtigungen an, die Ihre VMs mit der zugrunde liegenden Infrastruktur Azure verwendet. Der Hauptknotentyp ermöglicht dieses Privileg Service Fabric VM Infrastruktur Anfragen (wie VM Neustart, VM neu abbilden oder VM-Migration), die das Quorum Auflagen für die Dienste und die statusbehaftete Dienste angehalten. In den nicht primären Knoten kann dieses Recht Service Fabric anhalten Anfragen Infrastruktur VM VM Neustart VM neu abbilden, VM-Migration usw., die das Quorum Auflagen für die statusbehaftete Dienste ausgeführt.

Dieses Recht wird in die folgenden Werte angegeben:

- Gold - Infrastruktur Jobs kann für eine Dauer von 2 Stunden pro UD angehalten

- Silber - Infrastruktur Jobs kann für eine Dauer von 30 Minuten pro UD angehalten werden (Dies ist derzeit nicht für die Verwendung aktiviert. Sobald aktiviert ist, werden standard VMs single-Core oder höher verfügbar).

- Bronze - keine Berechtigungen. Dies ist die Standardeinstellung.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Die Zuverlässigkeit des Clusters

Zuverlässigkeit Tier wird verwendet, um die Anzahl der Replikationen Systemdienste festlegen, die auf dem primären Knoten im Cluster ausgeführt werden soll. Weitere Anzahl der Replikate, desto zuverlässiger sind die Dienste im Cluster.  

Zuverlässigkeit Tier kann folgende Werte annehmen.

- Platin - führen Sie die Dienste mit einer Replikatgruppe Anzahl 9

- Gold - führen Sie die Dienste mit einer Replikatgruppe Anzahl 7

- Silber - führen Sie die Dienste mit einer Replikatgruppe Anzahl 5

- Bronze - führen Sie die Dienste mit einer Replikatgruppe Anzahl 3

>[AZURE.NOTE] Zuverlässigkeit Tier gewählte bestimmt die Mindestanzahl der Knoten müssen Ihren primären Knotentyp. Die Zuverlässigkeit Ebene hat keinen Einfluss auf die maximale Größe des Clusters. So können Sie einen Cluster mit 20 Knoten, ist am Bronze Zuverlässigkeit ausgeführt.

 Sie können die Zuverlässigkeit des Clusters von einer Ebene auf eine andere aktualisieren. Dies löst Cluster-Upgrades, System Services Replikat ändern Anzahl fest benötigt. Warten Sie auf Aktualisierung ausgeführt, bevor andere Änderungen zum Cluster Knoten usw. hinzufügen.  Sie können die Aktualisierung auf Service Fabric-Explorer oder [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx) überwachen

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss Ihrer Kapazität, Planen und Einrichten eines Clusters lesen Sie Folgendes:
- [Service Fabric-Cluster-Sicherheit](service-fabric-cluster-security.md)
- [Fabric Service Health Model Einführung](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
