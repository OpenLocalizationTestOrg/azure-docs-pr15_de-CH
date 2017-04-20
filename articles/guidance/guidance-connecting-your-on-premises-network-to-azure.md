<properties
   pageTitle="Azure mit Ihrem lokalen Netzwerk | Microsoft Azure"
   description="Erläutert und vergleicht die verschiedenen Methoden für die Verbindung mit Microsoft-Clouddienste Azure, Office 365 und Dynamics CRM Online."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>Azure herstellen lokalen Netzwerk

Microsoft bietet verschiedene Arten von Cloud-Diensten. Zu den Diensten über das Internet herstellen können, können Sie auch einige der Dienste einen virtuelles privates Netzwerk (VPN) Tunnel über das Internet oder über eine direkte Verbindung zu Microsoft verbinden. In diesem Artikel können Sie ermitteln, welche Konnektivitätsoption am besten wird Ihre Bedürfnisse basierend auf der Microsoft-Cloud-Diensten, die Sie benötigen. Die meisten Organisationen verwenden mehrere Verbindungstypen beschrieben.

## <a name="connecting-over-the-public-internet"></a>Herstellen einer Verbindung über das Internet

Dieser Verbindungstyp ermöglicht den Zugriff auf Microsoft-Clouddienste direkt über das Internet wie unten dargestellt.

![Internet] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Diese Verbindung ist normalerweise der erste Typ für die Verbindung mit Microsoft-Cloud-Diensten verwendet. Die folgende Tabelle enthält die vor- und Nachteile dieser Verbindungstyp.



| **Vorteile**| **Hinweise**|
|---------|---------|
|Erfordert keine Änderung mit dem lokalen Netzwerk als Client-Geräte haben uneingeschränkten Zugriff auf alle IP-Adressen und Ports im Internet.|Wenn Datenverkehr oft über HTTPS verschlüsselt ist, können während der Übertragung abgefangen werden, da das öffentliche Internet.|
|Können auf alle Microsoft-Clouddienste im öffentlichen Internet offen gelegt.|Unvorhersehbare Wartezeiten, da die Verbindung das Internet durchsucht.|
|Ihre Internet-Verbindung verwendet.||
|Nicht erfordern alle Geräte Verbindung.||

Diese Verbindung hat keine Konnektivität oder Bandbreitenkosten Ihre Internet-Verbindung verwenden. 

## <a name="connecting-with-a-point-to-site-connection"></a>Herstellen einer Verbindung mit einer Punkt-zu-Standort-Verbindung

Dieser Verbindungstyp ermöglicht den Zugriff auf einige Microsoft-Clouddienste Tunnel Secure Socket Tunneling-Protokoll (SSTP) über das Internet, wie unten dargestellt.

![P2S] (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "Punkt-zu-Standort-Verbindung")

Die Verbindung erfolgt über Ihre Internet-Verbindung, jedoch benötigen Sie ein VPN-Gateway Azure. Die folgende Tabelle enthält die vor- und Nachteile dieser Verbindungstyp.

| **Vorteile**| **Hinweise**|
|---------|---------|
|Erfordert keine Änderung mit dem lokalen Netzwerk als Client-Geräte haben uneingeschränkten Zugriff auf alle IP-Adressen und Ports im Internet.|Wenn Datenverkehr mit IPSec verschlüsselt ist, können während der Übertragung abgefangen werden, da das öffentliche Internet.|
|Ihre Internet-Verbindung verwendet.|Unvorhersehbare Wartezeiten, da die Verbindung das Internet durchsucht.|
|Durchsatz von bis zu 200 Mb/s pro Gateway.|Erstellung und Verwaltung von eigenen Verbindungen jedes Gerät im lokalen Netzwerk und jedes Gateway muss jedes Gerät herstellen muss.|
|Dienen zum Azure Services herstellen, die eine Azure Virtual Networks (VNet) mit, Azure Virtual Machines und Azure Cloud Services.|Minimaler Verwaltungsaufwand laufenden eines Azure VPN-Gateways.|
||Kann Verbindung zu Microsoft Office 365 Dynamics CRM Online verwendet werden.
||Kann nicht zum Herstellen von Azure Services, die mit einem VNet verbunden werden kann.|

Erfahren Sie mehr über das [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) Service, [Preise](https://azure.microsoft.com/pricing/details/vpn-gateway)und ausgehende Daten Transfer [Preise](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Herstellen einer Verbindung mit einer Standort-zu-Standort-Verbindung

Dieser Verbindungstyp ermöglicht den Zugriff auf einige Microsoft Cloud-Dienste über einen IPSec-Tunnel über das Internet, wie unten dargestellt.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Standort-zu-Standort-Verbindung")

Die Verbindung erfolgt über Ihre Internet-Verbindung, jedoch benötigen Sie ein VPN-Gateway Azure mit zugeordneten Preis-ausgehende Datenübertragung Preise. Die folgende Tabelle enthält die vor- und Nachteile dieser Verbindungstyp.

| **Vorteile**| **Hinweise**|
|---------|---------|
|Alle Geräte in Ihrem lokalen Netzwerk kommunizieren mit Azure Dienstleistungen ein VNet besteht keine Notwendigkeit, eigene Verbindung für jedes Gerät konfigurieren.|Wenn Datenverkehr mit IPSec verschlüsselt ist, können während der Übertragung abgefangen werden, da das öffentliche Internet.|
|Ihre Internet-Verbindung verwendet.|Unvorhersehbare Wartezeiten, da die Verbindung das Internet durchsucht.|
|Kann Azure Services, die mit einem VNet wie virtuelle Computer verbunden werden können und Cloud-Diensten verwendet werden.|Konfigurieren und Verwalten einer validierten VPN-Geräte * müssen in Räumen.|
|Durchsatz von bis zu 200 Mb/s pro Gateway.|Minimaler Verwaltungsaufwand laufenden eines Azure VPN-Gateways.|
|Ausgehenden Datenverkehr vom Cloud virtuelle Computer im lokalen Netzwerk für Überprüfung und Protokollierung mit benutzerdefinierten Routen oder Border Gateway Protocol (BGP) erzwingen können **.|Kann Verbindung zu Microsoft Office 365 Dynamics CRM Online verwendet werden.|
||Kann nicht zum Herstellen von Azure Services, die mit einem VNet verbunden werden kann.|
||Wenn Sie Dienste, die Verbindungen zu lokalen Geräte und Ihre Sicherheitsrichtlinien ist erforderlich, eine Firewall zwischen dem lokalen Netzwerk und Azure möglicherweise.|

- * Liste der [VPN-Geräte überprüft](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices).
- ** Weitere Informationen zur Verwendung von [benutzerdefinierten Routen](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) oder [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) routing von Azure VNets zu einem lokalen Gerät erzwingen.

## <a name="connecting-with-a-dedicated-private-connection"></a>Verbindung über eine dedizierte private Verbindung

Dieser Verbindungstyp ermöglicht den Zugriff auf alle Microsoft Cloud-Dienste über eine dedizierte private Verbindung an Microsoft, das nicht das Internet durchsuchen wie unten dargestellt.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute-Verbindung")

Die Verbindung muss der ExpressRoute-Dienst und eine Verbindung mit einem konnektivitätsanbieter. Die folgende Tabelle enthält die vor- und Nachteile dieser Verbindungstyp.

| **Vorteile**| **Hinweise**|
|---------|---------|
|Datenverkehr kann nicht während der Übertragung über das Internet abgefangen werden, da eine dedizierte Verbindung über einen Dienstanbieter verwendet wird.|Lokale Router Management erfordert.|
|Bandbreite bis zu 10 Gb/s pro ExpressRoute-Verbindung und Durchsatz von bis zu 2 Gb/s, jedes Gateway.|Erfordert eine dedizierte Verbindung eines Konnektivität.|
|Vorhersagbare Latenz ist eine dedizierte Verbindung an Microsoft, das nicht das Internet durchsuchen.|Erfordern minimal laufende Verwaltung von mindestens Azure VPN-Gateways (falls VNets die Verbindung herstellen).|
|Verschlüsselten Kommunikation erfordert keine Wenn Sie ggf. den Datenverkehr verschlüsseln können.| Wenn Sie Cloud-Dienste, die Verbindung zu lokalen Geräten initiieren verwenden, eine Firewall zwischen dem lokalen Netzwerk und Azure möglicherweise.|
|Alle Microsoft Cloud-Dienste mit wenigen Ausnahmen * können direkt verbinden.|Erfordert die Netzwerkadressübersetzung (NAT) der lokalen IP-Adressen in Microsoft-Rechenzentren für Dienste, die mit VNet.* * verbunden werden kann|
|Ausgehenden Datenverkehr vom Cloud virtuelle Computer im lokalen Netzwerk zur Überprüfung und Protokollierung mithilfe von BGP können erzwingen.|

- * Zeigen Sie eine [Liste der Dienste](../expressroute/expressroute-faqs.md#supported-services) , die mit ExpressRoute verwendet werden kann an. Azure-Abonnement muss genehmigt werden, um Verbindung mit Office 365.  Finden Sie den [Azure ExpressRoute für Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) für Details.
- ** Weitere Informationen zu [NAT](../expressroute/expressroute-nat.md) ExpressRoute Vorschriften.

Erfahren Sie mehr über [ExpressRoute](../expressroute/expressroute-introduction.md), zugeordnete [Preise](https://azure.microsoft.com/pricing/details/expressroute)und [konnektivitätsanbieter](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>Weitere Aspekte

- Einige der Optionen haben verschiedene Höchstgrenzen für Verbindungen, Gateway-Verbindungen und anderen Kriterien VNet unterstützen. Es wird empfohlen, Sie die Azure [Netzwerk begrenzt](../azure-subscription-service-limits.md#networking-limits) verstehen überprüfen, wenn diese die Konnektivität auswirken, die Sie verwenden. 
- Wenn Sie dieselbe VNet als ExpressRoute Gateway Gateway eine Standort-zu-Standort-VPN-Verbindung herstellen möchten, sollten Sie mit wichtigen zuerst vertraut. Finden Sie im Artikel [ExpressRoute konfigurieren und Koexistenz Verbindungen zwischen Standorten](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) Weitere Informationen.

## <a name="next-steps"></a>Nächste Schritte

Ressourcen erläutert in diesem Artikel behandelten Verbindungstypen implementieren.

-   [Implementieren Sie eine Punkt-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [Implementieren einer Standort-zu-Standort-Verbindung](guidance-hybrid-network-vpn.md)
-   [Implementieren Sie eine dedizierte private Verbindung](guidance-hybrid-network-expressroute.md)
-   [Implementieren Sie eine dedizierte private Verbindung eine Standort-zu-Standort-Verbindung für hohe Verfügbarkeit](guidance-hybrid-network-expressroute-vpn-failover.md)
