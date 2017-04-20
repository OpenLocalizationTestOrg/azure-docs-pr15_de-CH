<properties 
   pageTitle="Migrieren von Gruppen mit einem regionalen virtuellen Netzwerk (VNet)"
   description="Informationen Sie zum Migrieren von Gruppen zu regionalen vnets"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Migrieren von Gruppen mit einem regionalen virtuellen Netzwerk (VNet)

Eine Gruppe können Sie sicherstellen, dass Ressourcen innerhalb der gleichen Gruppe Affinität physisch vom Server gehostet werden, die nahe beieinander liegende diese Ressourcen schneller kommunizieren. In der Vergangenheit wurden Gruppen die Voraussetzung für die Erstellung von virtuellen Netzwerken (VNets). Zu diesem Zeitpunkt kann der Network Manager-Dienst, der VNets verwaltet nur innerhalb einer Gruppe von Servern oder Skalierungseinheit arbeiten. Verbesserte Architektur stiegen Rahmen zu einem Bereich.

Durch diese verbesserte Architektur werden Gruppen nicht empfohlen oder erforderlich für virtuelle Netzwerke. Die Verwendung von Gruppen für VNets wird nach Regionen ersetzt. VNets Regionen zugeordnet sind, heißen regionalen VNets.

Zusätzlich empfehlen wir, dass Sie Gruppen im Allgemeinen nicht. Neben der Anforderung VNet wurden Gruppen wichtig, um sicherzustellen, dass Ressourcen wie Compute und Speicher nebeneinander platziert wurden. Jedoch entfallen aktuelle Azure Netzwerkarchitektur solcher Platzierung. Die einige verbleibenden Fälle, Sie möglicherweise eine Gruppe möchten, finden Sie unter [Gruppen und VMs](#Affinity-groups-and-VMs) .

## <a name="creating-and-migrating-to-regional-vnets"></a>Erstellen und regionalen VNets migrieren

Vorwärts, beim Erstellen neuer VNets *Region*verwenden. Sie sehen dies als Option im Verwaltungsportal. Beachten Sie, dass in der Netzwerk-Konfigurationsdatei als *Speicherort*zeigt.

>[AZURE.IMPORTANT] Zwar weiterhin technisch möglich, ein virtuelles Netzwerk erstellen, das einer Gruppe zugeordnet ist, gibt es keinen zwingenden Grund dafür. Viele neue Features, wie Netzwerk-Sicherheitsgruppen stehen nur bei Verwendung einer regionalen VNet und stehen nicht für virtuelle Netzwerke Gruppen zugeordnet sind.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>Über VNets derzeit zugeordnete Gruppen

VNets derzeit Gruppen zugeordneten sind für die Migration zu regionalen VNets aktiviert. Um einen regionalen VNet zu migrieren, gehen Sie folgendermaßen vor:

1. Exportieren Sie Netzwerk-Konfigurationsdatei. Sie können PowerShell oder Verwaltungsportal. Informationen über das Verwaltungsportal finden Sie unter [Konfigurieren der VNet mit einer Netzwerk-Konfigurationsdatei](virtual-networks-using-network-configuration-file.md).

1. Bearbeiten Sie Ihr Netzwerk Konfigurationsdatei ersetzen die alten Werte mit den neuen Werten. 

    > [AZURE.NOTE] **Speicherort** ist der Bereich, den Sie für die Gruppe angegeben, die der VNet zugeordnet ist. Wenn Ihr VNet eine Gruppe zugeordnet, die ist bei der Migration im Westen der USA befindet, muss Ihr Lagerort US West zeigen. 
    
    Bearbeiten Sie die folgenden Zeilen in der Konfigurationsdatei Netzwerk durch eigene Werte ersetzen: 

    **Alter Wert:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 

    **Neuer Wert:** \<VirtualNetworkSitename = "VNetUSWest" Location = "West US"\>

1. Speichern der Änderungen und [Importieren](virtual-networks-using-network-configuration-file.md) die Netzwerkkonfiguration in Azure

>[AZURE.NOTE] Diese Migration verursacht keine Ausfallzeiten auf Ihre Dienste.

## <a name="affinity-groups-and-vms"></a>Gruppen und VMs

Wie bereits erwähnt, werden Gruppen nicht für virtuelle Computer empfohlen. Nur wenn mehrere VMs absolut niedrigste Netzwerklatenz zwischen VMs benötigen, sollten Sie eine Gruppe verwenden. VMs in einer Gruppe platzieren, werden die VMs alle in der gleichen Compute Cluster oder Skalierung platziert.

Es ist wichtig zu beachten, dass eine Gruppe mit zwei, möglicherweise negative folgen können:

- Mehrere VM-Größen werden mit den VM-Größen berechnen Skalierungseinheit angebotenen begrenzt.

- Es gibt eine höhere Wahrscheinlichkeit nicht zuweisen eine neue VM. Dies passiert, wenn die spezifische Einheit für die Gruppe Kapazität.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Wenn Sie einen virtuellen Computer in einer Gruppe haben

Virtuelle Computer, die derzeit in einer Gruppe müssen nicht aus der Gruppe entfernt werden.

Sobald ein virtueller Computer bereitgestellt wird, wird eine einzelne Skalierungseinheit bereitgestellt. Gruppen können mehrere VM-Größen für eine neue VM-Bereitstellung, aber alle vorhandenen VM bereitgestellt wird ist bereits auf den Satz von VM Größen in der Skala in der VM bereitgestellt wird. Aus diesem Grund haben einen virtuellen Computer aus der Gruppe entfernt keine Wirkung.
 
