<properties 
    pageTitle="Eingehenden Datenverkehr zu einer App Service-Umgebung steuern" 
    description="Konfigurieren von Netzwerksicherheitsregeln eingehenden Datenverkehr zu einer App Service-Umgebung steuern erfahren." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Eingehenden Datenverkehr zu einer App Service-Umgebung steuern

## <a name="overview"></a>Übersicht ##
Eine App Service-Umgebung erstellt werden **oder** ein virtuelles Netzwerk Azure Ressourcenmanager **oder** eine klassische Modell [virtuelles Netzwerk][virtualnetwork].  Ein neues virtuelles Netzwerk und neues Subnetz können gleichzeitig erstellten App Service-Umgebung definiert werden.  Alternativ kann eine App Service-Umgebung vorhandenen virtuellen Netzwerk und vorhandene Subnetz erstellt werden.  Mit einer aktuellen Änderung im Juni 2016 können Asen nun in virtuelle Netzwerke bereitgestellt, mit denen öffentliche Adressbereichen oder Adressräume RFC1918 (d. h. Private Adressen).  Weitere Informationen zum Erstellen einer App Service-Umgebung [Erstellen einer App Service-Umgebung]finden Sie unter[HowToCreateAnAppServiceEnvironment].

App Service-Umgebung muss immer innerhalb eines Subnetzes erstellt werden, da ein Subnetz eine Netzwerkgrenze bietet dem upstream Geräte und Dienste eingehenden Verkehr sperren, HTTP und HTTPS-Datenverkehr nur von bestimmten upstream-IP-Adressen akzeptiert werden können.

Eingehende und ausgehende Netzwerkverkehr in einem Subnetz mit einer [Netzwerk-Sicherheitsgruppe]gesteuert[NetworkSecurityGroups]. Steuerung des eingehenden Datenverkehrs benötigt Netzwerksicherheitsregeln in eine Netzwerk-Sicherheitsgruppe erstellen und Zuweisen von Network Security Gruppe das Subnetz mit der App Service-Umgebung.

Nach einem Subnetz eine Netzwerk-Sicherheitsgruppe zugewiesen wird, eingehender Datenverkehr Apps in der App Service-Umgebung ist zulässig/blockiert basierend auf zulassen und verweigern in der Netzwerk-Sicherheitsgruppe definierte Regeln.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>In einer App Service-Umgebung verwendete Netzwerkports ##
Vor dem Sperren eingehenden Netzwerkdatenverkehr mit einer Netzwerk-Sicherheitsgruppe unbedingt erforderlichen und optionalen verwendete Netzwerkports durch eine App Service-Umgebung kennen.  Versehentlich schließen Sie Datenverkehr auf Ports führt zu Verlust von Funktionalität in einer App Service-Umgebung.

Folgendes ist eine Liste der Ports, die von einer App Service-Umgebung. Alle Ports sind **TCP**, soweit deutlich:

- 454: **erforderliche Port** von Azure-Infrastruktur für die Verwaltung und Pflege der App Service-Umgebungen über SSL verwendet.  Verkehr zu diesem Port nicht blockiert.  Dieser Port ist immer an die öffentliche VIP ein ASE gebunden.
- 455: **erforderliche Port** von Azure-Infrastruktur für die Verwaltung und Pflege der App Service-Umgebungen über SSL verwendet.  Verkehr zu diesem Port nicht blockiert.  Dieser Port ist immer an die öffentliche VIP ein ASE gebunden.
- 80: default Port für HTTP-Datenverkehr Apps in App Service-Pläne in einer App Service-Umgebung ausgeführt.  Dieser Port ist ein ASE ILB aktiviert an ILB ASE gebunden.
- 443: default Port eingehenden SSL-Datenverkehr Apps in App Service-Pläne in einer App Service-Umgebung ausgeführt.  Dieser Port ist ein ASE ILB aktiviert an ILB ASE gebunden.
- 21: Kanal für FTP.  Dieser Port kann sicher gesperrt, wenn FTP nicht verwendet wird.  Auf eine ASE ILB aktiviert kann diesen Port für eine ASE an ILB gebunden.
- 990: Kanal für FTP.  Dieser Port kann sicher gesperrt, wenn FTP nicht verwendet wird.  Auf eine ASE ILB aktiviert kann diesen Port für eine ASE an ILB gebunden.
- 10001 10020: Kanäle für FTP.  Mit der Steuerungskanal können sicher diese Ports blockiert werden, wenn FTP nicht verwendet wird.  Auf eine ASE ILB aktiviert kann diesen Anschluss an die ASE ILB Adresse gebunden.
- 4016: remote Debuggen mit Visual Studio 2012 verwendet.  Dieser Port kann sicher blockiert werden, wenn die Funktion nicht verwendet wird.  Dieser Port ist ein ASE ILB aktiviert an ILB ASE gebunden.
- 4018: für das Remotedebugging mit Visual Studio 2013 verwendet.  Dieser Port kann sicher blockiert werden, wenn die Funktion nicht verwendet wird.  Dieser Port ist ein ASE ILB aktiviert an ILB ASE gebunden.
- 4020: remote Debuggen mit Visual Studio 2015 verwendet.  Dieser Port kann sicher blockiert werden, wenn die Funktion nicht verwendet wird.  Dieser Port ist ein ASE ILB aktiviert an ILB ASE gebunden.

## <a name="outbound-connectivity-and-dns-requirements"></a>Ausgehende Verbindungen und DNS-Vorschriften ##
Eine App Service-Umgebung funktionieren muss ausgehende Zugriff auf verschiedene Endpunkte. Eine vollständige Liste der externen Endpunkte ein ASE verwendet wird im Abschnitt "Erforderliche Netzwerkkonnektivität" [Netzwerkkonfiguration für ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) Artikel.

App Umgebungen erforderlich gültige DNS-Infrastruktur für das virtuelle Netzwerk konfiguriert.  Wenn aus irgendeinem Grund die DNS-Konfiguration geändert wird, nach dem Erstellen einer App Service-Umgebung können Entwickler eine App Service-Umgebung übernehmen die neue DNS-Konfiguration erzwingen.  Auslösen eines parallelen Umgebung Neustarts mit dem Symbol "Neustart" am oberen Rand der App Service-Umgebung Management Blade in [Azure-Portal] [ NewPortal] Umgebung übernehmen die neue DNS-Konfiguration verursacht.

Auch wird empfohlen, dass alle benutzerdefinierten DNS-Server die Vnet rechtzeitig vor dem Erstellen einer App Service-Umgebung werden.  Wenn ein virtuelles Netzwerk DNS-Konfiguration geändert wird, während der Erstellung einer App Service-Umgebung führt, die mit der App Service-Umgebung Erstellung fehlschlägt.  Ähnlich Wenn ein benutzerdefinierter DNS-Server am anderen Ende ein VPN-Gateway und DNS-Server nicht erreichbar oder nicht verfügbar ist ist, fehl der Erstellungsprozess App Service-Umgebung auch.

## <a name="creating-a-network-security-group"></a>Erstellen eine Netzwerk-Sicherheitsgruppe ##
Einzelheiten wie Netzwerk arbeiten Sicherheitsgruppen die folgenden [Informationen]finden Sie unter[NetworkSecurityGroups].  Die Details unten auf highlights netzwerksicherheitsgruppen mit Schwerpunkt auf Konfiguration und einem Subnetz, die App Service-Umgebung enthält eine Netzwerk-Sicherheitsgruppe zuweisen.

**Hinweis:** Netzwerk-Sicherheitsgruppen grafisch mit der [Azure-Portal](https://portal.azure.com) konfiguriert oder Azure PowerShell.

Netzwerk-Sicherheitsgruppen entstehen zunächst als eigenständige Entität ein Abonnement zugeordnet. Da netzwerksicherheitsgruppen in Azure-Region erstellt wurden, sicherstellen Sie, dass die Netzwerk-Sicherheitsgruppe im Bereich der App Service-Umgebung erstellt wird.

Das folgende Beispiel veranschaulicht, eine Netzwerk-Sicherheitsgruppe erstellen:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Nach eine Netzwerk-Sicherheitsgruppe erstellt wird, werden ein oder mehrere Netzwerksicherheitsregeln hinzugefügt.  Da die Regeln mit der Zeit ändern können, wird empfohlen, Prioritäten zu einfach zusätzliche Regeln mit der Zeit zum Nummerierungsschema Raum.

Das folgende Beispiel zeigt eine Regel, die explizit gewährt Zugriff auf die Management-Ports von Azure-Infrastruktur verwalten und Warten einer App Service-Umgebung benötigt.  Beachten Sie, dass alle Verwaltungsdatenverkehr über SSL und von Clientzertifikaten gesichert, obwohl die Ports geöffnet werden von allen anderen Azure-Infrastruktur nicht verfügbar sind.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Beim Zugriff auf port 80 und 443 "App Service-Umgebung upstream Geräte oder Dienste ausblenden", müssen Sie die upstream IP-Adresse.  Beispielsweise bei Verwendung eine Web Application Firewall (AAF) die WAF müssen eigene IP-Adresse (oder Adressen) bei Verwendung als Proxy für Datenverkehr zu einer nachgelagerten App Service-Umgebung.  Sie müssen diese IP-Adresse im Parameter *SourceAddressPrefix* eine Netzwerkregel Sicherheit verwenden.

Im folgenden Beispiel wird Datenverkehr von einer bestimmten upstream-IP-Adresse explizit zugelassen.  Adresse *1.2.3.4* dient als Platzhalter für die IP-Adresse des upstream WAF.  Ändern Sie den Wert entsprechend die Adresse vom übergeordneten Gerät oder Service.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
Ggf. FTP-Unterstützung können folgenden Regeln gewähren Zugriff auf die FTP-Steuerungsport und Data Channel-Ports als Vorlage verwendet werden.  Da FTP eine statusbehaftete Protokoll, nicht für FTP-Datenverkehr über eine herkömmliche HTTP/HTTPS-Firewall oder einen Proxyserver möglicherweise.  In diesem Fall müssen Sie legen Sie *SourceAddressPrefix* auf einen anderen Wert – z. B. den IP-Adressbereich Developer oder Deployment auf der FTP-Clients ausgeführt werden. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Hinweis:** Data Channel-Portbereich möglicherweise während der Probezeit ändern.)

Bei remote Debuggen mit Visual Studio veranschaulichen die folgenden Regeln gewähren.  Da jede Version einen anderen Anschluss für das Remotedebugging verwendet, besteht eine gesonderte Regel für jede unterstützte Version von Visual Studio.  Mit FTP-Zugriff können remote debugging nicht korrekt über einen herkömmlichen WAF oder Proxygerät Verkehrsfluss.  *SourceAddressPrefix* kann stattdessen auf den IP-Adressbereich von Visual Studio Entwicklungscomputern festgelegt werden.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Ein Subnetz zuweisen eine Netzwerk-Sicherheitsgruppe ##
Eine Netzwerk-Sicherheitsgruppe hat eine Standardregel Sicherheit, die Zugriff auf alle externen Datenverkehr verweigert.  Das Ergebnis aus der Kombination der oben beschriebenen Netzwerksicherheitsregeln und Standard-Sicherheitsregel eingehenden Datenverkehr blockiert wird, dass nur Datenverkehr von Quell-Adresse zugeordnete Aktion *Zulassen* Bereiche Webverkehr apps in einer App Service-Umgebung ausgeführt werden.

Nachdem eine Netzwerk-Sicherheitsgruppe mit Sicherheitsregeln gefüllt wird, soll an das Subnetz mit der App Service-Umgebung zugewiesen werden.  Der Befehl Aufgabe verweist sowohl den Namen des virtuellen Netzwerks, in dem die App Service-Umgebung befindet, sowie den Namen des Teilnetzes, in dem die App Service-Umgebung erstellt wurde.  

Das folgende Beispiel zeigt eine Netzwerk-Sicherheitsgruppe, Subnetz virtuelles Netzwerk zugewiesen:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Sobald die Netzwerk Sicherheit Zuordnung erfolgreich die Zuordnung ist eine lang andauernde Vorgänge und kann einige Minuten dauern nur eingehender Datenverkehr *Zulassen* Regeln erfolgreich apps in der App Service-Umgebung erreichen.

Vollständigkeit veranschaulicht das folgende Beispiel entfernen und somit-Netzwerk-Sicherheitsgruppe aus dem Subnetz:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Besonderheiten bei expliziter IP-SSL ##
Eine Anwendung mit einer expliziten IP-SSL konfiguriert ist (für Asen, die nur eine öffentliche VIP) Adresse, anstatt die standardmäßige IP-Adresse der App Service-Umgebung, HTTP und HTTPS Datenverkehr fließt in das Subnetz über einen anderen Satz von Ports außer Port 80 und Port 443.

Einzelne Paar von jeder IP-SSL-Adresse verwendeten Ports finden in der Portal-Benutzeroberfläche die App Service-Umgebung Details UX Blatt.  Wählen Sie "Alle suchen"--> "Adressen".  "IP-Adressen" Blade zeigt eine Tabelle aller explizit konfigurierte SSL IP-Adressen für die App Service-Umgebung mit speziellen Port-Paar, die für HTTP- und HTTPS-Datenverkehr jedes SSL-IP-Adresse verwendet wird.  Es handelt sich Port, die beim Konfigurieren von Regeln in einer Netzwerk-Sicherheitsgruppe für die DestinationPortRange-Parameter verwendet werden.

App ein ASE konfiguriert IP-SSL verwenden, externe Kunden sehen und nicht die speziellen Port-Paar Zuordnung kümmern müssen.  Die Apps wird normalerweise an IP-SSL-konfigurierten Verkehrsfluss.  Die Übersetzung zu den speziellen Port automatisch intern bei der endgültigen routing des Datenverkehrs mit ASE Subnet. 

## <a name="getting-started"></a>Erste Schritte

Zunächst mit App-Dienst finden Sie unter [Einführung in App Service-Umgebung][IntroToAppServiceEnvironment]

Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Details zum Herstellen einer sicheren Verbindung zum Back-End-Ressource von einer App Service-Umgebung apps finden Sie unter [Herstellen einer sicheren Verbindung zum Back-End-Ressourcen aus einer App Service-Umgebung][SecurelyConnecttoBackend]

Weitere Informationen über die Plattform Azure App Service finden Sie in [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
