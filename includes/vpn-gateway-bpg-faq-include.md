### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Unterstützt BGP alle Azure VPN-Gateway SKUs?

Nein, BGP Azure **Standard** und **leistungsfähige** VPN-Gateways unterstützt. **Grundlegende** SKU wird nicht unterstützt.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Kann ich BGP mit Azure Policy-Based VPN-Gateways verwenden?

Nein, BGP Route basierende VPN-Gateways unterstützt.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Kann private ASNs (Autonomous System Zahlen) verwenden?

Ja, können Sie eigene öffentliche ASNs oder private ASNs für lokale Netzwerke und Azure virtuelle Netzwerke.

#### <a name="are-there-asns-reserved-by-azure"></a>Gibt es ASNs Azure reserviert?

Ja, folgende ASNs von Azure internen und externen Peerings vorbehalten sind:

- Öffentliche ASNs: 8075, 8076, 12076
- Private ASNs: 65515, 65517, 65518, 65519, 65520

Sie können für Ihre auf lokalen VPN-Geräte Azure VPN-Gateways mit diesen ASNs angeben.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Kann ich dieselbe ASN für lokalen VPN-Netzwerke und Azure VNets verwenden?

Nein, müssen Sie verschiedene ASNs zwischen der lokalen und der Azure-VNets zuweisen, wenn Sie sie mit BGP verbinden. Azure VPN-Gateways verfügen über den Standardwert ASN 65515 zugewiesen, ob BGP nicht für die standortübergreifende Verbindung aktiviert ist. Sie können diese Standardeinstellung überschreiben, indem verschiedene ASN zuweisen, wenn VPN-Gateway erstellen oder ändern das ASN nach Gateway erstellt wird. Sie müssen die entsprechenden Azure lokalen Netzwerkgateways Ihre lokalen ASNs zuweisen.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Welche Adresspräfixe kündigt Azure VPN-Gateways für mich?

Azure VPN-Gateway werden folgenden Routen auf lokale BGP Geräte ankündigen:

- Die VNet-Adresspräfixe
- Adresspräfixe für jeden lokalen Netzwerkgateways verbunden Azure VPN-Gateway
- Routen von anderen Peers BGP-Sitzungen Azure VPN-Gateway **Standardroute oder Routen überlappt mit Präfix VNet**verbunden.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Können die Standardroute (0.0.0.0/0) zum Azure VPN-Gateways ankündigen?

Derzeit nicht.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Kann die genaue Präfixe als mein virtuelles Netzwerk Präfixe werden angekündigt?

Nein, die gleichen Präfixe als virtuelles Netzwerk Werbung Adresspräfixe blockiert oder aufgrund der Azure-Plattform. Sie können jedoch einen Präfix ankündigen, der eine Obermenge von haben innerhalb des virtuellen Netzwerks. Beispielsweise können virtuelle Netzwerk Adresse Speicherplatz 10.10.0.0/16 und konnte 10.0.0.0/8 ankündigen.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Kann ich mit meinen VNet VNet BGP verwenden?

Natürlich können Sie BGP für standortübergreifende Verbindungen und VNet VNet Verbindungen.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Kann ich für meine Azure VPN-Gateways BGP mit nicht BGP mischen?

Ja, können Sie sowohl BGP-BGP Verbindungen für dieselbe Azure VPN-Gateway kombinieren.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Unterstützt Azure VPN-Gateway Support BGP Übertragung routing?

Ja, BGP Übertragung routing, mit der Ausnahme unterstützt, Azure VPN-Gateways wird **keine** Standardrouten für andere Peers BGP ankündigen. Zum Aktivieren der Übertragung routing über mehrere Azure VPN-Gateways müssen Sie alle temporären VNet VNet Verbindungen BGP aktivieren.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Kann mehrere Tunnel zwischen Azure VPN-Gateway und lokale Netzwerk haben?

Ja, können Sie mehrere S2S VPN-Tunnel zwischen einem Azure VPN-Gateway und Ihrem lokalen Netzwerk herstellen. Beachten Sie, dass diese Tunnel insgesamt Tunnel für Ihren Azure VPN-Gateways angerechnet werden. Beispielsweise haben Sie zwei redundante Tunnel zwischen Azure VPN-Gateways und des lokalen Netzwerks beanspruchen 2 Tunnel insgesamt Kontingent für das Azure VPN-Gateway (10 für Standard) und 30 leistungsstarker.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Kann mehrere Tunnel zwischen zwei Azure VNets mit BGP haben?

Nein, redundante Tunnel zwischen einem Paar von virtuellen Netzwerken nicht unterstützt.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Kann ich in einer ExpressRoute-S2S VPN-Koexistenz BGP S2S VPN verwenden?

Derzeit nicht.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Welche Adresse verwendet Azure VPN-Gateway für BGP Peer IP?

Azure VPN-Gateway wird eine einzelne IP-Adresse aus dem Subnetzname für das virtuelle Netzwerk reserviert. Standardmäßig ist es die zweite letzte Adresse des Bereichs. Beispielsweise ist die Subnetzname 10.12.255.0/27, werden von 10.12.255.0 bis 10.12.255.31, dann die BGP Peer-IP-Adresse Azure VPN-Gateway 10.12.255.30. Diese Informationen finden, wenn Sie Azure VPN-Gateway Listen.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Was sind die Vorschriften für die BGP Peer-IP-Adressen auf VPN-Gerät?

Ihre lokale BGP peer-Adresse **Darf nicht** als öffentliche IP-Adresse des VPN-Geräts übereinstimmen. Verwenden Sie eine andere IP-Adresse auf dem VPN-Gerät für Ihre BGP Peer-IP. Es ist eine Adresse der Loopback-Schnittstelle auf dem Gerät. Geben Sie diese Adresse im entsprechenden lokalen Netzwerk-Gateway, das die Position darstellt.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Was sollte ich angeben als meine Adresspräfixe für das lokale Netzwerk-Gateway beim BGP verwenden?

Azure lokalen Netzwerk-Gateway gibt die anfängliche Adresspräfixe für das lokale Netzwerk. Mit BGP, müssen Sie das Host-Präfix zuweisen (/ 32 Präfix) Ihre BGP Peer-IP-Adresse als Adressraum für das lokale Netzwerk. Wenn Ihre BGP Peer-IP-10.52.255.254 ist, sollten Sie "10.52.255.254/32" als LocalNetworkAddressSpace des Netzwerk-Gateways, lokalen Netzwerk darstellt angeben. Dadurch wird sichergestellt, daß Azure VPN-Gateway BGP-Sitzung über die S2S VPN-Tunnel.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Was sollte ich für peering BGP-Sitzung auf dem lokalen VPN-Gerät hinzufügen?

Sie sollten eine Hostroute Azure BGP Peer-IP-Adresse auf dem VPN-Gerät IPsec S2S VPN-Tunnel auf Hinzufügen. Wenn Azure VPN-Peer-IP "10.12.255.30" ist, sollten Sie eine Hostroute für "10.12.255.30" mit einer Schnittstelle Nexthop übereinstimmende IPsec Tunnelschnittstelle auf dem VPN-Gerät hinzufügen.
