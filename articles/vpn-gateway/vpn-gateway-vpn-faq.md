<properties 
   pageTitle="Virtuelles Netzwerk VPN-Gateway FAQ | Microsoft Azure"
   description="VPN-Gateway, häufig gestellte Fragen. Häufig gestellte Fragen zu Microsoft Azure Virtual Network standortübergreifende Verbindung Hybrid Konfiguration Verbindungen und VPN-Gateways"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>VPN-Gateway FAQ

## <a name="connecting-to-virtual-networks"></a>Verbindung mit virtuellen Netzwerken

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Kann ich virtuelle Netzwerke in Azure Regionen verbinden?
Ja. Tatsächlich ist keine Region-Einschränkung. Ein anderes virtuelles Netzwerk in der gleichen Region oder in einer anderen Region Azure kann ein virtuelles Netzwerk herstellen.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Kann virtuelle Netzwerke in verschiedenen Subskriptionen anschließen?
Ja.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Kann ich mehrere Sites aus einem virtuellen Netzwerk verbinden?

An mehreren Standorten können mithilfe von Windows PowerShell und Azure REST-APIs. Siehe Abschnitt [mehrere Standorte und VNet VNet Konnektivität](#multi-site-and-vnet-to-vnet-connectivity) FAQ.
## <a name="what-are-my-cross-premises-connection-options"></a>Welche Optionen habe ich standortübergreifende Verbindung?

Die folgenden standortübergreifenden Verbindungen werden unterstützt:

- [Standort-zu-Standort](vpn-gateway-site-to-site-create.md) -VPN-Verbindung über IPsec (IKE v1 und v2 IKE). Diese Verbindung erfordert ein VPN-Gerät oder RRAS.

- [Punkt-zu-Standort-](vpn-gateway-point-to-site-create.md) -VPN-Verbindung über SSTP (Secure Socket Tunneling-Protokoll). Diese Verbindung erfordert keine VPN-Gerät.

- [VNet VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – Verbindung entspricht einer Standort-zu-Standort-Konfiguration. VNet zu VNet ist eine VPN-Verbindung über IPsec (IKE v1 und v2 IKE). Ein VPN-Gerät ist nicht erforderlich.

- [Mehrere Standorte](vpn-gateway-multi-site.md) – Dies ist eine Variante einer Standort-zu-Standort-Konfiguration, die lokalen Standorten zu einem virtuellen Netzwerk verbunden.

- [ExpressRoute](../expressroute/expressroute-introduction.md) -ExpressRoute ist eine direkte Verbindung zu Azure aus Ihrem WAN, nicht über das Internet. [Technische Übersicht über ExpressRoute](../expressroute/expressroute-introduction.md) und [ExpressRoute häufig gestellten Fragen](../expressroute/expressroute-faqs.md) Weitere Informationen anzeigen

Finden Sie weitere Informationen zu Verbindungen [Über VPN-Gateway](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Was ist der Unterschied zwischen einem Standort-zu-Standort-Verbindung und Punkt-zu-Standort?

**Standort-zu-Standort** -Verbindung können Sie die Verbindung zwischen den Computern in Ihren Geschäftsräumen virtuellen oder Rolleninstanz innerhalb des virtuellen Netzwerks je nachdem wie Sie routing konfigurieren. Es ist eine gute Option für eine jederzeit verfügbare standortübergreifende Verbindung und für Hybrid-Konfigurationen. Diese Art von Verbindung basiert auf einer IPsec VPN-Anwendung (Hardware oder weiche Appliance) am Rand des Netzwerks eingesetzt werden. Um diese Verbindung zu erstellen, müssen Sie die erforderliche Hardware VPN und nach außen gerichtete IPv4-Adresse.

**Punkt-zu-Standort-** Verbindung können Sie einem einzelnen Computer überall auf virtuellen Netzwerk befindet. Windows integrierten VPN-Client verwendet. Als Teil der Punkt-zu-Standort-Konfiguration installieren Sie ein Zertifikat und ein VPN-Client Konfigurationspaket enthält die Einstellungen, die virtuellen Computer oder Rolleninstanz im virtuellen Netzwerk herstellen kann. Es ist großartig, wenn ein virtuelles Netzwerk herstellen möchten, aber nicht lokal gespeichert. Wird auch eine gute Option haben Sie nicht Zugriff auf VPN-Hardware oder eine extern ausgerichteten IPv4-Adresse für eine Standort-zu-Standort-Verbindung erforderlich sind. 

Sie können virtuelle Netzwerk-Standorten und Punkt-zu-Standort gleichzeitig verwenden, die Standort-zu-Standort-Verbindung mit einer Route-basierte VPN-Typ für Ihr Gateway erstellen. Route-basierte VPN-Typen werden dynamische Gateways im klassischen Bereitstellungsmodell bezeichnet.

### <a name="what-is-expressroute"></a>Was ist ExpressRoute?

ExpressRoute können Sie private Verbindung zwischen Microsoft-Rechenzentren und Infrastruktur vor Ort oder in einer Umgebung mit gemeinsam. Mit ExpressRoute Sie Verbindung zu Microsoft Cloud-Services wie Microsoft Azure und Office 365 an einer ExpressRoute Partner gemeinsam oder direkt aus dem vorhandenen WAN-Netzwerk (wie einem MPLS-VPN von einem Netzwerkdienstanbieter bereitgestellt) verbinden.

ExpressRoute-Verbindungen bieten eine höhere Sicherheit, mehr Zuverlässigkeit höhere Bandbreite und niedriger Latenz als normalen Verbindungen über das Internet. In einigen Fällen kann die mit ExpressRoute Anschlüsse zur Datenübertragung zwischen dem lokalen Netzwerk und Azure auch erhebliche Kostenvorteile ergeben. Wenn Sie eine standortübergreifende Verbindung aus dem lokalen Netzwerk in Azure bereits erstellt haben, können Sie eine ExpressRoute-Verbindung und virtuelle Netzwerk intakt migrieren.

[ExpressRoute FAQ](../expressroute/expressroute-faqs.md) für mehr Details anzeigen

## <a name="site-to-site-connections-and-vpn-devices"></a>Standort-zu-Standort-Verbindung und VPN-Geräte

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Was sollten, wenn ein VPN-Gerät?

Wir haben eine Reihe von standardmäßigen Standort-zu-Standort-VPN-Geräten in Partnerschaft mit Gerätehersteller überprüft. Eine Liste der bekannten VPN-Geräte, Konfigurationshinweise oder Beispiele und Gerät Spezifikationen finden Sie [hier](vpn-gateway-about-vpn-devices.md). Virtuelles Netzwerk sollten alle Geräte als bekannte kompatible gerätefamilien arbeiten. Der VPN-Konfiguration hierzu finden Sie Konfigurationsbeispiel Gerät oder Verknüpfung, die entsprechende Produktfamilie entspricht.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Was tun, habe ein VPN-Gerät, das in der Liste der bekannten kompatibel ist?

Wenn das Gerät als einem bekannten kompatiblen VPN-Gerät nicht angezeigt und für die VPN-Verbindung verwendet werden soll, müssen Sie überprüfen, ob die unterstützten IPsec-IKE-Konfiguration Optionen und Parameter aufgelisteten [hier](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list)erfüllt. Geräte die Mindestanforderungen sollte mit VPN-Gateways funktionieren. Wenden Sie sich an den Gerätehersteller Hinweise zusätzlicher Support und Konfiguration.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Warum gehen mein Policy-basierte VPN-Tunnel Sie beim Datenverkehr im Leerlauf befindet?

Dies ist ein erwartetes Verhalten für Policy-basiertes (auch bekannt als statisches routing) VPN-Gateways. Wenn der Datenverkehr über den Tunnel für mehr als 5 Minuten im Leerlauf befindet, wird der Tunnel abgerissen. Wenn Datenverkehr in jede Richtung fließt, wird der Tunnel sofort wiederhergestellt.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Kann ich Software VPN Verbindung zu Azure verwenden?

Wir unterstützen Windows Server 2012 Routing und RAS (RRAS) Server-Standorten standortübergreifende Konfiguration.

Unsere Gateway sollten andere Software VPN-Lösung arbeiten, als Industry standard IPSec-Implementierung entsprechen. Hersteller der Software Konfiguration und Support.

## <a name="point-to-site-connections"></a>Punkt-zu-Standort-Verbindung

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Welche Betriebssysteme können mit Punkt-zu-Standort werden verwendet?

Die folgenden Betriebssysteme werden unterstützt:

- Windows 7 (32-Bit- und 64-Bit)

- Windows Server 2008 R2 (nur 64-Bit)

- Windows 8 (32-Bit- und 64-Bit)

- Windows 8.1 (32-Bit- und 64-Bit)

- Windows Server 2012 (nur 64-Bit)

- Windows Server 2012 R2 (nur 64-Bit)

- Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Kann ich alle Software-VPN-Client für Punkt-zu-Standort verwenden, der SSTP unterstützt?

Nein. Support ist nur auf der oben aufgeführten Versionen der Windows-Betriebssystem.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Wie viele VPN-Clientendpunkte können meinen Punkt-zu-Standort-Konfiguration enthalten?

Wir unterstützen bis zu 128 VPN-Clients gleichzeitig eine Verbindung zu einem virtuellen Netzwerk zu.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kann ich meinen eigenen internen PKI Stammzertifizierungsstelle für Punkt-zu-Standort-Verbindung verwenden?

Ja. Früher konnte nur selbstsignierte Stammzertifikate verwendet. Sie können immer noch 20 Stammzertifikate hochladen.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Können Proxys und Firewalls mit Punkt-zu-Standort-Funktion durchlaufen?

Ja. Wir verwenden SSTP (Secure Socket Tunneling-Protokoll) Tunnel durch Firewalls. Diese Tunnel werden als eine HTTPs-Verbindung angezeigt.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Wenn ein Clientcomputer für Punkt Neustart-zu-Standort-erneut VPN automatisch Verbindung?

Standardmäßig wird der Clientcomputer die VPN-Verbindung nicht automatisch wiederhergestellt.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Ist Punkt-zu-Standort-Unterstützung automatisch verbinden und DDNS auf VPN-Clients?

Automatisch verbinden und DDNS werden in Punkt-zu-Standort-VPNs derzeit nicht unterstützt.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kann ich zwischen Standorten und koexistieren Punkt-zu-Standort-Konfigurationen für das virtuelle Netzwerk?

Ja. Diese Lösung funktioniert, haben Sie ein RouteBased VPN für Ihr Gateway. Das klassische Bereitstellungsmodell benötigen Sie eine dynamische Gateway. Wir führen keine Unterstützung Punkt-zu-Standort für statische routing VPN-Gateways bzw. Gateways mit VpnType - Richtlinien.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kann ich eine Punkt-zu-Standort-Clients gleichzeitig eine Verbindung mit mehreren virtuellen Netzwerken konfigurieren?

Ja, das geht. Aber die virtuellen Netzwerke nicht überlappende IP-Präfixe und Punkt-zu-Standort-Adressräume müssen zwischen virtuellen Netzwerken nicht überlappen.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Wie viel Durchsatz kann ich über zwischen Standorten oder Punkt-zu-Standort-Verbindung?

Es ist schwierig, genauen Durchsatz von VPN-Tunneln aufrechtzuerhalten. IPsec und SSTP sind Crypto Heavy VPN-Protokolle. Durchsatz ist durch Latenz und Bandbreite zwischen Ort und das Internet auch begrenzt.

## <a name="gateways"></a>Gateways

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Was ist eine Policy-basierte (statische routing) Gateway?

Policy-basierte Gateways implementieren Sie Policy-basierte VPNs. Policy-basierte VPNs verschlüsselt und leiten Pakete durch IPSec-Tunnel basierend auf Kombinationen von Präfixen zwischen dem lokalen Netzwerk und Azure-VNet. Richtlinie (oder Datenverkehr Selector) wird üblicherweise definiert als eine Zugriffsliste der VPN-Konfiguration.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Was ist eine Route-basierte (dynamische routing) Gateway?

Route-basierte Gateways implementieren das Route-basierte VPNs. Route-basierte VPNs verwenden "Strecken" IP forwarding oder Routingtabelle Pakete direkt in die entsprechenden Tunnelschnittstellen. Die Tunnelschnittstellen dann verschlüsseln oder entschlüsseln die Pakete in den Tunnel. Der Richtlinie oder Datenverkehr Selektor Weg VPNs werden als n-n-konfiguriert (oder Platzhaltern).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Erhalte ich meine VPN-Gateway IP-Adresse, bevor ich erstellen?

Nein. Sie müssen Ihr Gateway um die IP-Adresse zu erstellen. Die IP-Adresse ändert sich, wenn Sie löschen und neu das VPN-Gateway erstellen.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Wie authentifiziert mein VPN-Tunnel?

Azure VPN verwendet PSK (Pre-Shared Key) Authentifizierung. Wir erstellen einen vorinstallierten Schlüssel (PSK) beim Erstellen des VPN-Tunnels. Sie können automatisch generierte PSK eigene festgelegt vorinstallierter Schlüssel PowerShell-Cmdlets oder REST-API ändern.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Können die API legen vorinstallierter Schlüssel Meine richtlinienbasierte konfigurieren verwenden VPN-Gateway (statisches routing)?

Ja, vorinstallierter Schlüssel API festgelegt und PowerShell-Cmdlet dienen Azure-(statisch) Policy-basierte VPNs und Route-basierten (dynamisch) routing VPN konfigurieren.

### <a name="can-i-use-other-authentication-options"></a>Können andere Authentifizierungsoptionen?

Wir sind vorinstallierte Schlüssel (PSK) für die Authentifizierung verwenden.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Was ist "gatewaysubnetz" und warum benötigt?

Wir haben eine Gatewaydienst ausgeführt um standortübergreifende Konnektivität zu aktivieren. 

Sie müssen zum Erstellen eines Subnetzes Gateway für das VNet eine VPN-Gateway konfigurieren. Alle Gateway-Subnetze müssen Subnetzname ordnungsgemäß benannt werden. Namen Sie keine gatewaysubnetz etwas. Und keine VMs oder anderes gatewaysubnetz bereitstellen.

Die minimale Größe des Gateway-Subnetz hängt ganz von der Konfiguration, die Sie erstellen möchten. Zwar kann ein so klein wie /29 für einige Konfigurationen erstellen empfiehlt ein gatewaysubnetz /28 oder größere erstellen (/ 28, /27, /26 usw..). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Kann ich virtuelle Maschinen oder Instanzen mein Gateway Subnetz bereitstellen?

Nein.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Festlegen der Netzwerkverkehr über das VPN-Gateway?

Bei Verwendung der Azure-Verwaltungsportal fügen Sie jeder Bereich über das Gateway für das virtuelle Netzwerk auf Netzwerke unter lokale Netzwerke gesendet hinzu.

### <a name="can-i-configure-forced-tunneling"></a>Kann ich Erzwungene Tunnel konfigurieren?

Ja. Finden Sie unter [Configure erzwungenen Tunneln](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Können eigene VPN-Server in Azure eingerichtet und die Verbindung mit dem lokalen Netzwerk verwenden?

Ja, können Sie Ihre eigenen VPN-Gateways oder Server in Azure aus Azure Marketplace oder erstellen Ihre eigenen VPN Router bereitstellen. Sie müssen konfigurieren benutzerdefinierte Routen im virtuellen Netzwerk sicher, dass Datenverkehr zwischen der lokalen und Subnets virtuelles Netzwerk ordnungsgemäß weitergeleitet wird.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Warum werden bestimmte Ports auf VPN Gateway geöffnet?

Sie sind für die Kommunikation der Azure-Infrastruktur erforderlich. Sie sind geschützt (gesperrt) von Azure-Zertifikaten. Ohne entsprechenden Zertifikate können externe Entitäten, einschließlich die Kunden diese Gateways keine Auswirkung auf die Endpunkte zu.

Eine VPN-Gateway ist im Grunde ein mehrfach vernetzten Gerät mit einer Netzwerkkarte in das private Netzwerk des Kunden und eine Netzwerkkarte mit dem öffentlichen Netzwerk. Azure-Infrastruktur Entitäten können nicht erschließen Kunden private Netzwerke aus Compliance-Gründen müssen sie öffentliche Endpunkte für Infrastruktur Kommunikation nutzen. Öffentliche Endpunkte werden regelmäßig von Azure Sicherheitswarnmeldung gescannt.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Weitere Informationen zum gatewaytypen, Vorschriften und Durchsatz

Weitere Informationen finden Sie unter [Über VPN-Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Mehrere Standorte und VNet VNet-Konnektivität

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Welche Art von Gateways unterstützen mehrere Standorte und VNet VNet-Konnektivität?

Nur Route-basierte VPNs (dynamisches routing).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Kann ich ein VNet mit einem RouteBased VPN-Typ zu einem anderen VNet mit PolicyBased VPN verbinden?

Keine, virtuelle Netzwerke müssen Route-basierte (dynamisches routing) verwenden VPNs.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>Ist die VNet VNet sicher?

Ja, es IPsec-IKE-Verschlüsselung geschützt.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>Werden VNet VNet Datenverkehr über Azure Backbone übertragen?

Ja.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Wie viele lokalen Sites und virtuelle Netzwerke kann ein virtuelles Netzwerk herstellen?

Max. 10 kombiniert für Gateways Basic und Standard dynamisches Routing. 30 für hohe Leistung VPN-Gateways.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Kann ich mit Punkt-zu-Standort-VPNs mit meinem virtuelles Netzwerk mit mehreren VPN-Tunnel?

Ja, Punkt-zu-Standort (P2S) VPNs mit VPN-Gateways mit lokalen Standorten und andere virtuelle Netzwerke dienen.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Kann ich mehrere Tunnel zwischen virtuellen Netzwerk und lokalen Website mit Multi-Standort-VPN konfigurieren?

Nein, redundante Tunnel zwischen Azure virtuelles Netzwerk und einem lokalen Standort werden nicht unterstützt.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Werden Adressräume virtuelle Netzwerke und lokale Standorte kann es überlappende?

Nein. Sich überlappende Adressräume verursacht Network Configuration Dateiupload oder "Erstellen virtuelle Netzwerk" Fehler.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Erhalte ich mehr Bandbreite mit mehr Standort-zu-Standort-VPNs als für ein virtuelles Netzwerk?

Nein, alle VPN-Tunnel, einschließlich Punkt-zu-Standort-VPNs gemeinsam dieselbe Azure VPN-Gateway und der verfügbaren Bandbreite.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Kann ich Azure VPN-Gateway verwenden, Datenverkehr zwischen meinem lokalen oder einem anderen virtuellen Netzwerk passieren?

**Klassische Bereitstellungsmodell**<br>
Transitverkehr über Azure VPN-Gateway ist mit dem klassischen Bereitstellungsmodell beruht auf statisch definierten Adressräume in der Netzwerk-Konfigurationsdatei BGP ist Azure virtuelle Netzwerke und VPN-Gateways mit klassischen Bereitstellungsmodell noch nicht unterstützt. Ohne BGP ist manuell Adressräume Übertragung sehr Fehler anfällig und nicht empfohlen.<br>
**Ressourcen-Manager-Bereitstellungsmodell**<br>
Bei Verwendung das Ressourcen-Manager-Bereitstellungsmodell finden Sie im Abschnitt [BGP](#bgp) Weitere Informationen.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Generiert Azure denselben vorinstallierten IPsec-IKE-Schlüssel für alle VPN-Verbindungen für das virtuelle Netzwerk?

Nein, generiert Azure standardmäßig verschiedene vorinstallierte Schlüssel für verschiedene VPN-Verbindungen. Legen Sie VPN-Gateway Schlüssel REST-API oder PowerShell-Cmdlet können Sie jedoch der Wert gesetzt, in den Sie arbeiten. Der Schlüssel muss alphanumerische Zeichenfolge der Länge zwischen 1 und 128 Zeichen sein.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Berechnet Azure zwischen virtuellen Netzwerken?

Für den Datenverkehr zwischen verschiedenen Azure virtuelle Netzwerke Gebühren Azure nur für Datenverkehr wobei Azure Region. Die Gebührensatz wird der Azure- [VPN-Gateway Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/) Seite aufgeführt.


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Kann ich ein virtuelles Netzwerk mit IPsec VPNs, ExpressRoute-Verbindung anschließen?

Ja, dies unterstützt. Weitere Informationen finden Sie unter [ExpressRoute konfigurieren und Standort-zu-Standort-VPN-Verbindungen, die Koexistenz](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Standortübergreifende Konnektivität und VMs

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Wenn der virtuelle Computer in einem virtuellen Netzwerk und habe eine standortübergreifende Verbindung, wie soll ich für die VM verbinden?

Sie haben einige Optionen. Wenn Sie aktiviert RDP und einen Endpunkt erstellt haben, können Sie den virtuellen Computer mithilfe der VIP verbinden. In diesem Fall geben Sie die VIP-Adresse und den Port, dem Sie möchten. Sie müssen den Port auf dem virtuellen Computer für den Datenverkehr zu konfigurieren. Normalerweise würde Sie zur Azure-Verwaltungsportal und Einstellungen für RDP-Verbindung zu Ihrem Computer. Die Einstellungen enthalten die erforderlichen Verbindungsinformationen.

Haben Sie ein virtuelles Netzwerk mit Cross-Verbindungen konfiguriert, können Sie mithilfe der internen DIP oder private IP-Adresse auf dem virtuellen Computer verbinden. Sie können auch zum virtuellen Computer durch interne DIP von einem anderen virtuellen Computer verbinden, die sich im gleichen virtuellen Netzwerk befinden. RDP werden mit Ihrer virtuellen Maschine DIP verwenden, wenn Sie von einem Standort außerhalb des virtuellen Netzwerks Verbinden nicht. Beispielsweise kann nicht Wenn Sie ein virtuelles Punkt-zu-Standort-Netzwerk konfiguriert und Sie keine von Ihrem Computer Verbindung, den virtuellen Computer durch DIP herstellen.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Ist der virtuelle Computer in einem virtuellen Netzwerk standortübergreifende Konnektivität, geht der Datenverkehr aus meinem VM über diese Verbindung?

Nein. Nur den Verkehr, Ziel gehen in virtuellen Netzwerk lokale Netzwerk IP-Adressbereiche enthalten ist, die angegebene IP über das virtuelle Netzwerkgateway. Verkehr hat das Ziel befindet sich innerhalb des virtuellen Netzwerks IP innerhalb des virtuellen Netzwerks bleibt. Anderer Datenverkehr ist über den Lastenausgleich für öffentliche Netzwerke gesendet oder erzwungenen Tunneln verwendet, Azure VPN-Gateway gesendet. Wenn Sie Probleme haben, ist es wichtig sicherzustellen, dass Sie in Ihrem lokalen Netzwerk, der über das Gateway senden möchten aufgeführten Bereiche haben. Überprüfen Sie, ob lokale Netzwerkadressbereiche mit Adressbereiche in das virtuelle Netzwerk nicht überlappen. Sie möchten auch überprüfen, ob der DNS-Server verwendete Name in die korrekte IP-Adresse aufgelöst wird.


## <a name="virtual-network-faq"></a>Virtuelles Netzwerk FAQ

Zusätzliche virtuelle Netzwerkinformationen im [Virtuellen Netzwerk FAQ](../virtual-network/virtual-networks-faq.md)angezeigt.
 
