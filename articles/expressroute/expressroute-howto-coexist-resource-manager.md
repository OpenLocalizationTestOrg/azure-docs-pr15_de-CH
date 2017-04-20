<properties
   pageTitle="Expressroute und Standort-zu-Standort-VPN-Verbindungen, die für den Ressourcen-Manager-Bereitstellungsmodell koexistieren konfigurieren | Microsoft Azure"
   description="Dieser Artikel führt Sie durch die Konfiguration von ExpressRoute und eine Standort-zu-Standort-VPN-Verbindung, die Ressourcen-Manager-Modell gleichzeitig vorhanden sein kann."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="charleywen"/>

# <a name="configure-expressroute-and-site-to-site-coexisting-connections-for-the-resource-manager-deployment-model"></a>Konfigurieren Sie ExpressRoute und zwischen Standorten Koexistenz Verbindungen für das Bereitstellungsmodell der Ressourcen-Manager

> [AZURE.SELECTOR]
- [PowerShell - Ressourcen-Manager](expressroute-howto-coexist-resource-manager.md)
- [PowerShell - klassisch](expressroute-howto-coexist-classic.md)

Konfigurieren von Standort zu Standort VPN und ExpressRoute hat mehrere Vorteile. Sie können Standort-zu-Standort-VPN als sicheres Failover Pfad für ExressRoute, oder Standort-zu-Standort-VPNs Verbindung zu Websites, die nicht durch ExpressRoute verwenden. Die Schritte zum Konfigurieren von beiden Szenarien in diesem Artikel behandelt wird. Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Diese Konfiguration ist nicht verfügbar in Azure-Portal.


**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

>[AZURE.IMPORTANT] ExpressRoute Stromkreise müssen vorab konfiguriert werden, bevor die folgenden Anweisungen. Stellen Sie sicher, dass Sie Hilfslinien [ExpressRoute-Verbindung erstellen](expressroute-howto-circuit-arm.md) und [Konfigurieren von routing](expressroute-howto-routing-arm.md) befolgt haben, bevor Sie die folgenden Schritte.

## <a name="limits-and-limitations"></a>Grenzwerte und Grenzen

- **Übertragung routing wird nicht unterstützt.** Sie können nicht zwischen dem lokalen Netzwerk Standort-zu-Standort-VPN-Verbindung und das lokale Netzwerk angeschlossen ExpressRoute (über Azure) weiterleiten.
- **Grundlegende SKU-Gateway wird nicht unterstützt.** Sie müssen nicht grundlegende SKU Gateway [ExpressRoute Gateway](expressroute-about-virtual-network-gateways.md) und das [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)verwenden.
- **Nur Route-basierte VPN-Gateway unterstützt.** Route-basierte [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)verwenden.
- **Statische Route sollte für das VPN-Gateway konfiguriert werden.** Wenn das lokale Netzwerk mit ExpressRoute und ein Standort-zu-Standort-VPN verbunden ist, müssen Sie eine statische Route im lokalen Netzwerk die Standort-zu-Standort-VPN-Verbindung mit dem öffentlichen Internet weiterleiten konfiguriert.
- **ExpressRoute-Gateway muss zuerst konfiguriert werden.** Vor dem Hinzufügen von Standort zu Standort VPN-Gateway, müssen Sie zuerst das Gateway ExpressRoute erstellen.


## <a name="configuration-designs"></a>Konfiguration-designs

### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Konfigurieren Sie eine Standort-zu-Standort-VPN als Failover-Pfad für ExpressRoute

Sie können eine Standort-zu-Standort-VPN-Verbindung als Sicherung für ExpressRoute konfigurieren. Dies gilt nur für virtuelle Netzwerke Azure private peering Pfad verknüpft. Gibt keine VPN-basierten Failoverlösung für Microsoft Peerings und Azure public. ExpressRoute-Verbindung ist immer der primäre Link. Daten fließen über den Standort-zu-Standort-VPN-Pfad nur, wenn ExpressRoute-Verbindung fehlschlägt.
>[AZURE.NOTE] ExpressRoute-Verbindung ist bevorzugte Standort-zu-Standort-VPN, wenn beide identisch sind, verwendet Azure mehretappenfahrt präfixübereinstimmung die Route des Pakets Ziel auswählen.

![Nebeneinander](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a>Konfigurieren eines Standort-zu-Standort-VPN Verbindung zu Websites über ExpressRoute nicht verbunden

Sie können Ihr Netzwerk, einige Websites verbinden direkt in Azure Standort-zu-Standort-VPN und einige Websites über Expressroute, konfigurieren. 

![Nebeneinander](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

>[AZURE.NOTE] Konfigurieren eines virtuellen Netzwerks als Router während der Übertragung nicht möglich.

## <a name="selecting-the-steps-to-use"></a>Schritte zur Auswahl

Es gibt zwei unterschiedliche Vorgehensweisen wählen, um Verbindungen zu konfigurieren, die gleichzeitig vorhanden sein können. Die gewählte Konfiguration hängt davon ab, ob Sie haben ein vorhandenes virtuelles Netzwerk, das Sie möchten oder ein neues virtuelles Netzwerk erstellen möchten.


- Ich habe ein VNet und erstellen müssen.
    
    Haben Sie bereits ein virtuelles Netzwerk, führt dieses Verfahren Sie durch Erstellen eines neuen virtuellen Netzwerks Bereitstellungsmodell Ressourcenmanager und neue ExpressRoute und Standort-zu-Standort-VPN-Verbindungen. Konfigurieren der Schritte im Artikelabschnitt [Erstellen Sie ein neues virtuelles Netzwerk und Koexistenz Verbindungen](#new).

- Ich habe bereits ein Ressourcen-Manager-Bereitstellungsmodell VNet.

    Sie haben bereits ein virtuelles Netzwerk mit einem vorhandenen Standort-zu-Standort-VPN-Verbindung oder ExpressRoute-Verbindung. Abschnitt [Coexsiting Anschlüsse für eine bereits vorhandene VNet konfigurieren](#add) gehen Sie das Gateway löschen und erstellen neue ExpressRoute und Standort-zu-Standort-VPN-Verbindungen. Beachten Sie, dass die Schritte beim Erstellen von neuen Verbindungen in einer ganz bestimmten Reihenfolge ausgeführt werden müssen. Verwenden der Anleitung in anderen Artikeln um ein Gateways und Verbindungen zu erstellen.

    In diesem Verfahren wird erstellen koexistieren können Verbindungen müssen Sie Ihr Gateway löschen und neue Gateways konfigurieren. Dies bedeutet, dass Sie Ausfallzeiten für die standortübergreifende Verbindung beim Löschen und neu erstellen die Gateway- und Verbindungen, Sie müssen jedoch Ihre VMs oder Services ein neues virtuelles Netzwerk migrieren. Ihr virtueller Computer und Dienste werden während Ihres Gateways konfigurieren, wenn sie dazu konfiguriert, über den Lastenausgleich kommunizieren.


## <a name="new"></a>Erstellen Sie ein neues virtuelles Netzwerk und Koexistenz Verbindungen

Diese Prozedur erstellt ein VNet durchlaufen und zwischen Standorten und ExpressRoute Verbindungen, die gemeinsam verwendet werden.
    
1. Sie müssen die neueste Version von Azure PowerShell-Cmdlets installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets. Beachten Sie, dass die Cmdlets, die Sie für diese Konfiguration verwenden als Sie kennen möglicherweise etwas anders. Werden Sie verwenden Sie die Cmdlets in dieser Anleitung angegeben.

2. Anmeldung Ihr Konto und Einrichten der Umgebung.
    
        login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
        $location = "Central US"
        $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location

3. Erstellen Sie ein virtuelles Netzwerk Gatewaysubnetz. Weitere Informationen über die virtuelle Netzwerkkonfiguration finden Sie unter [Azure Virtual Network Configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).

    >[AZURE.IMPORTANT] Gateway-Subnetz muss /27 oder kürzere Präfix (z. B. /26 oder /25).
    
    Erstellen Sie eine neue VNet.

        $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16" 

    Fügen Sie Subnetze hinzu.

        Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
        Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

    Speichern Sie die VNet-Konfiguration.

        $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

4. <a name="gw"></a>Erstellen Sie einen ExpressRoute-Gateway. Weitere Informationen über die ExpressRoute Gateway-Konfiguration finden Sie unter [ExpressRoute Gateway-Konfiguration](expressroute-howto-add-gateway-resource-manager.md). Die GatewaySKU muss *Standard*, *leistungsstarker*oder *UltraPerformance*sein.

        $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
        $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
        $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard 

5. Verknüpfen Sie Gateway ExpressRoute auf ExpressRoute-Verbindung. Nachdem dieser Schritt abgeschlossen ist, wird die Verbindung zwischen dem lokalen Netzwerk und Azure über ExpressRoute, hergestellt. Weitere Informationen über die Verknüpfungsoperation finden Sie unter [Link-VNets, ExpressRoute](expressroute-howto-linkvnet-arm.md).

        $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
        New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute

6. <a name="vpngw"></a>Erstellen Sie anschließend das Standort-zu-Standort-VPN-Gateway. Weitere Informationen zu VPN-Gateway-Konfiguration finden Sie unter [Konfigurieren einer VNet mit einer Standort-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md). Die GatewaySKU muss *Standard*, *leistungsstarker*oder *UltraPerformance*sein. Die VpnType muss *RouteBased*.

        $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
        $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
        New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"

    Azure VPN-Gateway unterstützt das BGP. EnableBgp - Geben Sie folgenden Befehl ein.

        $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -EnableBgp $true

    Sie finden das BGP peering IP und AS-Nummer, die Azure für VPN-Gateway in $azureVpn.BgpSettings.BgpPeeringAddress und $azureVpn.BgpSettings.Asn verwendet. Weitere Informationen finden Sie unter [Konfigurieren von BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) Azure VPN-Gateway.

7. Erstellen einer lokalen Website VPN-Gateway Entität. Dieser Befehl konfigurieren nicht das lokale VPN-Gateway. Stattdessen können Sie das lokale Gateway Einstellungen, wie öffentliche IP-Adresse und der lokalen Adresse Speicherplatz, sodass Azure VPN-Gateway herstellen kann.

    Wenn Ihre lokale VPN-Gerät nur statisches routing unterstützt, können Sie die statischen Routen auf folgende Weise konfigurieren.

        $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
        $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress

    Wenn das lokale VPN-Gerät das BGP unterstützt und dynamisches routing aktivieren möchten, müssen Sie wissen, BGP peering IP und AS-Nummer, die das lokale VPN-Gerät verwendet.

        $localVPNPublicIP = "<Public IP>"
        $localBGPPeeringIP = "<Private IP for the BGP session>"
        $localBGPASN = "<ASN>"
        $localAddressPrefix = $localBGPPeeringIP + "/32"
        $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN

8. Konfigurieren Sie das lokale VPN-Gerät die Verbindung neue Azure VPN-Gateway. Weitere Informationen zu VPN-Konfiguration finden Sie unter [VPN-Konfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).

9. Verknüpfen Sie Standort-zu-Standort-VPN-Gateway auf Azure mit dem lokalen Gateway.

        $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
        New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>


## <a name="add"></a>Coexsiting-Anschlüsse für eine bereits vorhandene VNet konfigurieren

Wenn Sie ein vorhandenes virtuelles Netzwerk haben, überprüfen Sie Gateway Subnetz Größe. Ist das gatewaysubnetz /28 oder /29, müssen Sie das virtuelle Netzwerkgateway löschen und vergrößern Sie Gateway Subnetz. Die Schritte in diesem Abschnitt zeigen Ihnen dazu.

Ist das gatewaysubnetz /27 oder das virtuelle Netzwerk angeschlossen ExpressRoute und Schritte überspringen und fahren Sie mit ["Schritt 6 - erstellen eine Standort-zu-Standort-VPN-Gateway"](#vpngw) im vorherigen Abschnitt. 

>[AZURE.NOTE] Beim Löschen des vorhandenen Gateways verlieren Ihnen vor Ort die Verbindung mit dem virtuellen Netzwerk, während dieser Konfiguration arbeiten. 

1. Sie müssen die neueste Version von Azure PowerShell-Cmdlets installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets. Beachten Sie, dass die Cmdlets, die Sie für diese Konfiguration verwenden als Sie kennen möglicherweise etwas anders. Werden Sie verwenden Sie die Cmdlets in dieser Anleitung angegeben. 

2. Löschen Sie vorhandene ExpressRoute oder Standort-zu-Standort-VPN-Gateway. 

        Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>

3. Löschen Sie Gatewaysubnetz
        
        $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> 
        Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet

4. Hinzufügen eines Subnetzes Gateway, die /27 oder größer.
    >[AZURE.NOTE] Wenn genügend IP-Adressen im virtuellen Netzwerk Gateway Subnetz vergrößert haben, müssen Sie weitere IP-Adressraum hinzufügen.

        $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
        Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

    Speichern Sie die VNet-Konfiguration.

        $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

5. Zu diesem Zeitpunkt haben Sie eine VNet mit keine Gateways. Um neue Gateways erstellen und Ihre Verbindung abzuschließen, können Sie mit [Schritt 4: erstellen ein ExpressRoute-Gateways](#gw)finden in den vorhergehenden Schritten fortfahren.

## <a name="to-add-point-to-site-configuration-to-the-vpn-gateway"></a>VPN-Gateway Punkt-zu-Standort-Konfiguration hinzufügen
Sie können so das VPN-Gateway in einer Einrichtung Koexistenz Punkt-zu-Standort-Konfiguration hinzufügen folgen.

1. VPN-Client-Adresspool hinzufügen. 

        $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
        Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"

2. Hochladen Sie VPN-Stammzertifikat für das VPN-Gateway in Azure. In diesem Beispiel wird davon ausgegangen, dass das Stammzertifikat auf dem lokalen Computer gespeichert ist, auf dem folgenden PowerShell-Cmdlets ausgeführt werden. 

        $p2sCertFullName = "RootErVpnCoexP2S.cer"
        $p2sCertMatchName = "RootErVpnCoexP2S"
        $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName}
        if ($p2sCertToUpload.count -eq 1){
            write-host "cert found"
        } else {
            write-host "cert not found"
            exit
        } 
        $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData)
        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData

Weitere Informationen über Punkt-zu-Standort-VPN finden Sie unter [Konfigurieren einer Punkt-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu ExpressRoute finden Sie im [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md).
