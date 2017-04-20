<properties
   pageTitle="Übersicht über hochverfügbaren Konfigurationen mit Azure VPN-Gateways | Microsoft Azure"
   description="Dieser Artikel bietet eine Übersicht über Azure VPN-Gateways mit hoch verfügbaren Konfigurationsoptionen."
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
   ms.date="09/24/2016"
   ms.author="yushwang"/>

# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Hochverfügbare standortübergreifenden und VNet VNet-Konnektivität

Dieser Artikel bietet eine Übersicht über hochverfügbaren Konfigurationsoptionen für standortübergreifende und VNet VNet Konnektivität mit Azure VPN-Gateways.

## <a name = "activestandby"></a>Über Azure VPN-Gateway Redundanz

Jede Azure VPN-Gateway besteht aus zwei Instanzen eine aktiv-Standby-Konfiguration. Für geplante Wartungsarbeiten oder ungeplanten Unterbrechung aktive Instanz geschieht, würde Standby-Instanz (Failover) automatisch übernehmen und S2S VPN oder VNet VNet Verbindung fortzusetzen. Der Wechsel wird eine kurze Unterbrechung verursachen. Für geplante Wartungsarbeiten muss die Verbindung innerhalb von 10 bis 15 Sekunden wiederhergestellt werden. Nicht geplanter Probleme werden die Wiederherstellung der Verbindung länger, etwa 1 Minute 1 und anderthalb Minuten im schlimmsten Fall. P2S VPN-Clientverbindungen zum Gateway P2S Verbindung getrennt und die Benutzer müssen die Clientcomputer aus wiederherstellen.

![Aktiv-Standby](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Standortübergreifende hochverfügbare Konnektivität

Zur besseren Verfügbarkeit für Verbindungen zwischen Räumen sind verschiedene Optionen verfügbar:

- Mehrere lokale VPN-Geräte
- Aktive Azure VPN-gateway
- Kombination

### <a name = "activeactiveonprem"></a>Mehrere lokale VPN-Geräte

Mehrere VPN-Geräte können aus dem lokalen Netzwerk Ihr Gateway Azure VPN-Verbindung wie in der folgenden Abbildung dargestellt:

![Mehrere lokale VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Diese Konfiguration bietet mehrere aktive Tunnel vom gleichen Azure VPN-Gateway auf lokale Geräte am selben Speicherort. Es gibt einige Vorschriften und Nebenbedingungen:

1. Sie müssen mehrere S2S VPN-Verbindungen von VPN-Geräten in Azure erstellen. Wenn Sie mehrere VPN-Geräte in Azure aus dem gleichen lokalen Netzwerk verbinden, müssen Sie eine lokale Netzwerk-Gateway für jeden VPN-Gerät und eine Verbindung von Ihrem Azure VPN-Gateway zum lokalen Netzwerk-Gateway erstellen.

2. VPN-Geräte für LAN-Gateways müssen eindeutige öffentliche IP-Adressen in der Eigenschaft "GatewayIpAddress".

3. BGP ist für diese Konfiguration erforderlich. Jedes LAN Gateway darstellt ein VPN-Gerät muss eine eindeutige BGP Peer-IP-Adresse in der Eigenschaft "BgpPeerIpAddress".

4. Jedes LAN Gateway Feld Eigenschaft AddressPrefix dürfen nicht überlappen. Geben Sie "BgpPeerIpAddress" in /32 CIDR-Format im Feld AddressPrefix, z. B. 10.200.200.254/32.

5. BGP verwenden die gleichen Präfixe des gleichen lokalen Netzwerkpräfixe für das Azure VPN-Gateway und der Datenverkehr weitergeleitet werden über diesen Tunnel gleichzeitig ankündigen.

6. Jede Verbindung angerechnet Tunnel für das Azure VPN-Gateway 10 Basic und Standard-SKUs maximal 30 HighPerformance SKU. 

In dieser Konfiguration ist Azure VPN-Gateway noch in aktiv-Standby-Modus, so wie beschrieben [über](#activestandby)Failoververhalten und kurze Unterbrechung auftreten werden. Aber dieses Setup schützt vor Fehlern oder Unterbrechung im lokalen Netzwerk und VPN-Geräte.
 
### <a name="active-active-azure-vpn-gateway"></a>Aktive Azure VPN-gateway

Sie können jetzt eine Azure VPN-Gateway in einer Aktiv / Aktiv-Konfiguration erstellen, beide Instanzen von VMs Gateway S2S VPN-Tunnel zu dem lokalen VPN-Gerät einrichten, wie im folgende Diagramm dargestellt:

![Aktive](./media/vpn-gateway-highlyavailable/active-active.png)

In dieser Konfiguration jeweils Azure Gateway muss eine eindeutige öffentliche IP-Adresse und stellt jeweils einen IPsec-IKE S2S VPN-Tunnel auf Ihre lokalen VPN-Gerät in Ihrem lokalen Netzwerk-Gateway und Verbindung angegeben. Beachten Sie, dass beide VPN-Tunnel Bestandteil derselben Verbindung. Sie müssen weiterhin konfigurieren Sie das lokale VPN-Gerät übernehmen oder diese beiden Azure VPN-Gateway öffentlicher IP-Adressen zwei S2S VPN-Tunnel einrichten.

Da Azure Gateway Instanzen aktiv / aktiv-Konfiguration sind, wird der Datenverkehr von Azure virtuelle Netzwerk mit Ihrem lokalen Netzwerk sowohl Tunnel gleichzeitig weitergeleitet, selbst wenn Ihre lokalen VPN-Gerät eine gegenüber der anderen bevorzugen möglicherweise. Beachten Sie jedoch der gleiche TCP- oder UDP-Fluss immer den Tunnel oder den Pfad durchlaufen wird, wenn auf eine der Instanzen einer wartungsereignis eintritt.

Fall einer geplanten Wartung oder ungeplante Ereignisse auf ein Gateway-Instanz wird der IPSec-Tunnel von dieser Instanz mit dem lokalen VPN-Gerät getrennt. Die entsprechenden Routen auf VPN-Geräte werden entfernt oder automatisch zurückgezogen, damit der Datenverkehr zu den aktiven IPSec-Tunnel umgeschaltet. Auf Azure erfolgt die Umstellung der betroffenen Instanz um die aktive Instanz automatisch.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Dual-Redundanz: aktive VPN-Gateways für Azure und lokale Netzwerke

Der zuverlässigste Weg ist aktive Gateways auf Ihr Netzwerk und Azure kombinieren, wie in der folgenden Abbildung dargestellt.

![Duale Redundanz](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Hier erstellen und Einrichten von Azure VPN-Gateway in einer Aktiv / Aktiv-Konfiguration und erstellt zwei LAN-Gateways und zwei Anschlüsse für die beiden lokalen VPN-Geräte wie oben beschrieben. Das Ergebnis ist eine full-Mesh-Konnektivität 4 IPSec-Tunnel zwischen Azure virtuelle Netzwerk und dem lokalen Netzwerk.

Alle Gateways und Tunnel werden von Azure Seite so der Datenverkehr zwischen allen 4 Tunnel gleichzeitig verteilt werden zwar alle TCP- oder UDP-Abläufe wieder denselben Tunnel oder Pfad Azure Seite folgen. Obwohl durch Zuweisen des Datenverkehrs Sie besseren Durchsatz über IPSec-Tunnel sehen können, ist das primäre Ziel dieser Konfiguration für hohe Verfügbarkeit. Und aufgrund der statistischen Natur der Verbreitung ist schwierig, die Messung wie Anwendung Datenverkehr unterschiedlich aggregierten Durchsatz beeinflussen.

Diese Topologie erfordert zwei LAN-Gateways und zwei Remoteverbindungen mit zwei lokalen VPN-Geräte unterstützen und BGP ist erforderlich, damit zwei Verbindungen mit dem gleichen lokalen Netzwerk. Diese Vorschriften entsprechen der [oben](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Hochverfügbare Konnektivität für VNet VNet Azure VPN-Gateways

Dieselbe aktive Konfiguration gilt Azure-VNet VNet Verbindung. Sie können aktive VPN-Gateways für beide virtuelle Netzwerke erstellen und miteinander verbinden zu 4 Tunnel zwischen zwei VNets dieselbe full-Mesh-Konnektivität wie in der folgenden Abbildung dargestellt:

![VNet VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Dadurch, dass es immer ein Tunnel zwischen den beiden virtuellen Netzwerken für geplante Wartungsarbeiten Ereignisse, bessere Verfügbarkeit. Obwohl die Topologie für die standortübergreifende Verbindung erfordert zwei Anschlüsse, benötigen oben VNet VNet-Topologie nur eine Verbindung jedes Gateway. Außerdem sei BGP optional Übertragung über die Verbindung VNet VNet routing erforderlich ist.


## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter [Konfigurieren der aktive VPN-Gateways für standortübergreifende und VNet VNet](vpn-gateway-activeactive-rm-powershell.md) Schritte aktive standortübergreifenden und VNet VNet Verbindung konfigurieren.
