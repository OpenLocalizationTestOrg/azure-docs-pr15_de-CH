<properties
   pageTitle="Azure Network Security Best Practices | Microsoft Azure"
   description="Dieser Artikel enthält eine Reihe von best Practices für Network Security mit Azure Funktionen integriert."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Azure Network Security Best Practices

Microsoft Azure können Sie virtuelle Computer und Geräte Netzwerkgeräte herstellen, indem sie auf Azure virtuelle Netzwerke. Ein Azure Virtual Network ist ein virtuelles Netzwerk-Konstrukt, das virtuelle Netzwerk-Schnittstellenkarten zu einem virtuellen Netzwerk TCP/IP-basierte Kommunikation zwischen Netzwerkgeräten aktiviert verbunden. Azure virtuelle Computer mit einem virtuellen Azure-Netzwerk verbunden sind Geräte im gleichen Azure virtuelle Netzwerk verschiedene Azure virtuelle Netzwerke auf dem Internet oder im lokalen Netzwerk herstellen.

In diesem Artikel besprechen wir eine Sammlung von Azure Network Security best Practices. Diese Vorgehensweisen sind aus unserer Erfahrung mit Azure-Netzwerken und Erfahrungen Kunden wie Sie.

Für jede best Practice erklären wir:

- Was ist die beste Vorgehensweise
- Warum soll, empfohlen
- Was möglicherweise scheitern am besten aktivieren
- Mögliche Alternativen empfohlen
- Wie erhalten Sie die besten aktivieren

In diesem Artikel Azure Network Security Best Practices basiert auf einen Konsens und Azure-Plattformfunktionen und Features, wie sie zum Zeitpunkt dieser Artikel geschrieben wurde. Stellungnahmen und Technologie Zeitverlauf und dieser Artikel wird in regelmäßigen Abständen entsprechend aktualisiert.

Azure Network Security Vorgehensweisen in diesem Artikel beschriebenen gehören:

- Logisch Segment Subnetze
- Routing-Verhalten steuern
- Erzwungene Tunnel aktivieren
- Virtuelle Netzwerk Geräte
- Bereitstellen von DMZ für Sicherheit zoning
- Vermeiden Sie das Internet mit dedizierten WAN links
- Verfügbarkeit und Leistung optimieren
- Globale Lastenausgleich verwenden
- RDP Azure Virtual Machines deaktivieren
- Aktivieren Azure Security Center
- Erweitern Sie Ihr Rechenzentrum in Azure


## <a name="logically-segment-subnets"></a>Logisch Segment Subnetze

[Azure Virtual Networks](https://azure.microsoft.com/documentation/services/virtual-network/) ähneln einem LAN im lokalen Netzwerk. Die Idee hinter einer Azure Virtual Network werden mit Ihrer [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/)findet ein einzelnes privates IP-Adresse Speicherplatz-Netzwerk erstellen. Die private IP-Adressbereiche verfügbar sind in der Klasse A (10.0.0.0/8), Klasse B (172.16.0.0/12) und C (192.168.0.0/16) Bereiche.

Ähnlich dem, was du lokal, größeren Adressraum in Subnetze unterteilen möchten. [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) -basierte Subnetting Prinzipien können Sie die Subnetze erstellen.

Routing zwischen Subnetzen erfolgt automatisch und nicht Routingtabellen manuell konfigurieren müssen. Die Standardeinstellung ist jedoch gibt es keine Netzwerk-Zugriffskontrolle zwischen Subnetzen Azure virtuelles Netzwerk erstellen. Um Netzwerk-Zugriffskontrolle zwischen Subnetzen zu erstellen, müssen Sie etwas zwischen den Subnetzen.

Eines der Dinge, die Sie verwenden können, um diese Aufgabe ist eine [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) (NSG). NSGs sind einfache stateful Packet Inspection Geräte, 5-Tupel (die IP-Quelladresse, Quellport IP-Zieladresse, Zielport und Layer 4-Protokoll) Ansatz Zulassen/Verweigern erstellt Regeln für den Netzwerkverkehr. Sie können gewähren oder verweigern Datenverkehr von und zu einzelnen IP-Adresse zu mehrere IP-Adressen oder sogar und von Teilnetzes.

NSGs können für Netzwerkzugriffsteuerung zwischen Subnetzen Sie Ressourcen zu, die der gleichen Sicherheitszone oder Rolle in ihren jeweiligen Subnetzen angehören. Z. B. eine einfache 3-Tier-Anwendung, die eine Webebene und eine Logik Anwendungsebene eine Datenbankebene vorstellen. Sie stellen virtuelle Computer, die auf jeder dieser Ebenen in ihren jeweiligen Subnetzen angehören. Dann verwenden Sie Datenverkehr zwischen den Subnetzen NSGs:

- Web-Tier-virtuelle Maschinen können nur Zugriff auf die Anwendung Logik Computer initiieren und akzeptieren nur Clientverbindungen aus dem Internet
- Anwendung Logik virtuelle Computer können nur mit Datenbankebene initiieren und akzeptieren nur Verbindungen von der Webebene
- Datenbank Tiering virtueller Maschinen kann nicht außerhalb ihrer eigenen Subnetz Verbindung initiieren und akzeptieren nur Verbindung von der Anwendungsebene Logik

Weitere Informationen zum Netzwerk-Sicherheitsgruppen und deren Verwendung logisch Segmentieren Ihrer Azure virtuelle Netzwerke finden Sie im Artikel [Was ist eine Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Routing-Verhalten steuern

Wenn Sie einen virtuellen Computer auf einem virtuellen Azure-Netzwerk, sehen Sie, den virtuellen Computer mit anderen virtuellen Computern auf demselben Azure virtuelle Netzwerk verbinden, auch wenn die anderen virtuellen Computer in verschiedenen Subnetzen. Die deshalb möglich ist, es ist eine Sammlung von systemrouten, die standardmäßig aktiviert sind, die diese Art der Kommunikation zulassen. Dieser Standardrouten ermöglichen virtuelle Computer im gleichen virtuellen Azure-Netzwerk Verbindungen untereinander und mit dem Internet (für die ausgehende Kommunikation mit dem Internet) herstellen.

Während System Standardrouten für viele Bereitstellungsszenarios nützlich sind, gibt die Routingkonfiguration für die Bereitstellung anpassen möchten. Diese Anpassung können Sie zum Konfigurieren der nächsten Abschnittsadresse um bestimmte Ziele zu erreichen.

Wir empfehlen Benutzer definierten Routen konfigurieren, wenn Sie eine virtuelles Netzwerk Security Appliance bereitgestellt in späteren besten besprochen werden.

> [AZURE.NOTE] Benutzer definierten Routen sind nicht erforderlich, und die Standardrouten System funktioniert in den meisten Fällen.

Sie erhalten weitere Informationen über Benutzer definierten Routen und den Artikel [Was sind Benutzer definiert und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md)konfiguriert.

## <a name="enable-forced-tunneling"></a>Erzwungene Tunnel aktivieren

Zum besseren Verständnis erzwungenen Tunnel ist es hilfreich zu verstehen, was "Split tunneling".
Das einfachste Beispiel Split-Tunneling wird mit VPN-Verbindungen angezeigt. Angenommen Sie, eine VPN-Verbindung von Hotelzimmer mit dem Unternehmensnetzwerk herstellen. Dieser Verbindung können Sie Ressourcen in Ihrem Unternehmensnetzwerk herstellen und alle Kommunikation auf Ressourcen im Unternehmensnetzwerk durchlaufen den VPN-Tunnel.

Was geschieht, wenn Sie Ressourcen im Internet herstellen möchten? Wenn Split-tunneling aktiviert ist, wechseln die Verbindung direkt mit dem Internet und nicht über den VPN-tunnel Einige Sicherheitsexperten betrachten Sie potenzielle Risiken und empfehlen, Split-tunneling zu deaktivieren und alle Verbindungen für das Internet und für Unternehmensressourcen, gehen über den VPN-Tunnel. Der Vorteil dabei ist Verbindung zum Internet müssen dann Sicherheitsgeräte Firmennetzwerk der Fall wäre, wenn der VPN-Client mit dem Internet außerhalb der VPN-Tunnel verbunden.

Jetzt Lasst uns zurück zu virtuellen Maschinen auf einem virtuellen Azure-Netzwerk. Standardrouten für ein virtuelles Netzwerk Azure ermöglichen virtuelle Computer Datenverkehr zum Internet initiieren. Diese kann auch ein Sicherheitsrisiko darstellen, und diese ausgehenden Verbindungen können die Angriffsfläche eines virtuellen Computers erhöhen von Angreifern genutzt werden.
Aus diesem Grund wird empfohlen, Erzwungene Tunnel auf virtuellen Maschinen bei standortübergreifende Konnektivität zwischen dem virtuellen Netzwerk Azure und lokalen Netzwerk aktivieren. Wir sprechen über cross-Verbindungen in diesem Azure networking best Practices Dokument.

Wenn Sie keinen Cross lokale Verbindung sicherstellen nutzen Sie Netzwerk-Sicherheitsgruppen (siehe oben) oder Azure virtual Security Appliances (diskutiert weiter) verhindern ausgehende Verbindungen zum Internet der Azure-Computer im Netzwerk.

Erfahren Sie mehr über tunneling, Aktivierung gezwungen lesen Sie Artikel [Erzwungene Tunnel konfigurieren PowerShell und Azure-Ressourcen-Manager verwenden](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Virtuelles Netzwerk Geräte

Sicherheitsgruppen und Benutzer definierten Routing eine gewisse Netzwerkdiensten in der Netzwerk- und Schichten des [OSI-Modells](https://en.wikipedia.org/wiki/OSI_model)bereit, werden es Situationen, wo Sie möchten oder müssen auf hoher Ebene des Stapels aktivieren. In solchen Situationen wird empfohlen, virtuelle Netzwerksicherheitsgeräte Azure Partner bereitstellen.

Netzwerksicherheitsgeräte Azure können liefern deutlich erhöhte Sicherheitsstufen von Kontrollen Netzwerk bereitgestellt werden. Die bereitgestellten virtuellen Netzwerksicherheitsgeräte Netzwerk Sicherheitsfunktionen gehören:

- Firewall
- Intrusion Detection/Intrusion Prevention
- Schwachstellen-management
- Anwendungskontrolle
- Netzwerkbasierte Normalbetriebswerte
- Web-Filterung
- Antivirus
- Botnet-Schutz

Benötigen Sie eine höhere Sicherheitsstufe Netzwerk als mit Netzwerk Zugriff zu erhalten, empfehlen wir, untersuchen und Azure virtual Network Security Appliances bereitzustellen.

Erfahren Sie darüber, welche Azure virtual Network Security Appliances verfügbar sind und ihre Funktionen finden Sie auf [Azure Marketplace](https://azure.microsoft.com/marketplace/) und suchen Sie nach "Sicherheit" und "Network Security".

##<a name="deploy-dmzs-for-security-zoning"></a>Bereitstellen von DMZ für Sicherheit zoning
Eine DMZ oder "Umkreisnetzwerk" einem physischen oder logischen Netzwerksegment ermöglichen eine zusätzliche Sicherheitsebene zwischen Ihr und dem Internet. Die DMZ soll spezielle Access Control Netzwerkgeräte auf Rand im DMZ-Netzwerk platzieren und in Ihre Azure Virtual Network Security Netzwerkgerät erwünschter Datenverkehr zulässig ist.

DMZ sind nützlich, da Sie Ihr Steuerelement Verwaltung, Überwachung, Protokollierung und Berichterstattung Geräte am Rand des Netzwerks Azure Virtual konzentrieren können. Hier würden Sie normalerweise DDoS-Blockierung, Intrusion Detection-Intrusion Prevention-Systeme (IDS/IPS), Firewall-Regeln und Richtlinien, Webfilter, Netzwerk Antimalware und mehr aktivieren. Das Netzwerk zwischen dem Internet und Ihrem Azure Virtual Network sitzen und Schnittstelle in beiden Netzwerken.

Die grundlegende Gestaltung einer DMZ ist, gibt es viele verschiedene DMZ Designs, wie kaskadierende dreifach vernetzte, mehrfach vernetzt und andere.

Es empfiehlt sich für hohe Sicherheitssysteme, Bereitstellen einer DMZ um die Network Security Azure Ressourcen zu verbessern.

Erfahren Sie mehr über DMZ und Bereitstellung in Azure Lesen Sie den Artikel [Microsoft Cloud-Dienste und Sicherheit](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Vermeiden Sie das Internet mit dedizierten WAN links
Viele Organisationen haben die hybride IT-Route. Hybride IT gehören Informationsressourcen des Unternehmens in Azure, während andere lokale bleiben. In vielen Fällen läuft einige Komponenten eines Dienstes in Azure während andere Komponenten lokal bleiben.

Hybrid-IT-Szenario ist normalerweise ein standortübergreifende Konnektivität. Das standortübergreifende Konnektivität ermöglicht Unternehmen, ihre lokale Netzwerke Azure Virtual Networks. Zwei standortübergreifende Konnektivitätslösungen stehen zur Verfügung:

- Standort-zu-Standort-VPN
- ExpressRoute

[Standort-zu-Standort-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) stellt eine virtuelle private Verbindung zwischen dem lokalen Netzwerk und ein virtuelles Azure-Netzwerk. Diese Verbindung erfolgt über das Internet und ermöglicht es Ihnen, Informationen in eine verschlüsselte Verbindung zwischen dem Netzwerk und Azure "Tunnel". Standort-zu-Standort-VPN ist eine sichere, ausgereifte Technologie, die Unternehmen aller Größen jahrzehntelang bereitgestellt wurde. Tunnel-Verschlüsselung erfolgt im [IPSec-Tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx).

Standort-zu-Standort-VPN eine vertrauenswürdige, zuverlässige und etablierte Technologie ist, Datenverkehr innerhalb des Tunnels im Internet durchsuchen. Darüber hinaus ist eingeschränkter Bandbreite relativ maximal zu 200Mbps.

Benötigen Sie ein außergewöhnliches Maß an Sicherheit und Leistung für die standortübergreifende Verbindung empfohlen Azure ExpressRoute für die standortübergreifende Verbindung verwenden. ExpressRoute ist ein dediziertes WAN Verbindung zwischen Ihren lokalen Standort oder einem Hostinganbieter Exchange. Dies ist eine Telco-Verbindung Daten nicht über das Internet übertragen und daher nicht der Internetkommunikation potenziellen Risiken ausgesetzt.

Lesen Sie den Artikel [ExpressRoute technische Übersicht](../expressroute/expressroute-introduction.md), erfahren Sie mehr über die Funktionsweise von Azure ExpressRoute und bereitstellen.

## <a name="optimize-uptime-and-performance"></a>Verfügbarkeit und Leistung optimieren
Vertraulichkeit, Integrität und Verfügbarkeit (CIA) besteht aus der Triade einflussreichsten Sicherheitsmodell von heute. Vertraulichkeit wird zur Verschlüsselung und Datenschutz und Integrität wird sichergestellt, dass Daten durch Unbefugte nicht geändert und Verfügbarkeit sicherstellen, dass autorisierte Anwender können sie berechtigt sind, Zugriff auf Informationen zuzugreifen. Fehler in einem dieser Bereiche darstellt eine mögliche Verletzung Sicherheit.

Verfügbarkeit ist sozusagen als über Verfügbarkeit und Leistung. Wenn ein Dienst ausgefallen ist, kann auf Informationen zugegriffen werden. Wenn Leistung so schlecht, Daten unbrauchbar ist, dann berücksichtigen wir nicht zugegriffen werden. Aus Gründen der Sicherheit müssen wir daher alles tun, um sicherzustellen, dass unsere Services optimale Verfügbarkeit und Leistung.
Eine beliebte und effektive Methode zur Verbesserung der Verfügbarkeit und Leistung verwendet werden Lastenausgleich verwendet. Lastenausgleich ist eine Methode zum Verteilen von Netzwerkdatenverkehr auf Servern, die Teil eines Diensts. Beispielsweise haben Sie Front-End-Server als Teil Ihres-Dienstes können Sie Netzwerklastenausgleich zum Verteilen des Datenverkehrs auf mehrere Front-End-Webserver.

Diese Verteilung von Datenverkehr erhöht die Verfügbarkeit, da Wenn einer der Webserver ausfällt, Lastenausgleich beenden zu diesem Server Datenverkehr senden und der Server noch online Datenverkehr umleiten. Lastenausgleich trägt ebenfalls zur Leistungssteigerung, da Server Prozessor, Netzwerk und Speicher Aufwand für Anfragen mit verteilt werden, wenn die Last ausgeglichen.

Wir empfehlen Sie Lastenausgleich möglich und entsprechend Ihrer Dienste einsetzen. Wir werden Angemessenheit in den folgenden Abschnitten befassen.
Ebene Azure Virtual Network bietet Azure mit drei primäre Lastenausgleich geladen:

- HTTP-basierte Lastenausgleich
- Externer Lastenausgleich
- Interne Lastenausgleich

## <a name="http-based-load-balancing"></a>HTTP-basierte Lastenausgleich
HTTP-Lastenausgleich baut entscheiden, welche Server Verbindung über Eigenschaften des HTTP-Protokolls an. Azure hat ein mit dem Namen Application Gateway HTTP-Lastenausgleich.

Empfehlen Sie uns Azure Application Gateway bei:

- Clientanwendungen, die Anfragen aus der gleichen Benutzer-Client-Sitzung zu demselben Back-End virtuellen Computer benötigen. Beispiele hierfür wäre Warenkorb apps und Web-Mail-Server kaufen.
- Clientanwendungen, die Web-Serverfarmen von SSL-Beendigung oben frei Application Gateway [SSL des](https://f5.com/glossary/ssl-offloading) Features nutzen möchten.
- Programme wie einem Inhaltszustellungsnetzwerk benötigen, die mehrere HTTP-Anfragen über dieselbe langer TCP-Verbindung weitergeleitet werden oder an verschiedene Back-End-Server verteilt.

Weitere Informationen zu Azure Application Gateway funktioniert und wie es die Bereitstellung Verwendung in lesen Sie den Artikel [Application Gateway Overview](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Externer Lastenausgleich
Externer Lastenausgleich erfolgt eingehende Verbindungen über das Internet auf Ihre Server in ein virtuelles Azure-Netzwerk verteilt sind. Externe Azure Lastenausgleich bieten Ihnen diese Funktion und Verwendung sticky Sessions nicht erforderlich oder offload SSL empfohlen.

Im Gegensatz zu HTTP-basierten Lastenausgleich verwendet externe Lastenausgleich Informationen auf Netzwerk und Transport des OSI-Modells Netzwerke zu auf welchem Server Saldo Verbindung zu laden.

Wir empfehlen die Verwendung externer Lastenausgleich Wenn [statusfreie Anwendung](http://whatis.techtarget.com/definition/stateless-app) akzeptiert eingehende Anfragen aus dem Internet.

Erfahren mehr über die Funktionsweise von Azure externer Lastenausgleich und wie bereitstellen, können Sie bitte lesen Sie den Artikel [Erste Schritte erstellen eine Internet Facing Lastenausgleich in Ressourcen-Manager mit PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Interne Lastenausgleich
Interne Lastenausgleich ähnelt dem externen Lastenausgleich und verwendet denselben Mechanismus Saldo Verbindungen zu den Servern dahinter geladen. Der einzige Unterschied ist, dass Lastenausgleich bei Clientverbindungen aus virtuellen Computern akzeptiert, die nicht auf das Internet. In den meisten Fällen sind die Verbindungen, die für den Lastenausgleich akzeptiert werden von Geräten in einem virtuellen Netzwerk Azure initiiert.

Wir empfehlen die Verwendung interner Lastenausgleich für Szenarien diese Fähigkeit profitieren, z. B. Wenn Sie Saldo Verbindungen zu SQL Server oder interne Webserver geladen.

Weitere Informationen wie Azure internen Lastenausgleich funktioniert und wie Sie es bereitstellen können, lesen Sie den Artikel [Erste Schritte erstellen eine interne Lastenausgleich mithilfe von PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Globale Lastenausgleich verwenden
Öffentliche Cloud computing macht es global bereitgestellt Applikationen Komponenten in Rechenzentren auf der ganzen Welt verteilt. Dies ist aufgrund der Azure globale Datencenter auf Microsoft Azure möglich. Im Gegensatz zu den Lastenausgleich Technologies erwähnten ermöglicht der globale Lastenausgleich Dienste verfügbar machen auch ganze Rechenzentren möglicherweise nicht mehr verfügbar sind.

Sie erhalten diese Art von globaler Lastenausgleich in Azure [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)nutzen. Traffic Manager macht Saldo Verbindungen auf Ihre Dienste anhand der Position des Benutzers geladen.

Wenn der Benutzer eine Anforderung an den Dienst aus der EU macht, wird die Verbindung auf Ihre Dienste in einem EU-Rechenzentrum weitergeleitet. Dieser Teil der Traffic Manager global laden Lastenausgleich trägt zur Leistungssteigerung, da der nächsten Rechenzentrum ist schneller mit weit Rechenzentren.

Auf Verfügbarkeit stellt ein globaler Lastenausgleich sicher, dass Ihr Dienst verfügbar ist, sollte auch ein gesamtes Rechenzentrum verfügbar werden.

Beispielsweise sollte ein Azure-Rechenzentrum nicht verfügbar aus ökologischen Gründen oder Ausfällen (z. B. regionale Netzwerkfehler), Remoteverbindungen mit Ihrem Dienst würde werden umgeleitet zu der nächsten Online-Rechenzentrum. Diese globale Lastenausgleich erfolgt nutzen DNS-Richtlinien, die in Traffic Manager erstellen können.

Wir empfehlen die Verwendung von Traffic Manager für eine Cloudlösung, die Sie entwickeln, die einen verbreiteten Bereich in mehreren Regionen und erfordert höchste Verfügbarkeit möglich.

Erfahren Sie mehr über Azure Traffic Manager und Bereitstellung, lesen Sie den Artikel [Was ist Traffic Manager](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Deaktivieren Sie RDP-SSH-Zugriff auf Azure Virtual Machines
Es ist möglich, Azure Virtual Machines über [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) und [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) Protokolle zu erreichen. Diese Protokolle verwalten virtueller Computer von Remotestandorten aus ermöglichen und Datacenter computing sind.

Das mögliche Sicherheitsproblem mit diesen Protokollen über das Internet ist, dass Angreifer Zugriff auf Azure Virtual Machines [brute-Force-](https://en.wikipedia.org/wiki/Brute-force_attack) Techniken verwenden können. Nachdem der Angreifer zugreifen können verwenden den virtuellen Computer als einen Ausgangspunkt für andere Computer im Netzwerk virtuelle Azure gefährden oder sogar Angriffe Netzwerkgeräten von Azure.

Aus diesem Grund wird empfohlen, direkten RDP und SSH-Zugriff auf Ihre Azure virtuellen Computer aus dem Internet zu deaktivieren. Nach direkter RDP und SSH-Zugriff aus dem Internet deaktiviert ist, haben Sie andere Optionen können auf diese virtuellen Computer für die Remoteverwaltung:

- Punkt-zu-Standort-VPN
- Standort-zu-Standort-VPN
- ExpressRoute

[Punkt-zu-Standort-VPN-](../vpn-gateway/vpn-gateway-point-to-site-create.md) ist ein anderer Begriff für eine RAS-VPN-Client-Server-Verbindung. Ein Punkt-zu-Standort-VPN ermöglicht einem einzelnen Benutzer über das Internet eine Verbindung mit einer Azure Virtual Network. Nachdem die Punkt-zu-Standort-Verbindung werden der Benutzer RDP oder SSH Verbindung alle virtuellen Computer im virtuellen Azure-Netzwerk, die der Benutzer über Punkt-zu-Standort-VPN-Verbindung verwenden. Dies setzt voraus, dass der Benutzer berechtigt ist, auf diese virtuellen Computer erreichen.

Punkt-zu-Standort-VPN ist sicherer als direkte RDP oder SSH-Verbindung, da der Benutzer zweimal mit einem virtuellen Computer authentifizieren. Der Benutzer muss zuerst zu authentifizieren (berechtigt) Punkt-zu-Standort-VPN-Verbindung, herstellen Zweitens muss der Benutzer authentifizieren (und berechtigt), RDP oder SSH-Sitzung herzustellen.

[Standort-zu-Standort-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) verbindet ein gesamtes Netzwerk zu einem anderen Netzwerk über das Internet. Ein Standort-zu-Standort-VPN können Sie ein virtuelles Netzwerk Azure lokalen Netzwerk herstellen. Wenn ein Standort-zu-Standort-VPN Benutzer auf bereitstellen lokalen Netzwerk werden Verbindung zum virtuellen Computer im virtuellen Netzwerk Azure über RDP oder SSH-Protokoll über die Standort-zu-Standort-VPN-Verbindung und erfordert keine direkten RDP oder SSH-Zugriff über das Internet zulässt.

Eine dedizierte WAN-Verbindung können Sie ähnliche Standort-zu-Standort-VPN-Funktionalität bereitzustellen. Die Hauptunterschiede sind 1. dedizierte WAN-Verbindung durchlaufen nicht im Internet und 2. dedizierte WAN Links sind in der Regel stabiler und leistungsfähige. Azure bietet eine dedizierte WAN-Verbindung Lösung in Form von [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Aktivieren Azure Security Center
Azure Security Center können Sie verhindern, erkennen und reagieren auf Gefahren und erhöhte Transparenz, und Kontrolle über die Sicherheit von Azure Ressourcen bietet. Bietet integrierte Überwachung und Policy-Management über Abonnements Azure, Gefahren, die andernfalls vielleicht übersehen würden und arbeitet mit einem ausgedehnten System Security Solutions hilft bei der Ermittlung.

Azure Security Center können Sie optimieren und Überwachen von Netzwerkdiensten durch:

- Bereitstellen von Netzwerk-Sicherheitsaspekte
- Überwachen des Status der Netzwerkkonfiguration Sicherheit
- Warnung vor netzwerkbasierten Angriffen beide Endpunkt und Netzwerk

Wir empfehlen, alle Ihre Azure-Bereitstellung Azure Security Center zu aktivieren.

Erfahren Sie mehr über Azure Security Center und für die Bereitstellung zu aktivieren, lesen Sie den Artikel [Einführung in Azure Security Center](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Sichere Erweiterung Rechenzentrum in Azure
Viele Unternehmen möchten Unternehmen in die Cloud anstatt ihre lokalen Rechenzentren immer erweitern. Diese Erweiterung stellt eine Erweiterung der vorhandenen IT-Infrastruktur in der öffentlichen Cloud. Nutzen standortübergreifende Konnektivitätsoptionen kann nur ein Subnetz Netzwerkinfrastruktur vor Ort Ihrer Azure virtuelle Netzwerke behandelt.

Es ist jedoch viel Planung Probleme, zuerst behandelt werden. Dies ist besonders wichtig im Bereich Sicherheit. Eine gut verständliche Weise ein Design ist ein Beispiel.

Microsoft hat die [Erweiterung Reference Architecture Datencenter](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) und Begleitmaterial erfahren diese Erweiterung Datacenter aussehen würde. Dadurch wird eine beispielhafte Referenz, die Sie zum Planen und Entwerfen einer secure Enterprise Datacenter-Erweiterung in die Cloud verwenden können. Wir empfehlen, dieses Dokument einen Überblick über die wichtigsten Komponenten einer sicheren Lösung überprüfen.

Weitere Informationen zum sicheren Rechenzentrum in Azure erweitern zeigen Sie video [Erweitert Ihr Rechenzentrum Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
