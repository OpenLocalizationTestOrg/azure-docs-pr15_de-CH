<properties
   pageTitle="Microsoft Cloud-Dienste und Netzwerk-Sicherheit | Microsoft Azure"
   description="Erfahren Sie mehr über die wichtigsten Features in Azure zum sicheren Netzwerk erstellen"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft Cloud-Dienste und Netzwerk-Sicherheit

Microsoft Cloud-Dienste bieten Hyperscale-Services und Infrastruktur, unternehmensweite Funktionen und Auswahl Hybrid-Konnektivität. Kunden können auf diese Dienste über das Internet oder mit Azure ExpressRoute Verbindung zum privaten Netzwerk bereit. Microsoft Azure-Plattform ermöglicht Kunden, nahtlos erweitern ihre Infrastruktur in der Cloud und mehrstufige Architektur. Darüber hinaus können dritten erweiterte Funktionen Sicherheitsdienste und virtuelle Appliances. Dieses White Paper enthält eine Übersicht über Sicherheit und Architektur, die Kunden bei Microsoft Cloud Services Zugriff über ExpressRoute verwenden. Dazu gehört auch das Erstellen sicherer Dienste in Azure virtuelle Netzwerke.

## <a name="fast-start"></a>Schnellstart
Das folgende Logik Diagramm können Sie ein bestimmtes Beispiel mit der Azure-Plattform viele Sicherheitsverfahren weiterleiten. Finden Sie eine Kurzübersicht Beispiel passt der Fall. Lesen Sie für ausführlichere Erklärung durch das Papier.
![Sicherheit Optionen Flussdiagramm][0]

[Beispiel 1: Erstellen Sie ein Umkreisnetzwerk (auch DMZ, Demilitarized Zone und überwachtes Subnetz) zum Schutz mit Netzwerk-Sicherheitsgruppen (NSGs).](#example-1-build-a-simple-dmz-with-nsgs)</br>
[Beispiel 2: Erstellen Sie ein Perimeternetzwerk zum Schutz mit einer Firewall und NSGs.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Beispiel 3: Erstellen Sie ein Perimeternetzwerk für Netzwerke mit Firewall, benutzerdefinierte Route (UDR) und NSG.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Beispiel 4: Hinzufügen einer Hybrid-Verbindungs zwischen Standorten, virtuelle Appliance virtual private Network (VPN).](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Beispiel 5: Hinzufügen einer hybridverbindung zwischen Standorten, Azure-Gateway VPN.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Beispiel 6: Hinzufügen einer hybridverbindung mit ExpressRoute.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Beispiele für Verbindungen zwischen virtuellen Netzwerken, hohe Verfügbarkeit und Verketten von Diensten werden in den nächsten Monaten zu diesem Dokument hinzugefügt.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft-Kompatibilität und Infrastruktur-Schutz
Microsoft hat eine Führungsposition Einhaltungsinitiativen Unternehmenskunden erforderliche Unterstützung übernommen. Im folgenden sind einige der Compliance-Zertifizierungen für Azure: ![Abzeichen Azure Compliance][1]

Weitere Informationen finden Sie Compliance [Microsoft Sicherheitscenter](https://azure.microsoft.com/support/trust-center/compliance/).

Microsoft hat einen umfassenden Ansatz zu Cloudinfrastruktur Hyperscale global Services ausgeführt. Microsoft-Cloudinfrastruktur umfasst Hardware, Software, Netzwerken und und Betriebspersonal neben physischen Rechenzentren.

![Azure-Sicherheitsfeatures][2]

Dieser Ansatz bietet eine sicherere Grundlage für Kunden ihre Dienste in der Cloud von Microsoft. Der nächste Schritt ist für Kunden zu Sicherheitsarchitektur Schutz dieser Dienste erstellen.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Herkömmliche Architekturen und Umkreisnetzwerken
Obwohl Microsoft stark investiert in die Cloud-Infrastruktur zu schützen, müssen Kunden auch ihre Cloud-Dienste und Ressourcen schützen. Ein mehrschichtigen Ansatz zur Sicherheit bietet die beste Verteidigung. Eine Sicherheitszone Perimeter Netzwerk schützt interne Netzwerkressourcen von einem nicht vertrauenswürdigen Netzwerk. Ein Perimeternetzwerk bezieht sich auf die Kanten oder Bereiche des Netzwerks dem Internet und den geschützten IT-Infrastruktur des Unternehmens zwischen.

In Unternehmensnetzwerken ist die zentrale Infrastruktur stark am Perimeter mit mehreren Ebenen von Sicherheitsgeräten befestigt. Geräte und Policy-Erzwingungspunkte besteht die Berandung der einzelnen Ebenen. Geräte können folgende: Firewalls, Distributed Denial of Service (DDoS) Prevention Intrusion Detection oder Systeme (IDS/IPS) und VPN-Geräte. Durchsetzung nehmen die Form von Firewallrichtlinien, Zugriffssteuerungslisten (ACLs) oder bestimmte routing. Die erste Verteidigungslinie im Netzwerk direkt akzeptieren eingehender Datenverkehr aus dem Internet ist eine Kombination dieser Mechanismen Angriffe blockieren und schädlichen Datenverkehr gleichzeitig zulässigen Anforderungen weiter in das Netzwerk. Diese Verkehrswege direkt auf Ressourcen im Perimeternetzwerk. Diese Ressource kann dann "auf Ressourcen im Netzwerk durch die nächste Berandung für die Validierung erste sprechen". Die äußerste Ebene ist im Perimeternetzwerk bezeichnet, da dieser Teil des Netzwerks mit dem Internet in der Regel mit auf beiden Seiten verfügbar gemacht wird. Die folgende Abbildung zeigt ein Beispiel für ein einzelnes Subnetz Umkreisnetzwerk in einem Unternehmensnetzwerk mit zwei Sicherheitsgrenzen.

![Ein Perimeternetzwerk in einem Unternehmensnetzwerk][3]

Es gibt viele Architekturen zum Implementieren eines Perimeternetzwerks aus einem einfachen Lastenausgleich vor einer Webfarm mit einem mehrere Subnetz Perimeternetzwerk mit unterschiedlichen an jeder Grenze und Datenverkehr tieferen Ebenen des Unternehmensnetzwerks. Auf die Bedürfnisse des Unternehmens und seiner gesamten Risikotoleranz hängt wie im Perimeternetzwerk erstellt wird.

Kunden Arbeitslasten auf Öffentliche Clouds verschieben, ist es wichtig, die ähnliche Funktionen für Perimeter-Netzwerk-Architektur in Azure Compliance und Sicherheit unterstützen. Dieses Dokument enthält Richtlinien zum Aufbau einer sicheren Umgebung in Azure durch Kunden. Schwerpunkt im Perimeternetzwerk, aber auch eine umfassende Diskussion viele Aspekte der Sicherheit. Die folgenden Fragen unterrichtet dieser Diskussion:

- Wie kann ein Perimeternetzwerk in Azure werden erstellt?
- Was sind einige der Azure-Funktionen im Perimeternetzwerk erstellen?
- Wie können Backend-Arbeitslasten geschützt?
- Wie erfolgt die Internetkommunikation Arbeitslasten in Azure?
- Wie können lokale Netzwerke von Bereitstellungen in Azure geschützt?
- Wann sollten systemeigene Azure Sicherheitsfunktionen Drittanbieter-Geräte oder Dienste verwendet?

Die folgende Abbildung zeigt verschiedene Sicherheitsebenen Azure Kunden bietet. Diese Schichten sind systemeigene in Azure Plattform und benutzerdefinierte Funktionen:

![Azure-Sicherheitsarchitektur][4]

Eingehende Internet Azure DDoS vor umfangreichen Angriffen gegen Azure Schutz. Die nächste Ebene ist benutzerdefiniert öffentliche Endpunkte, die bestimmen, welche Cloud-Dienst mit dem virtuellen Netzwerk passieren kann, verwendet werden. Systemeigene Azure virtual Netzwerkisolation vollständig isoliert von anderen Netzwerken gewährleistet und Datenfluss nur Benutzerpfade und Diese Pfade sind die nächste Ebene, NSGs, UDR und virtuelle Netzwerkgeräte Sicherheitsgrenzen Schutz anwendungsbereitstellung im geschützten Netzwerk erstellen verwendet werden kann.

Im nächste Abschnitt Überblick Azure virtuelle Netzwerke. Diese virtuelle Netzwerke von Kunden erstellt und was bereitgestellten Arbeitslasten mit verbunden sind. Virtuelle Netzwerke sind die Grundlage für alle Network Sicherheitsfunktionen erforderlich ein Perimeternetzwerk zum Schutz von Kunden Bereitstellung in Azure.

## <a name="overview-of-azure-virtual-networks"></a>Übersicht über Azure virtuelle Netzwerke
Vor Internetdatenverkehr Azure virtuelle Netzwerke erhalten kann, gibt es zwei Sicherheitsebenen in der Azure-Plattform:

1.  **DDoS-Schutz**: DDoS-Schutz ist ein Azure physischen Netzwerk, das die Azure-Plattform selbst umfangreiche internetbasierten Angriffen schützt. Diese Angriffe verwenden mehrere "Bot" Knoten versucht, um einen Internetdienst zu überlasten. Azure hat einen robusten DDoS Schutz Netz alle eingehenden Internet-Konnektivität. Diese DDoS-Schutz besitzt keine Benutzer konfigurierbare Attribute und ist nicht zugänglich. Azure als Plattform umfangreichen Angriffen schützt Dies schützt kundenanwendung nicht direkt. Zusätzliche Stabilität können vom Kunden gegen einen lokalisierten konfiguriert werden. Beispielsweise blockiert Kunden eine umfangreiche DDoS-Angriff auf einen öffentlichen Endpunkt angegriffen, Azure Verbindung zu diesem Dienst. Kunde konnte ein Failover zu einem anderen virtuellen Netzwerk oder service-Endpunkt nicht Angriff Wiederherstellungsmodus an. Beachten Sie, dass obwohl Kunde auf diesen Endpunkt Steuerelementobjekten keine anderen Dienste außerhalb dieser Endpunkt betroffen wären. Andere Kunden und Services werden, nicht beeinträchtigt, Angriff angezeigt.
2.  **Endpunkte**: Endpunkte ermöglichen Cloud Services oder Ressource Gruppen öffentliche IP Adressen und Ports ungeschützt. Network Address Translation (NAT) verwendet der Endpunkt Datenverkehr an interne Adresse und Port in Azure virtual Network. Dies ist der primäre Pfad für den externen Datenverkehr an das virtuelle Netzwerk übergeben. Die Endpunkte sind Benutzer konfigurierbare festzustellen, welche übergeben wird und wie und wo sie übersetzt das virtuelle Netzwerk.

Datenverkehr das virtuelle Netzwerk erreicht, werden viele Features kommen. Azure virtuelle Netzwerke bilden die Grundlage für Kunden an Arbeitslasten und grundlegende auf Sicherheit gilt. Es ist ein privates Netzwerk (virtual Network-Overlay) in Azure für Kunden mit folgenden Funktionen und Merkmale:
 
- **Isolierung von Datenverkehr**: ein virtuelles Netzwerk ist die Isolationsgrenze Datenverkehr auf der Azure-Plattform. Virtuelle Maschinen (VMs) in einem virtuellen Netzwerk kann nicht direkt mit VMs in einem anderen virtuellen Netzwerk kommunizieren, auch wenn beide virtuelle Netzwerke demselben Debitor erstellt werden. Dies ist eine wichtige Eigenschaft, die Kunden VMs gewährleistet und Kommunikation innerhalb eines virtuellen Netzwerks privat bleibt.
- **Multi-Topologie**: virtuelle Netzwerke ermöglichen Kunden Multi-Topologie von Subnetzen zuordnen und separate Adressräume für verschiedene Elemente oder "Ebenen" die Arbeitslasten festlegen. Diese Gruppierung und Topologien können Kunden verschiedene Richtlinien basierend auf der Arbeitslast definieren und steuern zudem Verkehrsfluss zwischen den Ebenen.
- **Standortübergreifende Konnektivität**: Kunden können in Azure standortübergreifende Verbindung zwischen einem virtuellen Netzwerk und lokalen Standorten oder anderen virtuellen Netzwerken herstellen. Dazu können Kunden Azure VPN-Gateways oder virtuelle Appliances von Drittanbietern verwenden. Azure unterstützt mithilfe von IPsec-IKE-Standardprotokollen und ExpressRoute private Verbindung zwischen Standorten (S2S) VPNs.
- **NSG** ermöglicht Kunden Regeln (ACLs) auf die gewünschte Granularitätsebene: Netzwerk-Schnittstellen, einzelnen VMs oder virtuellen Subnetze. Kunden können steuern den Zugriff zulassen oder verweigern Kommunikation Arbeitslasten in einem virtuellen Netzwerk aus Systemen Kundennetzwerke über standortübergreifende Konnektivität oder direkte Internet-Kommunikation.
- **UDR** und **IP-Weiterleitung** können Kunden die Kommunikationspfade zwischen verschiedenen Ebenen in einem virtuellen Netzwerk. Kunden können eine Firewall IDS/IPS und andere virtuelle Appliances bereitstellen und Netzwerk-Datenverkehr über diesen Sicherheits-Appliances für die Durchsetzung von Sicherheitsrichtlinien Grenze, Überwachung und Kontrolle.
- **Virtuelle Netzwerkgeräte** in Azure Marketplace: Sicherheitseinrichtungen wie Firewalls, Lastenausgleich und IDS/IPS-Azure und VM-Bildergalerie verfügbar sind. Kunden können diese Geräte in ihre virtuelle Netzwerke und insbesondere ihre Sicherheitsgrenzen (einschließlich Perimeter-Netzwerk-Subnetze) bereitstellen, um eine sichere Umgebung mehrstufigen abzuschließen.

Diese Features und Funktionen ist ein Beispiel für wie eine Perimeter-Netzwerk-Architektur in Azure erstellt werden konnte Folgendes:

![Ein Perimeternetzwerk in Azure virtual network][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Perimeter-Netzwerkeigenschaften und Vorschriften
Das Umkreisnetzwerk soll der front-End der Netzwerk Kommunikation aus dem Internet direkt verbunden. Eingehende Pakete sollten über Sicherheitseinrichtungen wie Firewalls, IDS und IPS vor Back-End-Server übertragen. Internet gerichtete Pakete von der Arbeitslast können auch über Sicherheits-Appliances im Umkreisnetzwerk für durchsetzen, Inspektion und Überwachung zu übertragen, bevor Sie das Netzwerk verlassen. Darüber hinaus bietet Umkreisnetzwerk standortübergreifenden VPN-Gateways zwischen virtuellen Kunden und lokalen Netzwerken.

### <a name="perimeter-network-characteristics"></a>Perimeter-Netzwerk-Merkmale
Auf der vorherigen Abbildung sind einige Merkmale einer guten Umkreisnetzwerk wie folgt:

- Internetzugriff:
    - Perimeter-Netzwerk-Subnetz selbst ist Internetzugriff, direkt mit dem Internet kommuniziert.
    - Öffentliche IP-Adressen, VIPs und Endpunkte übergeben Internetverkehr auf Front-End-Netzwerk und Geräte.
    - Eingehender Datenverkehr aus dem Internet durchläuft Sicherheitseinrichtungen vor anderen Ressourcen auf dem Front-End-Netzwerk.
    - Wenn ausgehender Sicherheit aktiviert ist, durchläuft Datenverkehr Sicherheitsgeräte als letzten Schritt vor dem Internet.
- Geschütztes Netzwerk:
    - Es ist kein direkter Weg vom Internet auf die Kerninfrastruktur.
    - Kanäle der Kerninfrastruktur müssen Sicherheitsgeräte wie NSGs, Firewalls und VPN-Geräte durchlaufen.
    - Andere Geräte müssen nicht Internet und die Kerninfrastruktur überbrücken.
    - Eine einzelne virtuelle Appliance mit gestaffelten Schnittstellen für jede Berandung möglicherweise Sicherheitseinrichtungen für den Internetzugriff und geschützten Netzwerk vor Grenzen des Umkreisnetzwerks (z. B. die beiden Firewalls Symbole in der vorherigen Abbildung dargestellt). (D. h. ein Gerät logisch getrennt behandeln Last für beide Grenzen des Umkreisnetzwerks.)
- Andere allgemeine Verfahren und Nebenbedingungen:
    - Arbeitslasten darf nicht Ihre geschäftskritischen Informationen speichern.
    - Zugriff und Aktualisierung Konfigurationen eines Umkreisnetzwerks und Bereitstellung sind nur autorisierte Administratoren auf.

### <a name="perimeter-network-requirements"></a>Perimeter Anforderungen
Um diese Merkmale zu aktivieren, Richtlinien Sie diese auf Virtuelles Netzwerk erfolgreich Umkreisnetzwerk implementieren:

- **Subnetz Architektur:** Geben Sie das virtuelle Netzwerk, ein ganzes Subnetz als Perimeternetzwerk getrennt von anderen Subnetzen im gleichen virtuellen Netzwerk vorgesehen ist. Dadurch den Datenverkehr zwischen dem Perimeternetzwerk und anderen internen oder privaten Subnetz Ebenen Abläufe durch eine Firewall oder ein IDS/IPS-virtuelle Appliance über Subnetzgrenzen benutzerdefinierte Routen.
- **NSG:** Perimeter-Netzwerk-Subnetz selbst sollte für die Kommunikation mit dem Internet offen, aber das bedeutet nicht, dass Kunden NSGs umgehen sollten. Führen Sie allgemeine Sicherheitsmaßnahmen zum Minimieren der Oberflächen Netzwerk mit dem Internet. Sperren Sie remote-Adressbereiche Bereitstellungen oder anwendungsspezifische Protokolle und Ports, die geöffnet sind zulässig. Möglicherweise Fällen, in denen dies nicht immer möglich ist. Beispielsweise haben Kunden eine externe Website in Azure Umkreisnetzwerk eingehende Webanfragen von öffentlichen IP-Adressen dürfen jedoch soll nur Web Application Ports: TCP: 80 und TCP:443.
- **-Routingtabelle:** Perimeter-Netzwerk-Subnetz selbst sollte direkt mit dem Internet kommunizieren, jedoch dürfen nicht direkte Kommunikation mit Back End oder lokale Netzwerke ohne Firewall oder Gerät.
- **Einheit Sicherheitskonfiguration:** Weiterleiten und überprüfen Sie Pakete zwischen dem Perimeternetzwerk und dem Rest der geschützte Netzwerke, Sicherheitseinrichtungen wie Firewalls, IDS und IPS können Geräte mehrfach vernetzt sein. Sie haben separate Netzwerkkarten für das Perimeternetzwerk und Backend-Subnetze. Die NICs im Perimeternetzwerk kommunizieren direkt in und aus dem Internet mit den entsprechenden NSGs und Perimeter Netzwerk-routing-Tabelle. Back-End-Subnetze mit NICs haben mehrere NSGs und Routingtabellen der entsprechenden Back-End-Subnetze eingeschränkt.
- **Einheit Sicherheitsfunktionen:** In der Regel im Umkreisnetzwerk bereitgestellt Sicherheits-Appliances führen folgende Funktionen:
    - Firewall: Erzwingen von Firewall-Regeln oder Zugriffsrichtlinien für eingehenden Anfragen.
    - Erkennung und Abwehr Bedrohung: erkennen und Beheben von Angriffen aus dem Internet.
    - Überwachen und protokollieren: detaillierte Protokolle für Überwachung und Analyse verwalten.
    - Reverse Proxy: eingehenden Anfragen an den entsprechenden Back-End-Server umgeleitet. Diese Zuordnung umfasst und übersetzen die Zieladressen auf Front-End-Geräten normalerweise firewalls, die Back-End-Server-Adressen.
    - Forward-Proxy: NAT bereitstellen und Ausführen der Überwachung für die Kommunikation initiiert von innerhalb des virtuellen Netzwerks mit dem Internet.
    - Router: Weiterleitung eingehender und Cross-Subnetz innerhalb des virtuellen Netzwerks.
    - VPN-Gerät: als standortübergreifenden VPN-Gateways für standortübergreifende VPN-Konnektivität zwischen lokalen Kundennetzwerke und Azure virtuelle Netzwerke.
    - VPN-Server: Azure virtuelle Netzwerke mit VPN-Clients akzeptiert.

>[AZURE.TIP] Trennen Sie die folgenden zwei Gruppen: Personen Zugriff auf Perimeter Network Security Ausrüstung sowie die Personen als Anwendungsadministratoren Entwicklung, Bereitstellung oder Operationen. Trennung dieser Gruppen ermöglicht die Trennung von Aufgaben und verhindert, dass eine einzelne Person Applikationen Sicherheit und Netzwerk-Sicherheitsmaßnahmen zu umgehen.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Fragen beim Netzwerkgrenzen
In diesem Abschnitt bedeutet, sofern ausdrücklich, "Netzwerke" private Azure virtuelle Netzwerke von Abonnementadministrator erstellt. Der Begriff bezieht sich nicht auf die zugrunde liegende physische Netzwerke in Azure.

Außerdem werden Azure virtuelle Netzwerke häufig zur herkömmlichen lokalen Netzwerke. Es ist möglich, zwischen Standorten oder ExpressRoute hybride networking Lösungen mit Perimeter Netzwerk integrieren. Dies ist ein wichtiger Aspekt beim Netzwerk Sicherheitsgrenzen erstellen.

Die folgenden drei Fragen sind unbedingt beantworten, wenn Sie ein Netzwerk mit einem Perimeternetzwerk und mehrere Sicherheitsgrenzen erstellen.

#### <a name="1-how-many-boundaries-are-needed"></a>(1) wie viele Grenzen werden benötigt?
Der erste Punkt ist zu entscheiden, wie viele Sicherheitsgrenzen eines bestimmten Szenarios erforderlich sind:

- Eine einzelne Berandung: eine Front-End-Perimeternetzwerk zwischen dem virtuellen Netzwerk und dem Internet.
- Zwei Grenzen: auf der Seite Internet Umkreisnetzwerk und zwischen Perimeter-Netzwerk-Subnetz und Backend-Subnetze in Azure virtuelle Netzwerke.
- Drei Grenzen: auf der Internet-Seite des Perimeternetzwerks zwischen Umkreisnetzwerk und Backend-Subnetze und zwischen Back-End-Subnetze und im lokalen Netzwerk.
- N Grenzen: eine Variable Anzahl. Je nach Sicherheit ist wirklich eine Anzahl von Sicherheitsgrenzen, die in einem Netzwerk angewendet werden können.

Anzahl und Typ der Grenzen benötigt Risikobereitschaft des Unternehmens und das Szenario implementiert abhängig ist. Dies ist häufig eine gemeinsame Entscheidung mehrere Gruppen innerhalb einer Organisation häufig ein Risiko und Compliance-Team, einem Netzwerk Platform-Team und eines Teams. Sicherheit, Daten und die verwendete Technologie wissen sollten eine diese Entscheidung für die geeignete Einstellung für jede Implementierung erhalten.

>[AZURE.TIP] Verwenden Sie die kleinstmögliche Anzahl von Berandungen, die für eine bestimmte Situation Sicherheit erfüllen. Weitere Grenzen schwieriger Betrieb und Problembehandlung kann, sowie mit Verwalten mehrerer Grenze Richtlinien langfristig Verwaltungsaufwand. Unzureichende Grenzen erhöhen Risiko. Die Balance ist wichtig.

![Hybrid-Netzwerk mit drei Sicherheitsgrenzen][6]

Die Abbildung oben zeigt einen allgemeinen Überblick über ein Grenznetzwerk drei. Die Grenzen sind zwischen Umkreisnetzwerk und dem Internet, Azure Front-End- und Back-End privater Subnetze, Azure Back-End-Subnetz und lokalen Unternehmensnetzwerk.

#### <a name="2-where-are-the-boundaries-located"></a>(2) wo befinden sich die Grenzen?
Beschließt die Anzahl der Grenzen kommt umzusetzen Nächstes. Generell gibt es drei Optionen:
- Mithilfe einer internetbasierten Vermittler (z. B. eine cloudbasierte Web Application Firewall, die in diesem Dokument nicht behandelt wird)
- Systemeigene Features oder virtuelle Netzwerkgeräte verwenden in Azure
- Mit der physischen Geräte im lokalen Netzwerk

In rein Azure Netzwerken sind die Optionen systemeigene Azure-Funktionen (z. B. Azure Lastenausgleich) oder virtuelle Netzwerkgeräte über umfangreiche Partnerökosystem Azure (z. B. Check Point Firewalls).

Eine Grenze zwischen Azure und einem lokalen Netzwerk erforderlich ist, können die Sicherheitseinrichtungen auf beiden Seiten der Verbindung (oder beidseitig) befinden. Daher muss auf den Ort entschieden werden Sicherheitsgeräte an.

In der vorherigen Abbildung Internet Perimeter-Netzwerk vor Back-End-Grenzen vollständig in Azure enthalten und müssen systemeigene Azure Funktionen oder Netzwerk virtuelle Appliances. Sicherheitseinrichtungen auf der Grenze zwischen Azure (Back-End-Subnetz) und dem Unternehmensnetzwerk können Azure Seite oder der lokalen Seite oder sogar aus Geräten beidseitig sein. Es kann erhebliche Vorteile und Nachteile wahlweise ernsthaft berücksichtigt werden müssen.

Beispielsweise hat vorhandene physische Sicherheitsgeräte auf Netzwerk lokal mit den Vorteil, dass keine neuer Ausrüstung erforderlich ist. Völlig neu konfiguriert. Der Nachteil ist jedoch, dass alle Verkehr wieder von Azure mit dem lokalen Netzwerk die Sicherheitsgeräte angezeigt werden muss. Daher konnte Azure Azure Datenverkehr erhebliche Wartezeiten entstehen und Anwendungsperformance beeinflussen und Benutzer kommen, wenn an das lokale Netzwerk für die Durchsetzung von Sicherheitsrichtlinien erzwungen wurde.

#### <a name="3-how-are-the-boundaries-implemented"></a>(3) sind die Grenzen Implementierung?
Jede Sicherheitsgrenze müssen wahrscheinlich andere Anforderungen (z. B. IDS und Firewall-Regeln auf Internet Umkreisnetzwerk jedoch nur ACLs zwischen dem Perimeternetzwerk und Back-End-Subnetz). Die zu verwendenden Geräte hängt das Szenario und Sicherheitserfordernissen. Im folgenden Abschnitt erläutern die Beispiele 1, 2 und 3 einige Optionen, die verwendet werden können. Azure systemeigene Netzwerkfunktionen und Azure ab das Partnerökosystem Geräte überprüfen veranschaulicht die zahllosen Optionen zu jedem Szenario.

Ein weiterer ist Schlüssel Implementierung Entscheidung mit Azure im lokalen Netzwerk verbinden. Verwenden Sie Azure virtual Gateway oder eine virtuelle Appliance? Diese Optionen werden im folgenden Abschnitt (Beispiel 4, 5 und 6) ausführlich beschrieben.

Darüber hinaus kann Datenverkehr zwischen virtuellen Netzwerken in Azure erforderlich. Diese Szenarien werden zu einem späteren Zeitpunkt hinzugefügt werden.

Wenn Sie die Antworten auf die vorherigen Fragen kennen, kann [Schnell](#fast-start) Startabschnitt identifizieren die Beispiele für ein bestimmtes Szenario am besten geeignet sind.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Beispiele: Sicherheitsgrenzen mit Azure virtuelle Netzwerke erstellen
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Beispiel 1: Erstellen eines Perimeternetzwerks zum Schutz mit NSGs
[Fast Start an](#fast-start) | [detaillierte Anleitung für dieses Beispiel erstellen][Example1]

![Eingehende Perimeternetzwerk mit NSG][7]

#### <a name="environment-description"></a>Beschreibung der Umgebung
In diesem Beispiel wird ein Abonnement, das Folgendes enthält:

- Zwei cloud-Services: "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork" mit zwei Subnetzen: "FrontEnd" und "BackEnd"
- Eine Netzwerk-Sicherheitsgruppe, die auf beiden Subnetzen
- Ein Windows-Server, der einen Webserver für die Anwendung ("IIS01")
- Zwei Windows-Servern, die Anwendung Back-End-Server ("AppVM01", "AppVM02")
- Ein Windows-Server, die DNS-Server ("DNS01")

Skripts und Azure-Ressourcen-Manager-Vorlage finden Sie [Detaillierte Hinweise][Example1].

#### <a name="nsg-description"></a>NSG Beschreibung
In diesem Beispiel NSG Group erstellt und dann mit sechs Regeln geladen.

>[AZURE.TIP] Im Allgemeinen sollten Sie spezifischen Regeln "Zulassen", gefolgt von den allgemeineren "Verweigern" Regeln erstellen. Die angegebene Priorität schreibt vor, welche Regeln zuerst ausgewertet werden. Datenverkehr für eine bestimmte Regel gelten gefunden, werden keine weiteren Regeln ausgewertet. NSG können in eingehenden oder ausgehenden Richtung (aus der Sicht des Subnetzes) gilt.

Deklarativ, die folgenden Regeln für eingehenden Datenverkehr entstehen:

1.  Interne DNS-Verkehr (Port 53) ist zulässig.
2.  RDP-Datenverkehr (Port 3389) aus dem Internet mit einem virtuellen Computer ist zulässig.
3.  HTTP-Datenverkehr (Port 80) über das Internet auf Webserver (IIS01) ist zulässig.
4.  Datenverkehr (alle Anschlüsse) von IIS01 zu AppVM1 ist zulässig.
5.  Datenverkehr (alle Ports) aus dem Internet auf das gesamte virtuelle Netzwerk (beide Subnetze) wurde verweigert.
6.  Datenverkehr (alle Ports) aus dem Front-End-Subnetz mit dem Back-End-Subnetz wurde verweigert.

Mit diesen Regeln verpflichtet jedes Subnetz wurde eine HTTP-Anforderung an den Webserver sowohl Regeln 3 aus dem Internet eingehenden (zulassen) und 5 (verweigern) gilt. Aber da Regel 3 eine höhere Priorität hat, würde nur gelten, und Regel 5 würde nicht ins Spiel. Damit die HTTP-Anforderung an den Webserver dürfen. Wenn diese denselben Datenverkehr wurde DNS01-Server erreichen, Regel 5 (verweigern) wäre die erste und der Datenverkehr würde dürfen nicht an den Server übergeben. Regel 6 (verweigern) blockiert das Front-End-Subnetz aus Gesprächen mit Back-End-Subnetz (außer zulässiger Datenverkehr Regeln 1 und 4). Dies schützt Back-End-Netzwerk für den Fall, dass ein Angreifer-Anwendung auf dem front-End gefährdet. Der Angreifer würde auf Back-End "geschützt" Netzwerk (nur auf die Ressourcen auf dem Server AppVM01 ausgesetzt) besitzen.

Es wird eine standardmäßige ausgehende Regel Datenverkehr aus dem Internet. In diesem Beispiel wir ausgehenden Datenverkehr zugelassen und ausgehenden Regeln nicht ändern. Benutzerdefinierte routing zum Sperren von Datenverkehr in beiden Richtungen erforderlich ist (siehe Beispiel 3).

#### <a name="conclusion"></a>Abschluss
Dies ist relativ einfach und unkompliziert wie der Backend-Subnetz eingehenden Datenverkehr isoliert. Weitere Informationen finden Sie [Detaillierte Hinweise][Example1]. Diese Schritte umfassen:

- Wie Sie diesem Perimeternetzwerk mit PowerShell-Skripts erstellen.
- Wie diesem Perimeternetzwerk mit einer Azure-Ressourcen-Manager-Vorlage erstellt.
- Detaillierte Beschreibung der einzelnen Befehle NSG.
- Detaillierte Traffic Flow Szenarien zeigen, wie Datenverkehr zugelassen oder verweigert in jeder Ebene.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Beispiel 2: Erstellen eines Perimeternetzwerks zum Schutz mit einer Firewall und NSGs
[Fast Start an](#fast-start) | [detaillierte Anleitung für dieses Beispiel erstellen][Example2]

![Eingehende Umkreisnetzwerk NSG mit NVA][8]

#### <a name="environment-description"></a>Beschreibung der Umgebung
In diesem Beispiel wird ein Abonnement, das Folgendes enthält:

- Zwei cloud-Services: "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork" mit zwei Subnetzen: "FrontEnd" und "BackEnd"
- Eine Netzwerk-Sicherheitsgruppe, die auf beiden Subnetzen
- Eine virtuelle Appliance, in diesem Fall eine Firewall mit dem Front-End-Subnetz verbunden
- Ein Windows-Server, der einen Webserver für die Anwendung ("IIS01")
- Zwei Windows-Servern, die Anwendung Back-End-Server ("AppVM01", "AppVM02")
- Ein Windows-Server, die DNS-Server ("DNS01")

Skripts und Azure-Ressourcen-Manager-Vorlage finden Sie [Detaillierte Hinweise][Example2].

#### <a name="nsg-description"></a>NSG Beschreibung
In diesem Beispiel NSG Group erstellt und dann mit sechs Regeln geladen.

>[AZURE.TIP] Im Allgemeinen sollten Sie spezifischen Regeln "Zulassen", gefolgt von den allgemeineren "Verweigern" Regeln erstellen. Die angegebene Priorität schreibt vor, welche Regeln zuerst ausgewertet werden. Datenverkehr für eine bestimmte Regel gelten gefunden, werden keine weiteren Regeln ausgewertet. NSG können in eingehenden oder ausgehenden Richtung (aus der Sicht des Subnetzes) gilt.

Deklarativ, die folgenden Regeln für eingehenden Datenverkehr entstehen:

1.  Interne DNS-Verkehr (Port 53) ist zulässig.
2.  RDP-Datenverkehr (Port 3389) aus dem Internet mit einem virtuellen Computer ist zulässig.
3.  Alle Internet-Verkehr (alle Ports) virtuelles Netzwerkgerät (Firewall) ist zulässig.
4.  Datenverkehr (alle Anschlüsse) von IIS01 zu AppVM1 ist zulässig.
5.  Datenverkehr (alle Ports) aus dem Internet auf das gesamte virtuelle Netzwerk (beide Subnetze) wurde verweigert.
6.  Datenverkehr (alle Ports) aus dem Front-End-Subnetz mit dem Back-End-Subnetz wurde verweigert.

Mit diesen Regeln verpflichtet jedes Subnetz wurde eine HTTP-Anforderung aus dem Internet eingehenden Firewall beide Regeln 3 (zulassen) und 5 (verweigern) gilt. Aber da Regel 3 eine höhere Priorität hat, würde nur gelten, und Regel 5 würde nicht ins Spiel. Damit die HTTP-Anforderung an die Firewall dürfen. Wenn, denselben Datenverkehr wurde IIS01 Server erreichen, obwohl die Front-End-Subnetz ist, Regel 5 (verweigern) gilt, und der Datenverkehr wird nicht an den Server übergeben. Regel 6 (verweigern) blockiert das Front-End-Subnetz aus Gesprächen mit Back-End-Subnetz (außer zulässiger Datenverkehr Regeln 1 und 4). Dies schützt Back-End-Netzwerk für den Fall, dass ein Angreifer-Anwendung auf dem front-End gefährdet. Der Angreifer würde auf Back-End "geschützt" Netzwerk (nur auf die Ressourcen auf dem Server AppVM01 ausgesetzt) besitzen.

Es wird eine standardmäßige ausgehende Regel Datenverkehr aus dem Internet. In diesem Beispiel wir ausgehenden Datenverkehr zugelassen und ausgehenden Regeln nicht ändern. Benutzerdefinierte routing zum Sperren von Datenverkehr in beiden Richtungen erforderlich ist (siehe Beispiel 3).

#### <a name="firewall-rule-description"></a>Beschreibung der Firewall
Auf die Firewall sollte Weiterleitungsregeln erstellt. Da in diesem Beispiel nur Internetverkehr gebunden an den Firewall weitergeleitet und anschließend an den Webserver nur eine Weiterleitung Netzwerkadressübersetzung (NAT) ist Regel erforderlich.

Die Weiterleitungsregel akzeptiert alle eingehenden Quelladresse, die Firewall zu HTTP (Port 80 oder 443 für HTTPS) versuchen erreicht. Die Firewall lokale Schnittstelle gesendet und auf den Webserver mit der IP-Adresse des 10.0.1.5 umgeleitet.

#### <a name="conclusion"></a>Abschluss
Dies ist relativ einfach die Anwendung mit einer Firewall und Back-End-Subnetz eingehenden Datenverkehr isoliert. Weitere Informationen finden Sie [Detaillierte Hinweise][Example2]. Diese Schritte umfassen:

- Wie Sie diesem Perimeternetzwerk mit PowerShell-Skripts erstellen.
- Zum Erstellen dieses Beispiels mit einer Azure-Ressourcen-Manager-Vorlage.
- Detaillierte Beschreibung der einzelnen NSG Befehl und Firewall-Regeln.
- Detaillierte Traffic Flow Szenarien zeigen, wie Datenverkehr zugelassen oder verweigert in jeder Ebene.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Beispiel 3: Erstellen eines Perimeternetzwerks um Netzwerke mit Firewall, UDR und NSG
[Fast Start an](#fast-start) | [detaillierte Anleitung für dieses Beispiel erstellen][Example3]

![Bidirektionale Umkreisnetzwerk NSG, NVA und UDR][9]

#### <a name="environment-description"></a>Beschreibung der Umgebung
In diesem Beispiel wird ein Abonnement, das Folgendes enthält:

- Drei cloud-Services: "SecSvc001", "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork" mit drei Subnetzen: "SecNet", "FrontEnd" und "BackEnd"
- Eine virtuelle Appliance, in diesem Fall eine Firewall mit dem Subnetz verbunden SecNet
- Ein Windows-Server, der einen Webserver für die Anwendung ("IIS01")
- Zwei Windows-Servern, die Anwendung Back-End-Server ("AppVM01", "AppVM02")
- Ein Windows-Server, die DNS-Server ("DNS01")

Skripts und Azure-Ressourcen-Manager-Vorlage finden Sie [Detaillierte Hinweise][Example3].

#### <a name="udr-description"></a>UDR Beschreibung
Standardmäßig werden die folgenden systemrouten definiert als:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Der VNETLocal ist immer prefix(es) definierten Adresse des virtuellen Netzwerks für das bestimmte Netzwerk (d. h. es ändert sich von virtuelles Netzwerk virtuelle Netzwerk je nach jedes bestimmtes virtuelles Netzwerk Definition). Die restlichen systemrouten sind statische und wie in der Tabelle.

In diesem Beispiel werden zwei routing-Tabellen erstellt, für die Front-End- und Back-End-Subnetze. Jede Tabelle wird mit statischen Routen für das angegebene Subnetz geladen. In diesem Beispiel wird jede Tabelle verfügt über drei Routen, die allen Datenverkehr (0.0.0.0/0) direkt über die Firewall (des nächsten Abschnitts = Virtual Appliance IP-Adresse):

1. Datenverkehr im lokalen Subnetz mit keine nächste Hop definiert lokalen Subnetz Datenverkehr die Firewall umgeht.
2. Virtuelle Netzwerkverkehr mit einem Firewall als nächsten Hop; Dies überschreibt die Standardregel, die lokale virtuelle Netzwerk Datenverkehr direkt weitergeleitet.
3. Alle übrigen Datenverkehr (0/0) mit einer Firewall als nächsten Hop.

>[AZURE.TIP] Nicht mit der lokalen Subnetz-Eintrag in der UDR wird die lokalen Subnetz Kommunikation unterbrochen. 
> - In unserem Beispiel 10.0.1.0/24 auf VNETLocal ist als andernfalls Paket verlassen (10.0.1.4) auf einen anderen lokalen Server (Beispiel) 10.0.1.25 bestimmt Failover wird wie sie über die NVA erhalten die mit dem Subnetz gesendet wird, und im Subnetz erneut senden NVA und so weiter.
> - Eine Routingschleife sind normalerweise höher Multi-Nic-Geräte, die direkt an jedes Subnetz kommunizieren, die häufig von traditionellen lokal Einheiten. 

Sobald die Routingtabellen erstellt wurden, sind sie Subnetze gebunden. Die Front-End-Subnetz Routingtabelle einmal erstellt und an das Subnetz würde wie folgt aussehen:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] UDR kann nun gatewaysubnetz zugewiesen werden ExpressRoute-Verbindung verbunden ist.
>
> Beispiele zum Perimeternetzwerk mit ExpressRoute oder Standort-zu-Standort-Netzwerk aktivieren werden in Beispiel 3 und 4 angezeigt.


#### <a name="ip-forwarding-description"></a>IP-Forwarding Beschreibung
IP-Weiterleitung ist eine Companion-Funktion UDR. Dies ist eine Einstellung auf einer virtuellen Appliance, die Datenverkehr an die Appliance nicht gesondert behandelt wird und dieser Datenverkehr an das letztendliche Ziel weiterleiten kann.

Beispielsweise wenn Datenverkehr vom AppVM01 zum DNS01 Server anfordert, weitergeleitet UDR dies an den Firewall. Mit IP-Weiterleitung aktiviert der Datenverkehr für das Ziel DNS01 (10.0.2.4) akzeptiert die Appliance (10.0.0.4) und dann das letztendliche Ziel (10.0.2.4) an. Ohne IP-Firewall aktiviert würde Datenverkehr nicht von der Appliance akzeptiert, obwohl die Routentabelle Firewall als Nächster Hop hat. Um eine virtuelle Anwendung verwenden, ist es wichtig, die IP-Weiterleitung in Verbindung mit UDR aktivieren.

#### <a name="nsg-description"></a>NSG Beschreibung
In diesem Beispiel eine NSG Gruppe erstellt und mit einer einzelnen Regel geladen. Diese Gruppe wird dann nur für die Front-End- und Back-End-Subnetze (nicht SecNet) gebunden. Die folgende Regel wird deklarativ erstellt:

- Datenverkehr (alle Ports) aus dem Internet auf das gesamte virtuelle Netzwerk (alle Subnetze) wurde verweigert.

Obwohl NSGs in diesem Beispiel verwendet werden, ist die als sekundäre Verteidigung gegen falsche manuelle Konfiguration. Ziel ist es, eingehenden Datenverkehr aus dem Internet auf den Front-End- oder Back-End Subnets blockieren. Verkehrsfluss sollte nur über das Subnetz SecNet mit der Firewall (und gegebenenfalls auf den Front-End- oder Back-End-Subnetzen). Darüber hinaus würde mit der UDR Regeln, Datenverkehr, der in der Front-End- oder Back-End-Subnetze machen, Firewall (durch UDR) weitergeleitet. Die Firewall sieht dies als eine asymmetrische Flow und ausgehenden Datenverkehr fallen würde. So gibt es drei Sicherheitsebenen schützen die Subnetze:
- Keine geöffneten Endpunkte auf FrontEnd001 und BackEnd001 cloud-Services.
- NSGs Datenverkehr aus dem Internet verweigern.
- Die Firewall asymmetrischem Daten ablegen.

Ein interessanter Punkt über NSG in diesem Beispiel ist, dass es nur eine Regel verweigern Internetverkehr auf das gesamte virtuelle Netzwerk, einschließlich Sicherheit Subnetz ist. Aber die NSG nur den Front-End- und Back-End-Subnetzen gebunden ist, die Regel ist nicht verarbeitet Verkehr eingehende Sicherheit Subnetz. Daher wird die Sicherheit Subnetz Verkehrsfluss.

#### <a name="firewall-rules"></a>Firewall-Regeln
Auf die Firewall sollte Weiterleitungsregeln erstellt. Da die Firewall blockieren oder Weiterleitung aller eingehende, ausgehende und innerhalb virtueller Netzwerkverkehr ist, sind viele Firewall-Regeln erforderlich. Auch treffen alle eingehender Datenverkehr Sicherheitsdienst öffentliche IP-Adresse (auf verschiedene Ports) die Firewall verarbeitet werden. Empfiehlt die logische Abläufe vor dem Einrichten der Subnetze Diagramm und Firewall-Regeln zu später überarbeiten. Die folgende Abbildung zeigt eine logische Ansicht der Firewallregeln für dieses Beispiel:
 
![Logische Ansicht der Firewall-Regeln][10]

>[AZURE.NOTE] Basierend auf der Virtual Network Appliance verwendet, variiert die Management-Ports. In diesem Beispiel wird die Häfen 22 801 807 verwendet Barracuda NextGen Firewall verwiesen. Dokumentation der Appliance Hersteller verwendeten Gerät verwendete Ports genau zu.

#### <a name="firewall-rules-description"></a>Beschreibung der Firewall-Regeln
Im vorherigen logischen Diagramm wird Sicherheit Subnetz nicht angezeigt. Ist die Firewall ist die einzige Ressource in diesem Subnetz und dieses Diagramm zeigt die Firewallregeln und wie sie logisch zulassen oder verweigern Verkehrswerte nicht den eigentlichen Routingpfad. Externe Anschlüsse für RDP-Datenverkehr liegen gewählt reichte Ports (8014 – 8026) auch die letzten zwei Oktette der lokalen IP-Adresse Lesbarkeit etwas ausgerichtet wurden (z. B. lokale Adresse 10.0.1.4 ist mit externen Port 8014). Alle höheren fehlerfreien Ports können jedoch verwendet werden.

Beispielsweise benötigen wir sieben verschiedene Arten von Regeln:

- Externe Regeln (für eingehenden Datenverkehr):
  1.    Firewallregel-Management: dieser Anwendung umgeleitet Regel lässt Datenverkehr zu Management-Ports des virtuellen Netzwerkgerät.
  2.    RDP-Regeln (für jede Windows-Server): Diese vier Regeln (eine für jeden Server) Verwaltung der einzelnen Server über RDP zulassen. Auch kann in einer Regel je nach Netzwerk virtuelle Appliance verwendeten gebündelt werden.
  3.    Anwendungsregeln Datenverkehr: zwei dieser Regeln zuerst auf den Front-End-Datenverkehr und die zweite für den Back-End-Datenverkehr (z. B. Webserver auf Datenebene). Die Konfiguration dieser Regeln hängt von der Netzwerkarchitektur (wobei Server befinden) und Datenverkehr fließt (welche Richtung Verkehrswerte und welche ports verwendet werden).
      - Die erste Regel kann den Anwendungsdatenverkehr Application Server erreichen. Sicherheits- und andere Regeln ermöglichen, sind Verkehrsregeln Anwendung externe Benutzer oder Dienste die Anwendung(en) zugreifen können. Beispielsweise gibt ein Webserver auf Port 80. So leitet eine einzelne Anwendung Firewallregel eingehenden Datenverkehr zur externen IP Web Server internen IP-Adresse. Sitzung Verkehr umgeleitet wird an den internen Server über NAT übersetzt werden.
      - Die zweite Regel gilt Back-End-Webserver mit AppVM01 Server (aber nicht AppVM02) ermöglicht über einen beliebigen Port.
- Geschäftsordnung (innerhalb virtueller Netzwerkverkehr)
  4.    Ausgehende Regel Internet: Diese Regel lässt Datenverkehr von einem Netzwerk an ausgewählte Netzwerke übergeben. Diese Regel ist in der Regel eine Standardregel bereits auf die Firewall jedoch deaktiviert. Diese Regel sollte beispielsweise aktiviert werden.
  5.    DNS-Regel: Diese Regel ermöglicht nur DNS (Port 53) Datenverkehr an den DNS-Server. In dieser Umgebung ist den meisten Datenverkehr vom front-End, Back-End blockiert. Diese Regel ermöglicht DNS speziell vom lokalen Subnetz.
  6.    Subnetz Subnetz Regel: Diese Regel soll jeder Server im Subnetz Backend-Verbindung zu einem Server auf dem Front-End-Subnetz (aber nicht umgekehrt).
- Ausfallsichere Regel (für Datenverkehr, der eines der vorherigen entspricht):
  7.    Allen Datenverkehr Verweigerungsregel: Dies sollte immer die letzte Regel (als Priorität) und so fällt ein Verkehrswert überein vorangehenden Regeln von dieser Regel gelöscht. Dies ist eine Standardregel und normalerweise aktiviert. Im Allgemeinen sind keine Änderungen erforderlich.

>[AZURE.TIP] Für die zweite Anwendung Datenverkehr Regel zur Vereinfachung dieses Beispiels einen beliebigen Anschluss darf. In einem realen Szenario sollte den spezifischsten Port und Adressbereiche verwendet werden, zum Reduzieren der Angriffsfläche von dieser Regel.

Nachdem alle vorherigen Regeln erstellt werden, ist es wichtig, überprüfen Sie die Prioritätsreihenfolge der einzelnen Regeln sichergestellt Datenverkehr zugelassen oder verweigert wie gewünscht. In diesem Beispiel sind die Regeln nach Priorität sortiert.

#### <a name="conclusion"></a>Abschluss
Dies ist eine komplexere jedoch umfassendere Möglichkeit und isolieren das Netzwerk als die vorherigen Beispiele. (Beispiel 2 schützt die Anwendung und Beispiel 1 isoliert nur Subnetze). Dieses Design ermöglicht Überwachung Datenverkehr in beiden Richtungen und schützt nicht nur den eingehende Anwendungsserver erzwingt die Netzwerk-Sicherheitsrichtlinie für alle Server im Netzwerk Außerdem können abhängig von der Appliance verwendet vollständige Datenverkehr überwachen und Bewusstsein erreicht werden. Weitere Informationen finden Sie [Detaillierte Hinweise][Example3]. Diese Schritte umfassen:

- Wie dieses Beispiel Perimeternetzwerk mit PowerShell-Skripts erstellen.
- Zum Erstellen dieses Beispiels mit einer Azure-Ressourcen-Manager-Vorlage.
- Detaillierte Beschreibung der einzelnen UDR NSG Befehl und Firewall-Regel.
- Detaillierte Traffic Flow Szenarien zeigen, wie Datenverkehr zugelassen oder verweigert in jeder Ebene.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Beispiel 4: Hinzufügen einer hybridverbindung zwischen Standorten, virtuelle Appliance virtual private Network (VPN)
[Zurück zu Fast Start](#fast-start) | Schnell detaillierte Anleitung Erstellen

![Perimeternetzwerk mit NVA verbundenen hybrid][11]

#### <a name="environment-description"></a>Beschreibung der Umgebung
Hybrid-Netzwerk mit virtuellen Netzwerkgerät (NVA) kann in Beispiel 1, 2 oder 3 beschriebenen Perimeter Network Typen hinzugefügt werden.

Wie in der vorherigen Abbildung dargestellt, wird eine VPN-Verbindung über das Internet (von Standort zu Standort) verwendet ein Azure virtuelles Netzwerk über ein NVA einer lokalen Netzwerk herstellen.

>[AZURE.NOTE] Wenn Sie bei aktivierter Option Azure öffentliche Peering ExpressRoute verwenden, sollte eine statische Route erstellt werden. Dies sollte die NVA VPN-IP-Adresse Ihr Unternehmen Internet und nicht über ExpressRoute WAN weiterleiten. Benötigt die Option ExpressRoute Azure öffentliche Peering NAT kann die VPN-Sitzung unterbrechen.

Nachdem VPN ist, wird die NVA zentralen Hub für alle Netzwerke und Subnets. Weiterleitung Firewallregeln fest, welche Verkehrswerte dürfen, über NAT übersetzt, umgeleitet werden oder verloren (auch für Verkehrswerte zwischen dem lokalen Netzwerk und Azure).

Verkehrswerte sorgfältig berücksichtigen, dieses Entwurfsmusters heruntergestuft je nach den Anwendungsfall oder optimiert werden.

Die Umgebung in Beispiel 3 integriert, und dann eine Standort-zu-Standort-VPN-Hybrid Netzwerkverbindung erzeugt das folgende Design:

![Perimeternetzwerk mit NVA verbunden mit einem Standort-zu-Standort-VPN][12]

Der lokale Router oder andere Netzwerkgerät, das mit der NVA für VPN, wäre der VPN-Client. Dieses Gerät wird für initiieren und Verwalten der VPN-Verbindung mit der NVA verantwortlich.

Logisch sieht auf der NVA Netzwerk vier Separate "Sicherheitszonen" die Regeln auf den primären Leiter des Datenverkehrs zwischen diesen NVA:

![Logisches Netzwerk aus NVA Sicht][13]

#### <a name="conclusion"></a>Abschluss
Das Hinzufügen einer Standort-zu-Standort-VPN-Hybrid Netzwerk Verbindung Azure virtuellen Netzwerk kann im lokalen Netzwerk in Azure sicher. Mit einer VPN-Verbindung, der Datenverkehr ist verschlüsselt und Routen im Internet. In diesem Beispiel NVA zentral zu erzwingen die Sicherheitsrichtlinie verwalten. Weitere Informationen finden Sie detaillierte (in Kürze). Diese Schritte umfassen:

- Wie dieses Beispiel Perimeternetzwerk mit PowerShell-Skripts erstellen.
- Zum Erstellen dieses Beispiels mit einer Azure-Ressourcen-Manager-Vorlage.
- Detaillierte Traffic Flow Szenarien wie Datenfluss mit diesem Design angezeigt.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Beispiel 5: Hinzufügen einer hybridverbindung zwischen Standorten, Azure-Gateway VPN
[Zurück zu Fast Start](#fast-start) | Schnell detaillierte Anleitung Erstellen

![Perimeternetzwerk mit Gateway verbundenen hybrid][14]

#### <a name="environment-description"></a>Beschreibung der Umgebung
Hybrid-Netzwerk über einen Azure VPN-Gateway kann entweder in Beispiel 1 oder 2 beschriebenen Typ des Umkreisnetzwerks hinzugefügt werden.

Wie in der vorherigen Abbildung dargestellt, wird eine VPN-Verbindung über das Internet (von Standort zu Standort) zum lokalen Netzwerk herstellen ein Azure virtuelles Netzwerk über ein Azure VPN-Gateway verwendet.

>[AZURE.NOTE] Wenn Sie bei aktivierter Option Azure öffentliche Peering ExpressRoute verwenden, sollte eine statische Route erstellt werden. Dies sollte die NVA VPN-IP-Adresse Ihr Unternehmen Internet und nicht über ExpressRoute WAN weiterleiten. Benötigt die Option ExpressRoute Azure öffentliche Peering NAT kann die VPN-Sitzung unterbrechen.

Die folgende Abbildung zeigt zwei netzwerkkanten in dieser Option. Auf der ersten Seite NVA und NSGs Verkehrswerte innerhalb Azure Netzwerke und zwischen Azure und dem Internet steuern. Die zweite Kante ist Azure VPN-Gateway ist ein vollständig separater und isolierten Netzwerkrand zwischen lokalen und Azure.

Verkehrswerte sorgfältig berücksichtigen, dieses Entwurfsmusters heruntergestuft je nach den Anwendungsfall oder optimiert werden.

Mit der Umgebung in Beispiel 1, und dann eine Standort-zu-Standort-VPN-Hybrid Netzwerkverbindung erzeugt das folgende Design:

![Perimeternetzwerk mit Gateway verbunden über eine ExpressRoute-Verbindung][15]

#### <a name="conclusion"></a>Abschluss
Das Hinzufügen einer Standort-zu-Standort-VPN-Hybrid Netzwerk Verbindung Azure virtuellen Netzwerk kann im lokalen Netzwerk in Azure sicher. Systemeigene Azure VPN-Gateway, Ihren Datenverkehr ist IPSec verschlüsselt und über das Internet weitergeleitet. Außerdem kann mit Azure VPN-Gateway kostengünstigere Option (keine zusätzliche Lizenzierung Kosten mit Drittanbieter-NVAs) bereitstellen. Dies ist wirtschaftlichste in Beispiel 1, in denen keine NVA verwendet. Weitere Informationen finden Sie detaillierte (in Kürze). Diese Schritte umfassen:

- Wie dieses Beispiel Perimeternetzwerk mit PowerShell-Skripts erstellen.
- Zum Erstellen dieses Beispiels mit einer Azure-Ressourcen-Manager-Vorlage.
- Detaillierte Traffic Flow Szenarien wie Datenfluss mit diesem Design angezeigt.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Beispiel 6: Hinzufügen einer hybridverbindung mit ExpressRoute
[Zurück zu Fast Start](#fast-start) | Schnell detaillierte Anleitung Erstellen

![Perimeternetzwerk mit Gateway verbundenen hybrid][16]

#### <a name="environment-description"></a>Beschreibung der Umgebung
Hybrid-Netzwerk über eine private peering ExpressRoute-Verbindung kann entweder in Beispiel 1 oder 2 beschriebenen Typ des Umkreisnetzwerks hinzugefügt werden.

Wie in der vorherigen Abbildung gezeigt, bietet die ExpressRoute private peering eine direkte Verbindung zwischen dem lokalen Netzwerk und Azure virtuelles Netzwerk. Datenverkehr ihrer Durchquerung nur den Service-Anbieter und Microsoft Azure Netzwerk, berührt nicht das Internet.

>[AZURE.NOTE] Gibt es ExpressRoute, die Komplexität des dynamischen routing in Azure virtual Gateway verwendet UDR mit Einschränkungen. Diese lauten wie folgt:
>
>- UDR nicht mit dem gatewaysubnetz anzuwenden ExpressRoute verknüpfte Azure virtual Gateway verbunden ist.
>- ExpressRoute verknüpfte Azure virtual Gateway nicht NextHop Gerät für andere UDR Subnetze gebunden.
>
>
<br />

>[AZURE.TIP] ExpressRoute hält corporate Netzwerkverkehr aus dem Internet für eine höhere Sicherheit und Leistung erheblich erhöht. Es ermöglicht auch Dienstverträge Anbieters ExpressRoute. Azure-Gateway kann bis zu 2 Gb/s mit ExpressRoute, mit Standort-zu-Standort-VPNs Azure Gateway maximale Durchsatz 200 Mb/s ist.

Wie im folgenden Diagramm dargestellt, mit dieser Option die Umgebung verfügt jetzt über zwei netzwerkkanten. NVA und NSG steuern Verkehrswerte innerhalb Azure Netzwerke und zwischen Azure und dem Internet das Gateway ist ein vollständig separater und isolierten Netzwerkrand zwischen lokalen und Azure.

Verkehrswerte sorgfältig berücksichtigen, dieses Entwurfsmusters heruntergestuft je nach den Anwendungsfall oder optimiert werden.

Mit der Umgebung in Beispiel 1, und dann eine Netzwerkschnittstelle ExpressRoute hybride erzeugt das folgende Design:

![Perimeternetzwerk mit Gateway verbunden über eine ExpressRoute-Verbindung][17]

#### <a name="conclusion"></a>Abschluss
Zusätzlich eine Verbindung ExpressRoute Private Peering kann im lokalen Netzwerk in Azure eine sichere, niedrigere Latenz, leistungsfähigere Weise erweitern. Verwenden der systemeigenen Azure-Gateways wie in diesem Beispiel stellt außerdem kostengünstigere Option (keine Lizenzierung mit Drittanbieter-NVAs). Weitere Informationen finden Sie detaillierte (in Kürze). Diese Schritte umfassen:

- Wie dieses Beispiel Perimeternetzwerk mit PowerShell-Skripts erstellen.
- Zum Erstellen dieses Beispiels mit einer Azure-Ressourcen-Manager-Vorlage.
- Detaillierte Traffic Flow Szenarien wie Datenfluss mit diesem Design angezeigt.

## <a name="references"></a>Referenzen
### <a name="helpful-websites-and-documentation"></a>Hilfreiche Websites und Dokumentation
- Zugriff auf Azure mit Azure Ressourcen-Manager:
- Zugriff auf Azure PowerShell: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Virtuelle Dokumentation: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Network Security Group-Dokumentation: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Benutzerdefinierte routing-Dokumentation: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure virtual Gateways: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- Standort-zu-Standort-VPNs: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- ExpressRoute Dokumentation (werden Sie sich in den Abschnitten "Erste Schritte" und "How To"): [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Sicherheit Optionen Flussdiagramm"
[1]: ./media/best-practices-network-security/compliancebadges.png "Azure Compliance-Kennzeichen"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Azure-Sicherheitsfeatures"
[3]: ./media/best-practices-network-security/dmzcorporate.png "Eine DMZ in einem Unternehmensnetzwerk"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Azure-Sicherheitsarchitektur"
[5]: ./media/best-practices-network-security/dmzazure.png "Eine DMZ in Azure Virtual Network"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Hybrid-Netzwerk mit drei Sicherheitsgrenzen"
[7]: ./media/best-practices-network-security/example1design.png "Eingehende DMZ mit NSG"
[8]: ./media/best-practices-network-security/example2design.png "Eingehende DMZ NSG mit NVA"
[9]: ./media/best-practices-network-security/example3design.png "Bidirektionale DMZ NSG, NVA und UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "Logische Ansicht der Firewall-Regeln"
[11]: ./media/best-practices-network-security/example4designoptions.png "DMZ mit NVA hybride Netzwerk verbunden"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ mit NVA verbunden mit einem Standort-zu-Standort-VPN"
[13]: ./media/best-practices-network-security/example4networklogical.png "Logisches Netzwerk aus NVA Sicht"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ Azure Gateway verbunden zwischen Standorten Hybrid-Netzwerk"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ Azure mithilfe von Standort zu Standort VPN Gateway"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ Azure Gateway verbunden ExpressRoute hybride Netzwerk"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ Azure Gateway eine ExpressRoute-Verbindung"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
