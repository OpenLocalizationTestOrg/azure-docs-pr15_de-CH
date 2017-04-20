<properties 
   pageTitle="ExpressRoute-Fehlersuche - ARP-Tabellen abrufen | Microsoft Azure"
   description="Diese Seite enthält Hinweise auf ARP Tabellen für eine ExpressRoute-Verbindung"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="ganesr"/>

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>ExpressRoute-Fehlerbehebungshandbuch - erste ARP-Tabellen in der Ressourcen-Manager-Bereitstellungsmodell

> [AZURE.SELECTOR]
[PowerShell - Ressourcen-Manager](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - Klassisch](expressroute-troubleshooting-arp-classic.md)

Dieser Artikel führt Sie durch die Schritte, um die ARP-Tabellen für die ExpressRoute-Verbindung zu. 

>[AZURE.IMPORTANT] Dieses Dokument soll diagnose und einfach beheben. Es dient kein Ersatz für Microsoft Support. Wenn das Problem mit der unten beschriebenen Anleitung können, müssen Sie Support-Ticket mit [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) öffnen.

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresse Resolution Protocol (ARP) und ARP-Tabellen
(ARP = Address Resolution Protocol) ist ein Layer-2-Protokoll in [RFC 826](https://tools.ietf.org/html/rfc826)definiert. ARP wird verwendet, um die Ethernet-Adresse (MAC-Adresse) die IP-Adresse zuordnen.

ARP-Tabelle enthält eine Gegenüberstellung der IPv4-Adresse und MAC-Adresse für einen bestimmten peering. Die ARP-Tabelle für eine ExpressRoute Verbindung peering enthält die folgenden Informationen für jede Schnittstelle (Primär und sekundär)

1. Zuordnung von lokalen Router Schnittstelle IP-Adresse, die MAC-Adresse
2. Zuordnung von ExpressRoute Router-Schnittstelle IP-Adresse, die MAC-Adresse
3. Alter der Zuordnung

ARP-Tabellen können Layer-2-Konfiguration überprüfen und grundlegende Problembehandlung layer 2 Konnektivitätsprobleme. 

Beispiel-ARP-Tabelle: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Die folgenden Informationen auf die ARP-Tabellen ExpressRoute Edge-Router angezeigt Anzeige. 

## <a name="prerequisites-for-learning-arp-tables"></a>Erforderliche Komponenten zum Erlernen von ARP-Tabellen

Sicherstellen Sie, dass Sie Folgendes, bevor Sie weiter

 - Eine gültige ExpressRoute-Verbindung mit mindestens peering konfiguriert. Die Verbindung muss vollständig vom konnektivitätsanbieter konfiguriert werden. Sie (oder Ihr konnektivitätsanbieter) muss mindestens Peerings (Azure private, Azure Public und Microsoft) auf dieser Verbindung konfiguriert haben.
 - IP-Adressbereiche konfiguriert Peerings (Azure private, Azure Public und Microsoft). Überprüfen Sie die IP-Adresse Zuweisung Beispiele [ExpressRoute routing Seite](expressroute-routing.md) zu verstehen, wie Schnittstellen auf der Seite und auf ExpressRoute IP-Adressen zugeordnet werden. Peering Konfiguration informieren [peering Konfigurationsseite ExpressRoute](expressroute-howto-routing-arm.md)überprüfen.
 - Informationen aus Ihrem Netzwerkteam / konnektivitätsanbieter auf die MAC-Adressen der Schnittstellen mit dieser IP-Adressen verwendet.
 - Sie müssen aktuelle PowerShell-Modul für Azure (1,50 oder neuere Version).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Abrufen von ARP Tabellen für ExpressRoute-Verbindung
Dieser Abschnitt beschreibt wie die ARP-Tabellen pro peering mit PowerShell anzeigen können. Sie oder Ihr konnektivitätsanbieter muss vor der weiteren Verarbeitung befassen peering konfiguriert haben. Jede Verbindung verfügt über zwei Pfade (Primär und sekundär). Sie können die ARP-Tabelle für jeden Pfad unabhängig überprüfen.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-Tabellen für Azure private peering
Das folgende Cmdlet enthält ARP Tabellen für Azure private peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Beispielausgabe für einen nachfolgend

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-Tabellen für Azure öffentliche peering
Das folgende Cmdlet enthält ARP Tabellen für Azure öffentliche peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Beispielausgabe für einen nachfolgend

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-Tabellen für Microsoft peering
Das folgende Cmdlet enthält ARP Tabellen für Microsoft peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Beispielausgabe für einen nachfolgend

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Verwendung dieser Informationen
Die ARP-Tabelle von einer peering wird bestimmen Layer-2-Konfiguration und Konnektivität zu überprüfen. In diesem Abschnitt Überblick ARP-Tabellen in verschiedenen Szenarien aussehen.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP-Tabelle wird eine Verbindung im operativen Zustand (erwartet)

 - Die ARP-Tabelle wird ein Eintrag für die lokale Seite mit einer gültigen IP-Adresse und MAC-Adresse und einen solchen Eintrag für die Microsoft-Seite haben. 
 - Das letzte Oktett der lokalen IP-Adresse werden immer eine ungerade Zahl.
 - Das letzte Oktett der IP-Adresse von Microsoft wird immer eine gerade Zahl sein.
 - Dieselbe MAC-Adresse wird auf der Seite Microsoft alle 3 Peerings (primäre / sekundäre) angezeigt. 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP Tabelle, wenn lokale Konnektivität Anbieterseite hat Probleme

 - Nur ein Eintrag wird in der ARP-Tabelle angezeigt. Dies zeigt die Zuordnung zwischen der MAC-Adresse und die IP-Adresse auf der Microsoft-Seite. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Öffnen einer Support-Anforderung Dienstanbieter Konnektivität Probleme Debuggen. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP-Tabelle beim Microsoft Probleme hat

 - Eine ARP-Tabelle für ein peering treten Probleme auf Microsoft wird nicht angezeigt. 
 -  Öffnen einer Support-Ticket mit [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geben Sie an, dass ein mit Layer 2-Konnektivität Problem. 

## <a name="next-steps"></a>Nächste Schritte

 - Layer-3-Konfigurationen für ExpressRoute-Verbindung überprüfen
     - Erhalten Sie Route ermittelt BGP Sitzungen Zusammenfassung 
     - Route-Tabelle bestimmen die Präfixe über ExpressRoute angekündigt
 - Überprüfen Sie Datenübertragung mithilfe von Bytes /
 - Öffnen Sie Support-Ticket mit [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , wenn weiterhin Probleme auftreten.
