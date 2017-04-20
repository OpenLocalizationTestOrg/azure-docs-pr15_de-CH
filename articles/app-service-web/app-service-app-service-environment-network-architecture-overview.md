<properties 
    pageTitle="Netzwerk Architekturübersicht App Service-Umgebung" 
    description="Übersicht über die Architektur der Netzwerk-Topologie OfApp Umgebungen." 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>Netzwerk Architekturübersicht App Service-Umgebung

## <a name="introduction"></a>Einführung ##
App Service-Umgebungen immer innerhalb eines Subnetzes ein [virtuelles Netzwerk] erstellt[ virtualnetwork] -apps im App Service-Umgebung mit privaten Endpunkte innerhalb derselben virtuellen Netzwerktopologie kommunizieren können.  Da Kunden Teile ihrer virtuellen Infrastruktur sperren können, ist es wichtig, die Datenflüsse Kommunikation kennen, die mit einer App Service-Umgebung auftreten.

## <a name="general-network-flow"></a>Allgemein-Datenfluss ##
 
Wenn eine App Service-Umgebung (ASE) eine öffentliche virtuelle IP-Adresse (VIP) für apps verwendet, wird alle eingehender Datenverkehr, öffentliche VIP.  Umfasst HTTP- und HTTPS-Datenverkehr für apps und anderen Datenverkehr für FTP, remote Debugfunktionen und Azure Verwaltungsvorgänge.  Eine vollständige Liste der bestimmte Ports (erforderlich und optional), die auf die öffentliche VIP finden auf [eingehenden Datenverkehr] [ controllinginboundtraffic] einer App Service-Umgebung. 

App Service-Umgebung unterstützen auch ausgeführten apps, die nur für eine virtuelle interne Netzwerkadresse, auch als ILB (interne Lastenausgleich) Adresse gebunden sind.  Auf einer ILB aktiviert ASE, HTTP und HTTPS-Datenverkehr für apps sowie remote Debuggen, kommen die Adresse ILB.  Häufigste ILB ASE Konfigurationen kommen FTP-FTP-Datenverkehr auch ILB-Adresse.  ASE aktiviert Azure Verwaltungsvorgänge noch die öffentliche VIP ein ILB Ports 454/455 fließen jedoch.

Das Diagramm zeigt einen Überblick über die verschiedenen ein-und ausgehenden Flows für eine App Service-Umgebung, die apps öffentliche virtuelle IP-Adresse gebunden sind:

![Allgemeine Datenflüssen][GeneralNetworkFlows]

Eine App Service-Umgebung kann mit einer Vielzahl von Privatkunden Endpunkte kommunizieren.  Beispielsweise können apps im App Service-Umgebung mit IaaS virtuellen Computer im gleichen virtuellen Netzwerktopologie unter Datenbankserver verbinden.

>[AZURE.IMPORTANT] Netzplandiagramm betrachtet werden die "andere Compute-Ressourcen" in einem anderen Subnetz als die App Service-Umgebung bereitgestellt. Bereitstellen von Ressourcen im selben Subnetz mit ASE blockiert Konnektivität von ASE Ressourcen (außer bestimmten Intra-ASE routing). Stattdessen bereitstellen Sie in ein anderes Subnetz (in demselben VNET). Die App Service-Umgebung werden dann eine Verbindung herstellen. Es ist keine weitere Konfiguration erforderlich.

App Service-Umgebung kommunizieren auch mit Sql-Datenbank und Azure-Speicher für die Verwaltung und den Betrieb einer App Service-Umgebung erforderlichen Ressourcen.  Sql und Speicher-Ressourcen, mit denen eine App Service-Umgebung kommuniziert befinden sich in der gleichen Region wie die App Service-Umgebung, während andere in Azure Regionen befinden.  Ausgehende Internet-Verbindung ist daher immer für eine App Service-Umgebung funktionieren. 

Da eine App Service-Umgebung in einem Subnetz bereitgestellt wird, können netzwerksicherheitsgruppen eingehenden Datenverkehr an das Subnetz verwendet werden.  Wie eingehende Datenverkehr zu einer App Service-Umgebung finden Sie im folgenden [Artikel][controllinginboundtraffic].

Informationen zu ausgehenden Internetkonnektivität von App Service-Umgebung finden Sie im folgenden Artikel arbeiten mit [Express][ExpressRoute].  In diesem Artikel beschriebene Vorgehensweise gilt, wenn Standort-zu-Standort-Verbindung verwenden und müssen mit tunneling.

## <a name="outbound-network-addresses"></a>Ausgehende Netzwerkadressen ##
App Service-Umgebung ausgehende aufruft, wird eine IP-Adresse immer ausgehende Anrufe zugeordnet.  Die spezifische IP-Adresse hängt davon ab, ob der Endpunkt aufgerufen wird innerhalb der virtuellen Netzwerktopologie oder außerhalb der virtuellen Netzwerktopologie ist.

Endpunkt aufgerufen wird ist **außerhalb** der virtuellen Netzwerktopologie, dann werden ausgehende Adresse (aka die ausgehende Adresse NAT) öffentliche VIP App Service-Umgebung.  Diese Adresse kann in der Portal-Benutzeroberfläche für die App Service-Umgebung Eigenschaften Blatt gefunden.
 
![Ausgehende IP-Adresse][OutboundIPAddress]

Diese Adresse kann auch für Asen bestimmt werden, die nur eine öffentliche VIP app in App Service-Umgebung erstellen und anschließend eine *Nslookup* die app-Adresse ausführen. Die resultierende Adresse ist sowohl öffentliche VIP sowie die App Service-Umgebung ausgehende NAT-Adresse.

Ist der Endpunkt aufgerufen wird **innerhalb** der virtuellen Netzwerktopologie, werden ausgehende Adresse der aufrufenden Anwendung interne IP-Adresse der einzelnen computeressource die Anwendung.  Es ist jedoch keine permanente Zuordnung von virtuellen internen IP-Netzwerkadressen apps.  Apps können über verschiedene Serverressourcen und den Pool der verfügbaren Compute, Ressourcen in einer App Service-Umgebung durch Skalierung Vorgänge ändern können, bewegen.

Da App Service-Umgebung immer in einem Subnetz befindet, ist Sie jedoch garantiert, dass die interne IP-Adresse einer Compute-Ressource mit einer Anwendung immer CIDR außerhalb des Subnetzes liegen.  Daher bei abgestimmte ACLs oder netzwerksicherheitsgruppen Zugang zu anderen Endpunkte innerhalb des virtuellen Netzwerks muss Teilnetzes enthält die App Service-Umgebung zugreifen.

Das folgende Diagramm zeigt diese Konzepte ausführlicher:

![Ausgehende Netzwerkadressen][OutboundNetworkAddresses]

In der obigen Abbildung:

- Da die öffentliche VIP App Service-Umgebung 192.23.1.2 ist, ist die ausgehende IP-Adresse "Internet" Endpunkte aufrufen.
- Die CIDR mit Subnetz für die App Service-Umgebung ist 10.0.1.0/26.  Aufrufe von apps sehen andere Endpunkte innerhalb derselben virtuellen Netzwerkinfrastruktur als irgendwo innerhalb dieser Adressbereich aus.

## <a name="calls-between-app-service-environments"></a>Aufrufe zwischen App Service ##
Ein komplexeres Szenario kann auftreten, wenn mehrere App Service-Umgebungen im gleichen virtuellen Netzwerk bereitstellen und ausgehende von einer App Service-Umgebung in eine andere App Service-Umgebung Aufrufe.  Diese Art von cross App Service-Umgebung Aufrufe auch als "Internet" Aufrufe behandelt werden.

Das folgende Diagramm zeigt ein Beispiel für eine mehrstufige Architektur mit apps auf eine App Service-Umgebung (z. B. "Vordertür" webapps) apps für eine zweite App Service-Umgebung (z.B. interne Backend-API-apps nicht aus dem Internet zugegriffen werden soll) aufrufen. 

![Aufrufe zwischen App Service][CallsBetweenAppServiceEnvironments] 

Im obigen Beispiel hat die App Service-Umgebung "ASE One" ausgehende IP-Adresse 192.23.1.2.  Wenn eine Anwendung auf diese behandelt App Service-Umgebung macht ein ausgehender Aufruf eine app auf einer zweiten App Service Umgebung ("ASE zwei") im gleichen virtuellen Netzwerk Ausgehender Anruf gespeichert werden als ein Aufruf "Internet".  Daher den Netzwerkverkehr auf die zweite App Service-Umgebung wird angezeigt als 192.23.1.2 (d. h. nicht den Subnetz Adressbereich des ersten App Service-Umgebung) aus.

Obwohl beide App Service-Umgebungen in derselben Azure-Region befinden, Netzwerkverkehr regionalen Azure Netzwerk bleiben und fließt nicht physisch über das öffentliche Internet, Aufrufe zwischen verschiedenen App Service wie "Internet" Aufrufe behandelt werden.  Dadurch können Sie eine Netzwerk-Sicherheitsgruppe Subnetz die zweite App Service-Umgebung nur eingehende Anrufe aus der ersten App Service Umgebung (ausgehende IP-Adresse 192.23.1.2 ist), damit sichere Kommunikation zwischen der App-Dienst zulassen.

## <a name="additional-links-and-information"></a>Zusätzliche Links und Informationen ##
Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Informationen über eingehende Ports von App Service-Umgebungen verwendet und mit netzwerksicherheitsgruppen eingehenden Datenverkehr steuern steht [hier][controllinginboundtraffic].

Details zur Verwendung von Benutzer definierten Routen ausgehenden Internetzugriff App Service-Umgebungen ist in diesem [Artikel][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

