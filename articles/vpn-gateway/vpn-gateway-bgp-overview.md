<properties
   pageTitle="Übersicht über BGP mit Azure VPN-Gateways | Microsoft Azure"
   description="Dieser Artikel bietet eine Übersicht über BGP mit Azure VPN-Gateways."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Übersicht über BGP mit Azure VPN-Gateways

Dieser Artikel bietet eine Übersicht über BGP (Border Gateway Protocol) Unterstützung in Azure VPN-Gateways.

## <a name="about-bgp"></a>Über BGP

BGP ist das standard Protokoll routing und Erreichbarkeit Informationsaustausch zwischen zwei oder mehr Netzwerke im Internet verbreitet. Im Kontext von virtuellen Netzwerken Azure ermöglicht BGP Azure VPN-Gateways und Ihre lokalen VPN-Geräte BGP Peers genannt Nachbarn austauschen "weitergeleitet", die beide Gateways zu Verfügbarkeit und Erreichbarkeit für diese Präfixe zu Gateways oder Router informieren beteiligt. BGP ermöglichen auch die Übertragung routing zwischen mehreren Netzwerken Routen lernt BGP Gateway von einem BGP Peer an alle anderen Peers BGP weitergegeben.
 
### <a name="why-use-bgp"></a>Warum verwenden BGP?

BGP ist ein optionales Feature mit Azure Route-basierten VPN-Gateways verwenden. Sie sollten auch sicherstellen, dass Ihre lokalen VPN-Geräte BGP unterstützt, bevor Sie die Funktion aktivieren. Sie können weiterhin Azure VPN-Gateways und Ihre lokalen VPN-Geräte ohne BGP verwenden. Es entspricht mit statischen Routen (ohne BGP) *und* dynamisches routing mit BGP zwischen Netzwerken und Azure verwenden.

Es gibt mehrere Vorteile und neue Funktionen mit BGP:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>Automatische und flexible Präfix Updates unterstützt

Mit BGP müssen Sie nur einen minimalen Präfix bestimmte BGP Peer über IPsec S2S VPN-Tunnel deklarieren. Kann so klein wie ein Host-Präfix (/ 32) BGP Peer-IP-Adresse des lokalen VPN-Geräts. Sie steuern die Netzwerkpräfixe lokal zu werben Azure Azure virtuelle Netzwerk zugreifen können.
    
Sie können auch eine größere Präfixe Werbung, die Ihre Adresspräfixe VNet wie eine große privaten IP-Adressraum (z.B. 10.0.0.0/8) gehören können. Hinweis Obwohl die Präfixe identisch mit der VNet-Präfixe werden können. Diese Routen mit Ihrem VNet Präfixe werden abgelehnt.

>[AZURE.IMPORTANT] Derzeit wird die Standardroute (0.0.0.0/0) zu Azure VPN-Gateways Werbung blockiert. Aktualisierung wird bereitgestellt, wenn diese Funktion aktiviert ist.

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Unterstützung mehrerer Tunnel zwischen einem VNet und einer lokalen Website mit automatischem Failover basierend auf BGP

Sie können mehrere Verbindungen zwischen der Azure-VNet und lokalen VPN-Geräte am selben Speicherort herstellen. Diese Funktion bietet mehrere Tunnel (Pfade) zwischen den beiden Netzwerken in einer Aktiv / Aktiv-Konfiguration. Tunnel getrennt, die entsprechenden Routen über BGP zurückgezogen und der Datenverkehr wird automatisch die verbleibenden Tunnel verschoben.
    
Das folgende Diagramm zeigt ein einfaches Beispiel für diese hohe Verfügbarkeit einrichten:
    
![Mehrere aktive Pfade](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Unterstützung während der Übertragung zwischen den lokalen Netzwerken und mehrere Azure VNets routing

BGP kann mehrere Gateways zu verbreiten Präfixe aus verschiedenen Netzwerken, ob sie direkt oder indirekt verbunden sind. Damit können Übertragung routing mit Azure VPN-Gateways zwischen lokalen Standorten oder in mehreren virtuellen Azure-Netzwerken.
    
Die folgende Abbildung zeigt ein Beispiel einer Multi-Hop-Topologie mit mehreren Pfaden, die zwischen zwei lokale Netzwerke über Azure VPN-Gateways in Microsoft Networks transit können:

![Multi-Hop-Übertragung](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>BGP-FAQs


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter [Erste Schritte mit BGP auf Azure VPN-Gateways](./vpn-gateway-bgp-resource-manager-ps.md) Schritte BGP für standortübergreifende und VNet VNet Verbindung konfigurieren.

