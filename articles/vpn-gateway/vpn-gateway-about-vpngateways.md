<properties 
   pageTitle="Über VPN-Gateway | Microsoft Azure"
   description="VPN-Gateway Verbindungen für virtuelle Netzwerke Azure erfahren."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>Über VPN-Gateway


Ein virtuelles Netzwerk-Gateway zum Netzwerkverkehr zwischen Azure virtuelle Netzwerke und lokale sowie zwischen virtuellen Netzwerken in Azure (VNet VNet) senden. Wenn Sie eine VPN-Gateway konfigurieren, müssen Sie erstellen und ein virtuelles Netzwerk-Gateway und eine Gateway-Verbindung konfigurieren.

Wenn Sie ein virtuelles Netzwerk-Gateway erstellen, geben Sie im Ressourcen-Manager-Bereitstellungsmodell mehrere Einstellungen. Eine erforderliche Einstellung '-GatewayType'. Stehen zwei virtuelle Netzwerk-Gateway: VPN- und ExpressRoute. 

Netzwerkverkehr über eine dedizierte private Verbindung gesendet wird, verwenden Sie den Gatewaytyp 'ExpressRoute'. Dies wird auch als ExpressRoute-Gateway bezeichnet. Netzwerkverkehr über eine öffentliche Verbindung verschlüsselt ist, verwenden Sie den Gatewaytyp "Vpn". Dies wird als ein VPN-Gateway bezeichnet. Standort zu Standort Punkt-zu-Standort- und VNet VNet Anschlüsse alle verwenden eine VPN-Gateway.

Jedes virtuelles Netzwerk haben nur ein virtuelles Netzwerk-Gateway pro Gateway. Sie können z. B. ein virtuelles Netzwerk-Gateway, das GatewayType - ExpressRoute verwendet und, - GatewayType Vpn verwendet. Dieser Artikel konzentriert sich hauptsächlich auf VPN-Gateway. Weitere Informationen zu ExpressRoute finden Sie unter [ExpressRoute Technical Overview](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Preisgestaltung

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>Gateway-SKUs

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Weitere Informationen zu Gateway SKUs finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Geschätzte aggregierte Durchsatz von Lagerhaltungsdaten

Die folgende Tabelle zeigt die Gateways und geschätzte aggregierte Durchsatz. Diese Tabelle gilt für den Ressourcen-Manager und klassischen Bereitstellungsmodelle.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>Eine VPN-Gateway konfigurieren

Wenn Sie eine VPN-Gateway konfigurieren, hängen Anleitung verwendeten Bereitstellungsmodell, mit dem Sie Ihr virtuelle Netzwerk erstellen. Beispielsweise erstellt das VNet mit dem klassischen Bereitstellungsmodell verwenden Sie die Richtlinien und klassischen Bereitstellungsmodell erstellen und konfigurieren Sie Ihre VPN-Gateway. Weitere Informationen Bereitstellungsmodelle finden Sie unter [Understanding Resource Manager und klassischen Bereitstellungsmodelle](../resource-manager-deployment-model.md).

Ein VPN-Gateway basiert auf mehrere Ressourcen mit Einstellungen konfiguriert werden. Die meisten Ressourcen können separat konfiguriert werden, obwohl in einer bestimmten Reihenfolge in einigen Fällen konfiguriert werden muss. Sie können erstellen und Konfigurieren von Ressourcen mit einem Konfigurationstool wie Azure-Portal starten. Sie können dann später ein anderes Tool wie PowerShell zusätzliche Ressourcen konfigurieren oder ändern vorhandene Ressourcen bei Bedarf wechseln. Derzeit kann nicht jede Ressource und Ressource in Azure-Portal konfigurieren. Laut Artikel für jede Verbindungstopologie angeben, wenn bestimmte Konfigurationstool erforderlich ist. Informationen über einzelne Ressourcen und Einstellungen für VPN-Gateway finden Sie [über VPN-Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).

Die folgenden Abschnitte enthalten Tabellen mit einer Liste:

- Verfügbare Bereitstellungsmodell
- Verfügbare Konfigurations-tools
- Links, die Sie direkt zu einem Artikel ggf.

Verwenden Sie die Diagramme und Verbindungstopologie Ihren Erfordernissen auszuwählen. Die Diagramme zeigen die wichtigsten Baseline-Topologien, aber es ist möglich, Diagramme als Richtlinie mit komplexere Konfigurationen erstellen.

## <a name="site-to-site-and-multi-site"></a>Standort, Standort und mehrere Standorte

### <a name="site-to-site"></a>Zwischen Standorten

Eine Standort-zu-Standort (S2S) VPN-gatewayverbindung ist eine Verbindung über IPsec/IKE (IKEv1 oder IKEv2) VPN-Tunnel. Diese Verbindung erfordert ein VPN-Gerät befindet sich lokal, die eine öffentliche IP-Adresse zugewiesen wurde und nicht hinter einem NAT-Gerät S2S-Anschlüsse für standortübergreifende und Hybrid verwendet werden Konfigurationen.   

![S2S Verbindung] (./media/vpn-gateway-about-vpngateways/demos2s.png "zwischen Standorten")


### <a name="multi-site"></a>Mehrere Standorte

Sie können erstellen und konfigurieren ein VPN-Gateway zwischen Ihrem VNet und mehreren lokalen Netzwerken. Beim Arbeiten mit mehreren erforderlich RouteBased VPN-Typ (dynamische Gateway für klassische VNets). Da ein VNet nur ein VPN-Gateway kann, teilen alle Verbindungen über das Gateway die verfügbare Bandbreite. Dies ist häufig eine Verbindung "Multi-Site" bezeichnet.
 

![Multi-Site-Verbindung] (./media/vpn-gateway-about-vpngateways/demomulti.png "mehrere Standorte")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Bereitstellung und-Modelle für Standort-zu-Standort und mehrere Standorte

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet VNet

Ein virtuelles Netzwerk mit einem anderen virtuellen Netzwerk ähnelt (VNet VNet) ein VNet mit einem lokalen Standort. Beide Konnektivität verwenden eine VPN-Gateway für einen sicheren Tunnel mit IPsec-IKE. VNet VNet Kommunikation mit Multi-Site-Verbindung können auch kombiniert werden. Dadurch können Sie die Netzwerktopologien einzurichten, die standortübergreifende Konnektivität zwischen virtuellen Netzwerkkonnektivität kombinieren.

VNets Sie eine Verbindung herstellen kann:

- im gleichen oder in verschiedenen Regionen
- in derselben oder einer anderen Abonnements 
- in der gleichen verschiedene Bereitstellungsmodelle


![VNet VNet-Verbindung] (./media/vpn-gateway-about-vpngateways/demov2v.png "Vnet vnet")

#### <a name="connections-between-deployment-models"></a>Verbindung zwischen Bereitstellungsmodelle

Azure hat derzeit zwei Bereitstellungsmodelle: classic und Ressourcen-Manager. Wenn Sie Azure für einige Zeit verwendet haben, haben Sie wahrscheinlich Azure VMs und Instanz Rollen im klassischen VNet. Ihr neuer VMs und Rolleninstanzen möglicherweise ein Ressourcen-Manager erstellten VNet ausgeführt werden. Sie können eine Verbindung zwischen VNets in einem VNet kommunizieren direkt mit Ressourcen in anderen Ressourcen zu erstellen.

#### <a name="vnet-peering"></a>VNet peering

Sie können VNet als virtuelle Netzwerk bestimmte Anforderungen zum Erstellen der Verbindungs peering verwenden können. VNet peering wird ein virtuelles Netzwerk-Gateway nicht verwendet. Weitere Informationen finden Sie unter [VNet peering](../virtual-network/virtual-network-peering-overview.md).


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Bereitstellung und-Modelle für VNet VNet

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Punkt-zu-Standort

Eine Punkt-zu-Standort (P2S) VPN-Gateway-Verbindung können Sie eine sichere Verbindung mit dem virtuellen Netzwerk aus einem einzelnen Clientcomputer erstellen. P2S ist eine VPN-Verbindung über SSTP (Secure Socket Tunneling-Protokoll). P2S Verbindung erfordern keine VPN-Gerät oder eine öffentliche IP-Adresse zu. Sie herstellen die VPN-Verbindung vom Clientcomputer ab. Dieser Lösung ist nützlich, wenn Sie möchten das VNet von einem Remotestandort aus wie von zu Hause oder einer Konferenz oder wenn nur wenige Clients, die mit einem VNet verbinden. P2S Verbindung können in Verbindung mit S2S über denselben VPN-Gateway verwendet werden, vorausgesetzt, dass alle erforderlichen Konfiguration für beide Verbindungen kompatibel sind.


![Punkt-zu-Standort-Verbindung] (./media/vpn-gateway-about-vpngateways/demop2s.png "Punkt-zu-Standort")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Bereitstellung und-Modelle für Punkt-zu-Standort

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

In eine ExpressRoute-Verbindung ist ein virtuelles Netzwerk-Gateway mit dem Gateway 'ExpressRoute' statt 'Vpn' konfiguriert. Weitere Informationen zu ExpressRoute finden Sie unter [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Koexistenz zwischen Standorten und ExpressRoute-Verbindung

ExpressRoute ist eine direkte, dedizierte Verbindung aus Ihrem WAN (nicht über das öffentliche Internet) an Microsoft Services, einschließlich Azure. Standort-zu-Standort-VPN-Datenverkehr wird verschlüsselte über das Internet übertragen. Standort-zu-Standort-VPN- und ExpressRoute-Anschlüsse für das virtuelle Netzwerk konfiguriert hat mehrere Vorteile.

Sie können eine Standort-zu-Standort-VPN als sicheres Failover Pfad für ExpressRoute, oder verwenden Sie VPNs zwischen Standorten eine Verbindung zu Websites, die nicht Teil des Netzwerks sind, jedoch durch ExpressRoute verbunden sind. Beachten Sie, dass hierfür zwei virtuelle Netzwerkgateways für im gleichen virtuellen Netzwerk mit GatewayType - Vpn und die andere - GatewayType ExpressRoute.


![Verbindung behalten] (./media/vpn-gateway-about-vpngateways/demoer.png "Expressroute-site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Bereitstellungsmodelle und Methoden S2S und ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Nächste Schritte

Planen Sie Ihre VPN-Gateway-Konfiguration. Siehe [VPN-Gateway Planung und Design](vpn-gateway-plan-design.md) und [Azure lokalen Netzwerk verbinden](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 
