<properties 
   pageTitle="Erzwungene Tunnel für Standort-zu-Standort-Verbindung mit dem klassischen Bereitstellungsmodell konfigurieren | Microsoft Azure"
   description="Wie umleiten oder 'force' alle Internet gerichtete Datenverkehr an Ihrem Standort vor Ort."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Konfigurieren Sie Erzwungene Tunnel mit dem klassischen Bereitstellungsmodell

> [AZURE.SELECTOR]
- [PowerShell - klassisch](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - Ressourcen-Manager](vpn-gateway-forced-tunneling-rm.md)

Erzwungene Tunnel ermöglicht die Umleitung oder "Force" alle Internet gerichtete Datenverkehr wieder in Ihren lokalen Standort über eine Standort-zu-Standort-VPN-Tunnel für Inspektion und Überwachung. Dies ist erforderlich für die meisten Unternehmen wichtige Security Policies. 

Ohne erzwungenen Tunneln wird Internet gerichtete Datenverkehr von der VMs in Azure immer Durchlaufen von Azure Infrastruktur direkt, ohne mit dem Internet, überprüfen oder überwachen den Datenverkehr zugelassen. Internet-Zugriff kann Offenlegung von Informationen oder andere Arten von Sicherheitslücken führen.

Dieser Artikel führt Sie durch die Konfiguration gezwungen tunneling für virtuelle Netzwerke mit dem klassischen Bereitstellungsmodell erstellt. 

**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Bereitstellungsmodelle und Tools für das erzwungene Tunneln**

Eine erzwungene Tunneln Verbindung kann für das klassische Bereitstellungsmodell und Bereitstellungsmodell Ressourcen-Manager konfiguriert werden. In der folgenden Tabelle Weitere Informationen anzeigen Wir aktualisieren diese Tabelle neue Artikel, neue Bereitstellungsmodelle und weitere Tools für diese Konfiguration verfügbar. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt aus der Tabelle.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Vorschriften und Hinweise

Erzwungene Tunnel in Azure über virtuelle benutzerdefinierte Netzwerkrouten (UDR) konfiguriert. Umleiten der Datenverkehr zu einem lokalen Standort als Standardroute Azure VPN-Gateway ausgedrückt. Im folgenden Abschnitt werden aktuelle Beschränkung routing und Routen für ein virtuelles Netzwerk Azure aufgelistet:


-  Jedes Subnetz virtuelles Netzwerk hat eine integrierte, System-Routingtabelle. Die Routingtabelle System hat die folgenden drei Gruppen von Routen:

    - **Lokale VNet Routen:** Direkt an das Ziel virtueller Computer im gleichen virtuellen Netzwerk
    
    - **Für lokale Routen:** Azure VPN-Gateway
    
    - **Standard-Route:** Direkt mit dem Internet. Pakete, die privaten IP-Adressen nicht die vorherigen zwei Routen werden gelöscht.


-  Mit benutzerdefinierten Routen können erstellen eine Routingtabelle eine Standardroute hinzu und ordnen Sie die routing-Tabelle auf Ihrem VNet Subnetze ermöglichen Erzwungene Tunnel in diesen Subnetzen.

- Legen Sie eine "Standardwebsite" unter standortübergreifende Standorte mit dem virtuellen Netzwerk verbunden werden sollen.

- Erzwungene Tunnel muss ein VNet zugeordnet, das dynamische routing VPN-Gateway (nicht statische Gateway).
 
- Erzwungene Tunnel ExpressRoute über diesen Mechanismus nicht konfiguriert, aber stattdessen wird aktiviert, indem eine Standardroute über Peers ExpressRoute BGP-Sessions Werbung. Finden Sie die [ExpressRoute Dokumentation](https://azure.microsoft.com/documentation/services/expressroute/) Weitere Informationen.



## <a name="configuration-overview"></a>(Übersicht)

Im folgenden Beispiel getunnelte Frontend Subnetz nicht erzwungen wird. Arbeitslasten im Front-End-Subnetz können weiterhin akzeptiert und auf Kundenanfragen aus dem Internet direkt. Mid-Tier- und Back-End-Subnetze müssen getunnelte. Jede ausgehenden Verbindungen von diesen zwei Subnetzen mit dem Internet werden gezwungen oder auf einer lokalen Website über einen S2S VPN-Tunnel umgeleitet.

Dadurch beschränken und Internetzugriff über die virtuellen Computer überprüfen und cloud-Services in Azure gleichzeitig ermöglichen der Multi-Tier-Architektur erforderlich. Sie können auch die erzwungene tunneling mit der gesamten virtuellen Netzwerken gibt es kein Internetzugriff Arbeitslasten in virtuellen Netzwerken anwenden.


![Erzwungene Tunnel](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Bevor Sie beginnen

Überprüfen Sie Folgendes vor der Konfiguration.

- Ein Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.

- Konfigurierte virtuelle Netzwerk. 

- Die neueste Version von Azure PowerShell-Cmdlets. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.


## <a name="configure-forced-tunneling"></a>Erzwungene Tunnel konfigurieren

Das folgende Verfahren hilft Ihnen erzwungenen Tunneln für ein virtuelles Netzwerk angeben. Die Konfigurationsschritte entsprechen VNet Netzwerk-Konfigurationsdatei.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

In diesem Beispiel hat das virtuelle Netzwerk "Multi-VNet" drei Subnetze: *Midtier*, *Front-End*und *Back-End-* Subnetze mit vier Cross betrieben: *DefaultSiteHQ*und drei *Zweigniederlassungen*. 

Die Schritte werden *DefaultSiteHQ* als standardmäßige Website-Verbindung für Erzwungene Tunnel und Konfigurieren der Midtier und Backend-Subnetze mit Erzwungene Tunnel.


1. Erstellen einer Routingtabelle. Verwenden Sie das folgende Cmdlet die Routingtabelle zu erstellen.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Fügen Sie eine Standardroute zur Routingtabelle. 

    Im folgenden Beispiel wird der in Schritt 1 erstellte Routingtabelle eine Standardroute hinzugefügt. Nur unterstützt ist das Ziel Präfix "0.0.0.0/0", "VPNGateway" Schnittstellenname.
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Verknüpfen Sie die routing-Tabelle mit den Subnetzen. 

    Nach eine Routingtabelle erstellt und eine Route verwenden Sie dabei hinzufügen oder Zuordnen der Routentabelle Subnetz VNet. Im Beispiel hinzugefügt Midtier und Back-End-Subnetze VNet Multi-VNet die Routentabelle "MyRouteTable".

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Weisen Sie eine Standardwebsite für Tunnel. 

    Im vorherigen Schritt Beispielskripts Cmdlet die routing-Tabelle erstellt und verknüpft die Routentabelle zwei Subnetzen VNet. Die verbleibende Schritte ist eine lokale Website unter standortübergreifende Verbindung des virtuellen Netzwerks als Standardwebsite oder Tunnel auswählen.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Weitere PowerShell-cmdlets

### <a name="to-delete-a-route-table"></a>Löschen eine Routentabelle

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Liste eine Route-Tabelle

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Löschen eine Route aus einer Routentabelle

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Ein Subnetz eine Route entfernen

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Liste die Routentabelle mit einem Subnetz verbunden
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>VNet VPN-Gateway eine Standardwebsite entfernt

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






