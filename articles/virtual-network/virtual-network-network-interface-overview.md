<properties 
   pageTitle="Netzwerk-Schnittstellen | Microsoft Azure"
   description="Erfahren Sie mehr über Azure Netzwerkschnittstellen in Azure-Ressourcen-Manager."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Netzwerk-Schnittstellen

Eine Netzwerkschnittstelle (NIC) ist zwischen einer Virtual Machine (VM) und der Netzwerk-Software. Dieser Artikel beschreibt eine Netzwerkschnittstelle ist und Verwendung in Azure-Ressourcen-Manager-Bereitstellungsmodell.

Microsoft empfiehlt, neue Ressourcen mit dem Ressourcen-Manager-Bereitstellungsmodell, aber Sie können auch mit der Netzwerkkonnektivität im [klassischen](virtual-network-ip-addresses-overview-classic.md) Bereitstellungsmodell VMs bereitstellen. Wenn Sie das klassische Modell vertraut, gibt es wichtige Unterschiede bezüglich der VM-Netzwerke in der Ressourcen-Manager-Bereitstellungsmodell. Erfahren Sie mehr über die Unterschiede [virtuellen Netzwerk - Classic](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) lesen.

In Azure eine Netzwerkschnittstelle:

1. Eine Ressource, die erstellt werden kann, gelöscht, mit konfigurierbaren Einstellungen.
2. Müssen verbunden sein, ein Subnetz ein Azure Virtual Network (VNet) beim Erstellen. Wenn Sie nicht mit VNets vertraut sind, erfahren Sie mehr über diese [virtuellen Netzwerkübersicht](virtual-networks-overview.md) lesen. Die NIC muss mit einem VNet verbunden sein, die in derselben Azure [Speicherort](https://azure.microsoft.com/regions) und [Abonnement](../azure-glossary-cloud-terminology.md#subscription) Netzwerkkarte vorhanden ist Nach dem Erstellen eine Netzwerkkarte Subnetz verbunden zu ändern, aber kann nicht geändert werden VNet verbunden ist.
3. Hat ein Namen zugewiesen, die nach der Erstellung der Netzwerkkarte geändert werden kann. Der Name muss in einer Azure [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups)aber muss eindeutig innerhalb des Abonnements, Azure Speicherort erstellt wird, oder VNet verbunden ist. Mehrere NICs werden normalerweise innerhalb von Azure-Abonnement erstellt. Es wird empfohlen, eine Benennungskonvention, bei der Verwaltung mehrerer Netzwerkkarten einfacher entwickeln als Standardnamen. Finden Sie den [Namenskonventionen für Azure Ressourcen empfohlen](../guidance/guidance-naming-conventions.md) für Vorschläge.
4. Einer VM angefügt werden, aber nur einer einzigen VM am Netzwerkkarte vorhanden ist angefügt werden kann
5. Hat eine MAC-Adresse mit der NIC beibehalten wird, solange einer VM angefügt wird. Die MAC-Adresse beibehalten, ob die VM neu gestartet wird (vom Betriebssystem) oder (aufgehoben) und Azure-Portal, Azure PowerShell oder der Azure-Befehlszeilenschnittstelle. Ein virtueller Computer entfernt und einer anderen VM zugeordnet, erhält die NIC eine andere MAC-Adresse. Wenn die Netzwerkkarte gelöscht wird, wird anderen Netzwerkkarten die MAC-Adresse zugewiesen.
6. Eine primäre **private** *IPv4* statische oder dynamische IP-Adresse muss zugewiesen haben.
8. Zu kann einer öffentlichen IP-Adressenressource zugeordnet.
9. Unterstützt schnellere Netzwerke mit einzelnen Stamm e/a-Virtualisierung (SR-IOV) für bestimmte VM-Größen mit bestimmten Versionen des Betriebssystems Microsoft Windows Server. Erfahren Sie mehr über dieses Feature Vorschau Artikel die [beschleunigte Vernetzung für einen virtuellen Computer](virtual-network-accelerated-networking-powershell.md) .
10. Erhalten nicht Datenverkehr zu privaten IP-Adressen zugewiesen, wenn IP-Weiterleitung für die Netzwerkkarte aktiviert ist Wenn ein virtueller Computer beispielsweise Firewallsoftware ausgeführt wird, leitet Pakete nicht an IP-Adressen. Die VM muss Software weiterleiten oder Weiterleitung zwar ausgeführt, aber dazu muss IP-Weiterleitung für eine Netzwerkkarte aktiviert sein
11. Erstellt in derselben Ressourcengruppe wie die VM zugeordnet ist oder die gleichen VNet, die es verbunden ist, obwohl es nicht erforderlich sein.

## <a name="vms-with-multiple-network-interfaces"></a>VMs mit mehreren Netzwerkschnittstellen

NICs können einer VM, aber wenn dabei berücksichtigen Sie folgende zugeordnet werden:  

- Die VM muss mehrere Netzwerkkarten unterstützt. Erfahren Sie mehr über die VM NICs unterstützen, Artikel der [Windows Server-VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md) oder [Linux VM-Größen](../virtual-machines/virtual-machines-linux-sizes.md) .   
- Die VM muss mit mindestens zwei NICs erstellt werden. Wenn die VM mit nur einer Netzwerkkarte erstellt wird, auch wenn mehr als ein virtueller Speicher unterstützt, können nicht Sie zusätzliche Netzwerkkarten nach der Erstellung der VM anfügen. Solange die VM mit zwei Netzwerkkarten erstellt wurde, können Sie anfügen zusätzliche Netzwerkkarten der VM nach seiner Erstellung virtueller Speicher mehr als zwei Netzwerkkarten unterstützt.  
- Sie können sekundäre NICs (primäre NIC nicht getrennt werden) aus einer VM trennen, hat die VM mindestens drei Netzwerkkarten verbunden. NICs kann nicht getrennt werden, wenn die VM zwei hat oder weniger Netzwerkkarten an.  
- Die Reihenfolge der Netzwerkkarten von innerhalb des virtuellen Computers werden zufällige und kann auch über Azure-Infrastruktur Updates ändern. Die IP-Adressen und die entsprechenden Ethernet MAC behebt jedoch, bleibt. Angenommen Sie, dass das Betriebssystem Azure NIC1 als Eth1 identifiziert. Eth1 hat 10.1.0.100 und MAC-Adresse 00-0D-3A-B0-39-0D. Nach der Azure-Infrastruktur zu aktualisieren und neu starten, das Betriebssystem kann jetzt Azure NIC1 als Eth2 identifizieren, aber die IP- und MAC-Adressen werden die gleichen wie beim Betriebssystem Azure NIC1 als Eth1 identifiziert. Wird ein Neustart Kunden initiiert, wird die NIC-Reihenfolge innerhalb des Betriebssystems unverändert.  
- VM Mitglied eine [Verfügbarkeit festgelegt](../azure-glossary-cloud-terminology.md#availability-set)ist, müssen alle VMs innerhalb der Verfügbarkeit einer NIC oder mehrere Netzwerkkarten haben. Die VMs mehrere Netzwerkkarten, die sie haben ist gleich sein, solange jede VM zwei Netzwerkkarten besitzt.

## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zum Erstellen eines virtuellen Computers mit einer einzelnen NIC durch [Erstellen einer VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) lesen.
- So erstellen Sie einen virtuellen Computer lesen [Bereitstellen virtueller Computer mit mehreren Netzwerkkarten](virtual-network-deploy-multinic-arm-ps.md) mit mehreren NICs.
- Erfahren Sie, wie eine NIC mit mehreren IP-Konfigurationen erstellen, indem Sie [mehrere IP-Adressen für Azure virtuelle Computer](virtual-network-multiple-ip-addresses-powershell.md) lesen.
