<properties
    pageTitle="Netzwerk-Infrastruktur Richtlinien | Microsoft Azure"
    description="Erfahren Sie mehr über wichtigen Entwurf und Implementierung von Richtlinien für die Bereitstellung von virtuellen Netzwerken in Azure Infrastrukturdienste."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Netzwerk-Infrastruktur Richtlinien

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dieser Artikel konzentriert sich auf die erforderlichen Planungsschritte für virtuelle Netzwerke in Azure und Konnektivität zwischen vorhandenen auf Prem verstehen.


## <a name="implementation-guidelines-for-virtual-networks"></a>Implementierungsrichtlinien für virtuelle Netzwerke

Entscheidung:

- Art des virtuellen Netzwerks müssen Sie Ihre IT-Arbeitslast oder Infrastruktur (nur Cloud oder standortübergreifend)?
- Für standortübergreifende virtuelle Netzwerke wie viel müssen Sie Subnetze und VMs jetzt und in Zukunft angemessen Erweiterung hosten?
- Wollen Sie zentralisierte virtuelle Netzwerke erstellen oder einzelne virtuelle Netzwerke für jede Ressourcengruppe erstellen?

Aufgaben:

- Definieren Sie den Adressraum für virtuelle Netzwerke erstellt werden.
- Festlegen der Subnetze und des Adressraums für jeden.
- Für standortübergreifende definieren virtuelle Netzwerke die LAN-Adressräume für die lokalen Lagerorte, die die virtuellen Computer im virtuellen Netzwerk erreichen.
- Arbeiten mit lokalen Netzwerkteam der sicherzustellen beim Erstellen virtueller Netzwerke standortübergreifende konfiguriert ist.
- Das virtuelle Netzwerk mit der Namenskonvention zu erstellen.


## <a name="virtual-networks"></a>Virtuelle Netzwerke

Virtuelle Netzwerke sind erforderlich, um die Kommunikation zwischen virtuellen Maschinen (VMs). Definieren Subnetze, benutzerdefinierten IP-Adresse, DNS-Einstellungen Sicherheitsfilter und Lastenausgleich, mit physischen Netzwerken. Mithilfe eines [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) oder [Express Route Circuit](../expressroute/expressroute-introduction.md)können Sie Azure virtuelle Netzwerke mit dem lokalen Netzwerk verbinden. Erfahren Sie mehr über [virtuelle Netzwerke und deren Komponenten](../virtual-network/virtual-networks-overview.md).

Mithilfe von Ressourcengruppen müssen Sie Flexibilität bei der Gestaltung der virtuellen Netzwerkkomponenten. VMs können virtuelle Netzwerke außerhalb ihrer eigenen Ressourcengruppe verbinden. Ein gemeinsamen Ansatz wäre zentrale Ressourcengruppen, die Infrastruktur des Hauptnetzwerks enthalten, die von einem gemeinsamen Team verwaltet werden können. VMs und deren Anwendung separate Ressourcengruppen bereitgestellt. Dieser Ansatz ermöglicht Besitzer Zugriff auf die Ressourcengruppe, die VMs ohne Zugriff auf die Konfiguration der größeren virtuellen Netzwerken Ressourcen enthält.

## <a name="site-connectivity"></a>Website-Verbindung

### <a name="cloud-only-virtual-networks"></a>Cloud nur virtuelle Netzwerke
Lokale Benutzer und Computer nicht kontinuierliche Verbindung mit VMs in Azure virtuelles Netzwerk benötigen, ist virtual Netzwerkentwurf direkt:

![Einfaches Cloud nur virtuelles Netzwerkdiagramm](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Dieser Ansatz ist in der Regel für Internetzugriff Arbeitslasten wie eine Internet-basierte Webserver. Sie können diese virtuellen Computer über SSH oder Punkt-zu-Standort-VPN-Verbindungen verwalten.

Da sie nicht mit Ihrem lokalen Netzwerk verbunden sind, können nur Azure virtuelle Netzwerke Teil der privaten IP-Adressraum. Adressraum kann denselben privaten Adressraum, die lokal verwendet wird.


### <a name="cross-premises-virtual-networks"></a>Standortübergreifende virtuelle Netzwerke
Wenn lokale Benutzer und Computer laufende Verbindung mit VMs in Azure virtual Network benötigen, erstellen Sie ein virtuelles Netzwerk standortübergreifende. Das virtuelle Netzwerk zu Ihrem lokalen Netzwerk mit einem ExpressRoute oder Standort-zu-Standort-VPN-Verbindung herstellen

![Standortübergreifende virtuelles Netzwerkdiagramm](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

In dieser Konfiguration ist das Azure virtuelles Netzwerk im Wesentlichen eine cloudbasierte Erweiterung Ihres lokalen Netzwerks.

Da sie mit Ihrem lokalen Netzwerk verbinden, standortübergreifende virtuelle Netzwerke verwenden müssen einen Teil des Adressraums der Organisation eindeutig ist. Dabei, die verschiedene Unternehmensstandorte spezielles IP-Subnetz zugeordnet sind, wird Azure erweitern Sie Ihr Netzwerk einen anderen Speicherort.

Pakete von standortübergreifenden virtuellen Netzwerk mit Ihrem lokalen Netzwerk übertragen lässt, müssen Sie entsprechende lokale Adresse Präfixen als Teil der Definition der lokalen Netzwerk für das virtuelle Netzwerk konfigurieren. Je nach den Adressraum des virtuellen Netzwerks und die entsprechenden lokalen Standorten kann es viele Adresspräfixe im lokalen Netzwerk.

Konvertieren eines virtuellen Netzwerks Cloud nur zu einem virtuellen Netzwerk standortübergreifenden, wahrscheinlich erfordert jedoch Re-IP der virtuellen Netzwerkadressbereich und Azure-Ressourcen. Überlegen Sie daher sollte ein virtuelles Netzwerk mit dem lokalen Netzwerk verbunden werden, wenn ein IP-Subnetz zuweisen.

## <a name="subnets"></a>Subnetze
Subnetze können Sie Ressourcen verwalten, die in Beziehung stehen, entweder logisch (z. B. ein Subnetz für dieselbe Anwendung zugeordnete VMs) oder physisch (z. B. einem Subnetz pro Ressourcengruppe). Sie können auch Subnetz isolationstechniken zur Erhöhung der Sicherheit einsetzen.

Für standortübergreifende virtuelle Netzwerke sollten Sie Subnetze mit den gleichen Konventionen entwerfen, die für lokale Ressourcen verwendet. **Immer Azure verwendet die ersten drei IP-Adressen des Adressbereichs für jedes Subnetz**. Ermitteln die benötigten Anzahl von Adressen für das Subnetz zählen VMs jetzt müssen starten. Für zukünftiges Wachstum schätzen und dann die Größe des Subnetzes bestimmt anhand der folgenden Tabelle.

Anzahl der VMs erforderlich | Bitanzahl Host erforderlich | Größe des Subnetzes
--- | --- | ---
1 bis 3 | 3 | / 29
4 – 11     | 4 | von 28
12-27 | 5 | / 27
28 bis 59 | 6 | von 26
60-123 | 7 | / 25

> [AZURE.NOTE] Für normale lokalen Subnetzen ist die maximale Anzahl von Adressen für ein Subnetz mit n Host 2<sup>n</sup> -2. Für ein Subnetz Azure ist die maximale Anzahl von Adressen für ein Subnetz mit n Host 2<sup>n</sup> – 5 (2 plus 3 für die Adressen, die in jedem Subnetz Azure verwendet).

Wenn Sie eine Subnetz Größe, die zu klein ist, müssen Sie Re-IP und erneut die virtuellen Computer im Subnetz.


## <a name="network-security-groups"></a>Netzwerk-Sicherheitsgruppen
Sie können zuweisen Filterregeln der Datenverkehr fließt über virtuelle Netzwerke mit Netzwerk-Sicherheitsgruppen. Steuern des eingehenden und ausgehenden Datenverkehr, Quelle und Ziel IP-Bereiche zulässig, Anschlüsse usw. können Sie präzise Filterregeln Sichern Ihrer virtuellen Netzwerk Umgebung erstellen. Netzwerk-Sicherheitsgruppen können Subnetze in einem virtuellen Netzwerk oder direkt auf bestimmte virtuelle Netzwerkschnittstelle angewendet werden. Es wird empfohlen, einige Netzwerk Sicherheitsgruppenfilter Verkehr auf Ihren virtuellen Netzwerken haben. Erfahren Sie mehr über [Netzwerk-Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).


## <a name="additional-network-components"></a>Zusätzliche Komponenten
Wie bei lokalen physischen Netzwerk Infrastruktur kann Azure virtuelle Netzwerke mehr Subnetze und IP-Adressen enthalten. Eigene entwerfen, möchten Sie einige dieser zusätzlichen Komponenten enthalten:

- [VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md) - Azure virtuelle Netzwerke mit anderen Azure virtuellen Netzwerken verbinden oder lokale Netzwerke über eine Standort-zu-Standort-VPN-Verbindung herstellen. Express Route Verbindungen für dedizierte, sichere Verbindungen zu implementieren. Punkt-zu-Standort-VPN-Verbindungen können auch direkten Zugriff bieten.
- [Lastenausgleich](../load-balancer/load-balancer-overview.md) - bietet Lastenausgleich des Verkehrs für externe und interne wie gewünscht.
- [Application Gateway](../application-gateway/application-gateway-introduction.md) - Lastenausgleich HTTP Lastenausgleich auf Anwendungsebene, bietet zusätzliche Vorteile für ASP.NET-Webanwendungen statt die Azure bereitstellen.
- [Traffic Manager](../traffic-manager/traffic-manager-overview.md) - DNS-datenverkehrsverteilung an Endbenutzer am nächsten verfügbaren Anwendungsendpunkt direkt zum Hosten der Anwendung von Azure Rechenzentren in unterschiedlichen Regionen ermöglicht.


## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 