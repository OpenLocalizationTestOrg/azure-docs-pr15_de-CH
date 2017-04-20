<properties
   pageTitle="ExpressRoute FAQ"
   description="FAQ ExpressRoute enthält Informationen zu Azure Services unterstützt, Kosten, Daten und Verbindungen, SLA, Anbieter und Standorte, Bandbreite und weitere technische Details."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="expressroute-faq"></a>ExpressRoute FAQ


## <a name="what-is-expressroute"></a>Was ist ExpressRoute?
ExpressRoute ist ein Azure Service, private Verbindung zwischen Microsoft-Rechenzentren und Infrastruktur vor Ort oder in einem Colocation erstellen. ExpressRoute-Verbindung nicht über das öffentliche Internet gehen und bieten höhere Sicherheit und Zuverlässigkeit mit niedriger Latenz als normalen Verbindungen über das Internet.

### <a name="what-are-the-benefits-of-using-expressroute-and-private-network-connections"></a>Was sind die Vorteile von ExpressRoute und private Netzwerk?
ExpressRoute-Verbindung nicht über das öffentliche Internet gehen und bieten höhere Sicherheit und Zuverlässigkeit mit niedriger und konsistente Wartezeiten als normalen Verbindungen über das Internet. In einigen Fällen kann mit ExpressRoute Anschlüsse zur Datenübertragung zwischen lokalen Geräte und Azure erhebliche Kostenvorteile.

### <a name="what-microsoft-cloud-services-are-supported-over-expressroute"></a>Welche Microsoft Cloud-Dienste werden über ExpressRoute unterstützt?
ExpressRoute unterstützt die meisten Microsoft Azure Services heute einschließlich Office 365.  Schnell auf allgemeine Verfügbarkeit nach Updates suchen.

### <a name="where-is-the-service-available"></a>Der Dienst verfügbar ist?
Seite für Speicherort und Verfügbarkeit: [ExpressRoute Partner und Speicherorte](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-to-connect-to-microsoft-if-i-dont-have-partnerships-with-one-of-the-expressroute-carrier-partners"></a>Wie kann ich ExpressRoute Verbindung an Microsoft, wenn ich Partnerschaften mit ExpressRoute-Carrier Partner verwenden?
Wählen einen regionalen Netzbetreiber und land Ethernet-Anschlüsse eines unterstützten Exchange Provider Speicherorte. Sie können dann mit Microsoft an Anbieter peer. Überprüfen Sie im letzten Abschnitt [ExpressRoute Partner und Speicherorte](expressroute-locations.md) Dienstanbieter in einer Exchange-Standorte vorhanden ist. Sie können dann eine ExpressRoute-Verbindung durch den Dienstanbieter Verbindung zu Azure bestellen.

### <a name="how-much-does-expressroute-cost"></a>Wie viel kostet ExpressRoute?
Überprüfen Sie für Preisinformationen [Preise](https://azure.microsoft.com/pricing/details/expressroute/) .

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-the-vpn-connection-i-purchase-from-my-network-service-provider-have-to-be-the-same-speed"></a>Wenn ich eine ExpressRoute-Verbindung eine bestimmte Bandbreite bezahlen, muss die VPN-Verbindung aus der Netzwerk-Dienstanbieter kaufen die gleiche Geschwindigkeit?
Nein. Sie können eine VPN-Verbindung Geschwindigkeit von Ihrem Dienstanbieter erwerben. Die Verbindung zum Azure werden ExpressRoute Circuit Bandbreite auf jedoch, die Sie erwerben.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-the-ability-to-burst-up-to-higher-speeds-if-required"></a>Wenn ich eine ExpressRoute-Verbindung eine bestimmte Bandbreite bezahlen, habe können bis zu einer höheren Geschwindigkeit burst ggf.?
Ja. ExpressRoute-Schaltkreise konfiguriert Fälle unterstützen, in denen Sie zweimal Max. Bandbreite Platzen können ohne zusätzliche Kosten beschafft. Wenn sie diese Funktion unterstützen, Ihren Service-Anbieter wenden.

### <a name="can-i-use-the-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Kann ich dieselbe VPN-Verbindung gleichzeitig virtuelles Netzwerk mit anderen Azure-Dienste verwenden?
Ja. ExpressRoute-Verbindung einmal Setup in einem virtuellen Netzwerk und andere Azure Dienste gleichzeitig zugreifen können Sie. Sie werden für virtuelle Netzwerke über private peering Pfad und andere Dienste über öffentliche peering Pfad verbunden.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>Bietet ExpressRoute Service Level Agreement (SLA)?
Siehe [Seite ExpressRoute SLA](https://azure.microsoft.com/support/legal/sla/) für Weitere Informationen.

## <a name="supported-services"></a>Unterstützte Dienste
Die meisten Azure Services werden über ExpressRoute unterstützt.

- Verbindung mit virtuellen Computern und Clouddienste in virtuellen Netzwerken bereitgestellt werden über private peering Pfad.
- Azure Websites werden über öffentliche peering Pfad unterstützt.
- IoT Hub wird über öffentliche peering Pfad unterstützt.
- Office 365 ist über Microsoft peering Pfad unterstützt.
- Alle anderen Dienste stehen über öffentliche peering Pfad. Ausnahmen sind wie folgt.

    **Die folgenden Dienste werden nicht unterstützt:**

    - CDN
    - Visual Studio Team Services Auslastungstests
    - Mehrstufige Authentifizierung
    - Traffic Manager

## <a name="data-and-connections"></a>Daten und Anschlüsse

### <a name="are-there-limits-on-the-amount-of-data-that-i-can-transfer-using-expressroute"></a>Gibt es auf die Datenmenge, die ich mit ExpressRoute übertragen?
Wir sind nicht die zu übertragenden Datenmenge beschränken. Finden Sie Informationen auf Bandbreite [Preise](https://azure.microsoft.com/pricing/details/expressroute/) .

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>Welche Geschwindigkeit unterstützt ExpressRoute?
Unterstützte Bandbreite bietet:

| 50 Mbit/s, 100 Mbit/s, 200 Mbit/s, 500 Mbit/s, 1 Gbit/s, 2 Gbit/s, 5 Gbit/s, 10 |

### <a name="which-service-providers-are-available"></a>Der Dienstanbieter sind verfügbar?
[ExpressRoute Partner und Speicherorte](expressroute-locations.md) für die Liste und Positionen anzeigen

## <a name="technical-details"></a>Technische details

### <a name="what-are-the-technical-requirements-for-connecting-my-on-premises-location-to-azure"></a>Was sind die technischen Vorschriften für Azure lokalen Standort herstellen?
Anforderungen finden Sie unter [Seite erforderliche Komponenten ExpressRoute](expressroute-prerequisites.md) .

### <a name="are-connections-to-expressroute-redundant"></a>Sollen Verbindungen ExpressRoute redundant?
Ja. Jede Verbindung Express Route hat eine redundante cross-Verbindungen für hohe Verfügbarkeit konfiguriert.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>Verliere ich Konnektivität, wenn eine meiner ExpressRoute links?
Sie verlieren nicht Konnektivität bei Ausfall eines der Querverbindungen. Eine redundante Verbindung ist die Auslastung des Netzwerks unterstützen. Sie können außerdem mehrere Stromkreise in peering anderswo Fehler Stabilität zu erstellen.

### <a name="onep2plink"></a>Muss ich zwei Anschlüsse zwischen lokalen Netzwerk und Microsoft bestellen, wenn ich nicht am Cloud Exchange und Service-Anbieter Verbindung bietet? 
Nein, benötigen Sie nur eine einzige physische Verbindung wenn zwei Ethernet-virtual Circuits Ihren Service-Anbieter über die physische Verbindung hergestellt werden kann. Die physische Verbindung (z. B. ein Glasfaser) wurde auf einer Ebene 1 (L1) Gerät (siehe unten). Die zwei Ethernet-virtual Circuits werden mit unterschiedlichen VLAN-IDs für die primäre Verbindung und eine für den sekundären markiert. Die VLAN-IDs sind in der äußeren 802.1Q Ethernet-Header. Innere 802.1Q Ethernet-Header (nicht dargestellt) einer bestimmten [ExpressRoute routing-Domäne](expressroute-circuit-peerings.md)zugeordnet ist. 

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)


### <a name="can-i-extend-one-of-my-vlans-to-azure-using-expressroute"></a>Kann ich einen meiner VLANs in Azure erweitern ExpressRoute verwenden?
Nein. Wir unterstützen keine Layer 2-Konnektivität Extensions in Azure.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Kann mehr als eine ExpressRoute-Verbindung im Abonnement enthalten?
Ja. Mehrere ExpressRoute-Verbindung können Sie Ihr Abonnement. Das Standardlimit für die Anzahl der dedizierten Stromkreise wird auf 10 festgelegt. Sie erhalten Support von Microsoft zu der bei Bedarf erhöhen.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>Kann ExpressRoute Stromkreise von Dienstleistungserbringern haben?
Ja. Sie können viele ExpressRoute Circuits enthält. ExpressRoute-Verbindung werden nur einen Anbieter zugeordnet.

### <a name="how-do-i-connect-my-virtual-networks-to-an-expressroute-circuit"></a>Wie wird eine ExpressRoute-Verbindung Meine virtuelle Netzwerke herstellen
Die grundlegenden Schritte werden im folgenden beschrieben.

- Sie müssen eine ExpressRoute-Verbindung herstellen und Dienstanbieter können.
- Sie oder der Anbieter muss die BGP peering (s) konfigurieren.
- Sie müssen das virtuelle Netzwerk an der ExpressRoute verknüpfen.

Weitere Informationen finden Sie unter [ExpressRoute Workflows für Circuit Bereitstellung und Circuit Zustände](expressroute-workflows.md) .

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>Gibt es Konnektivität Grenzen für ExpressRoute-Verbindung?
Ja. Seite [ExpressRoute-Partner und](expressroute-locations.md) bietet eine Übersicht über Konnektivität Grenzen für eine ExpressRoute-Verbindung. Konnektivität für ExpressRoute-Verbindung ist eine geopolitische Region auf. Konnektivität erweiterbar geopolitische Regionen Cross ExpressRoute Premium-Funktion aktivieren.

### <a name="can-i-link-to-more-than-one-virtual-network-to-an-expressroute-circuit"></a>Kann ich mehrere virtuelle Netzwerke auf ExpressRoute-Verbindung verbinden?
Ja. Sie können bis zu 10 virtuelle Netzwerke an einem ExpressRoute verknüpfen.

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-to-a-single-expressroute-circuit"></a>Ich habe mehrere Azure-Abonnements, die virtuelle Netzwerke enthalten. Kann virtuelle Netzwerke verbinden, die in separaten Abonnements für eine ExpressRoute-Verbindung?
Ja. Sie können bis zu 10 anderen Azure-Abonnements mit einer ExpressRoute-Verbindung autorisieren. Dieses Limit kann durch Aktivieren der ExpressRoute Premium erhöht werden.

Weitere Informationen finden Sie in der [Freigabe ExpressRoute-Verbindung über mehrere Abonnements](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-to-the-same-circuit-isolated-from-each-other"></a>Werden virtuelle Netzwerke sind voneinander isoliert Stromkreis verbunden?
Nein. Alle virtuellen Netzwerke mit demselben ExpressRoute-Verbindung verknüpft sind Teil derselben Domäne routing und sind nicht voneinander hinsichtlich routing. Benötigen Sie Route Isolation, müssen Sie eine separate ExpressRoute-Verbindung erstellen.

### <a name="can-i-have-one-virtual-network-connected-to-more-than-one-expressroute-circuit"></a>Kann ein virtuelles Netzwerk mit mehreren ExpressRoute-Verbindung verbunden sind?
Ja. Sie können ein einzelnes virtuelles Netzwerk mit bis zu 4 ExpressRoute verknüpfen. Sie müssen über 4 verschiedenen [ExpressRoute Positionen](expressroute-locations.md)bestellt werden.

### <a name="can-i-access-the-internet-from-my-virtual-networks-connected-to-expressroute-circuits"></a>Kann ich von meinem virtuellen Netzwerken ExpressRoute Stromkreise angeschlossen Internet zugreifen?
Ja. Wenn Standardrouten (0.0.0.0/0) oder Internet Route Präfixe über BGP-Sitzung nicht angekündigt haben, werden Sie von einem virtuellen Netzwerk verknüpft eine ExpressRoute-Verbindung zum Internet herstellen können.

### <a name="can-i-block-internet-connectivity-to-virtual-networks-connected-to-expressroute-circuits"></a>Kann Verbindung mit virtuellen Netzwerken verbunden ExpressRoute Stromkreise blockieren?
Ja. Ankündigen des Standardrouten (0.0.0.0/0) blockieren alle Internetkonnektivität für virtuelle Computer in einem virtuellen Netzwerk bereitgestellt, und alle Datenverkehr, durch die ExpressRoute-Verbindung. Beachten Sie, dass wenn Sie Standard-Routen ankündigen, wir Datenverkehr an Dienste über öffentliche peering (wie Azure-Speicher und SQL DB) zurück zu Ihrem Firmensitz erzwingt. Sie müssen die Router Rückkehr Datenverkehr in Azure über öffentliche peering Pfad oder über das Internet konfigurieren.

### <a name="can-virtual-networks-linked-to-the-same-expressroute-circuit-talk-to-each-other"></a>Können virtuelle Netzwerke mit demselben ExpressRoute-Verbindung verknüpft zueinander?
Ja. Virtuelle Netzwerke verbunden ExpressRoute Stromkreis eingesetzt können miteinander kommunizieren.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>Können Standort-zu-Standort-Verbindung für virtuelle Netzwerke zusammen mit ExpressRoute werden verwendet?
Ja. ExpressRoute kann mit Standort-zu-Standort-VPNs verwendet werden.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-to-use-expressroute"></a>Kann ein virtuelles Netzwerk von Standort zu Standort / Punkt-zu-Standort-Konfiguration ExpressRoute werden übertragen?
Ja. Sie müssen einen ExpressRoute-Gateway innerhalb des virtuellen Netzwerks erstellen. Eine kleine Ausfallzeiten durch den Prozess werden.

### <a name="what-do-i-need-to-connect-to-azure-storage-over-expressroute"></a>Was muss ich über ExpressRoute Azure-Speicher herstellen?
Müssen Sie eine ExpressRoute-Verbindung einrichten und Konfigurieren von Routen für Öffentliche peering.

### <a name="are-there-limits-on-the-number-of-routes-i-can-advertise"></a>Gibt es auf der Anzahl der Routen angekündigt werden kann?
Ja. Wir akzeptieren 4000 Route Präfixe für private peering bis 200 öffentlichen peering und Microsoft peering. Können Sie erhöhen, 10.000 Routen für private peering ExpressRoute Premium-Funktion aktivieren.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-the-bgp-session"></a>Gibt es Beschränkungen IP-Adressbereiche, die ich über BGP-Sitzung werben können?
Wir akzeptieren keine privaten Präfixe (RFC1918) in der Öffentlichkeit und Microsoft peering BGP-Sitzung.

### <a name="what-happens-if-i-exceed-the-bgp-limits"></a>Was geschieht, wenn ich das BGP überschreiten beschränkt?
BGP-Sitzungen werden gelöscht. Sie werden nach der Präfix unterhalb der Zähler zurückgesetzt.

### <a name="what-is-the-expressroute-bgp-hold-time-can-it-be-adjusted"></a>Was ist die ExpressRoute BGP halten? Kann es werden angepasst?
Die Wartezeit ist 180. Keepalive-Nachrichten werden alle 60 Sekunden gesendet. Diese werden Einstellungen auf Microsoft behoben, die nicht geändert werden kann.

### <a name="after-i-advertise-the-default-route-00000-to-my-virtual-networks-i-cant-activate-windows-running-on-my-azure-vms-how-to-i-fix-this"></a>Nach Ankündigung die Standardroute (0.0.0.0/0) an meinen virtuellen Netzwerke kann nicht Windows Azure-VMs unter aktiviert werden. Wie, ich dieses Problem?
Die folgenden Schritte helfen Azure Aktivierungsanfrage erkennen:

1. Stellen Sie öffentliche peering für ExpressRoute-Verbindung her.
2. Eine DNS-Suche durchführen und die IP-Adresse des **kms.core.windows.net**
3. Führen Sie eine der folgenden beiden Elemente der Schlüsselverwaltungsdienst erkennt, dass die Anfrage zur Aktivierung von Azure und die Anforderung berücksichtigt.
    - Weiterleiten Sie auf Ihrem lokalen Netzwerk Datenverkehr für die IP-Adresse (in Schritt 2 abgerufen) in Azure über öffentliche peering.
    - Haben Sie Ihr NSP Anbieter Haare-Pin der in Azure über öffentliche peering.

### <a name="can-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Kann die Bandbreite von ExpressRoute-Verbindung ändern?
Ja. Die Bandbreite von ExpressRoute-Verbindung können ohne es zu beenden. Sie müssen bei Ihrem konnektivitätsanbieter sicherstellen, dass sie in ihren Netzwerken unterstützen die Bandbreite Drosselung aktualisieren verfolgen. Sie werden jedoch nicht die Bandbreite von ExpressRoute-Verbindung zu können. Wenn niedriger Bandbreite ein Tear down bedeutet Erholung ExpressRoute-Verbindung.

### <a name="how-do-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Ändern die Bandbreite von ExpressRoute-Verbindung
Sie können die Bandbreite von ExpressRoute-Verbindung mit dem Update widmet Circuit-API und PowerShell-Cmdlet.

## <a name="expressroute-premium"></a>ExpressRoute Premium

### <a name="what-is-expressroute-premium"></a>Was ist ExpressRoute Premium?
ExpressRoute Premium ist eine Auflistung der unten aufgeführten Funktionen.

 - Routing Tabelle auf 4000 Routen 10.000 Routen für private peering erhöht.
 - Anzahl der VNets mit ExpressRoute-Verbindung verbunden werden kann (Standard ist 10). Siehe unten für weitere Details.
 - Globale Konnektivität über Microsoft Core-Netzwerk. Sie künftig können ein VNet in einem geopolitischen Region mit einem ExpressRoute in einer anderen Region. **Beispiel:** Sie können eine VNet erstellt eine ExpressRoute-Verbindung erstellt im Silicon Valley in Westeuropa verknüpfen.
 - Verbindung mit Office 365-Diensten und CRM Online.

### <a name="how-many-vnets-can-i-link-to-an-expressroute-circuit-if-i-enabled-expressroute-premium"></a>Wie viele VNets können an einem ExpressRoute verknüpfen, wenn ich ExpressRoute Premium aktiviert?
Folgenden Tabellen zeigen die Grenzen ExpressRoute und die Anzahl der VNets pro ExpressRoute-Verbindung.


[AZURE.INCLUDE [expressroute-limits](../../includes/expressroute-limits.md)]


### <a name="how-do-i-enable-expressroute-premium"></a>Aktivieren ExpressRoute premium
ExpressRoute Premium-Funktionen können aktiviert werden, wenn das Feature aktiviert und kann durch Aktualisieren des Zustands Circuit heruntergefahren werden. Kann ExpressRoute Premium zur Erstellungszeit Circuit aktivieren oder Update dedizierte Verbindung API aufrufen können / PowerShell-Cmdlet zum ExpressRoute Premium aktivieren.

### <a name="how-do-i-disable-expressroute-premium"></a>Wie deaktivieren Sie ExpressRoute premium
Deaktivieren Sie ExpressRoute Premium durch Aufruf von Update dedizierte Verbindung API / PowerShell-Cmdlets müssen Sie sicherstellen, dass die Verbindung skaliert haben muss die Standardgrenzwerte bevor ExpressRoute Premium deaktivieren. Wir scheitern Anforderung ExpressRoute Premium deaktivieren, wenn die Auslastung Grenzen standardmäßig skaliert.

### <a name="can-i-pick-and-choose-the-features-i-want-from-the-premium-feature-set"></a>Können I die Funktionen wählen Funktionsumfang Premium möchten?
Nein. Sie werden keine Features auswählen, die Sie benötigen. Wir aktivieren alle Features, wenn Sie ExpressRoute Premium aktivieren.

### <a name="how-much-does-expressroute-premium-cost"></a>Wie viel kostet ExpressRoute Premium?
Siehe [Preise](https://azure.microsoft.com/pricing/details/expressroute/) für Kosten.

### <a name="do-i-pay-for-expressroute-premium-in-addition-to-standard-expressroute-charges"></a>Für ExpressRoute Premium neben standard ExpressRoute zahle?
Ja. ExpressRoute anfallen Premium ExpressRoute Circuit Gebühren und Kosten der Konnektivität erforderlich.

## <a name="expressroute-and-office-365-services-and-crm-online"></a>ExpressRoute und Office 365-Dienste und CRM Online

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-to-connect-to-office-365-services-and-crm-online"></a>Erstellen einer ExpressRoute-Verbindung mit Office 365-Diensten und CRM Online herstellen

1. Überprüfen der [ExpressRoute erforderliche](expressroute-prerequisites.md) Seite um sicherzustellen, dass die Mindestanforderungen.
2. Überprüfen Sie die Liste der Dienstanbieter und Speicherorte [Speicherorte](expressroute-locations.md) zu ExpressRoute Partner Ihre Konnektivitätsanforderungen erfüllt sind.
3. Planen der Kapazitätsbedarf [Netzwerk](http://aka.ms/tune/)und Performance-Optimierung für Office 365.
4. Befolgen Sie die Schritte im setup-Konnektivität [ExpressRoute Workflows für Circuit Bereitstellung und Circuit Staaten](expressroute-workflows.md)nach Workflows.

>[AZURE.IMPORTANT] Stellen Sie sicher, dass Sie ExpressRoute Premium Add-on aktiviert, wenn eine Verbindung zu Office 365-Diensten und CRM Online konfigurieren.

### <a name="do-i-need-to-enable-azure-public-peering-to-connect-to-office-365-services-and-crm-online"></a>Muss ich Azure öffentliche Peering Verbindung zu Office 365-Diensten und CRM Online aktivieren?
Nein, Sie müssen nur Microsoft Peering aktivieren. Authentifizierungsdatenverkehr Azure AD wird durch Microsoft Peering gesendet. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-to-office-365-services-and-crm-online"></a>Können meine vorhandene ExpressRoute-Schaltkreise eine Verbindung zu Office 365-Diensten und CRM Online unterstützt?
Ja. Vorhandene ExpressRoute-Verbindung kann konfiguriert Verbindung mit Office 365-Diensten unterstützen. Sicherstellen Sie, dass Sie ausreichend Kapazität für Office 365-Diensten verbinden und sicherstellen, dass Sie Premium Add-on aktiviert haben. [Netzwerk und Performance-Optimierung für Office 365](http://aka.ms/tune/) hilft Ihnen Ihre Konnektivitätsanforderungen planen. Siehe auch [Erstellen und Ändern einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Welche Office 365 über eine ExpressRoute-Verbindung zugegriffen werden können?

Siehe Seite [Office 365 URLs und IP-Adressbereiche](http://aka.ms/o365endpoints) für eine Liste von Diensten über ExpressRoute unterstützt.

### <a name="how-much-does-expressroute-for-office-365-services-and-crm-online-cost"></a>Wieviel ExpressRoute für Office 365-Diensten und CRM Online Kosten?
Office 365-Diensten und CRM Online erfordert Premium Add-on aktiviert werden. Die [Preise Seite](https://azure.microsoft.com/pricing/details/expressroute/) erläutert Kosten für ExpressRoute.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Welche Bereiche wird ExpressRoute für Office 365 unterstützt?
Weitere [ExpressRoute Partner und Standorten](expressroute-locations.md) Weitere Informationen in der Liste der Partner und Speicherorte, ExpressRoute unterstützt wird.

### <a name="can-i-access-office-365-over-the-internet-even-if-expressroute-was-configured-for-my-organization"></a>Kann ich Office 365 über das Internet zugreifen, wenn ExpressRoute für meine Organisation konfiguriert wurde?
Ja. Office 365 Endpunkte sind über das Internet erreichbar ExpressRoute für Ihr Netzwerk konfiguriert wurde. Sind Sie an einem Speicherort, der Office 365-Diensten durch ExpressRoute Verbindung konfiguriert ist, werden Sie durch ExpressRoute verbunden.

### <a name="can-dynamics-ax-online-be-accessed-over-an-expressroute-connection"></a>Möglich Dynamics AX Online über eine ExpressRoute-Verbindung?
Nein, es wird nicht unterstützt.
