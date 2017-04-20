<properties
   pageTitle="ExpressRoute-Fehlerbehebung: Abrufen von ARP-Tabellen | Microsoft Azure"
   description="Diese Seite enthält eine Anleitung zum Abrufen des ARP Tabellen für eine ExpressRoute-Verbindung."
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

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>ExpressRoute-Fehlerbehebung: erste ARP Tabellen im klassischen Bereitstellungsmodell

> [AZURE.SELECTOR]
[PowerShell - Ressourcen-Manager](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - Klassisch](expressroute-troubleshooting-arp-classic.md)

Dieser Artikel führt Sie durch die Schritte zum Abrufen der Tabellen (ARP = Address Resolution Protocol) für Ihre Azure ExpressRoute-Verbindung.

>[AZURE.IMPORTANT] Dieses Dokument soll diagnose und einfach beheben. Es dient kein Ersatz für Microsoft Support. Wenn Sie das Problem mithilfe der folgenden Anleitung lösen können, öffnen Sie eine Anfrage mit [Microsoft Azure-Hilfe und Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresse Resolution Protocol (ARP) und ARP-Tabellen
ARP ist ein Layer 2-Protokoll, die in [RFC 826](https://tools.ietf.org/html/rfc826)definiert. ARP ist eine Ethernet-Adresse (MAC-Adresse) einer IP-Adresse zuordnen verwendet.

Eine ARP-Tabelle enthält eine Gegenüberstellung der IPv4-Adresse und MAC-Adresse für einen bestimmten peering. Die ARP-Tabelle für eine ExpressRoute Verbindung peering enthält die folgenden Informationen für jede Schnittstelle (Primär und sekundär):

1. Zuordnung von lokalen Router Schnittstelle IP-Adresse eine MAC-Adresse
2. Zuordnung von ExpressRoute Router-Schnittstelle IP-Adresse eine MAC-Adresse
3. Das Alter der Zuordnung

ARP-Tabellen können mit Layer-2-Konfiguration überprüfen und Problembehandlung bei grundlegenden Layer 2-Konnektivität.

Es folgt ein Beispiel einer ARP-Tabelle:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Der folgende Abschnitt enthält Informationen an ExpressRoute Edge-Router gelten die ARP-Tabellen.

## <a name="prerequisites-for-using-arp-tables"></a>Voraussetzung für die Verwendung von ARP-Tabellen

Sicherstellen Sie, dass Sie Folgendes, bevor Sie fortfahren:

 - Eine gültige ExpressRoute-Verbindung, die mit mindestens peering konfiguriert ist. Die Verbindung muss vollständig vom konnektivitätsanbieter konfiguriert werden. Sie (oder Ihr konnektivitätsanbieter) muss mindestens Peerings (Azure private, Azure Public oder Microsoft) auf dieser Verbindung konfigurieren.

 - IP-Adressbereiche, die zum Konfigurieren von Peerings (Azure private, Azure Public und Microsoft). Überprüfen Sie die IP-Adresse Zuweisung Beispiele [ExpressRoute routing Seite](expressroute-routing.md) zu verstehen, wie Schnittstellen in Ihre Aise und ExpressRoute auf IP-Adressen zugeordnet werden. Informationen zur Konfiguration peering erhalten [peering Konfigurationsseite ExpressRoute](expressroute-howto-routing-classic.md)überprüfen.

 - Informationen von Ihrem Netzwerk Team oder Konnektivität MAC-Adressen der Schnittstellen, die mit dieser IP-Adressen verwendet werden.

 - Das neueste Windows PowerShell-Modul für Azure (ab Version 1.50).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP-Tabellen für die ExpressRoute-Verbindung
Dieser Abschnitt enthält Hinweise auf die ARP-Tabellen für jeden mithilfe von PowerShell peering anzeigen. Bevor Sie fortfahren, muss Sie oder Dienstanbieter Konnektivität der peering konfigurieren. Jede Verbindung verfügt über zwei Pfade (Primär und sekundär). Sie können die ARP-Tabelle für jeden Pfad unabhängig überprüfen.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-Tabellen für Azure private peering
Das folgende Cmdlet enthält ARP Tabellen für Azure private peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Beispielausgabe für einen folgt:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-Tabellen für Azure öffentliche peering
Das folgende Cmdlet enthält ARP Tabellen für Azure öffentliche peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Beispielausgabe für einen folgt:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Beispielausgabe für einen folgt:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-Tabellen für Microsoft peering
Das folgende Cmdlet enthält ARP Tabellen für Microsoft peering:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Beispielausgabe für einen unten angezeigt:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Verwendung dieser Informationen
Die ARP-Tabelle von einer peering lässt sich Layer-2-Konfiguration und Konnektivität zu überprüfen. Dieser Abschnitt enthält einen Überblick über die Darstellung von ARP-Tabellen in verschiedenen Szenarien.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>ARP-Tabelle wird ein Schaltkreis in einen betriebsbereiten Zustand (erwartet)

 - Die ARP-Tabelle hat einen Eintrag für die lokale Seite mit einer gültigen IP- und MAC-Adresse und einen solchen Eintrag für die Microsoft-Seite.
 - Das letzte Oktett der lokalen IP-Adresse ist immer eine ungerade Zahl.
 - Das letzte Oktett der Microsoft IP Adresse ist immer eine gerade Zahl.
 - Dieselbe MAC-Adresse wird auf der Seite Microsoft für alle drei Peerings (primär/Sekundär).


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>ARP-Tabelle wird lokal oder wenn der Konnektivität Anbieterseite Probleme

 In der ARP-Tabelle wird nur ein Eintrag angezeigt. Es zeigt die Zuordnung zwischen der MAC-Adresse und die IP-Adresse, die auf der Microsoft-Seite verwendet wird.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Wenn ein Problem auftritt, öffnen Sie eine Anfrage mit Ihrem konnektivitätsanbieter beheben.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>Wenn die Microsoft-Seite Probleme hat ARP-Tabelle

 - Eine ARP-Tabelle für ein peering treten Probleme auf Microsoft wird nicht angezeigt.
 -  Öffnen Sie eine Anfrage mit [Microsoft Azure-Hilfe und Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geben Sie an, dass ein mit Layer 2-Konnektivität Problem.

## <a name="next-steps"></a>Nächste Schritte

 - Überprüfen Sie Layer-3-Konfigurationen für ExpressRoute-Verbindung:
     - Abrufen einer Route Zusammenfassung BGP Sitzungen ermittelt.
     - Erhalten Sie eine Routentabelle bestimmen die Präfixe über ExpressRoute bekannt gegeben werden.
 - Überprüfen Sie Datenübertragung und Bytes überprüfen.
 - Öffnen Sie per mit [Microsoft Azure-Hilfe und Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , wenn weiterhin Probleme auftreten.
