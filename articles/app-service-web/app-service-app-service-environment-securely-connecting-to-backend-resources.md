<properties 
    pageTitle="Herstellen einer sicheren Verbindung zum Back-End-Ressourcen aus einer App Service-Umgebung" 
    description="Informationen Sie über das sichere Verbinden mit Back-End-Ressourcen aus einer App Service-Umgebung." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Herstellen einer sicheren Verbindung zum Back-End-Ressourcen aus einer App Service-Umgebung #

## <a name="overview"></a>Übersicht ##
Seit einer App Service-Umgebung immer **entweder** ein virtuelles Netzwerk Azure Ressourcenmanager **oder** eine klassische Modell [virtuelles Netzwerk][virtualnetwork], ausgehende Verbindungen von App Service-Umgebung zu anderen Back-End-Ressourcen können ausschließlich über das virtuelle Netzwerk übertragen.  Mit einer aktuellen Änderung im Juni 2016 können Asen auch in virtuellen Netzwerken bereitgestellt werden, die öffentliche Adressbereiche oder Adressräume RFC1918 (d. h. verwenden Private Adressen).  

Z. B. möglicherweise ein SQL Server auf einem Cluster virtuellen Maschinen mit Port 1433 gesperrt.  Der Endpunkt kann ACLd nur von anderen Ressourcen im gleichen virtuellen Netzwerk zugänglich sein.  

Beispielsweise vertrauliche Endpunkte können lokal ausführen und Azure entweder [Zwischen Standorten] an[ SiteToSite] oder [Azure ExpressRoute] [ ExpressRoute] Verbindung.  Folglich werden auf lokale Endpunkte nur Ressourcen virtuelle Netzwerke zwischen Standorten oder ExpressRoute Tunnel verbunden.

Für alle diese Szenarios können apps auf eine App Service-Umgebung sicher auf verschiedenen Servern und Ressourcen verbinden.  Ausgehender Datenverkehr von apps im App Service-Umgebung für private Endpunkte im gleichen virtuellen Netzwerk (oder im gleichen virtuellen Netzwerk verbunden) wird nur über das virtuelle Netzwerk.  Private Endpunkte ausgehender Datenverkehr fließt nicht über das Internet.

Eine Einschränkung gilt für ausgehenden Datenverkehr von einer App Service-Umgebung mit Endpunkten in einem virtuellen Netzwerk.  App Service-Umgebung erreichen nicht Endpunkte von virtuellen Maschinen in **demselben** Subnetz wie die App Service-Umgebung.  Dies sollte normalerweise ein Problem nicht als App Service-Umgebungen in einem Subnetz reserviert exklusiv nur die App Service-Umgebung bereitgestellt werden.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Ausgehende Verbindungen und DNS-Vorschriften ##
Eine App Service-Umgebung funktionieren muss ausgehende Zugriff auf verschiedene Endpunkte. Eine vollständige Liste der externen Endpunkte ein ASE verwendet wird im Abschnitt "Erforderliche Netzwerkkonnektivität" [Netzwerkkonfiguration für ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) Artikel.

App Umgebungen erforderlich gültige DNS-Infrastruktur für das virtuelle Netzwerk konfiguriert.  Wenn aus irgendeinem Grund die DNS-Konfiguration geändert wird, nach dem Erstellen einer App Service-Umgebung können Entwickler eine App Service-Umgebung übernehmen die neue DNS-Konfiguration erzwingen.  Das Symbol "Neustart" am oberen Rand der App Service-Umgebung Management Blade im Portal mit parallelen Umgebung Neustart auslösen verursacht Umgebung neue DNS-Konfiguration übernehmen.

Auch wird empfohlen, dass alle benutzerdefinierten DNS-Server die Vnet rechtzeitig vor dem Erstellen einer App Service-Umgebung werden.  Wenn ein virtuelles Netzwerk DNS-Konfiguration geändert wird, während der Erstellung einer App Service-Umgebung führt, die mit der App Service-Umgebung Erstellung fehlschlägt.  Ähnlich Wenn ein benutzerdefinierter DNS-Server am anderen Ende ein VPN-Gateway und DNS-Server nicht erreichbar oder nicht verfügbar ist ist, fehl der Erstellungsprozess App Service-Umgebung auch.

## <a name="connecting-to-a-sql-server"></a>Herstellen einer Verbindung mit SQL Server
Eine allgemeine SQL Server-Konfiguration hat einen Endpunkt auf Port 1433:

![SQL Server-Endpunkt][SqlServerEndpoint]

Es gibt zwei Ansätze zum Einschränken des Datenverkehrs an diesen Endpunkt aus:


- [Network Access Control Lists] [ NetworkAccessControlLists] (Netzwerk ACLs)

- [Netzwerk-Sicherheitsgruppen][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Einschränken des Zugriffs mit ACL

Port 1433 kann über eine Netzwerk-Zugriffssteuerungsliste geschützt werden.  Beispiel weiße Client Adressen innerhalb eines virtuellen Netzwerks aus und sperrt den Zugriff auf alle anderen Clients.

![Network Access Control Liste Beispiel][NetworkAccessControlListExample]

Alle Programme App Service-Umgebung im gleichen virtuellen Netzwerk als SQL Server die SQL Server-Instanz **VNet interne** IP-Adresse für den virtuellen SQL Server-Computer herstellen können.  

Das Beispiel unten verwiesen wird SQL Server private IP-Adresse.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Auch virtuelle Computer sowie einen öffentlichen Endpunkt verfügt, werden versucht, eine Verbindung mit der öffentlichen IP-Adresse Netzwerk ACL abgelehnt. 

## <a name="restricting-access-with-a-network-security-group"></a>Einschränken des Zugriffs mit einer Netzwerk-Sicherheitsgruppe
Eine Alternative zum Sichern des Zugriffs ist mit einem Netzwerk-Sicherheitsgruppe.  Netzwerk-Sicherheitsgruppen können einzelne virtuelle Maschinen oder ein Subnetz mit virtuellen Computern angewendet werden.

Zuerst muss eine Netzwerk-Sicherheitsgruppe erstellt werden:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Einschränken des Zugriffs auf nur die internen Datenverkehr VNet ist sehr einfach mit einer Netzwerk-Sicherheitsgruppe.  Die Regeln in einer Netzwerk-Sicherheitsgruppe Zugriff nur von anderen Netzwerkclients im gleichen virtuellen Netzwerk.

Daher Zugriff auf SQL Server-Sperren ist so einfach wie eine Netzwerk-Sicherheitsgruppe mit die Standardregeln an beiden virtuellen Computer mit SQL Server oder das Subnetz mit den virtuellen Computern.

Im folgenden Beispiel betrifft eine Netzwerk-Sicherheitsgruppe mit Subnetz:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
Das Endergebnis ist ein Satz von Sicherheitsregeln, die externen Zugriff VNet internen Zugriff blockieren:

![Standard-Netzwerksicherheitsregeln][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Erste Schritte
Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Zunächst mit App-Dienst finden Sie unter [Einführung in App Service-Umgebung][IntroToAppServiceEnvironment]

Details eingehenden Datenverkehr für Ihre App Service-Umgebung steuern finden Sie unter [steuern eingehenden Datenverkehr zu einer App Service-Umgebung][ControlInboundASE]

Weitere Informationen über die Plattform Azure App Service finden Sie in [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
