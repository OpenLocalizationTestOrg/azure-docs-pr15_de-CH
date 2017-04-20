<properties
   pageTitle="Erste Schritte mit Microsoft Azure | Microsoft Azure"
   description="Dieser Artikel bietet eine Übersicht über Microsoft Azure Sicherheitsfunktionen und allgemeine Informationen für Unternehmen, die ihre Ressourcen zu einem Cloudanbieter migrieren."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/19/2016"
   ms.author="yuridio"/>

#<a name="getting-started-with-microsoft-azure-security"></a>Erste Schritte mit Microsoft Azure-Sicherheit

Beim Erstellen oder Migrieren von IT-Ressourcen für einen Cloudanbieter setzen auf diese Organisation schützen die Anträge und Daten Vertrauen ihrer Dienste und die Sicherheitsmechanismen, die sie zum Steuern der Sicherheit Ihrer Cloud-basierte Ressourcen bieten.

Azure Infrastruktur soll aus dem Millionen Kunden gleichzeitig hosten und die Bereitstellung einer vertrauenswürdigen Grundlage auf der Unternehmen ihre Sicherheitsbedürfnisse erfüllen können. Azure bietet darüber hinaus die unterschiedlichsten konfigurierbaren Sicherheitsoptionen und steuern sie Sicherheit eindeutige Anforderungen für die Bereitstellung anpassen.

In diesem Artikel auf die Sicherheit von Azure betrachten wir:

-   Azure-Dienste und Funktionen können Sie Ihre Dienste und Daten in Azure sichern

-   Wie schützt Microsoft Azure-Infrastruktur zum Schutz Ihrer Daten und Programme

##<a name="identity-and-access-management"></a>Identitäts- und Zugriffsmanagement


Steuern des Zugriffs auf IT-Infrastruktur, Daten und Programme ist wichtig. Diese Funktionen werden in Microsoft Azure von Diensten wie Active Directory Azure Azure-Speicher und Unterstützung für zahlreiche Standards und APIs.

[Azure Active Directory](../active-directory/active-directory-whatis.md) (Azure AD) ist eine Identität Repository und Engine, die Authentifizierung, Autorisierung und Zugriffskontrolle für Benutzer, Gruppen und Objekte in einer Organisation bereitstellt. Azure AD bietet Entwicklern effektiv Identitätsmanagement in ihre Anwendung integrieren. Standardprotokollen wie [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx)und [OpenID anmelden](http://openid.net/connect/) ermöglicht das Anmelden auf einer Vielzahl von Plattformen .NET, Java und Node.js, PHP.

Die REST-basierten Graph-API ermöglicht Entwicklern, lesen und Schreiben in das Verzeichnis von jeder Plattform. Durch Unterstützung von [OAuth 2.0](http://oauth.net/2/)können Entwickler eine mobile und Webapplikationen, die Integration mit Microsoft und Drittanbietern APIs und erstellen eigene sichere Web-APIs. Open-Source-Clientbibliotheken stehen für .NET, Windows Store iOS und Android zusätzliche Bibliotheken in der Entwicklung.

### <a name="how-azure-enables-identity-and-access-management"></a>Wie kann Azure Identitäts- und Zugriffsmanagement

Azure AD kann als eigenständige cloudverzeichnis für Ihre Organisation oder als integrierte Lösung mit vorhandenen lokalen Active Directory verwendet werden. Einige Integrationsfunktionen zählen Verzeichnis synchronisieren und einmaliges Anmelden (SSO). Diese Erweiterung Ihrer vorhandenen lokalen Identitäten in der Cloud und Administrator und Benutzer verbessern.

Einige Funktionen für Identitäts-und umfassen:

-   Azure AD kann [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) SaaS-Applikationen, unabhängig davon, wo sie gehostet werden. Einige Programme mit Azure verbundene und andere Kennwort SSO verwenden. Föderierte Applikationen unterstützen Benutzer bereitstellen und Kennwort Vaulting.

-   Zugriff auf Daten in [Azure Storage](https://azure.microsoft.com/services/storage/) erfolgt mittels Authentifizierung. Jedes Storage-Konto hat einen Primärschlüssel ([Konto Speicherschlüssel](https://msdn.microsoft.com/library/azure/ee460785.aspx)oder SAK) und sekundäre geheimen Schlüssel (SAS oder SAS).

-   Azure AD bietet Identität als Dienst Verbunds (mit [Active Directory Federation Services](../active-directory/fundamentals-identity.md), Synchronisierung und Replikation lokale Verzeichnisse.

-   [Azure mehrstufige Authentifizierung (MFA)](../multi-factor-authentication/multi-factor-authentication.md) ist der kombinierten Authentifizierung, die Benutzer auch Anmelden mit mobile-app, Telefonanruf oder SMS überprüfen. Es ist verfügbar in Azure AD für lokale Ressourcen Azure MFA-Server, und andere Programme und Verzeichnisse mit dem SDK.

-   [Azure Active Directory-Domänendiensten](https://azure.microsoft.com/services/active-directory-ds/) können Sie Azure virtuelle Computer einer Domäne ohne Domänencontroller bereitstellen hinzufügen. Benutzer können diese virtuellen Computer mit ihren Unternehmen Active Directory-Anmeldeinformationen anmelden und Domäne virtuelle Computer mithilfe von Gruppenrichtlinien erzwingen von Sicherheitsbaselines für alle Ihre Azure virtuellen Computer verwalten.

-   [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) bietet einen hohe Verfügbarkeit, eine globale Identität Verwaltungsdienst kundenorientierter Anwendungsmöglichkeiten, der Hunderte von Millionen von Identitäten skaliert. Integrierbar in Mobile und Plattformen. Ihre Kunden können den Clientanwendungen anpassbare Erfahrungen mit bestehenden sozialen Gesellschaften oder neue Anmeldeinformationen anmelden.

##<a name="data-access-control-and-encryption"></a>Daten-Zugriffskontrolle und Verschlüsselung

Microsoft setzt die Grundsätze der Trennung von Aufgaben und [Geringsten](https://en.wikipedia.org/wiki/Principle_of_least_privilege) in Azure Operationen. Zugriff auf Daten von Azure Supportmitarbeitern erfordert die ausdrückliche Berechtigung und wird gewährt auf "just-in-Time" Basis, die protokolliert und überwacht dann aufgehoben nach Abschluss des Projekts.

Darüber hinaus bietet Azure mehrere Funktionen zum Schutz von Daten in Transit und at-Rest, einschließlich der Verschlüsselung für Daten, Dateien, Programme, Dienste, Kommunikation und Laufwerke. Sie können Azure versehen sowie Speichern von Schlüsseln in Ihren Rechenzentren lokal verschlüsselt.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig1.PNG)

### <a name="azure-encryption-technologies"></a>Azure Verschlüsselungstechniken

Sie können administrativen Zugriff für Ihre Umgebung Abonnement Informationen mithilfe von [Azure AD Reporting](../active-directory/active-directory-reporting-audit-events.md). Sie können [BitLocker Drive Encryption](https://technet.microsoft.com/library/cc732774.aspx) auf Festplatten mit vertraulichen Informationen in Azure konfigurieren.

Andere Funktionen in Azure, die Sie zum Schutz Ihrer Daten unterstützen:

-   Anwendung können Entwickler Verschlüsselung in Azure bereitgestellten Anwendung mit Windows [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) und.NET Framework.

- Clientseitige Verschlüsselung für Microsoft-BLOB-Speicher können Sie die Schlüssel vollständig steuern.  Der Speicherdienst nie sieht die Schlüssel und nicht entschlüsseln der Daten.

-   [Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) ( [RMS-SDK](https://msdn.microsoft.com/library/dn758244.aspx) bietet Datei- und Verschlüsselung der Daten und Daten Leak Prevention über richtlinienbasierte Verwaltung.

-   Azure [Tabelle und Spalte Verschlüsselung (TDE/CLE)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) unterstützt SQL Server virtueller Maschinen und unterstützt von Drittanbietern lokalen Key Management Server in Rechenzentren von Kunden.

-   Konto Speicherschlüssel, Shared Access Signatures Verwaltungszertifikate und andere Tasten sind für jede Azure Mieter eindeutig.

-   Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) Hybrid-Speicher verschlüsselt Daten über eine 128-Bit-öffentliche private Schlüsselpaar vor Azure-Speicher hochgeladen.

-   Azure unterstützt und zahlreiche Verschlüsselungsmechanismen einschließlich SSL/TLS und IPsec AES, je nach den Datentypen und Container Transporte verwendet.

##<a name="virtualization"></a>Virtualisierung

Die Azure-Plattform verwendet eine virtuelle Umgebung. Benutzerinstanzen als eigenständige virtuelle Computer, die keinen Zugriff auf einem physischen Server und diese Isolierung physischen [Prozessor (Ring-0/Ring-3) Berechtigungsebenen](https://en.wikipedia.org/wiki/Protection_ring)erzwungen.

Ring 0 ist die bevorzugte und 3 ist die am wenigsten. Das Gastbetriebssystem wird in weniger privilegierten Ring 1 und Applikationen im letzten privilegierten Ring 3. Diese Virtualisierung von physischen Ressourcen führt eine klare Trennung zwischen Gastbetriebssystem und Hypervisor, wodurch zusätzliche Trennung zwischen den beiden.

Azure Hypervisor wie Micro-Kernel und alle Hardware Anforderungen an den Host für die Verarbeitung mit shared Memory Klassenschnittstelle VMBus von Gastcomputern übergibt. Dies verhindert, dass Benutzer lesen, schreiben, ausführen Zugang zum System und vermindert das Risiko von Systemressourcen freigeben.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig2.PNG)

### <a name="how-azure-implements-virtualization"></a>Implementierung von Virtualisierung in Azure

Azure verwendet eine Hypervisor Firewall (Paketfilter) in den Hypervisor implementiert und von einem Agent Fabric Controller konfiguriert. Dadurch Mieter vor unbefugtem Zugriff schützen. Standardmäßig beim Erstellen eine VM sämtlicher Datenverkehr blockiert und der Fabric Controller-Agent konfiguriert anschließend Paketfilter *Regeln und Ausnahmen* autorisierten Datenverkehr zulässt.

Es gibt zwei Regeln, die hier programmiert werden:

-   **Computerkonfiguration oder Infrastruktur**: standardmäßig die gesamte Kommunikation blockiert wird. Es gibt Ausnahmen zu einer VM DHCP- und DNS-Datenverkehr senden und empfangen. VMs können "public" Internet Datenverkehr senden und anderen virtuellen Computern im Cluster und OS Aktivierungsserver Datenverkehr senden. Die VMs ausgehende Zielliste enthält keinen, Azure Router Subnetze, Azure Management Back-End und andere Microsoft-Eigenschaften zulässig.

-   **Konfigurationsdatei Rolle**: eingehende ACLs basierend auf den Mieter Service Model definiert. Beispielsweise verfügt ein Mieter eine Web-Front-End-Port 80 auf einer bestimmten VM, öffnet dann Azure TCP-Port 80 auf alle IP-Adressen Wenn Sie einen Endpunkt in [Azure Service Management](../resource-manager-deployment-model.md) -Modell konfigurieren. Wenn die VM Backend oder Arbeitskraft ausgeführt, ist dann wir die Arbeitskraft nur für die VM in demselben Mandanten öffnen.

##<a name="isolation"></a>Isolierung

Trennung verhindern nicht autorisierten und unbeabsichtigten Übertragung von Informationen zwischen Bereitstellung in einer freigegebenen Multi-Tenant-Architektur ist weitere wichtige Cloud-Sicherheit.

Azure implementiert [Netzwerkzugriffsteuerung](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) und Trennung durch VLAN-Isolierung ACLs Balancers und IP-Filter geladen. Externe eingehende Netzwerkverkehr der virtuellen ist auf Ports und Protokolle definieren. Filterung wird implementiert, um den gefälschten Datenverkehr verhindern und den eingehenden und ausgehenden Datenverkehr vertrauenswürdigen Plattformkomponenten beschränkt. Datenverkehr Fluss Richtlinien werden standardmäßig auf Berandung Geräte, die Verkehr verweigern implementiert.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig3.PNG)

Network Address Translation (NAT) wird verwendet, um interne Netzwerkverkehr von externen Datenverkehr getrennt. Binnenverkehr ist nicht extern routingfähig. [Virtuelle IP-Adressen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) , die extern geroutet werden werden [Interne dynamische IP-](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) Adressen nur in Azure routingfähige übersetzt.

Externen Datenverkehr Azure virtuellen Maschinen über Zugriffssteuerungslisten (ACLs) auf Routern Lastenausgleich, Firewall und Layer 3-switches. Nur bestimmte bekannte Protokolle sind zulässig. ACLs sind Verkehr von Gastcomputern andere verwendete VLANs begrenzen. Darüber hinaus Datenverkehr gefiltert über IP-Filter auf dem Host-Betriebssystem, Datenverkehr auf beide Daten verknüpfen und Netzwerkebenen weiter einschränken.

### <a name="how-azure-implements-isolation"></a>Implementierung von Azure isolation

Azure Fabric Controller ist verantwortlich für die Zuweisung von Infrastrukturressourcen um Arbeitslasten Mieter und verwaltet die unidirektionale Kommunikation vom Server zum virtuellen Computer. Azure Hypervisor setzt Speicher und Prozess Trennung zwischen VMs und sichere Routen Netzwerkdatenverkehr Gast OS Mieter. Azure implementiert auch Isolierung für Mieter, Speicher und virtuelle Netzwerke:

-   Jede Azure AD-Mandanten ist logisch isolierten Sicherheitsgrenzen verwenden.

-   Azure Storage-Konten sind für jedes Abonnement eindeutig und Zugriff mit einem Storage-Kontoschlüssel authentifiziert werden muss.

-   Virtuelle Netzwerke sind logisch isolierten aus eindeutigen privaten IP-Adressen, Firewalls und IP-ACLs. Load Balancers Datenverkehr an die entsprechenden Mieter Endpunktdefinitionen Grundlage.

##<a name="virtual-network-and-firewall"></a>Virtuelles Netzwerk und firewall

[Verteilte und virtuelle Netzwerke](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) in Azure sicherstellen, dass Ihre privaten Netzwerkdatenverkehr logisch von Datenverkehr auf anderen virtuellen Azure-Netzwerken isoliert ist.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig4.PNG)

Ihr Abonnement kann mehrere isolierte private Netzwerke enthalten (und sind Firewall, Lastenausgleich und Netzwerkadressübersetzung).

Azure bietet drei primäre Netzwerk Trennung in Clustern Azure logisch Aufteilen des Datenverkehrs. [Virtuelle lokale Netzwerke](https://azure.microsoft.com/services/virtual-network/) (VLANs) werden zum Kundenverkehr vom Rest der Azure-Netzwerk trennen. Zugriff auf den Azure-Netzwerk außerhalb des Clusters ist durch Lastenausgleich.

Netzwerkverkehr zu und von VMs muss Hypervisor virtuellen Switch passieren. IP-Filter-Komponente im Stammverzeichnis BS isoliert von den virtuellen Gastcomputern und Gastcomputern voneinander Stammcomputer. Es filtert Datenverkehr Kommunikation zwischen Mieter Knoten und dem öffentlichen Internet (basierend auf der Konfiguration des Kunden) aus anderen Mandanten Trennung beschränken.

IP-Filter verhindert Gastcomputern aus:

- Generieren von gefälschten Datenverkehr

- Nicht an sie gerichteten Datenverkehr empfangen

- Den Verkehr auf geschützte Infrastrukturendpunkte

- Senden oder Empfangen von ungeeigneten broadcast-Verkehr

Sie können die virtuellen Computer auf [Azure virtuelle Netzwerke](https://azure.microsoft.com/documentation/services/virtual-network/)setzen. Diese virtuelle Netzwerke ähneln Netzwerke in einer lokalen Umgebung konfigurieren, werden normalerweise mit einem virtuellen Switch verbunden. Virtuelle Computer im gleichen virtuellen Azure-Netzwerk verbunden können miteinander ohne zusätzliche Konfiguration kommunizieren. Sie können auch die verschiedene Subnetzen innerhalb des virtuellen Netzwerks Azure konfigurieren.

Die folgenden Azure Virtual Network-Technologie können um sichere Kommunikation im Azure virtuelle Netzwerk:

-   [**Netzwerk-Sicherheitsgruppen (NSG)**](../virtual-network/virtual-networks-nsg.md). NSG Steuerelement Datenverkehr an eine oder mehrere Virtual Machine (VM) Instanzen können des virtuellen Netzwerks. Eine NSG enthält Zugriffsregeln, die gewährt oder verweigert Datenverkehr anhand der Richtung des Datenverkehrs, Protokoll Quelladresse und Port und Adresse und Port.

-   [**Benutzerdefinierte Routing**](../virtual-network/virtual-networks-udr-overview.md). Steuern Sie das routing von Paketen über ein virtuelles Gerät erstellen benutzerdefinierte Routen, die den nächsten Hop für Pakete, die zu einem bestimmten Subnetz zu geben eine der virtual Network Security Appliance.

-   [**IP-Weiterleitung**](../virtual-network/virtual-networks-udr-overview.md). Eine virtuelles Netzwerk Security Appliance muss eingehenden Datenverkehr empfangen, der sich nicht behandelt. Damit eine VM an andere Ziele gerichtete Datenverkehr empfangen können, können Sie IP-Weiterleitung für die VM.

-   [**Erzwungene Tunnel**](../vpn-gateway/vpn-gateway-about-forced-tunneling.md). Erzwungene Tunneln können umleiten oder "zwingen", die alle Internet gerichtete Datenverkehr durch die virtuellen Computer in Azure Virtual zurück in das lokale Verzeichnis über eine Standort-zu-Standort-VPN-Tunnel für Inspektion und Überwachung

-   [ **Endpunkt** ACLs](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md). Sie können steuern, welche Computer eingehende Verbindungen über das Internet mit einem virtuellen Computer im virtuellen Netzwerk Azure dürfen Endpunkt ACLs definieren.

-   [**Partner Network Security Solutions**](https://azure.microsoft.com/marketplace/). Es gibt zahlreiche Partner Network Security Solution Azure Marketplace zugreifen können.

### <a name="how-azure-implements-virtual-networks-and-firewall"></a>Implementierung von Azure virtual Networks und firewall

Azure implementiert Paketfilter Firewalls auf allen Host und Gast VMs standardmäßig. Betriebssystem Windows Azure Galeriebilder können Windows Firewall standardmäßig aktiviert. Lastenausgleich am Perimeter Azure für Netzwerke Steuerelement Öffentlichkeitsarbeit basierend auf IP-ACLs von Administratoren des Kunden verwaltet.

Wenn Azure Daten eines Kunden als Teil des normalen Betriebs oder bei einem Notfall verschiebt, wird es über private, verschlüsselte Kommunikationskanäle. Virtuelle Netzwerke und Firewall andere Funktionen von Azure genutzt werden:

-   **Systemeigene Hostfirewall**: Azure Fabric und Speicher auf einem einheitlichen OS hat keine Hypervisor und daher die Windows-Firewall mit der obigen Regeln konfiguriert. Speicher wird in optimieren.

-   **Hostfirewall**: Host-Firewall das Hostbetriebssystem zu den Hypervisor ausgeführt wird. Die Regeln werden nur den Fabric Controller und Felder mit dem Host-Betriebssystem über einen bestimmten Port springen programmiert. Andere Ausnahmen sind DHCP-Antwort und DNS-Antworten. Azure verwendet eine Computerkonfiguration Datei über die Vorlage von firewall-Regeln für das Hostbetriebssystem. Host selbst ist eine Windows-Firewall so konfiguriert, dass nur Kommunikation von bekannten, authentifizierten Quellen vor externen Angriffen geschützt.

-   **Gast-Firewall**: repliziert die Regeln in der VM Switch Paketfilter aber andere Software (d. h. der Windows-Firewall Teil das Gastbetriebssystem) programmiert. Firewall VM Guest kann konfiguriert werden Kommunikation zu oder von der VM-Gast einzuschränken, wenn auf dem Host IP-Filter die Kommunikation zulässig ist. Sie können z. B. Gast VM Firewall verwenden, um Kommunikation zwischen die vnets einschränken, die miteinander Verbindung konfiguriert wurde.

-   **Speicher-Firewall (FW)**: die Firewall auf dem Front-End-Speicher filtert den Datenverkehr auf Ports 80/443 und anderen erforderlichen Dienstprogramm Ports zu. Firewall auf den Back-End-Speicher beschränkt erst von Front-End-Speicher-Server-Kommunikation.

-   **Virtuelle Netzwerk-Gateway**: Kreuz Gateways, die auf lokalen Websites Ihre Arbeitslasten in Azure Virtual Network an lokale [Virtuelle Netzwerkgateways Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) dienen. Es muss auf lokalen Websites über [IPSec-Standort-zu-Standort-VPN-Tunnel](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)oder [ExpressRoute](../expressroute/expressroute-introduction.md) Stromkreise herstellen. Für IPsec-IKE VPN-Tunnel Gateways führen Handshakes IKE und IPsec S2S VPN-Tunnel zwischen virtuellen Netzwerken und lokalen Websites einrichten. Virtuelle Netzwerkgateways beendet auch [Punkt-zu-Standort-VPNs](../vpn-gateway/vpn-gateway-point-to-site-create.md).

##<a name="secure-remote-access"></a>Secure Remote Access

Daten in der Cloud müssen ausreichende Garantien verhindern Angriffe und Vertraulichkeit und Integrität während in Transit aktiviert. Dazu gehören mit einer Organisation Policy-basierte, überprüfbaren Identitäts- und Mechanismen Netzwerk-Steuerelemente.

Integrierter Kryptografie-Technologie können Sie zum Verschlüsseln der Kommunikation innerhalb und zwischen bis zwischen Azure und Azure zu lokalen Rechenzentren. Administratorzugriff auf virtuelle Maschinen über [Remotedesktopsitzungen](../virtual-machines/virtual-machines-windows-classic-connect-logon.md) [Remote Windows PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx)und Azure-Verwaltungsportal werden immer verschlüsselt.

Um sichere lokalen Datencenter in die Cloud erweitern, Azure bietet sowohl [Standort-zu-Standort-VPN-](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) und [Punkt-zu-Standort-VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md)sowie dedizierte Links mit [ExpressRoute](../expressroute/expressroute-introduction.md) (Verbindungen zu Azure Virtual Networks VPN sind verschlüsselt).

### <a name="how-azure-implements-secure-remote-access"></a>Implementierung von Azure sicheren remote-Zugriff

Verbindung zum Azure-Portal müssen immer authentifiziert und erfordern SSL/TLS. Sie können Management Zertifikate zur sicheren Verwaltung zu ermöglichen. Sichere Industriestandardprotokolle [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) wie [IPsec](https://en.wikipedia.org/wiki/IPsec) werden unterstützt.

[Azure-ExpressRoute](../expressroute/expressroute-introduction.md) können Sie private Verbindung zwischen Azure-Datencentern und Infrastruktur vor Ort oder in einer Umgebung mit gemeinsam. ExpressRoute-Verbindung gehen nicht über das öffentliche Internet. Sie bieten mehr Zuverlässigkeit, beschleunigt, niedriger Latenz und höhere Sicherheit als Standard internetbasierten Links. In einigen Fällen kann die mit ExpressRoute Verbindung zum Übertragen von Daten zwischen lokalen und Azure auch erhebliche Kostenvorteile ergeben.

##<a name="logging-and-monitoring"></a>Protokollierung und Überwachung

Azure bietet authentifizierten Protokollierung von sicherheitsrelevanten Ereignisse, die einen Audit-Trail generieren und wurde entwickelt, um gegen Manipulationen. Dies umfasst Systeminformationen wie Sicherheitsereignisprotokolle in Azure Infrastruktur VMs und Azure AD. Event-monitoring umfasst Ereignisse wie DHCP- oder DNS-Server IP-Adressen, Zugriffsversuche auf Ports, Protokolle gesammelt oder IP-Adressen, die vom blockiert Konstruktionsänderungen, Sicherheitsrichtlinien oder Firewallregeln, Gruppe erstellen, andere Prozesse oder Treiberinstallation.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig5.PNG)

Überwachungsprotokolle Aufzeichnung privilegierter Zugriff und Aktivitäten, autorisierten und nicht autorisierten Zugriffsversuchen Systemausnahmen und Informationen, die Sicherheit für einen bestimmten Zeitraum aufbewahrt werden. Die Speicherung der Protokolle liegt in Ihrem Ermessen, da Protokoll Erfassung und Aufbewahrung an Ihre eigenen Bedürfnisse konfigurieren.

### <a name="how-azure-implements-logging-and-monitoring"></a>Implementierung von Azure und-Protokollierung

Azure Ausbringen von Agenten Management Agents (MA) und Azure Security Monitor (ASM) auf jeder Compute, Speicher oder Fabric-Knoten Verwaltung systemeigene oder virtuellen. Jedes Management Agents konfiguriert ein Speicherkonto Service-Team mit einem Zertifikat aus dem Azure Zertifikatspeicher abgerufen und Vorwärts vorkonfigurierte Diagnose und Ereignisdaten an das Speicherkonto authentifizieren. Diese Agenten werden Kunden virtuellen Maschinen nicht bereitgestellt.

Azure-Administratoren Zugriff auf Protokolle über ein Webportal für authentifizierte und kontrollierten Zugriff auf die Protokolle. Ein Administrator kann analysieren, filtern, Korrelation und Protokolle analysieren. Azure-Team Speicher Dienstkonten Protokolle sind direkte Administratorzugriff auf Hilfe gegen Manipulationen Protokoll geschützt.

Microsoft sammelt Protokolle über das Syslog-Protokoll Netzwerkgeräte und Host-Servern mit Microsoft Audit Collection Services (ACS). Diese Protokolle befinden sich in einer Protokoll-Datenbank aus denen Alerts verdächtige Ereignisse direkt an Microsoft Administrator generiert werden. Der Administrator kann Zugriff auf und Analysieren von Ereignisprotokollen.

[Azure-Diagnose](https://msdn.microsoft.com/library/azure/gg433048.aspx) ist ein Feature von Azure, das Erfassen von Daten von einer Anwendung in Azure ermöglicht. Dies ist die Diagnosedaten Debuggen und Problembehandlung Messen der Leistung, Überwachen der Ressourcenverwendung, Analyse und Planung und Überwachung. Nach der Diagnose Daten können sie in Azure Speicherkonto für die Dauerhaftigkeit übertragen. Übertragung können entweder geplant oder bei Bedarf.

##<a name="threat-mitigation"></a>Threat Mitigation

Neben Isolierung, Verschlüsselung und Filtern beschäftigt Azure eine Bedrohung Minderung Mechanismen und Verfahren zu Infrastruktur und alle Dienste. Dazu gehören internen Steuermechanismen und Verfahren zum Erkennen und beheben erweiterte Bedrohung wie DDoS, Ausweitung von Berechtigungen und [OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

Die Sicherheitskontrollen und Risikomanagement Microsoft um die Cloud-Infrastruktur sichern Risiko von Sicherheitsvorfällen. Aber bei eines Vorfalls auftreten Team Security Incident Management (SIM) im Microsoft Online-Sicherheit und Compliance (OSSC) Team bereit 24 x 7 reagieren.

### <a name="how-azure-implements-threat-mitigation"></a>Implementierung von Azure Threat mitigation

Azure hat Sicherheit Steuerelemente Bedrohung Schadensbegrenzende und Kunden Bedrohung in ihrer Umgebung verringern. Der folgenden Liste zusammengefasst Threat Mitigation Funktionen von Azure:

-   [Azure Anti-Malware](../security/azure-security-antimalware.md) ist standardmäßig auf allen Infrastrukturservern aktiviert. Sie können optional in eigene VMs aktivieren.

-   Microsoft bietet eine kontinuierliche Überwachung über Server, Netzwerke und Applikationen Gefahren erkennen und verhindern Angriffe. Automatisierte Signale benachrichtigen Administratoren anormalen Verhaltensweisen zu Maßnahmen auf internen und externen Gefahren.

-   Sie können 3rd Party Sicherheitsfunktionen in Ihre Abonnements wie Web Application Firewalls von [Barracuda](https://techlib.barracuda.com/ng54/deployonazure)bereitstellen.

-   Microsoft Methode zum Penetration Tests enthält "[Rot Teaming](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)", umfasst Microsoft-Sicherheitsexperten auf (nicht Kunde) live Produktionssysteme in Azure Verteidigungsmaßnahmen gegen reale, erweiterte dauerhafte testen.

-   Integrierte Bereitstellung Systeme verwalten die Verteilung und Installation von Sicherheitspatches in der Azure-Plattform.

##<a name="next-steps"></a>Nächste Schritte

[Azure-Sicherheitscenter](https://azure.microsoft.com/support/trust-center/)

[Azure Security-Teamblog](http://blogs.msdn.com/b/azuresecurity/)

[Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

[Active Directory-Blog](http://blogs.technet.com/b/ad/)
