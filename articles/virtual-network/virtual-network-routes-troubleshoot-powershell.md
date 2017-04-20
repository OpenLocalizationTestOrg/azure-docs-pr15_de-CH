<properties 
   pageTitle="Problembehandlung bei Routen - PowerShell | Microsoft Azure"
   description="Informationen Sie zum Routen in der Azure-Ressourcen-Manager-Bereitstellungsmodell mit Azure PowerShell zu beheben."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-routes-using-azure-powershell"></a>Problembehandlung bei Verwendung von Azure PowerShell Routen

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-routes-troubleshoot-portal.md)
- [PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Wenn Sie Probleme mit der Netzwerkkonnektivität zu oder von Ihrem Azure Virtual Machine (VM) haben, möglicherweise Routen der VM Verkehrswerte beeinträchtigen. Dieser Artikel bietet eine Übersicht über Diagnosefunktionen für Routen zu diagnostizieren.

Routetabellen Subnetzen zugeordnet sind und gelten für alle Netzwerkschnittstellen (NIC) in diesem Subnetz. Die folgenden Arten von Routen können jede Netzwerkschnittstelle zugewiesen werden:

- **Systemrouten:** Standardmäßig verfügt jedes Subnetz eine Azure Virtual Network (VNet) erstellt Route-Systemtabellen, die lokalem VNet lokalen Datenverkehr über VPN-Gateways und Internet-Datenverkehr zulassen. Systemrouten sind auch für hervorragendem VNets vorhanden.
- **BGP-Routen:** Netzwerk-Schnittstellen durch ExpressRoute oder Standort-zu-Standort-VPN-Verbindung übertragen. Erfahren Sie mehr über BGP routing von [BGP mit VPN-Gateways](../vpn-gateway/vpn-gateway-bgp-overview.md) und [ExpressRoute Übersicht über](../expressroute/expressroute-introduction.md) Artikel lesen.
- **Benutzerdefinierte Routen (UDR):** Bei Verwendung virtueller Netzwerkgeräte oder sind erzwungene Tunneln Datenverkehr zu einem lokalen Netzwerk über ein VPN-Standort-zu-Standort möglicherweise benutzerdefinierte Routen (Decision) die Routingtabelle Subnetz zugeordnet. Wenn Sie Decision Artikel [benutzerdefinierte Routen](virtual-networks-udr-overview.md#user-defined-routes) nicht kennen.

Mit verschiedenen Routen, die einer Schnittstelle zugewiesen werden können, kann es schwierig sein festzustellen welche aggregate wirksam sind. Um VM-Netzwerkkonnektivität beheben, die effektiven Routen für eine Netzwerkschnittstelle Bereitstellungsmodell Azure-Ressourcen-Manager angezeigt.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Verwendung von effektiven Routen VM-Datenverkehr

In diesem Artikel verwendet das folgende Szenario als Beispiel, wie effektiven Routen für eine Netzwerkschnittstelle beheben:

VM (*VM1*) mit den VNet verbunden (*VNet1*, Präfix: 10.9.0.0/16) keine Verbindung zu einer VM(VM3) neu hervorragendem VNet (*VNet3*, Präfix 10.10.0.0/16). Gibt keine Decision oder BGP leitet der VM verbundenen Netzwerkschnittstelle VM1 NIC1 angewendet, nur systemrouten angewendet.

Ermitteln Sie die Ursache der fehlgeschlagenen Verbindung mit effektiven Routen Funktion in Azure Ressourcenmanagement Bereitstellungsmodell erläutert.
Beim Beispiel nur systemrouten dieselben Schritte dienen zum eingehenden und ausgehenden Verbindungsfehlern beliebige Route zu bestimmen.

>[AZURE.NOTE] Überprüfen Sie die VM mehrere verbundene NIC effektive Routen für jeden Netzwerkkarten Netzwerkverbindungsprobleme zu und von einer VM diagnostizieren.

### <a name="view-effective-routes-for-a-virtual-machine"></a>Effektive Arbeitspläne für einen virtuellen Computer anzeigen

Aggregate Routen finden, die einer VM gelten folgende Schritte durch:

### <a name="view-effective-routes-for-a-network-interface"></a>Effektive Arbeitspläne anzeigen für eine Netzwerkschnittstelle

Gehen Sie dazu aggregate Routen, die einer Schnittstelle zugewiesen werden:

1.  Starten einer Azure PowerShell-Sitzung und bei Azure. Wenn Sie nicht mit Azure PowerShell vertraut sind, lesen Sie den Artikel [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) .

2.  Der folgende Befehl gibt alle Routen eine Ressourcengruppe *RG1* *VM1 NIC1* ernannt Netzwerkschnittstelle zugewiesen.

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Wenn Sie den Namen einer Netzwerkschnittstelle nicht kennen, geben Sie folgenden Befehl Abrufen die Namen aller Netzwerkschnittstellen Ressource Administratorkonten

        Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name

    Die folgende Ausgabe ähnelt der Ausgabe für jede Route angewendet auf das Subnetz, mit dem die Netzwerkkarte verbunden ist:

        Name :
        State : Active
        AddressPrefix : {10.9.0.0/16}
        NextHopType : VNetLocal
        NextHopIpAddress : {}

        Name :
        State : Active
        AddressPrefix : {0.0.0.0/16}
        NextHopType : Internet
        NextHopIpAddress : {}

    Beachten Sie folgende Ausgabe:
    - **Name**: Name der effektiven Route kann leer sein, sofern nicht ausdrücklich angegeben, für benutzerdefinierte Routen. 
    - **Status**: Status der Weg gibt. Mögliche Werte sind "Aktiv" oder "Ungültig"
    - **AddressPrefixes**: Gibt das Adresspräfix den Weg in CIDR-Notation. 
    - **NextHopType**: Gibt den nächsten Hop für die angegebene Route. Mögliche Werte sind *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*oder *Null*. Ein Wert von *Null* für **NextHopType** in einer UDR möglicherweise ungültige Route. Beispielsweise ist **NextHopType** *VirtualAppliance* und dem virtuellen Netzwerk Appliance VM nicht in einem Zustand bereitgestellt und ausgeführt. Wenn **NextHopType** *VPNGateway ist* und kein Gateway in bestimmten VNet bereitgestellt und ausgeführt, kann die Route ungültig.
    - **NextHopIpAddress**: die IP-Adresse des nächsten Hops den Weg.
    
    Der folgende Befehl gibt die Routen in einem einfacher zu Tabelle:

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table

    Die folgende Ausgabe ist Teil der Ausgabe des zuvor beschriebenen Szenarios erhalten:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {0.0.0.0/0} Internet {}
    

3. Es ist keine Route aus dem vorherigen Schritt *WestUS VNet3* VNet (Präfix 10.10.0.0/16)* * von *WestUS VNet1* (Präfix 10.9.0.0/16) in der Ausgabe aufgeführt. Wie in der folgenden Abbildung dargestellt, wird die Peeringverbindung VNet *WestUS VNet3* vnet *getrennt*
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Bidirektionale Verbindung für die peering unterbrochen ist, erklärt Warum VM1 nicht in *WestUS VNet3* VNet VM3 herstellen konnte. Eine bidirektionale VNet Peeringverbindung erneut für *WestUS VNet1* und *WestUS VNet3* VNets einrichten. Die Ausgabe nach VNet Peeringverbindung ordnungsgemäß eingerichtet ist folgende:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
        
    Wenn Sie das Problem bestimmen, können Sie hinzufügen, entfernen oder ändern Routen und Tabellen weiterleiten. Geben Sie den folgenden Befehl auf der dazu verwendeten Befehle aufgelistet:

        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Hinweise

Ein paar Dinge zu bedenken, wenn die Liste der Routen zurückgegeben:

- Routing basiert auf längste Präfix Übereinstimmung (LPM) unter Decision, BGP und System Routen. Wenn mehrere Routen mit den gleichen LPM ist wird eine Route basierend auf seinen Ursprung in der folgenden Reihenfolge ausgewählt:
    - Benutzerdefinierte route
    - BGP-Routen
    - Systemroutetabelle (Standard)

    Effektive Routen nur effektive Routen angezeigt, die LPM basierend auf alle Routen verfügbar sind. Zeigt, wie die Routen für eine bestimmte Netzwerkkarte tatsächlich ausgewertet werden, erleichtert dies viel bestimmte Routen zu behandeln, die Verbindung nach Ihrer VM beeinträchtigen kann.

- Wenn Sie Decision Datenverkehr an eine virtuelle Appliance (NVA) mit *VirtualAppliance* als **NextHopType**senden, damit IP-Weiterleitung empfangen die NVA aktiviert oder Pakete werden verworfen. 
- Erzwungene Tunneln aktiviert ist, werden alle ausgehender Internet-Datenverkehr zu lokalen weitergeleitet. RDP/SSH Internet VM funktioniert nicht mit dieser Einstellung, je nachdem, wie der lokale Datenverkehr behandelt. 
  Erzwungene Tunnel kann aktiviert werden.
    - Wenn Standort-zu-Standort-VPN, indem Sie eine benutzerdefinierte Route (UDR) mit NextHopType als VPN-Gateway
    - Wenn eine Standardroute über BGP angekündigt wird
- VNet peering Datenverkehr ordnungsgemäß funktioniert muss eine Route System mit **NextHopType** *VNetPeering* für hervorragendem VNet Präfix Bereich vorhanden ist. Wenn eine Route nicht vorhanden und VNet Peeringverbindung einwandfrei:
    - Warten Sie einige Sekunden, und wiederholen Sie eine neu eingerichtete Peeringverbindung ist. Manchmal dauert es länger Routen auf alle Netzwerkschnittstellen in einem Subnetz übertragen.
    - Network Security Group (NSG) Regeln können die Verkehrswerte beeinträchtigen. Weitere Informationen finden Sie im Artikel [Problembehandlung bei Netzwerk-Sicherheitsgruppen](virtual-network-nsg-troubleshoot-powershell.md) .
