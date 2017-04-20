<properties 
   pageTitle="VPN-Gateway Planung | Microsoft Azure"
   description="Erfahren Sie mehr über VPN-Gateway Planung und Entwurf für standortübergreifende, Hybrid- und VNet VNet-Verbindung"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Planung und Entwurf für VPN-Gateway

Planen und Entwerfen der standortübergreifenden und VNet VNet-Konfigurationen kann einfach oder kompliziert, je nach Ihren Anforderungen. Dieser Artikel führt Sie durch einfache Planung und Design Considerations.

## <a name="planning"></a>Planung


### <a name="compare"></a>Standortübergreifende Konnektivitätsoptionen

Sie Ihre lokalen Standorten sicher zu einem virtuellen Netzwerk verbinden möchten, haben Sie drei verschiedene Arten tun: Site zu Site, Punkt-zu-Standort- und ExpressRoute. Vergleichen der verschiedenen standortübergreifenden Verbindungen verfügbar. Die gewählte Option kann von verschiedenen Aspekten abhängen:


- Erfordert welche Durchsatz die Lösung?
- Möchten Sie über das Internet über sichere VPN oder über eine private Verbindung kommunizieren?
- Haben Sie eine öffentliche IP-Adresse verwendet?
- Planen Sie ein VPN-Gerät verwenden? In diesem Fall ist es kompatibel?
- Verbinden Sie das wenigen Computern oder möchten Sie eine ständige Verbindung für Ihre Website?
- Welche VPN-Gateway ist erforderlich für die Projektmappe erstellen möchten?
- Das Gateway SKU sollten Sie verwenden?


In der folgenden Tabelle helfen Ihnen die geeignetste Konnektivitätsoption für die Lösung entscheiden.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Gateway-Anforderungen SKU mit VPN-Typ

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Weitere Informationen zu Gateway SKUs finden Sie unter [VPN-Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Aggregierte Durchsatz von SKU und VPN

Die folgende Tabelle zeigt die Gateways und geschätzte aggregierte Durchsatz. Der geschätzte Gesamtdurchsatz möglicherweise entscheidend für Ihren Entwurf.
Preisgestaltung unterscheidet zwischen Gateway-SKUs. Informationen zu Preisen finden Sie unter [VPN-Gateway Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/). Diese Tabelle gilt für den Ressourcen-Manager und klassischen Bereitstellungsmodelle.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Unterstützte Konfigurationen von SKU und VPN

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="wf"></a>Workflow

Die folgende Liste nennt allgemeinen Workflow für Cloud-Konnektivität:

1.  Entwerfen Sie und Planen Sie der Netzwerktopologie und Listen Sie die Adressräume für alle Netzwerke, die Sie verbinden möchten.
2.  Erstellen Sie ein Azure virtuelles Netzwerk. 
3.  Erstellen Sie eine VPN-Gateway für das virtuelle Netzwerk.
4.  Erstellen Sie und konfigurieren Sie Verbindung mit lokalen Netzwerken oder andere virtuelle Netzwerke (nach Bedarf).
5.  Erstellen und Konfigurieren einer Punkt-zu-Standort-Verbindung für das Azure VPN-Gateway (nach Bedarf).
 

## <a name="design"></a>Design

### <a name="topologies"></a>Verbindung-Topologien

Zunächst die Diagramme im Artikel [Über VPN-Gateway](vpn-gateway-about-vpngateways.md) . Der Artikel enthält Standarddiagrammen Bereitstellungsmodelle für jede Topologie (Ressourcenmanager oder Classic) und welche Bereitstellungstools können Sie Ihre Konfiguration bereitstellen.   

### <a name="designbasics"></a>Grundlagen

Den folgenden Abschnitten werden die Grundlagen für die VPN-Gateways. Berücksichtigen Sie [networking Services eingeschränkt](../articles/azure-subscription-service-limits.md#networking-limits).


#### <a name="subnets"></a>Über Subnetze

Wenn Sie Verbindungen erstellen, müssen Sie Ihr Subnetbereiche berücksichtigen. Kann Ihnen nicht überlappende Adressbereiche Subnetz. Eine überlappende Subnetz ist ein virtuelles Netzwerk oder lokal denselben Adressraum enthält, die die Position enthält. Daher benötigen Netzwerk Ingenieuren für Ihre lokalen lokale Netzwerke sich ein Bereich für Ihren Azure IP verwenden Speicherplatz-Subnetze an. Sie benötigen Adressbereich, die nicht im lokalen lokalen Netzwerk verwendet wird. 

Vermeiden überlappende Subnetze ist außerdem wichtig, bei der Arbeit mit VNet VNet. Wenn die Subnetze überlappen und eine IP-Adresse senden und Ziel VNets vorhanden ist, fehl VNet VNet Verbindung. Azure kann nicht Daten an das VNet weitergeleitet werden, weil die Zieladresse senden VNet gehört. 

VPN-Gateways müssen ein bestimmtes Subnetz bezeichnet ein. Alle Gateway-Subnetze müssen Subnetzname ordnungsgemäß benannt werden. Sie keinen gatewaysubnetz unterschiedliche Namen und Bereitstellen Sie keine VMs oder anderes gatewaysubnetz. Siehe [Gateway Subnetze](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Zum lokalen Netzwerk-gateways

Das lokale Netzwerk-Gateway verweist normalerweise auf die lokalen. Im klassischen Bereitstellungsmodell wird das lokale Netzwerk-Gateway zum lokalen Netzwerk bezeichnet. Beim Konfigurieren einer lokalen Netzwerk-Gateway Sie benennen sie öffentliche IP-Adresse des lokalen VPN-Gerät und Adresspräfixe, die im lokalen Speicherort angeben. Azure Ziel Adresspräfixe Netzwerkverkehr untersucht, konsultiert die angegebene Konfiguration für das lokale Netzwerk-Gateway und leitet Pakete entsprechend. Sie können diese Adresspräfixe nach Bedarf ändern. Weitere Informationen finden Sie unter [lokale Netzwerkgateways](vpn-gateway-about-vpn-gateway-settings.md#lng).


#### <a name="gwtype"></a>Zu gateway

Auswählen der richtigen Gateway für Ihre Topologie ist wichtig. Wenn Sie den falschen Typ auswählen, funktioniert Ihr Gateway nicht ordnungsgemäß. Gatewaytyp gibt an, wie das Gateway verbindet und eine erforderliche Einstellung für den Ressourcen-Manager-Bereitstellungsmodell.

Die gatewaytypen sind:

- VPN
- ExpressRoute

#### <a name="connectiontype"></a>Über Verbindungstypen

Jede Konfiguration erfordert einen bestimmten Verbindungstyp. Die Verbindung sind:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="vpntype"></a>Über VPN-Typen

Jede Konfiguration erfordert bestimmten VPN-Typ. Wenn Sie zwei Konfigurationen wie Erstellen einer Standort-zu-Standort-Verbindung und eine Punkt-zu-Standort-Verbindung mit demselben VNet kombinieren müssen Sie ein VPN verwenden, die beide Anforderungen erfüllt.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

Die folgenden Tabellen zeigen VPN-Typ wie jede Konfiguration der Verbindung zugeordnet ist. Stellen Sie sicher, dass VPN-Typ für das Gateway die Konfiguration übereinstimmt, die Sie erstellen möchten. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="devices"></a>VPN-Geräte für Standort-zu-Standort-Verbindung

Zum Konfigurieren einer Verbindung zwischen Standorten unabhängig Bereitstellungsmodell, benötigen Sie Folgendes:

- Ein VPN-Gerät mit Azure VPN-gateways
- Eine öffentliche IPv4-IP-Adresse nicht hinter einem NAT-Gerät

Sie müssen Erfahrung im Konfigurieren von VPN-Gerät oder eine Person, die das Gerät für Sie konfigurieren kann. Weitere Informationen zu VPN-Geräten finden Sie unter [über VPN-Geräte](vpn-gateway-about-vpn-devices.md). VPN-Geräte Artikel enthält Informationen geprüften Geräte, Vorschriften für Geräte, die nicht validiert und Links zu Konfigurationsdokumente Gerät, falls verfügbar.

### <a name="forcedtunnel"></a>Prüfen, erzwungenen Tunnel routing

Für die meisten Konfigurationen können Sie einen erzwungenen Tunnel konfigurieren. Erzwungene Tunnel ermöglicht die Umleitung oder "Force" alle Internet gerichtete Datenverkehr wieder in Ihren lokalen Standort über eine Standort-zu-Standort-VPN-Tunnel für Inspektion und Überwachung. Dies ist erforderlich für die meisten Unternehmen wichtige Security Policies. 

Ohne erzwungenen Tunneln wird Internet gerichtete Datenverkehr von der VMs in Azure immer Durchlaufen von Azure Infrastruktur direkt, ohne mit dem Internet, überprüfen oder überwachen den Datenverkehr zugelassen. Internet-Zugriff kann Offenlegung von Informationen oder andere Arten von Sicherheitslücken führen.

**Erzwungene Tunneling-Diagramm**

![Erzwungene Tunnel Verbindung] (./media/vpn-gateway-plan-design/forced-tunnel.png "Erzwungene Tunnel")

Eine erzwungene Tunneling-Verbindung kann in beiden und mit verschiedenen Tools konfiguriert werden. In der folgenden Tabelle Weitere Informationen anzeigen Wir aktualisieren diese Tabelle neue Artikel, neue Bereitstellungsmodelle und weitere Tools für diese Konfiguration verfügbar. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt aus der Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Nächste Schritte

Finden Sie Artikel [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md) und [Über VPN-Gateway](vpn-gateway-about-vpngateways.md) für Weitere Informationen, die Ihnen in den Entwurf.

Weitere Informationen zu bestimmten Gateways finden Sie unter [Über VPN-Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).




