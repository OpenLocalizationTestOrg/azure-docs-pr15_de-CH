<properties
   pageTitle="Erstellen Sie ein virtuelles Netzwerk mit einer Standort-zu-Standort-VPN-Verbindung Azure Resource Manager und PowerShell | Microsoft Azure"
   description="Dieser Artikel führt Sie durch ein VNet mit Ressourcen-Manager-Bereitstellungsmodell und Herstellen einer Verbindung mit dem lokalen lokalen Netzwerk eine Verbindung S2S VPN-Gateway erstellen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Erstellen Sie ein VNet mit einer Standort-zu-Standort-Verbindung mit PowerShell

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Classic - Verwaltungsportal](vpn-gateway-site-to-site-create.md)

Dieser Artikel führt Sie durch das Erstellen eines virtuellen Netzwerks und ein Standort-zu-Standort-VPN-Gateway mit dem lokalen Netzwerk des Azure-Ressourcen-Manager-Bereitstellungsmodells. Standort-zu-Standort-Verbindung können für standortübergreifende und Hybrid verwendet Konfigurationen.

![Standort-zu-Standort-Diagramm] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "zwischen Standorten") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Bereitstellung und-Modelle für Standort-zu-Standort-Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Die folgende Tabelle zeigt die derzeit Bereitstellungsmodelle und Methoden für Standort-zu-Standort-Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Zusätzliche Konfigurationen

Wenn Sie VNets verbinden, aber keine Verbindung zu einem lokalen Speicherort erstellen, finden Sie unter [Konfigurieren einer Verbindung VNet VNet](vpn-gateway-vnet-vnet-rm-ps.md). Eine Standort-zu-Standort-Verbindung zu einem VNet hinzufügen, die bereits eine Verbindung, finden Sie unter [Hinzufügen einer S2S-Verbindung zu einem VNet mit einer vorhandenen VPN-Gateway-Verbindung](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Bevor Sie beginnen

Überprüfen Sie Folgendes vor der Konfiguration.

- Ein kompatibles VPN-Gerät und wer konfigurieren kann. [VPN-Geräte](vpn-gateway-about-vpn-devices.md)finden Sie unter. Vertraut mit Ihrem VPN-Gerät konfiguriert oder sind nicht mit der IP-Adresse befindet sich Bereiche in Ihrem lokalen Netzwerk-Konfiguration, müssen Sie diese Angaben können Sie mit anderen Personen koordinieren.

- Eine nach außen gerichtete öffentliche IP-Adresse für das VPN-Gerät. Diese IP-Adresse kann nicht hinter einem NAT befinden
    
- Ein Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.
    
- Die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.


## <a name="Login"></a>1. Verbinden Sie 1. Ihr Abonnement 

Stellen Sie sicher, dass PowerShell Modus Ressourcenmanager Cmdlets wechseln. Weitere Informationen finden Sie unter [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

Öffnen Sie die PowerShell-Konsole und Ihr Konto verbinden. Verwenden Sie Folgendes Beispiel zu verbinden:

    Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto.

    Get-AzureRmSubscription 

Geben Sie das Abonnement, das Sie verwenden möchten.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2. erstellen Sie ein virtuelles Netzwerk und ein

Die Beispiele verwenden eine gatewaysubnetz /28. Es ist, zwar möglich, ein /29 möglichst klein erstellen empfiehlt sich die größeren Subnetzes erstellen, die mehrere Adressen enthält mindestens /28 oder /27. Damit wird genügend Adressen, die in Zukunft möglicherweise zusätzliche Konfigurationen aufzunehmen.

Wenn bereits ein virtuelles Netzwerk mit einem gatewaysubnetz, die 29 oder größer, gelangen Sie vor [Ihrem lokalen Netzwerk-Gateway](#localnet)hinzufügen.


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Zum Erstellen eines virtuellen Netzwerks und ein

Verwenden Sie das folgende Beispiel zum Erstellen eines virtuellen Netzwerks und ein. Ersetzen Sie die Werte für Ihre eigenen. 

Erstellen Sie zuerst eine Ressourcengruppe:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Erstellen Sie anschließend virtuelle Netzwerk. Überprüfen Sie ob Adressräume Angabe eines die Adressräume nicht überlappen, die Sie in Ihrem lokalen Netzwerk.

Das folgende Beispiel erstellt ein virtuelles Netzwerk mit dem Namen *Testvnet* und zwei Subnetzen eine aufgerufene *Subnetzname* und anderen *Subnet1*aufgerufen. Es ist wichtig, insbesondere *Subnetzname*Namens ein Subnetz erstellen. Wenn Sie einen anderen Namen, scheitert die Verbindungskonfiguration. 

Setzen Sie die Variablen.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Erstellen des Vnets.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Ein bereits erstellte ein virtuelles Netzwerk hinzu

Dieser Schritt ist erforderlich, wenn ein ein VNet hinzufügen, die Sie zuvor erstellt haben müssen.

Mit dem folgenden Beispiel, um gatewaysubnetz zu erstellen. Achten Sie darauf, dass gatewaysubnetz 'Subnetzname' nennen. Wenn Sie einen anderen Namen, erstellen Sie ein Subnetz jedoch Azure wird nicht als ein behandelt.

Setzen Sie die Variablen.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Erstellen Sie gatewaysubnetz.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Festlegen der Konfiguration. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"> </a>Ihr lokales Netzwerkgateway hinzufügen

In einem virtuellen Netzwerk bezieht sich normalerweise an lokalen LAN Gateway. Sie geben Sie der Website einen Namen, Azure darauf verweisen kann, und auch das Adresspräfix Speicherplatz für das lokale Netzwerk-Gateway angeben. 

Azure verwendet IP-Adresspräfix, welcher Datenverkehr an Ihren lokalen Standort angeben. Dies bedeutet, dass jedes Adresspräfix angeben, Ihrem lokalen Netzwerk-Gateway zugeordnet werden. Diese Präfixe können problemlos aktualisieren, ändert sich Ihr lokales Netzwerk. 

Wenn PowerShell-Beispiele verwenden, beachten Sie Folgendes:
    
- *GatewayIPAddress* ist die IP-Adresse des lokalen VPN-Geräts. Das VPN-Gerät kann nicht hinter einem NAT befinden 
- *AddressPrefix* ist den lokalen Adressbereich.

Hinzufügen einer lokalen Netzwerk-Gateway mit ein einzelnes Adresspräfix:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Ein lokales Netzwerk-Gateway mit mehreren Präfixen hinzu

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>Ändern der IP-Adresse für Ihr LAN Gateway Präfixe

Manchmal ändern Ihr LAN Gateway Präfixe. Die Schritte zum Ändern der IP-Adresspräfixe hängt davon ab, ob Sie ein VPN-Gateway erstellt haben. Siehe Abschnitt [Adresspräfixe für das lokale Netzwerk-Gateway IP ändern](#modify) .


## <a name="PublicIP"></a>4. Anforderung eine öffentliche IP-Adresse des VPN-Gateways

Anschließend fordern Sie eine öffentliche IP-Adresse Ihre Azure VNet VPN-Gateway zugewiesen werden. Dies ist nicht die IP-Adresse, die dem VPN-Gerät zugewiesen ist. Stattdessen wird es die Azure VPN-Gateway zugewiesen. Sie können nicht die IP-Adresse angeben, die Sie verwenden möchten. Das Gateway wird dynamisch zugewiesen. Sie verwenden diese IP-Adresse bei Ihrem lokalen VPN-Konfiguration zum Gateway herstellen.

Azure VPN-Gateway für das Ressourcen-Manager-Bereitstellungsmodell derzeit unterstützt nur öffentliche IP-Adressen mithilfe der Methode der dynamischen Zuteilung. Dies bedeutet jedoch nicht, dass die IP-Adresse geändert wird. Nur Azure VPN-Gateway IP-Adressänderungen wird das Gateway gelöscht und neu erstellt wird. Gateway öffentliche IP-Adresse ändert sich nicht ändern, zurücksetzen oder andere interne Wartung und Upgrades Ihrer Azure VPN-Gateway.

Verwenden Sie das folgende PowerShell-Beispiel:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. die Gateway IP-Adressierung Konfiguration erstellen

Die Gateway-Konfiguration definiert das Subnetz und die öffentliche IP-Adresse verwenden. Verwenden Sie Folgendes Beispiel, um Ihre Gateway-Konfiguration zu erstellen.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. virtuelle Netzwerk-Gateway erstellen

In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway. Erstellen eines Gateways kann viel Zeit in Anspruch nehmen. Häufig 45 Minuten oder mehr. 

Verwenden Sie die folgenden Werte:

- *-GatewayType* für eine Standort-zu-Standort-Konfiguration ist *Vpn*. Gatewaytyp ist immer für die Konfiguration, die Sie implementieren. Beispielsweise erfordern andere Konfigurationen Gateway - GatewayType ExpressRoute. 

- *VpnType -* möglich *RouteBased* (bezeichnet als dynamische Gateway in Dokumentation) oder *Richtlinien* (bezeichnet als statische Gateway in Dokumentation). Weitere Informationen zu VPN-Gateways finden Sie unter [VPN-Gateways](vpn-gateway-about-vpngateways.md#vpntype).
- *GatewaySku -* möglich *Basic*, *Standard*oder *leistungsstarker*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. konfigurieren Sie das VPN-Gerät

Jetzt benötigen Sie öffentliche IP-Adresse des Gateways virtuelles Netzwerk für Ihre lokalen VPN-Gerät konfigurieren. Arbeiten Sie mit Ihrem Gerätehersteller Konfigurationsinformationen. [VPN-Geräte](vpn-gateway-about-vpn-devices.md) Informationen finden.

Verwenden Sie öffentliche IP-Adresse des virtuellen Netzwerkgateway finden im folgende Beispiel:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8 die VPN-Verbindung erstellen

Als Nächstes erstellen Sie die Standort-zu-Standort-VPN Verbindung virtuelles Netzwerk-Gateway und dem VPN-Gerät. Achten Sie darauf, dass Sie die Werte ändern. Der gemeinsame Schlüssel muss den Wert für die VPN-Konfiguration verwendet. Beachten Sie, dass die `-ConnectionType` für Standort-zu-Standort- *IPsec*ist. 

Setzen Sie die Variablen.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Erstellen der Verbindung.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Nach kurzer Zeit die Verbindung wird hergestellt. 

## <a name="toverify"></a>Überprüfen Sie die VPN-Verbindung

Es gibt unterschiedliche Weise überprüfen die VPN-Verbindung.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Ändern der IP-Adresse für das lokale Netzwerk-Gateway Präfixe

Verwenden Sie die Präfixe für Ihr lokales Netzwerkgateway ändern müssen, gehen. Zwei Sätze von Informationen dienen. Die Anweisungen wählen Sie hängen davon ab, ob Sie die gatewayverbindung bereits erstellt haben. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>So ändern Sie die Gateway IP-Adresse für das lokale Netzwerk-gateway

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte

- Sie können virtuelle Maschinen virtuelle Netzwerke hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

- Informationen über BGP finden Sie unter [Übersicht über BGP](vpn-gateway-bgp-overview.md) und [BGP konfigurieren](vpn-gateway-bgp-resource-manager-ps.md).

