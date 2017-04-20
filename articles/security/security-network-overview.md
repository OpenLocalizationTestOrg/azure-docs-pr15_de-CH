<properties
   pageTitle="Übersicht über die Sicherheit von Azure Netzwerk | Microsoft Azure"
   description=" Dieser Artikel erleichtert die verstehen, was Microsoft Azure im Bereich Sicherheit anzubieten. Wir erläutern grundlegenden Kernkonzepte Network Security Requirements und Informationen auf welche Azure in diesen Bereichen bieten. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Übersicht über die Sicherheit von Azure-Netzwerk

Microsoft Azure umfasst eine robuste Netzwerkinfrastruktur zur Unterstützung der Anwendung und Konnektivitätsanforderungen Service. Netzwerkkonnektivität ist zwischen Ressourcen in Azure zwischen lokalen und Azure Ressourcen und aus dem Internet und Azure.

Das Ziel dieses Artikels ist zu für Sie einfacher zu verstehen, was Microsoft Azure im Bereich Sicherheit anzubieten. Hier erläutern wir grundlegende für zentrale Netzwerk Sicherheitskonzepte und. Wir Ihnen auch Informationen auf welche Azure in diesen Bereichen bieten. Es gibt zahlreiche Links zu anderen Inhalten, mit denen Sie sich ein besseres Verständnis für die Bereiche, in dem Sie interessiert sind.

Dieser Übersicht über die Sicherheit von Azure-Netzwerk Artikel konzentriert sich auf Folgendes:

- Azure-Netzwerken
- Network Access control
- Secure remote Access und standortübergreifende Konnektivität
- Verfügbarkeit
- Protokollierung
- Auflösung
- DMZ-Architektur
- Azure Security Center

## <a name="azure-networking"></a>Azure-Netzwerken

Virtuelle Computer benötigen Netzwerkkonnektivität. Um diese Anforderung zu unterstützen, muss Azure virtuelle Computer ein virtuelles Azure-Netzwerk verbunden werden. Ein Azure Virtual Network ist ein logischer Konstrukt auf die physische Netzwerkstruktur Azure erstellt. Jede logische Azure Virtual Network ist von allen anderen virtuellen Azure-Netzwerken isoliert. Dies hilft sicherzustellen, dass Netzwerkverkehr in der Bereitstellung nicht anderen Kunden Microsoft Azure.

Weitere Informationen:

- [Virtuelles Netzwerk (Übersicht)](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Network Access Control
Netzwerkzugriffsteuerung ist die Verbindung mit bestimmten Geräten oder Subnetze ein virtuelles Azure-Netzwerk einschränken. Network Access Control soll sicherstellen, dass die virtuellen Computer und Dienste stehen nur Benutzern und Geräten, die sie zugreifen möchten. Zugriffskontrolle basieren auf zulassen oder Verweigern von Beschlüssen für Verbindungen, und der virtuelle Computer oder Dienst.

Azure unterstützt mehrere Typen von Network Access Control. Dazu gehören:

- Ebene steuern
- Steuerelement weiterleiten und Erzwungene Tunnel
- Virtuelles Netzwerk Sicherheits-appliances

### <a name="network-layer-control"></a>Ebene steuern
Sichere Bereitstellung erfordert einige Network Access Control. Network Access Control soll sicherstellen, dass die virtuellen Computer und Netzwerkdienste, die auf diese virtuellen Computer mit anderen Netzwerkgeräten kommunizieren können, sie kommunizieren und alle anderen Verbindungsversuche blockiert.

Benötigen Sie einfachen Zugriff Steuerelement (basierend auf der IP-Adresse und die TCP- oder UDP-Protokolle), können Sie Netzwerk-Sicherheitsgruppen verwenden. Network Security Group (NSG) ist eine grundlegende stateful Packet Filter Firewall und können zur Steuerung des Zugriffs auf ein [5-Tupel](https://www.techopedia.com/definition/28190/5-tuple)basiert. NSGs Überprüfung auf Anwendungsebene oder nicht authentifizierte Zugriffskontrollen.

Weitere Informationen:

- [Netzwerk-Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Route-Steuerelement und erzwungene Tunneln
Routingverhalten im Azure virtuelle Netzwerk steuern ist eine Funktion kritischen Sicherheits- und Steuerelement. Wenn das routing falsch konfiguriert ist, können Programme und Dienste auf dem virtuellen Computer werden mit Geräten Ihre Verbindung, einschließlich von Angreifer.

Azure-Netzwerken ermöglicht das Routingverhalten Netzwerkverkehr auf Azure virtuelle Netzwerke anpassen. So können Sie Standard-Routingtabelleneinträge des virtuellen Netzwerks Azure ändern. Steuerung der Routingverhalten können Sie sicherstellen, dass der gesamte Datenverkehr von einem bestimmten Gerät oder der Gerätegruppe betritt oder verlässt Azure virtuellen Netzwerks über einen bestimmten Standort.

Beispielsweise müssen Sie eine virtuelles Netzwerk Security Appliance im Azure virtuelle Netzwerk. Sie möchten sicherstellen, dass alle Datenverkehr zu und von Ihrem Azure Virtual Network, Security virtual Appliance durchläuft. Hierzu vom [Benutzer definierten Routen](../virtual-network/virtual-networks-udr-overview.md) in Azure konfigurieren.

[Erzwungene Tunnel](https://www.petri.com/azure-forced-tunneling) ist ein Mechanismus, um sicherzustellen, dass Ihre Dienste zum Einleiten einer Verbindung mit Geräten im Internet dürfen verwenden können. Beachten Sie, dass dieser eingehende Verbindungen akzeptieren und zum Antworten auf unterscheidet. Front-End-Webserver müssen Anfrage von Internethosts reagieren und so Internet stammende Datenverkehr kann eingehende diese Webserver und Webserver reagieren können.

Was soll nicht ist ein Front-End-Webserver eine ausgehende Anforderung initiieren. Ersuchen können ein Sicherheitsrisiko darstellen, da diese Verbindungen verwendet werden können, um Malware herunterzuladen. Auch wenn diese Front-End-Server ausgehende Internetanfragen initiieren möchten, empfiehlt es sich zu Ihrem lokalen Webproxys durchlaufen, damit Sie URL-Filterung und Protokollierung nutzen können.

Stattdessen möchten Sie veranschaulicht die erzwungene Tunnel. Beim Aktivieren der erzwungenen Tunneln müssen alle Verbindungen mit dem Internet über das lokale Gateway. Erzwungene Tunnel Benutzer definierten Routen nutzen können.

Weitere Informationen:

- [Was sind Benutzer definierten Routen und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Virtuelles Netzwerk Sicherheits-Appliances
Netzwerk-Sicherheitsgruppen Benutzer definierten Routen und Erzwungene Tunnel eine Sicherheitsstufe auf Netzwerk und Transport des [OSI-Modells](https://en.wikipedia.org/wiki/OSI_model)bereit haben, möglicherweise Ebene über das Netzwerk aktivieren möchten.

Beispielsweise können die Sicherheitserfordernisse enthalten:

- Authentifizierung und Autorisierung vor Zugriff auf die Anwendung
- Intrusion Detection und Intrusion-Antwort
- Überprüfung auf Anwendungsebene für allgemeine Protokolle
- URL-Filterung
- Netzwerk auf Viren und Malware
- Anti-Bot Schutz
- Anwendung-Zugriffskontrolle
- Zusätzliche DDoS-Schutz (über den Schutz der Azure-Struktur selbst bereitgestellten DDoS)

Diese erweiterte Netzwerksicherheitsfeatures möglich mit einer partnerlösung Azure. Der aktuelle Azure Partner finden Network Security Solutions Sie [Azure Marketplace](https://azure.microsoft.com/marketplace/) besuchen und nach "Sicherheit" und "Network Security" suchen.

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Sicheren Remote-Zugriff und Konnektivität zwischen lokalen
Installation, Konfiguration und Verwaltung der Azure-Ressourcen Remote durchgeführt werden müssen. Sie möchten außerdem [hybride IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) -Lösung bereitstellen, die Komponenten lokal in Azure öffentlichen Cloud. Diese Szenarien erfordern eine sichere remote-Zugriff.

Azure-Netzwerken unterstützt die folgenden Szenarien für sicheren remote-Zugriff:

- Einzelne Arbeitsstationen zu einem virtuellen Azure-Netzwerk verbinden
- Verbinden Sie lokalen Netzwerk mit einem virtuellen Netzwerk Azure VPN
- Lokalen Netzwerk mit einem virtuellen Netzwerk Azure eine dedizierte WAN-Verbindung herstellen
- Azure virtuelle Netzwerke miteinander verbunden

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Verbinden Sie einzelne Arbeitsstationen mit Azure Virtual Network
Unter Umständen einzelne Entwickler oder Operationen Personal virtueller Computer und Dienste in Azure verwalten möchten. Beispielsweise benötigen Sie Zugriff auf einen virtuellen Computer in einem virtuellen Netzwerk Azure und Ihre Sicherheitsrichtlinie RDP oder SSH Remotezugriff auf einzelnen virtuellen Maschinen nicht zulassen. In diesem Fall können Sie eine Punkt-zu-Standort-VPN-Verbindung.

Punkt-zu-Standort-VPN-Verbindung verwendet [SSTP VPN-](https://technet.microsoft.com/library/cc731352.aspx) Protokoll, eine private und sichere Verbindung zwischen dem Benutzer und Azure Virtual Network ermöglichen. Nachdem die VPN-Verbindung hergestellt wird, können der Benutzer RDP oder SSH über die VPN-Verbindung in einer virtuellen Maschine in Azure Virtual Network (vorausgesetzt, der Benutzer authentifizieren kann und darf).

Weitere Informationen:

- [Konfigurieren Sie eine Punkt-zu-Standort-Verbindung zu einem virtuellen Netzwerk mit PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Ein Azure virtuelles Netzwerk mit einem VPN lokalen Netzwerk herstellen
Sie möchten Ihre gesamte Unternehmensnetzwerk oder Teile davon ein virtuelles Azure-Netzwerk verbinden. Dies ist häufig in hybride IT Szenarien, in denen Unternehmen [erweitern ihre lokalen Datencenter in Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). In vielen Fällen hosten Unternehmen Dienste in Azure und Teile vor Ort, wie wenn eine Lösung Front-End-Webservern in Azure und lokalen Back-End-Datenbanken enthält. Diese Art von "standortübergreifende" Verbindungen stellen auch Management von Azure befindet sich Ressourcen mehr und Szenarien wie Active Directory-Domänencontroller in Azure erweitern.

Eine Möglichkeit hierfür ist die Verwendung eines [Standort-zu-Standort-VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). Die Standort-zu-Standort-VPN eine Punkt-zu-Standort-VPN unterscheidet, ein Punkt-zu-Standort-VPN ein Gerät mit einer Azure Virtual Network verbindet während ein Standort-zu-Standort-VPN ein gesamtes Netzwerk (z. B. lokale Netzwerk) zu einem virtuellen Azure-Netzwerk verbunden. Standort-zu-Standort-VPNs zu einem virtuellen Netzwerk Azure verwenden sehr sicheren IPsec-Tunnelmodus VPN-Protokoll.

Weitere Informationen:

- [Eine Standort-zu-Standort-VPN-Verbindung über das Azure-Portal erstellen Sie ein Ressourcen-Manager-VNet](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Planung und Entwurf für VPN-gateway](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Lokalen Netzwerk mit einem Azure virtuelle Netzwerk eine dedizierte WAN-Verbindung herstellen
Punkt-zu-Standort- und Standort-zu-Standort-VPN-Verbindungen sind für die standortübergreifende Verbindung aktivieren. Sollten Sie jedoch einige Organisationen haben die folgenden Nachteile:

- VPN-Verbindungen Datentransfer über das Internet – Dies macht diese Verbindungen potenzielle Sicherheitsprobleme mit Daten über ein öffentliches Netzwerk. Darüber hinaus können Zuverlässigkeit und Verfügbarkeit für Internetzugang garantiert werden.
- VPN-Verbindungen, virtuelle Netzwerke Azure Betracht Applikationen und Zwecke wie max am um 200Mbps Bandbreite eingeschränkt.

Organisationen, die höchste Sicherheit und Verfügbarkeit für die standortübergreifende Verbindung normalerweise verwenden dedizierte WAN Links Verbindung zu remote-Standorten. Azure ermöglicht eine dedizierte WAN-Verbindung verwenden, um ein virtuelles Netzwerk Azure lokalen Netzwerk herstellen. Dies wird durch Azure ExpressRoute aktiviert.

Weitere Informationen:

- [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Azure virtuelle Netzwerke miteinander verbunden
Es kann viele Azure virtuelle Netzwerke für die Bereitstellung verwenden. Es gibt viele Gründe, warum Sie dies tun können. Einer der Gründe möglicherweise zur Vereinfachung des Managements; ein anderes möglicherweise aus Sicherheitsgründen. Unabhängig von der Motivation und Nutzen von Ressourcen in Azure virtuellen Netzwerken möglicherweise Situationen Ressourcen aller Netzwerke miteinander verbinden.

Eine Möglichkeit wäre für Dienste auf einem virtuellen Azure-Netzwerk Verbindung zu Diensten auf anderen Azure Virtual Network "Schleife zurück" über das Internet. Verbindung zunächst in eine Azure Virtual Network, gehen über das Internet, und dann wieder zum Ziel Azure Virtual Network. Diese Option stellt die Verbindung zu Internet-basierte Kommunikation Sicherheitsprobleme.

Eine bessere Option möglicherweise ein Azure virtuelle Netzwerk Azure Virtual Network Standort-zu-Standort-VPN erstellen. Azure Virtual Network Azure Virtual Network Standort-zu-Standort VPN verwendet dasselbe Protokoll der [IPSec-Tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx) als standortübergreifende Standort-zu-Standort-VPN-Verbindung erwähnt.

Der Vorteil der Verwendung einer Azure virtuelle Netzwerk Azure Virtual Network Standort-zu-Standort-VPN ist, dass die VPN-Verbindung über Azure Netzwerkstruktur eingerichtet wurde. Es ist nicht über das Internet herstellen. Dies bietet eine zusätzliche Sicherheit im Vergleich zu Standort-zu-Standort-VPNs über das Internet herstellen.

Weitere Informationen:

- [Konfigurieren einer Verbindung VNet VNet mithilfe von Azure-Ressourcen-Manager und PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Verfügbarkeit
Verfügbarkeit ist eine Schlüsselkomponente jeder Sicherheitsprogramms. Wenn Ihre Anwender und Systeme sie über das Netzwerk zugreifen müssen zugreifen können, der Dienst kann als gefährdet. Azure hat Technologien, die die folgenden Hochverfügbarkeits-Mechanismen unterstützt:

- HTTP-basierte Lastenausgleich
- Ebene des Netzwerklastenausgleichs
- Der globale Lastenausgleich

Netzwerklastenausgleich ist ein Mechanismus zur Verbindung zwischen Geräten gleichmäßig verteilt. Die Ziele des Lastenausgleichs sind:

- Verfügbarkeit – Saldo Verbindung über mehrere Geräte laden, ein oder mehrere Geräte nicht verfügbar werden und die Dienste der verbleibenden online Geräte können weiterhin den Inhalt aus dem Dienst
- Mehr Performance – beim Laden Saldo Verbindung über mehrere Geräte ein Gerät muss der Prozessor getroffen. Stattdessen wird die Verarbeitung und Speicher Anforderungen für die Inhalte über mehrere Geräte verteilt.

### <a name="http-based-load-balancing"></a>HTTP-basierte Lastenausgleich
Organisationen, die webbasierte Services häufig ausführen wollen zu einem HTTP-basierten Lastenausgleich vor Web Services angemessene Performance und hohe Verfügbarkeit sicherzustellen. Im Gegensatz zu herkömmlichen netzwerkbasierte Lastenausgleich den Lastenausgleich Beschlüsse HTTP-basierte Lastenausgleich Merkmale des HTTP-Protokolls nicht auf Netzwerk und Transport Layer-Protokolle basieren.

Zum Bereitstellen von HTTP-basierten Lastenausgleich für die webbasierten Dienste bietet Azure Azure Application Gateway. Azure Application Gateway unterstützt:

- HTTP-basierte Lastenausgleich – Load balancing Entscheidungen basierend Eigenschaft spezielle des HTTP-Protokolls
- Cookiebasierte sitzungsaffinität – diese Funktion wird sichergestellt, dass Verbindungen zu einem Server hinter diesem Lastenausgleich zwischen Client und Server erhalten bleibt. Stabilität der Transaktionen wird sichergestellt.
- SSL-Verschiebung – Wenn eine Clientverbindung mit einem Lastenausgleich hergestellt, Sitzung zwischen dem Client und dem Lastenausgleich über die HTTPS verschlüsselt wird (SSL /) Protokoll. Um die Leistung zu verbessern, müssen Sie jedoch die Möglichkeit, die Verbindung zwischen dem Lastenausgleich und dem Webserver hinter Load Balancer verwendet das Protokoll HTTP (unverschlüsselt). Dies ist genannt "SSL-Verschiebung" Webserver hinter dem Lastenausgleich mit Verschlüsselung Prozessoroverhead treten und daher sollen Serviceanfragen schneller.
- URL-basierte Content routing – diese Funktion ermöglicht Lastenausgleich zu wo Verbindungen basierend auf dem Ziel-URL weitergeleitet. Dies bietet mehr Flexibilität als Lösungen Lastenausgleich Entscheidungen IP-Adressen zu laden.

Weitere Informationen:

- [Gateway Anwendungsübersicht](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Ebene des Netzwerklastenausgleichs
Im Gegensatz zu HTTP-basierten Lastenausgleich entscheidet auf Netzwerklastenausgleich laden Lastenausgleich basierend auf IP-Adresse und den Port (TCP oder UDP).
Sie erhalten die Vorteile der Ebene Netzwerklastenausgleich in Azure mithilfe von Azure Lastenausgleich. Einige Hauptmerkmale von Azure Lastenausgleich gehören:

- Ebene des Netzwerklastenausgleichs basierend auf IP-Adresse und Port-Nummern
- Unterstützung für alle Protokoll auf Anwendungsebene
- Laden von Salden Azure virtuellen Maschinen und Cloud services-Instanzen
- Für Internetzugriff (externer Lastenausgleich) und nicht-Internet dienen vor (interne Lastenausgleich) Applikationen und virtuellen Maschinen
- Endpunkt überwachen, die verwendet wird, um festzustellen, ob Dienste hinter den Lastenausgleich verfügbar sind

Weitere Informationen:

- [Internet Facing Lastenausgleich zwischen mehreren virtuellen Computern oder Diensten](../load-balancer/load-balancer-internet-overview.md)
- [Interne Load Balancer (Übersicht)](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Der globale Lastenausgleich
Einige Organisationen sollten die höchstmögliche Verfügbarkeit möglich. Eine Möglichkeit, dieses Ziel zu erreichen ist Host-Applikationen in Global verteilten Rechenzentren. Wenn eine Anwendung in Rechenzentren weltweit gehostet wird, ist es möglich, dass eine geopolitische Region nicht verfügbar und die Anwendung von.

Neben den Vorteilen der Verfügbarkeit durch hosting Applications in Global verteilten Rechenzentren erhalten, erhalten Sie auch Performance-Vorteile. Diese Vorteile erhalten mithilfe eines Mechanismus, das Anfragen für den Dienst für das Rechenzentrum, die am nächsten das Gerät die Anforderung weiterleitet.

Globaler Lastenausgleich bieten diese Vorteile. In Azure können Sie die Vorteile der globale Lastenausgleich mithilfe von Azure Traffic Manager erhalten.

Weitere Informationen:

- [Was ist Traffic Manager?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Protokollierung
Protokollierung auf Netzwerk ist eine wichtige Funktion für alle Network Security Szenario. In Azure können Sie Informationen für Netzwerk-Sicherheitsgruppen Netzwerkebene Protokollieren von Informationen zu protokollieren. NSG Protokollierung erhalten Sie Informationen über:

- Audit-Protokolle – diese Protokolle dienen alle Vorgänge übermittelt Ihre Azure-Abonnements anzuzeigen. Diese Protokolle sind standardmäßig aktiviert und können in Azure-Portal verwendet werden.
- Ereignisprotokolle – Informationen dieser Protokolle NSG Regeln angewendet wurden.
- Zähler Protokolle – diese Protokolle können Sie erkennen, wie oft jede Regel NSG verweigern oder Zulassen von Datenverkehr angewendet wurde.

Sie können auch [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/)eine leistungsstarke Visualisierungstool anzeigen und Analysieren von Ereignisprotokollen.

Weitere Informationen:

- [Protokollanalyse für Netzwerk-Sicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Auflösung
Auflösung ist eine wichtige Funktion für alle Dienste in Azure gehostet werden. Aus Gründen der Sicherheit führt Gefährdung der Name Resolution Funktion Angreifer Anfragen Ihrer Websites auf die Website des Angreifers umleiten. Sichere Auflösung ist eine Voraussetzung für Ihre gehostete Clouddienste.

Es gibt zwei Arten von Auflösung Adresse müssen:

- Interne Auflösung – interne Auflösung wird von Services auf Ihre Azure virtuelle Netzwerke oder Ihren lokalen Netzwerken verwendet. Namen für interne Auflösung sind nicht über das Internet zugänglich. Optimale Sicherheit ist es wichtig, dass Ihr internes Schema nicht für externe Benutzer.
- Auflösung externer Namen – externe Auflösung wird von Personen und Geräten außerhalb der lokalen und Azure virtuelle Netzwerke verwendet. Dies sind die Namen, die im Internet sichtbar und werden verwendet, um eine direkte Verbindung zu Ihrem Cloud-basierte Services.

Für interne Auflösung haben Sie zwei Optionen:

- Ein Azure virtuelle Netzwerk DNS-Server – beim Erstellen einer neuen Azure Virtual Network wird ein DNS-Server für Sie erstellt. Dieser DNS-Server lösen die Namen der Computer in einem virtuellen Netzwerk Azure befinden. Dieser DNS-Server ist nicht konfigurierbar und Azure Fabric Manager, damit sie eine sichere Name Resolution Lösung verwaltet.
- Bringen eigene DNS-Server – Sie haben die Möglichkeit einen DNS-Server Ihrer Wahl auf Ihre Azure Virtual Network. Dieser DNS-Server könnte eine Active Directory-integrierte DNS-Server oder einen dedizierten DNS-serverlösung von einem Azure Partner von Azure Marketplace erhalten können.

Weitere Informationen:

- [Virtuelles Netzwerk (Übersicht)](../virtual-network/virtual-networks-overview.md)
- [Verwalten von DNS-Server von einem virtuellen Netzwerk (VNet)](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

Für externe DNS-Auflösung haben Sie zwei Optionen:

- Eigene externe DNS-Server lokal gehostet
- Hosten Sie Ihre eigenen externen DNS-Server mit einem Internetdienstanbieter

Viele große Organisationen werden eigene DNS-Server lokal hosten. Sie können dafür sie Netzwerke Fachwissen und globale Präsenz zu tun haben.

In den meisten Fällen ist es besser, die DNS-Namensauflösungsdienste mit einem Internetdienstanbieter hosten. Dieser Dienstanbieter haben Netzwerkkenntnisse und globaler Präsenz auf hohe Verfügbarkeit für die Namensauflösungsdienste. Verfügbarkeit ist für DNS-Dienste, denn wenn die Namensauflösungsdienste fehlschlagen, niemand Ihre Dienste mit Internetzugriff zu.

Azure bietet eine hohe Verfügbarkeit und leistungsfähige externe DNS-Lösung in Form von Azure DNS. Externer Name Resolution Lösung nutzt weltweit Azure DNS-Infrastruktur. Dadurch werden Ihre Domäne in Azure die gleichen Anmeldeinformationen, APIs, Tools und als Ihre Azure Services hosten. Als Teil von Azure erbt es auch sicherer Steuerelemente in die Plattform integriert.

Weitere Informationen:

- [Azure DNS (Übersicht)](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ-Architektur
Viele Unternehmen verwenden DMZ zur Segmentierung ihrer Netzwerke erstellen eine Pufferzone zwischen dem Internet und die Dienste. DMZ Teil des Netzwerks ist niedrig Sicherheitszone und keine wertvollen Ressourcen sind in diesem Netzwerksegment. Sie sehen in der Regel Sicherheit Netzwerkgeräte, die eine Netzwerkschnittstelle im DMZ-Segment und einem anderen Netzwerk verbunden, ein Netzwerk mit virtueller Computer und Dienste, die aus dem Internet eingehende Verbindungen akzeptieren.

Es gibt eine Reihe von Variationen der DMZ Design und die Entscheidung zur Bereitstellung einer DMZ und welche DMZ verwenden möchten, verwenden Sie dann basiert auf Ihrem Netzwerk Sicherheit.

Weitere Informationen:

- [Microsoft Cloud-Dienste und Sicherheit](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Azure Security Center
Security Center können Sie verhindern, erkennen und reagieren auf Gefahren und erhöhte Transparenz, und Kontrolle über die Sicherheit von Azure Ressourcen bietet. Bietet integrierte Überwachung und Policy-Management über Abonnements Azure, Gefahren, die andernfalls vielleicht übersehen würden und arbeitet mit einem ausgedehnten System Security Solutions hilft bei der Ermittlung.

Azure Security Center können Sie optimieren und Überwachen von Netzwerkdiensten durch:

- Bereitstellen von Netzwerk-Sicherheitsaspekte
- Überwachen des Status der Netzwerkkonfiguration Sicherheit
- Warnung vor netzwerkbasierten Angriffen beide Endpunkt und Netzwerk

Weitere Informationen:

- [Einführung in Azure Security Center](../security-center/security-center-intro.md)
