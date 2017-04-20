<properties 
    pageTitle="Konfigurationsdetails für die Arbeit mit Express Netzwerk" 
    description="Netzwerk-Konfigurationsdetails für App Service-Umgebungen in einer virtuellen Netzwerken ausgeführt mit ExpressRoute-Verbindung verbunden ist." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Netzwerk-Konfigurationsdetails für App Umgebungen mit ExpressRoute 

## <a name="overview"></a>Übersicht ##
Kunden können eine [Azure ExpressRoute] [ ExpressRoute] zu ihrer virtuellen Netzwerkinfrastruktur so erweitern ihre lokalen Netzwerk in Azure.  Eine App Service-Umgebung kann in ein Subnetz des [virtuellen Netzwerks] erstellt[ virtualnetwork] Infrastruktur.  Apps auf die App Service-Umgebung können dann Verbindung mit Back-End-Ressourcen zugegriffen werden nur über die ExpressRoute-Verbindung herstellen.  

Eine App Service-Umgebung erstellt werden **oder** ein virtuelles Netzwerk Azure Ressourcenmanager **oder** eine klassische Modell virtuelles Netzwerk.  Mit einer aktuellen Änderung im Juni 2016 können Asen auch in virtuellen Netzwerken bereitgestellt werden, mit denen öffentliche Adressbereichen oder Adressräume RFC1918 (d. h. Private Adressen). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Erforderliche Netzwerkkonnektivität ##
Gibt es Konnektivitätsanforderungen für App Service-Umgebungen, die ursprünglich nicht in einem virtuellen Netzwerk mit einer ExpressRoute erfüllt werden kann.  App Service-Umgebungen benötigen die folgenden ordnungsgemäß funktioniert:


-  Ausgehende Netzwerkkonnektivität zum Azure-Speicher Endpunkte weltweit beide Ports 80 und 443.  Dies schließt Endpunkte befindet sich im selben Bereich wie der App Service-Umgebung sowie Speicher Endpunkte in **anderen** Azure-Regionen.  Unter den folgenden DNS-Domänen Azure Storage Endpunkte zu beheben: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* und *file.core.windows.net*.  
-  Ausgehende Netzwerkkonnektivität zum Azure Dateien Dienst auf Port 445.
-  Ausgehende Netzwerkkonnektivität zu Sql DB Endpunkte in der gleichen Region wie die App Service-Umgebung befindet.  SQL-DB-Endpunkte unter der folgenden Domäne auflösen: *database.windows.net*.  Dies erfordert Zugriff auf Ports 1433 11000 11999 und 14000 14999 öffnen.  Weitere Informationen finden Sie in [diesem Artikel auf Sql Datenbank V12 Anschlussverwendung](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Ausgehende Netzwerkkonnektivität zum Azure Management Ebene Endpunkte (ASM und ARM Endpunkte).  Dies schließt ausgehende Verbindung mit *management.core.windows.net* und *management.azure.com*. 
-  Ausgehende Netzwerkkonnektivität, *ocsp.msocsp.com*, *mscrl.microsoft.com* und *crl.microsoft.com*.  Dies ist erforderlich, um SSL-Funktionen zu unterstützen.
-  Die DNS-Konfiguration für das virtuelle Netzwerk muss alle Endpunkte und die früheren genannten Domänen auflösen.  Diese Endpunkte aufgelöst werden können, App Service-Umgebung Erstellversuche fehl und vorhandenen App Service-Umgebungen als fehlerhaft markiert.
-  Ausgehender Zugriff auf Port 53 ist erforderlich für die Kommunikation mit DNS-Servern.
-  Wenn ein benutzerdefinierter DNS-Server am anderen Ende ein VPN-Gateway muss der DNS-Server aus dem Subnetz enthält die App Service-Umgebung erreichbar sein. 
-  Ausgehende Netzwerkpfad kann nicht über interne corporate Proxys Reisen und können Kraft lokal getunnelt werden.  Dabei ändert die effektive NAT-Adresse ausgehenden Netzwerkverkehr von App Service-Umgebung.  Ändern der Adresse NAT ausgehenden Netzwerkverkehr einer App Service-Umgebung verursacht Verbindungsfehler auf viele der oben aufgeführten Endpunkte.  Dies führt zu App Service-Umgebung Erstellversuche sowie zuvor fehlerfrei App Umgebungen als fehlerhaft markiert.  
-  Eingehenden Netzwerkzugriff auf erforderliche Anschlüsse für App Service-Umgebungen darf in diesem [Artikel]beschriebenen[requiredports].

DNS-Anforderungen erreicht dadurch eine gültige DNS-Infrastruktur konfiguriert und für das virtuelle Netzwerk verwaltet.  Wenn aus irgendeinem Grund die DNS-Konfiguration geändert wird, nach dem Erstellen einer App Service-Umgebung können Entwickler eine App Service-Umgebung übernehmen die neue DNS-Konfiguration erzwingen.  Auslösen eines parallelen Umgebung Neustarts mit dem Symbol "Neustart" am oberen Rand der App Service-Umgebung Management Blade in [Azure-Portal] [ NewPortal] Umgebung übernehmen die neue DNS-Konfiguration verursacht.

Eingehende Network Access Anforderungen erreicht durch Konfigurieren einer [Netzwerk-Sicherheitsgruppe] [ NetworkSecurityGroups] die App Service-Umgebung Subnetz den erforderlichen Zugriff wie in diesem [Artikel]beschriebenen[requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Ausgehende Netzwerkkonnektivität aktivieren für eine App Service-Umgebung##
Neu erstellte ExpressRoute-Verbindung gibt standardmäßig eine Standardroute, die ausgehende Internet-Konnektivität ermöglicht.  Mit dieser Konfiguration werden in Verbindung mit anderen Endpunkten Azure App Service-Umgebung.

Jedoch ist eine gemeinsame Kundenkonfiguration, erzwingt ausgehenden Internetdatenverkehr stattdessen lokale, eigene Standardroute (0.0.0.0/0) definiert.  Dieser Datenverkehr wird immer App Service-Umgebungen da ausgehende Datenverkehr entweder blockierte lokal oder NAT auf Adressen, die nicht mehr mit verschiedenen Azure Endpunkte nicht erkannt.

Die Lösung ist eine (oder mehrere) benutzerdefinierte Routen (Decision) im Subnetz zu definieren, die App Service-Umgebung enthält.  Ein UDR definiert Subnet-spezifische Routen, die statt der Standard-Route berücksichtigt werden.

Gegebenenfalls empfiehlt es sich mit der folgenden Konfiguration:

- ExpressRoute-Konfiguration kündigt 0.0.0.0/0 und Kraft Tunnel standardmäßig alle ausgehenden Datenverkehr lokal.
- UDR für das Subnetz mit der App Service-Umgebung übernommen definiert 0.0.0.0/0 mit nächsten Hop Internet (Beispiel weiter unten in diesem Artikel).

Die Schritte ergibt, dass Subnetzebene UDR ExpressRoute erzwungene Tunneln, Vorrang damit ausgehenden Internetzugriff von App Service-Umgebung.

> [AZURE.IMPORTANT] In einer UDR **muss** definierten Routen sein von ExpressRoute Konfiguration angekündigte Routen Vorrang spezifisch genug.  Im folgenden Beispiel verwendet Bereich Breite 0.0.0.0/0 und so möglicherweise versehentlich durch übergangen Routenbekanntgaben Weitere Adressbereiche verwenden.
>
>App Service-Umgebungen sind nicht mit ExpressRoute unterstützt, **aus öffentlichen peeringpfad an den privaten Pfad peering Cross angekündigt**.  ExpressRoute Konfigurationen öffentliche peering konfiguriert, erhalten Routenbekanntgaben von Microsoft für eine Reihe von Microsoft Azure IP-Adressbereiche.  Diese Adressbereiche ist auf private peering Pfad Cross angekündigt, das Endergebnis werden alle ausgehende Netzwerkpakete die App Service-Umgebung Subnetz Infrastruktur des Kunden vor Ort geltenden getunnelt.  Dieser Datenfluss wird mit App Service derzeit nicht unterstützt.  Eine Lösung für dieses Problem ist zu Cross-Werbung Routen von öffentlichen peeringpfad private peering Pfad.

Hintergrundinformationen zu benutzerdefinierten Routen steht in dieser [Übersicht][UDROverview].  

Informationen zum Erstellen und Konfigurieren von benutzerdefinierten Routen steht in diesem [Führer wie][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>UDR Beispielkonfiguration einer App Service-Umgebung ##

**Erforderliche Komponenten**

1. Installieren von Azure Powershell [Azure-Downloadseite] [ AzureDownloads] (vom Juni 2015 oder später).  Klicken Sie unter"Befehlszeilenoptionen" ist ein Link "Installieren" unter "Windows Powershell", die die neuesten Powershell-Cmdlets installieren.

2. Es wird empfohlen, ein eindeutiges Subnetz für die exklusive Verwendung durch eine App Service-Umgebung erstellt wird.  Dadurch Decision angewendet auf das Subnetz nur ausgehenden Datenverkehr für die App Service-Umgebung öffnen.
3. **Wichtig**: die App Service-Umgebung nicht erst **nach** folgenden Konfigurationsschritten folgen bereitstellen.  Dadurch wird sichergestellt, dass ausgehende Netzwerkkonnektivität verfügbar ist, bevor Sie versuchen, eine App Service-Umgebung bereitstellen.

**Schritt 1: Erstellen Sie eine benannte Route-Tabelle**

Der folgende Codeausschnitt erstellt eine Route-Tabelle namens "DirectInternetRouteTable" in der Region West uns Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Schritt 2: Erstellen Sie eine oder mehrere Routen in der Routingtabelle**

Sie müssen eine oder mehrere Routen der Routingtabelle hinzufügen, um ausgehenden Internetzugriff zu ermöglichen.  

Die empfohlene Vorgehensweise für das Konfigurieren von ausgehenden Zugriff auf das Internet ist eine Route 0.0.0.0/0 wie folgt definieren.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Denken Sie daran, 0.0.0.0/0 Breite Adressbereich ist und daher durch spezifischere Adressbereiche angekündigt durch die ExpressRoute überschrieben.  Zum Wiederholen der obigen Empfehlung sollte ein UDR mit einer Route 0.0.0.0/0 zusammen mit einer ExressRoute Konfiguration verwendet, die nur 0.0.0.0/0 sowie wirbt. 

Als Alternative können Sie eine umfassende und aktualisierte Liste der CIDR-Bereiche von Azure verwendet.  Die XML-Datei mit allen Azure IP-Adressbereiche steht im [Microsoft Download Center][DownloadCenterAddressRanges].  

Beachten Sie jedoch, dass diese Bereiche im Laufe der Zeit ändern, daher erfordern regelmäßige manuelle Updates für die benutzerdefinierte Routen zu synchronisieren.  Auch standardmäßig oben maximal 100 Routen in einer einzigen UDR gibt, müssen Sie "Azure IP-Adressbereiche an höchstens 100 Route zusammenfassen" angekündigt dabei UDR Routen müssen als Routen definiert durch die ExpressRoute.  


**Schritt 3: Verbinden der Routentabelle an das Subnetz mit der App Service-Umgebung**

Letzte Konfigurationsschritt ist das zuordnen die Routentabelle mit dem Subnetz, in dem die App Service-Umgebung bereitgestellt werden.  Der folgende Befehl verknüpft die "DirectInternetRouteTable", "ASESubnet", die letztendlich eine App Service-Umgebung enthält.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Schritt 4: Abschließende Schritte**

Sobald die Routentabelle an das Subnetz gebunden ist, wird empfohlen, zunächst testen und bestätigen Sie die beabsichtigte Wirkung.  Beispielsweise einen virtuellen Computer im Subnet bereitstellen und bestätigen:


- Ausgehender Datenverkehr Azure und nicht Azure Endpunkte erwähnt die in diesem Artikel **nicht** unten ExpressRoute-Verbindung übertragen wird.  Es ist sehr wichtig, um dieses Verhalten zu überprüfen, da ausgehender Datenverkehr aus dem Subnetz wird weiterhin erzwungen lokal App Service-Umgebung getunnelt, die Erstellung schlägt fehl. 
- DNS-Lookups für die Endpunkte bereits erwähnt alle korrekt aufgelöst werden. 

Die oben beschriebenen Schritte bestätigt werden, müssen Sie den virtuellen Computer gelöscht werden, da das Subnetz muss "empty" Zeitpunkt erstellten App Service-Umgebung.
 
Fahren Sie mit dem Erstellen einer App Service-Umgebung.

## <a name="getting-started"></a>Erste Schritte
Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Zunächst mit App-Dienst finden Sie unter [Einführung in App Service-Umgebung][IntroToAppServiceEnvironment]

Weitere Informationen über die Plattform Azure App Service finden Sie in [Azure App Service][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
