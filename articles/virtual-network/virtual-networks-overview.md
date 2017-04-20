<properties
   pageTitle="Übersicht über Azure Virtual Network (VNet)"
   description="Erfahren Sie mehr über virtuelle Netzwerke (VNets) in Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="virtual-network-overview"></a>Virtuelles Netzwerk (Übersicht)

Ein Azure virtual Network (VNet) ist eine Darstellung des Netzwerks in der Cloud.  Es ist eine logische Isolierung der Azure-Cloud für Ihr Abonnement. Sie können die IP-Adressblöcke, DNS-Einstellungen Sicherheitsrichtlinien und Routetabellen im Netzwerk vollständig steuern. Können auch das VNet in Subnetzen segment und Azure IaaS virtuelle Maschinen (VMs) starten bzw. [Cloud Services (PaaS-Instanzen)](../cloud-services/cloud-services-choose-me.md). Darüber hinaus können Sie mit Ihrem lokalen Netzwerk mit einer der [Optionen für die Netzwerkkonnektivität](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) in Azure das virtuelle Netzwerk verbinden. Im Wesentlichen können Sie Ihr Netzwerk Azure Kontrolle auf IP-Adresse mit Unternehmen erweitern Azure bietet.

VNets besser zu verstehen, betrachten Sie die Abbildung zeigt eine vereinfachte lokalen Netzwerk.

![Lokalen Netzwerk](./media/virtual-networks-overview/figure01.png)

Die obige Abbildung zeigt ein lokales Netzwerk an das öffentliche Internet über einen Router verbunden. Sie sehen auch eine Firewall zwischen dem Router und hosting eines DNS-Servers und eine DMZ. Die Webserverfarm ist Lastenausgleich einer Lastenausgleichshardware interne Subnetz verbraucht, die für das Internet verfügbar gemacht. Das interne Subnetz wird aus der DMZ durch einen anderen Firewall und Active Directory-Domänencontroller Hostserver, Datenbankserver und Anwendungsserver getrennt.

Im gleiche Netzwerk kann in Azure gehostet werden, wie in der folgenden Abbildung dargestellt.

![Azure virtual network](./media/virtual-networks-overview/figure02.png)

Beachten Sie die Azure-Infrastruktur wie die Rolle des Routers Zugriff von Ihrem VNet an das öffentliche Internet ohne jede Konfiguration annimmt. Firewalls können vom Netzwerksicherheitsgruppen (NSGs) auf jedem einzelnen Subnetz angewendet ersetzt werden. Und physischen Lastverteiler durch Internet gegenüberliegende und internen Lastenausgleich in Azure ersetzt.

>[AZURE.NOTE] Es gibt zwei Bereitstellungsmethoden in Azure: Classic (auch bekannt als Service Management) und Azure Resource Manager (ARM). Klassische VNets konnte eine Gruppe hinzugefügt oder als regionalen VNet erstellt werden. Haben Sie ein VNet in einer Gruppe wird mit der [Migration zu einer regionalen VNet](virtual-networks-migrate-to-regional-vnet.md)empfohlen.

## <a name="virtual-network-benefits"></a>Vorteile des virtuellen Netzwerks

- **Isolierung**. VNets sind vollständig voneinander isoliert. Dadurch werden zerlegten Netzwerken für Entwicklung, Test und Produktion erstellen, die die gleichen CIDR-Adressblöcke verwenden.

- **Zugriff auf das Internet**. Alle VMs IaaS und PaaS-Instanzen in einem VNet können das öffentliche Internet standardmäßig zugreifen. Mithilfe von Netzwerk-Sicherheitsgruppen (NSGs), um Zugriff zu steuern.

- **Zugriff auf VMs innerhalb der VNet**. PaaS-Instanzen und IaaS VMs im gleichen virtuellen Netzwerk gestartet werden und sie können sich private IP-Adressen verwenden, auch wenn sie in verschiedenen Subnetzen ohne Gateway konfigurieren oder öffentliche IP-Adressen.

- **Auflösung**. Azure bietet interne Auflösung für IaaS VMs und PaaS-Instanzen in Ihrem VNet bereitgestellt. Sie können auch eigene DNS-Server bereitstellen und VNet um diese zu konfigurieren.

- **Sicherheit**. Datenverkehrs Beenden der virtuellen Maschinen und PaaS-Instanzen in einem VNet kann mithilfe von Netzwerk-Sicherheitsgruppen gesteuert werden.

- **Konnektivität**. VNets können durch Netzwerkgateways oder VNet peering miteinander verbunden werden. Lokale Rechenzentren über VPN-Netzwerke zwischen Standorten oder Azure ExpressRoute können VNets verbunden. Weitere Informationen zu Standort-zu-Standort-VPN-Verbindung finden Sie unter [über VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). Weitere Informationen zu ExpressRoute finden Sie auf [ExpressRoute technical Overview](../expressroute/expressroute-introduction.md). Weitere Informationen über VNet peering besuchen Sie [peering VNet](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Stellen Sie sicher, dass ein VNet vor der Bereitstellung von IaaS VMs oder PaaS-Instanzen in der Azure-Umgebung erstellen. ARM-basierten VMs benötigen ein VNet, und wenn Sie eine vorhandene VNet nicht angeben, Azure erstellt eine VNet, die möglicherweise einen Konflikt CIDR-Adresse Block mit Ihrem lokalen Netzwerk. Damit Sie Ihr VNet mit dem lokalen Netzwerk herstellen.

## <a name="subnets"></a>Subnetze

Teilnetz ist ein Bereich von IP-Adressen in der VNet, mehrere Subnetze für die Organisation und Sicherheit eine VNet unterteilen. VMs und PaaS Rolleninstanzen Subnetze (gleiche oder andere) ein VNet bereitgestellt können ohne zusätzliche Konfiguration kommunizieren. Sie können ein Subnetz auch Routentabelle und NSGs konfigurieren.

## <a name="ip-addresses"></a>IP-Adressen


Es gibt zwei Arten von IP-Adressen für Ressourcen in Azure: *Öffentliche* und *private*. Öffentliche IP-Adressen können Azure Ressourcen anderen Azure öffentliche Dienste wie [Azure Redis Cache](https://azure.microsoft.com/services/cache/) [Azure Ereignis Hubs](https://azure.microsoft.com/documentation/services/event-hubs/)mit Internet kommunizieren. Private IP-Adressen ermöglicht die Kommunikation zwischen Ressourcen in einem virtuellen Netzwerk sowie über ein VPN verbunden sind, ohne eine Internet routbare IP-Adressen.

Weitere Informationen zu IP-Adressen in Azure finden Sie auf [IP-Adressen im virtuellen Netzwerk](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Azure Lastenausgleich

Virtuelle Computer und Cloud-Services in einem virtuellen Netzwerk können über Azure Lastenausgleich mit Internet verfügbar gemacht werden. Business Applications, die interne können nur Lastenausgleich internen System zum Lastenausgleich sein.

- **Externer Lastenausgleich**. Einen externer Lastenausgleich können zur Bereitstellung hoher Verfügbarkeit für VMs IaaS und PaaS-Instanzen vom öffentlichen Internet zugegriffen.

- **Innere Lastenausgleichsmodul**. Ein interner Lastenausgleich können zur Bereitstellung hoher Verfügbarkeit für VMs IaaS und PaaS Instanzen von anderen Diensten in Ihrem VNet zugegriffen.

Weitere Informationen zu Netzwerklastenausgleich in Azure finden Sie auf [Balancer Überblick einlesen](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Netzwerk-Sicherheitsgruppe (NSG)

NSGs ein- und ausgehenden Zugriffskontrolle Netzwerkschnittstellen (NICs) können VMs und Subnetzen. Jede NSG enthält eine oder mehrere Regeln angegeben werden, ob Datenverkehr zugelassen oder verweigert wird basierend auf IP-Quelladresse, Quellport IP-Zieladresse und Zielport. Weitere Informationen zu NSGs finden Sie unter [Was ist eine Netzwerk-Sicherheitsgruppe](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Virtuelle appliances

Virtuelle Appliance ist nur ein VM in Ihrem VNet, die eine softwarebasierte Appliance Funktion oder Firewall, WAN-Optimierung, Intrusion Detection ausgeführt wird. Sie können eine Route in Azure für Ihr VNet-Datenverkehr über ein virtuelles Gerät seine Funktionen erstellen.

Beispielsweise können NSGs Sichern der VNet verwendet werden. NSGs bieten jedoch Ebene 4 Zugriffssteuerungsliste (ACL) auf eingehende und ausgehende Pakete. Wenn Sie einem Layer 7-Sicherheitsmodell verwenden möchten, müssen Sie ein Firewallgerät verwenden.

Virtuelle Appliances hängen [benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).

## <a name="limits"></a>Grenzen
Grenzen für die Anzahl von virtuellen Netzwerken in einem Abonnement zulässig, [Azure Networking](../azure-subscription-service-limits.md#networking-limits) Grenzen finden Sie weitere Informationen.

## <a name="pricing"></a>Preisgestaltung
Es wird keine zusätzlichen Kosten für die Verwendung von virtuellen Netzwerken in Azure. Innerhalb der Vnet Serverinstanzen pauschalen berechnet unter [Azure VM Preise](https://azure.microsoft.com/pricing/details/virtual-machines/). [VPN-Gateways](https://azure.microsoft.com/pricing/details/vpn-gateway/) und [öffentlicher IP-Adressen] (https://azure.microsoft.com/pricing/details/ip-addresses/) in der VNet verwendet werden berechnete Standardsätze.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer VNet](virtual-networks-create-vnet-arm-pportal.md) und Subnetzen.
- [Erstellen Sie einen virtuellen Computer in einem VNet](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Lernen Sie [NSGs](virtual-networks-nsg.md).
- Enthält Informationen Sie zu [benutzerdefinierten Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).
