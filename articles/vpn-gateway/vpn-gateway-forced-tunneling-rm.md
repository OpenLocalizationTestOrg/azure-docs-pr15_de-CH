<properties 
   pageTitle="Erzwungene Tunnel für Standort-zu-Standort-Verbindung mit dem Ressourcen-Manager-Bereitstellungsmodell konfigurieren | Microsoft Azure"
   description="Wie umleiten oder 'force' alle Internet gerichtete Datenverkehr an Ihrem Standort vor Ort."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Konfigurieren Sie Erzwungene Tunnel mit Bereitstellungsmodell Azure-Ressourcen-Manager

> [AZURE.SELECTOR]
- [PowerShell - klassisch](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - Ressourcen-Manager](vpn-gateway-forced-tunneling-rm.md)

Erzwungene Tunnel ermöglicht die Umleitung oder "Force" alle Internet gerichtete Datenverkehr wieder in Ihren lokalen Standort über eine Standort-zu-Standort-VPN-Tunnel für Inspektion und Überwachung. Dies ist erforderlich für die meisten Unternehmen wichtige Security Policies.

Ohne erzwungenen Tunneln wird Internet gerichtete Datenverkehr von der VMs in Azure immer Durchlaufen von Azure Infrastruktur direkt, ohne mit dem Internet, überprüfen oder überwachen den Datenverkehr zugelassen. Internet-Zugriff kann Offenlegung von Informationen oder andere Arten von Sicherheitslücken führen.

Dieser Artikel führt Sie durch Konfigurieren erzwungene Tunneln für virtuelle Netzwerke mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt.

**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Bereitstellungsmodelle und Tools für das erzwungene Tunneln**

Eine erzwungene Tunneln Verbindung kann für das klassische Bereitstellungsmodell und Bereitstellungsmodell Ressourcen-Manager konfiguriert werden. In der folgenden Tabelle Weitere Informationen anzeigen Wir aktualisieren diese Tabelle neue Artikel, neue Bereitstellungsmodelle und weitere Tools für diese Konfiguration verfügbar. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt aus der Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Über Erzwungene Tunnel


Das folgende Diagramm veranschaulicht, wie die erzwungene Tunneln funktioniert. 

![Erzwungene Tunnel](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Im obigen Beispiel getunnelte Frontend Subnetz nicht erzwungen wird. Arbeitslasten im Front-End-Subnetz können weiterhin akzeptiert und auf Kundenanfragen aus dem Internet direkt. Mid-Tier- und Back-End-Subnetze müssen getunnelte. Jede ausgehenden Verbindungen von diesen zwei Subnetzen mit dem Internet werden gezwungen oder auf einer lokalen Website über einen S2S VPN-Tunnel umgeleitet.

Dadurch beschränken und Internetzugriff über die virtuellen Computer überprüfen und cloud-Services in Azure gleichzeitig ermöglichen der Multi-Tier-Architektur erforderlich. Sie können auch die erzwungene tunneling mit der gesamten virtuellen Netzwerken gibt es kein Internetzugriff Arbeitslasten in virtuellen Netzwerken anwenden.

## <a name="requirements-and-considerations"></a>Vorschriften und Hinweise

Erzwungene Tunnel in Azure über virtuelle Netzwerk benutzerdefinierte Routen konfiguriert. Umleiten der Datenverkehr zu einem lokalen Standort als Standardroute Azure VPN-Gateway ausgedrückt. Weitere Informationen zu benutzerdefinierten routing und virtuelle Netzwerke finden Sie unter [benutzerdefinierte Routen und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md).

- Jedes Subnetz virtuelles Netzwerk hat eine integrierte, System-Routingtabelle. Die Routingtabelle System hat die folgenden drei Gruppen von Routen:

    - **Lokale VNet Routen:** Direkt an das Ziel virtueller Computer im gleichen virtuellen Netzwerk
    
    - **Für lokale Routen:** Azure VPN-Gateway
    
    - **Standard-Route:** Direkt mit dem Internet. Pakete, die privaten IP-Adressen nicht die vorherigen zwei Routen werden gelöscht.

-  Dieses Verfahren verwendet benutzerdefinierte Routen (UDR) zum Erstellen einer Routingtabelle eine Standardroute hinzu, und ordnen Sie die routing-Tabelle auf Ihrem VNet Subnetze ermöglichen Erzwungene Tunnel in diesen Subnetzen.

- Erzwungene Tunnel muss ein VNet zugeordnet, das eine Route-basierte VPN-Gateway. Legen Sie eine "Standardwebsite" unter standortübergreifende Standorte mit dem virtuellen Netzwerk verbunden werden sollen.

- Erzwungene Tunnel ExpressRoute über diesen Mechanismus nicht konfiguriert, aber stattdessen wird aktiviert, indem eine Standardroute über Peers ExpressRoute BGP-Sessions Werbung. Finden Sie die [ExpressRoute Dokumentation](https://azure.microsoft.com/documentation/services/expressroute/) Weitere Informationen.

## <a name="configuration-overview"></a>(Übersicht)

Das folgende Verfahren können Sie eine Ressourcengruppe und ein VNet erstellen. Sie dann erstellen Sie eine VPN-Gateway und Erzwungene Tunnel konfigurieren. In diesem Verfahren ist das virtuelle Netzwerk "Multi-VNet" 3 Subnetze: *Frontend*, *Midtier*und *Back-End*mit 4 standortübergreifende Verbindung: *DefaultSiteHQ*und 3 *Zweige*.

Die Verfahrensschritte legen Sie *DefaultSiteHQ* als Standard-Standort-Verbindung für Erzwungene Tunnel, Konfigurieren der Midtier und Back-End-Subnetze mit tunneling.

    
## <a name="before-you-begin"></a>Bevor Sie beginnen

Überprüfen Sie Folgendes vor der Konfiguration.

- Ein Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.

- Sie müssen die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets (Version 1.0 oder höher) installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.


## <a name="configure-forced-tunneling"></a>Erzwungene Tunnel konfigurieren

1. Melden Sie sich in der PowerShell-Konsole Azure-Konto an. Dieses Cmdlet aufgefordert, die Anmeldeinformationen für Ihr Konto Azure. Nach der Anmeldung gedownloadet Ihrer Konteneinstellungen Azure PowerShell verfügbar sind.

        Login-AzureRmAccount 

2. Eine Liste der Azure-Abonnements abrufen.

        Get-AzureRmSubscription

2. Geben Sie das Abonnement, das Sie verwenden möchten. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Erstellen Sie eine Ressourcengruppe.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Erstellen Sie ein virtuelles Netzwerk und geben Sie Subnetze an. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Erstellen der lokalen Netzwerkgateways.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. Erstellen der Route-Tabelle und Arbeitsplan.

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Zuordnen der Routentabelle mit Midtier und Back-End-Subnetzen.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Erstellen Sie das Gateway mit einer Standardwebsite. Dieser Schritt dauert einige Zeit, manchmal 45 Minuten oder mehr, da Sie erstellen und Konfigurieren von Gateway.<br> Die `-GatewayDefaultSite` der Cmdlet-Parameter, der ermöglicht erzwungene Routingkonfiguration arbeiten, also darauf konfigurieren dieser Einstellung richtig ist. Dieser Parameter ist in PowerShell 1.0 oder höher verfügbar.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. Standort-zu-Standort-VPN-Verbindungen herstellen.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        



