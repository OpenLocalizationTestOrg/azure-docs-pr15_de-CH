<properties 
   pageTitle="Problembehandlung bei Routen - Portal | Microsoft Azure"
   description="Informationen Sie zum Routen in der Azure-Ressourcen-Manager-Bereitstellungsmodell mithilfe der Azure-Portal zu beheben."
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

# <a name="troubleshoot-routes-using-the-azure-portal"></a>Problembehandlung bei Azure-Portal mit Routen

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

1. Melden Sie das Azure-Portal unter https://portal.azure.com.
2. Klicken Sie unter **Weitere Dienste**auf **virtuelle Computer** in der angezeigten Liste.
3. Wählen Sie VM Problembehandlung aus, das angezeigt wird und eine VM-Blade mit Optionen wird angezeigt.
4. Klicken Sie auf **diagnostizieren und lösen** und wählen Sie dann ein häufig auftretendes Problem. **Ich kann keine Verbindung mit meinem Windows VM** ist beispielsweise ausgewählt. 

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)

5. Schritte werden unter das Problem wie in der folgenden Abbildung dargestellt: 

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Klicken Sie auf *effektive Routen* in der Liste der empfohlenen Schritte.

6. **Effektive Routen** Blade wird wie in der folgenden Abbildung dargestellt:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Wenn die VM nur eine Netzwerkkarte verfügt, ist sie standardmäßig aktiviert. Haben Sie mehr als eine Netzwerkkarte, wählen Sie die NIC an effektiven Routen werden soll.

    >[AZURE.NOTE] Ist die Netzwerkkarte zugeordnete VM nicht ausgeführt, werden effektive Routen nicht angezeigt. Nur die ersten 200 effektiven Routen werden im Portal angezeigt. Die vollständige Liste klicken Sie auf **Download**. Sie können die Ergebnisse aus der heruntergeladenen weiter filtern.

    Beachten Sie folgende Ausgabe:
    - **Quelle**: Gibt den Typ der Route. *Standard*, Decision als *Benutzer* und Gateway Routen angezeigt werden systemrouten angezeigt (statische oder BGP) werden als *VPNGateway*angezeigt.
    - **Status**: Status der Weg gibt. Mögliche Werte sind *aktiv* oder *ungültig*.
    - **AddressPrefixes**: Gibt das Adresspräfix den Weg in CIDR-Notation. 
    - **NextHopType**: Gibt den nächsten Hop für die angegebene Route. Mögliche Werte sind *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*oder *Null*. Ein Wert von *Null* für **NextHopType** in einer UDR möglicherweise ungültige Route. Beispielsweise ist **NextHopType** *VirtualAppliance* und dem virtuellen Netzwerk Appliance VM nicht in einem Zustand bereitgestellt und ausgeführt. Wenn **NextHopType** *VPNGateway ist* und kein Gateway in bestimmten VNet bereitgestellt und ausgeführt, kann die Route ungültig.
    
7. Es ist keine Route von der *WestUS VNet1* (Präfix 10.9.0.0/16) in dem Bild im vorherigen Schritt *WestUS VNET3* VNet (Präfix 10.10.0.0/16) aufgeführt. In der folgenden Abbildung ist die Peeringverbindung in *getrennt* :
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Bidirektionale Verbindung für die peering unterbrochen ist, erklärt Warum VM1 nicht in *WestUS VNet3* VNet VM3 herstellen konnte.

8. Das folgende Bild zeigt die Routen nach dem Einrichten der bidirektionalen Peeringverbindung:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Problembehandlung Szenarien für erzwungene Tunneln und Route Bewertung lesen Sie [Aspekte](virtual-network-routes-troubleshoot-portal.md#Considerations) dieses Artikels.

### <a name="view-effective-routes-for-a-network-interface"></a>Effektive Arbeitspläne anzeigen für eine Netzwerkschnittstelle

Wenn für einen bestimmten Netzwerkadapter (NIC) des Netzwerkverkehrs beeinträchtigt wird, können Sie eine vollständige Liste der effektiven Routen direkt auf einer NIC anzeigen. Aggregate Routen finden, die zu einer Netzwerkkarte gelten folgende Schritte durch:

1. Melden Sie das Azure-Portal unter https://portal.azure.com.
2. Klicken Sie auf **Weitere Dienste** **Netzwerkschnittstellen**
3. Durchsuchen Sie die Liste für den Namen einer NIC oder wählen sie aus der angezeigten Liste. In diesem Beispiel wird **VM1 NIC1** ausgewählt.
4. Wählen Sie **effektive Routen** in Blade **-Schnittstelle** wie in der folgenden Abbildung dargestellt:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    Der **Bereich** standardmäßig die Netzwerkschnittstelle ausgewählt.

    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)


### <a name="view-effective-routes-for-a-route-table"></a>Effektive Arbeitspläne für eine Route-Tabelle anzeigen

Wenn benutzerdefinierte Routen (Decision) in einer Routentabelle ändern, sollten Sie die Auswirkungen Routen auf einer bestimmten VM hinzugefügt. Route-Tabelle kann eine beliebige Anzahl von Subnetzen zugeordnet werden. Jetzt sehen Sie die effektiven Routen für alle Netzwerkkarten, die eine bestimmte Route-Tabelle auf, ohne Kontext aus dem gegebenen Route Tabelle Blade.

Beispielsweise wird ein UDR (*UDRoute*) in einer Routentabelle (*UDRouteTable*) angegeben. Diese Route sendet alle Internetverkehr von *Subnet1* in VNet *WestUS VNet1* über eine virtuelle Appliance (NVA) in *Subnet2* von demselben VNet. Die Route wird in der folgenden Abbildung dargestellt:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Die aggregate Routen eine Routentabelle finden folgende Schritte durch:

1. Melden Sie das Azure-Portal unter https://portal.azure.com.
2. Klicken Sie auf **Weitere Dienste** **Routentabelle**
3. Durchsuchen Sie die Liste für die Routentabelle zu aggregieren Routen für auswählen möchten. In diesem Beispiel wird die **UDRouteTable** ausgewählt. Blade für die ausgewählte Route-Tabelle wird angezeigt, wie in der folgenden Abbildung dargestellt:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)

4. **Effektive Routen** in der **Routingtabelle** Blade auswählen **Bereich** wird der Routentabelle Auswahl festgelegt.
5. Route-Tabelle kann mehrere Subnetze angewendet werden. Wählen Sie das **Subnetz** aus der Liste überprüfen möchten. In diesem Beispiel wird die **Subnet1** ausgewählt.
6. Wählen Sie eine **Netzwerkschnittstelle**. Alle Netzwerkkarten mit dem ausgewählten Subnetz verbunden werden aufgelistet. In diesem Beispiel wird **VM1 NIC1** ausgewählt.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

    >[AZURE.NOTE] Wenn die NIC keinen ausgeführten virtuellen Computer zugeordnet ist, werden keine effektiven Routen angezeigt.

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
    - Network Security Group (NSG) Regeln können die Verkehrswerte beeinträchtigen. Weitere Informationen finden Sie im Artikel [Problembehandlung bei Netzwerk-Sicherheitsgruppen](virtual-network-nsg-troubleshoot-portal.md) .
