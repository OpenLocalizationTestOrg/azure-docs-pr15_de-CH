<properties 
    pageTitle="Mehrschichtige Sicherheitsarchitektur mit App Service" 
    description="Implementieren eine mehrschichtige Sicherheitsarchitektur App Service-Umgebungen." 
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
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Implementieren eine mehrschichtige Sicherheitsarchitektur mit App Service

## <a name="overview"></a>Übersicht ##
 
Da App Service-Umgebungen in einem virtuellen Netzwerk bereitgestellt, eine isolierte Runtime Umgebung bereitstellen, können Entwickler eine mehrschichtige Sicherheit-Architektur bietet verschiedene Ebenen des Netzwerkzugriffs für jede physische Anwendungsebene erstellen.

Wunsch ist API-Back-Ends von allgemeinen Internetzugang ausblenden und nur APIs von übergeordneten webapps aufgerufen werden.  [Netzwerk-Sicherheitsgruppen (NSGs)] [ NetworkSecurityGroups] wird in Subnetzen App Service-Umgebungen mit öffentlichen Zugriff auf API-Applikationen.

Das Diagramm unten zeigt eine Beispiel-Architektur mit einer WebAPI Basis app auf einer App Service-Umgebung bereitgestellt.  Drei separate Web app-Instanzen, auf drei separate App Service Umgebung bereitgestellt Anrufe Back-End mit der gleichen WebAPI Anwendung.

![Grundlegende Architektur][ConceptualArchitecture] 

Grüne Pluszeichen anzugeben, dass der Netzwerk-Sicherheitsgruppe im Subnetz mit "Apiase" eingehende Anrufe aus upstream Web apps als auch Anrufe von selbst ermöglicht.  Die gleiche Netzwerk-Sicherheitsgruppe verweigert jedoch explizit Zugriff auf allgemeine eingehenden Datenverkehr aus dem Internet. 

Dieses Thema geht über die Schritte zur Konfiguration der Netzwerk-Sicherheitsgruppe Subnetz mit "Apiase".

## <a name="determining-the-network-behavior"></a>Das Netzwerk Verhalten ##
Um zu wissen, welche Netzwerksicherheitsregeln erforderlich sind, müssen Sie bestimmen, welche Netzwerkclients darf die App Service-Umgebung mit API-app erreichen und welche Clients blockiert werden.

Da [Netzwerk-Sicherheitsgruppen (NSGs)] [ NetworkSecurityGroups] gelten für Subnetze App Service-Umgebungen in Subnetze bereitgestellt werden, in eine NSG enthaltenen Regeln gelten für **Alle** apps in einer App Service-Umgebung ausgeführt.  Mit der Beispielarchitektur für diesen Artikel nach eine Netzwerk-Sicherheitsgruppe an das Subnetz mit "Apiase" angewendet wird, werden alle apps unter "Apiase" App Service-Umgebung über denselben Satz von Sicherheitsregeln geschützt. 

- **Bestimmen die ausgehende IP-Adresse des upstream Aufrufer:**  Was ist die IP-Adresse des upstream Aufrufer?  Diese Adressen müssen explizit in der NSG zugreifen.  Da Aufrufe zwischen App Service-Umgebungen "Internetanrufe" gelten, bedeutet dies, dass die ausgehende IP-Adresse jedes der drei upstream App Service muss in NSG für "Apiase" Subnetz zugreifen zugewiesen.   Weitere Informationen zum Ermitteln der ausgehenden IP-Adresse apps im App Service-Umgebung [Netzwerkarchitektur] finden Sie[ NetworkArchitecture] Artikel.
- **Muss die Backend-API-app selbst aufrufen?**  Manchmal übersehener und Subtiler Punkt ist das Szenario, Back-End-Anwendung selbst aufrufen.  Wenn eine Backend-API-Anwendung eine App Service-Umgebung sich selbst aufruft muss, wird dies auch behandelt wie ein Aufruf von "Internet".  In der Beispielarchitektur erforderlich, der "Apiase" App Service-Umgebung sowie der ausgehenden IP-Adresse zulassen.

## <a name="setting-up-the-network-security-group"></a>Netzwerk-Sicherheitsgruppe einrichten ##
Sobald der ausgehenden IP-Adressen bezeichnet werden, besteht der nächste Schritt eine Netzwerk-Sicherheitsgruppe erstellen.  Netzwerk-Sicherheitsgruppen können für beide Ressourcen-Manager-virtuelle Netzwerke sowie klassische virtuelle Netzwerke basierte erstellt werden.  Den folgenden Beispielen erstellen und Konfigurieren einer NSG in einem klassischen virtuellen Netzwerk mithilfe von Powershell.

Beispiel-Architektur der Umgebung südlichen zentralen USA, befinden sich in eine leere NSG in diesem Bereich erstellt:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Explizit zulassen, Regel Azure Management-Infrastruktur wie im Artikel auf [eingehenden Datenverkehr] hinzugefügt[ InboundTraffic] für App-Dienst.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Anschließend werden zwei Regeln HTTP und HTTPS Aufrufe der ersten upstream App Service-Umgebung ("fe1ase") hinzugefügt.

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Spülen und wiederholen für die zweite und dritte upstream App Service ("fe2ase" und "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Abschließend gewähren Sie ausgehende IP-Adresse des Back-End-API App Service-Umgebung, damit sie wieder in sich selbst aufrufen kann.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Keine andere Netzwerksicherheitsregeln müssen konfiguriert werden, da jede NSG Standard Regeln verfügt, die eingehenden aus dem Internet Verkehr standardmäßig.

Die vollständige Liste der Regeln in der Netzwerk-Sicherheitsgruppe werden unten angezeigt.  Beachten Sie, wie die letzte Regel hervorgehoben, eingehenden Datenverkehr von allen Aufrufern nicht blockiert die explizit Zugriff gewährt wurde.

![NSG Konfiguration][NSGConfiguration] 

Der letzte Schritt ist das Subnetz die NSG zuweisen, die App Service-Umgebung "Apiase" enthält.  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Mit der NSG angewendet auf das Subnetz dürfen nur die drei upstream App Service-Umgebungen und die App Service-Umgebung mit API-Back-End in der Umgebung "Apiase" aufrufen.


## <a name="additional-links-and-information"></a>Zusätzliche Links und Informationen ##
Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Informationen zu [Netzwerk-Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md). 

Grundlegendes zu [ausgehenden IP-Adressen] [ NetworkArchitecture] und App-Dienst.

[Netzwerk-Ports] [ InboundTraffic] App Service-Umgebungen verwendet.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
