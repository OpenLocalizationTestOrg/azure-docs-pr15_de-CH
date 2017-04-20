<properties
   pageTitle="Service Fabric-Cluster oder skalieren | Microsoft Azure"
   description="Skalieren Sie Service Fabric-Cluster oder auf Nachfrage durch automatische Skalierung Regeln für jeden Knoten Typ-VM skalieren. Hinzufügen oder Entfernen von Knoten zu einem Cluster Service Fabric"
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


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Skalieren von Service Fabric-Cluster in oder mithilfe von Regeln automatisch skalieren

Virtual Machine sind Skala ein Azure computeressource, die zum Bereitstellen und Verwalten einer Auflistung von virtuellen Computern als Gruppe. Alle Knotentypen in einem Cluster Service Fabric definiert ist als separate VM-Skalierungsgruppe eingerichtet. Jeden Knotentyp kann dann skaliert oder unabhängig haben unterschiedliche Ports öffnen und haben unterschiedliche Kapazitäten Metriken. Erfahren sie in [Service Fabric Nodetypes](service-fabric-cluster-nodetypes.md) Dokument. Da Service Fabric VM Skalierung Typen von Knoten im Cluster aus im Backend festgelegt wird, müssen Sie Regeln für jeden Knoten Typ-VM Skalierung Automatische Skalierung einrichten.

>[AZURE.NOTE] Ihr Abonnement müssen genügend Kerne Hinzufügen neuer VMs, die diesem Cluster bilden. Es ist keine modellvalidierung derzeit so ein Bereitstellungsfehler Zeit erhalten, wenn keines der Kontingentgrenzen erreicht.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>Wählen Sie den Knoten Type-VM Maßstab skalieren

Derzeit können Sie nicht die automatische Skalierung Regeln für VM Maßstab legt das Portal, wir Azure PowerShell (1.0 +) verwenden, um die Knoten und dann automatisch skalieren Regeln hinzufügen.

Um die Liste der VM Skalierung festlegen, die den Cluster bilden, führen Sie die folgenden Cmdlets:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Regeln Sie automatische Skalierung für die Typ-VM Skalierung Knotengruppe

Wenn Cluster mehrere Knoten verfügt, wiederholen Sie für jeden Knoten Typen-VM Skalierung festgelegt (in oder out) skalieren möchten. Ziehen Sie der Anzahl der Knoten, bevor Sie die automatische Skalierung einrichten müssen. Die minimale Anzahl von Knoten, die den primären Knotentyp treibt Zuverlässigkeit auf gewählte. Weitere Informationen zur [Zuverlässigkeit](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Verkleinerung des primären Knotens geben Sie, der kleiner als die minimale Anzahl Stellen Cluster instabil oder Erliegen bringen. Datenverlust für die Anwendung und die Systemdienste möglich.

Feature automatische Skalierung wird derzeit nicht von Lasten gesteuert, der Anwendung Fabric Service melden können. So skalieren diesmal die automatische Skalierung erhalten Sie lediglich die Leistung gesteuert wird jede VM ausgegeben werden Leistungsindikatoren Satz Instanzen  

Folgen Sie dieser Anleitung [Automatische Skalierung für jeden VM Skalierung einrichten](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] In einem Szenario Herunterskalieren, wenn Ihren Knotentyp eine Haltbarkeit Gold und Silber hat müssen Sie [Entfernen-ServiceFabricNodeState-Cmdlet](https://msdn.microsoft.com/library/azure/mt125993.aspx) mit den entsprechenden Namen aufrufen.

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>VMs zu einem Knoten Typ-VM Skalierung manuell hinzufügen

Anweisungen Sie [Schnellstart Vorlagenkatalog](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) Ändern der Anzahl der VMs in jeder Nodetype. 

>[AZURE.NOTE] Hinzufügen von VMs dauert, also nicht die Additions unmittelbar zu erwarten. So planen Sie Kapazität rechtzeitig für 10 Minuten vor die VM-Kapazität für Replikate / service-Instanzen platziert.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>Entfernen Sie VMs aus der primären Knoten Typ-VM skalieren

>[AZURE.NOTE] Service Fabric-Systemdienste ausführen Geben Sie primären Knoten im Cluster. So sollte niemals heruntergefahren oder verkleinern die Anzahl der Instanzen, Knotentypen garantiert kleiner als die Zuverlässigkeit-Tier. Finden Sie [die Details auf Zuverlässigkeit Ebenen hier](service-fabric-cluster-capacity.md). 

Sie müssen folgende Schritte eine VM-Instanz ausführen. Dies ermöglicht die Dienste und die statusbehaftete Dienste ordnungsgemäß heruntergefahren werden, für die VM-Instanz, die Sie entfernen und neue Replikate auf andere Knoten erstellt.

1. Führen Sie [Deaktivieren ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) mit "RemoveNode" deaktivieren den Knoten (die höchste Instanz Knotentyps) entfernt werden soll.

2. Führen Sie [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) zu der Knoten deaktiviert tatsächlich gewechselt wurde. Wenn dies nicht der Fall ist, warten Sie, bis der Knoten deaktiviert ist. Sie können nicht dadurch schnell.

2. Anweisungen Sie [Schnellstart Vorlagenkatalog](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) Ändern der Anzahl der VMs von einer in diesem Nodetype. Die Instanz entfernt ist die höchste VM-Instanz. 

3. Wiederholen Sie die Schritte 1 bis 3 bei Bedarf jedoch nie verkleinern Sie die Anzahl der Instanzen in den primären Knoten unter Was Tier Zuverlässigkeit gewährleistet. Finden Sie [die Details auf Zuverlässigkeit Ebenen hier](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>Entfernen Sie VMs aus den nicht primären Knoten Typ-VM skalieren

>[AZURE.NOTE] Für eine statusbehaftete Dienst benötigen Sie eine bestimmte Anzahl von Knoten zu werden immer nach Verfügbarkeit beibehalten des Status des Dienstes. Mindestens benötigen Sie die Anzahl der Knoten gleich Ziel Replica Set Anzahl der Partition/Service. 

Sie benötigen die Ausführung der folgenden Schritte eine VM-Instanz gleichzeitig. Dadurch wird die Dienste und die statusbehaftete Dienste ordnungsgemäß für die VM-Instanz heruntergefahren werden, die Sie entfernen und neue Replikate erstellt andere Stelle.

1. Führen Sie [Deaktivieren ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) mit "RemoveNode" deaktivieren den Knoten (die höchste Instanz Knotentyps) entfernt werden soll.

2. Führen Sie [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) zu der Knoten deaktiviert tatsächlich gewechselt wurde. Wenn nicht warten, bis der Knoten deaktiviert ist. Sie können nicht dadurch schnell.

2. Anweisungen Sie [Schnellstart Vorlagenkatalog](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) Ändern der Anzahl der VMs von einer in diesem Nodetype. Dies entfernt jetzt die höchste VM-Instanz. 

3. Wiederholen Sie die Schritte 1 bis 3 bei Bedarf jedoch nie verkleinern Sie die Anzahl der Instanzen in den primären Knoten unter Was Tier Zuverlässigkeit gewährleistet. Finden Sie [die Details auf Zuverlässigkeit Ebenen hier](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Verhalten können Sie in Service Fabric-Explorer beobachten.

Beim Skalieren eines Clusters wider Explorer Fabric Service die Anzahl der Knoten (VM Skalierung festlegen Instanzen), die Teil des Clusters sind.  Jedoch beim Skalieren sehen ein Clusters Sie die entfernten Knoten-VM-Instanz in einem fehlerhaften Zustand angezeigt, wenn Sie mit den entsprechenden Namen [Entfernen ServiceFabricNodeState Cmd](https://msdn.microsoft.com/library/mt125993.aspx) aufrufen.   

Hier ist die Erklärung für dieses Verhalten.

Im Service Fabric-Explorer aufgeführten Knoten sind die Service Fabric Systemdienste (FM speziell) kennt die Anzahl der Knoten des Clusters hatte/hat. Beim Skalieren der VM-Skalierung festgelegt VM wurde gelöscht, aber FM-Dienst noch denkt, dass der Knoten (die die gelöschten VM zugeordnet wurde) wird. Service Fabric-Explorer weiterhin so Knotens angezeigt (wenn der Integritätsstatus Fehler oder unbekannt).

Um sicherzustellen, dass ein Knoten entfernt wird, wenn ein virtueller Computer entfernt wird, haben Sie zwei Optionen:

1) Wählen Sie eine Haltbarkeit Gold und Silber (Verfügbare bald) für welche Knoten im Cluster so erhalten Sie die Infrastruktur-Integration. Das wird dann automatisch entfernt Knoten aus unserem System Services (FM) Wenn Sie verkleinern.
Sehen Sie [die hier auf Haltbarkeit](service-fabric-cluster-capacity.md)

2) Nach die VM-Instanz verkleinert wurde, müssen Sie das [Cmdlet "Remove-ServiceFabricNodeState"](https://msdn.microsoft.com/library/mt125993.aspx)aufrufen.

>[AZURE.NOTE] Service Fabric-Cluster benötigen eine bestimmte Anzahl von Knoten werden jederzeit die Verfügbarkeit und Beibehalten des Status - genannt "Quorum verwalten". Daher ist es in der Regel unsicher auf allen Computern im Cluster Herunterfahren, wenn Sie zuerst eine [vollständige Sicherung des Systemstatus](service-fabric-reliable-services-backup-restore.md)ausgeführt haben.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den folgenden auch Cluster Kapazität planen und Aktualisieren eines Clusters Partitionierung Services erfahren:

- [Planung der Cluster-Kapazität](service-fabric-cluster-capacity.md)
- [Cluster-upgrades](service-fabric-cluster-upgrade.md)
- [Partition statusbehaftete Dienste für höchste](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
