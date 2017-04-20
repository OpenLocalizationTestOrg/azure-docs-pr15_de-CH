<properties
   pageTitle="Aktive S2S VPN-Verbindungen mit Azure-Ressourcen-Manager mit PowerShell Azure VPN-Gateways konfigurieren | Microsoft Azure"
   description="Dieser Artikel führt Sie durch aktive Verbindungen mit Azure-Ressourcen-Manager mit PowerShell Azure VPN-Gateways konfigurieren."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Konfigurieren Sie aktive S2S VPN-Verbindungen mit Azure-Ressourcen-Manager mit PowerShell Azure VPN-Gateways

Dieser Artikel führt Sie durch die aktive standortübergreifenden und VNet VNet-Verbindungen mit dem Ressourcen-Manager-Bereitstellungsmodell PowerShell.


**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Hochverfügbare standortübergreifenden Verbindungen

Zu hohe Verfügbarkeit für standortübergreifende und VNet VNet Konnektivität bereitzustellen mehrere VPN-Gateways und mehrere parallele Verbindung zwischen Netzwerken und Azure. Finden Sie [hoch verfügbaren standortübergreifenden und VNet VNet Konnektivität](./vpn-gateway-highlyavailable.md) Übersicht Konnektivitätsoptionen und Topologie.

Dieser Artikel beschreibt eine aktive standortübergreifende VPN-Verbindung und aktive Verbindung zwischen zwei virtuellen Netzwerken einrichten:

- [Teil 1: Erstellen und konfigurieren Sie Ihre Azure VPN-Gateway im Aktiv / Aktiv-Modus](#aagateway)

- [Teil 2 - aktive standortübergreifende Verbindung](#aacrossprem)

- [Teil 3 - aktive VNet VNet Verbindung](#aav2v)

- [Teil 4: Update vorhandenen Gateway zwischen aktiv / aktiv- und aktiv-standby](#aaupdate)

Sie kombinieren diese zu eine komplexeren, hochverfügbare Netzwerktopologie erstellen, die Ihren Bedürfnissen entspricht.

>[AZURE.IMPORTANT] Beachten Sie, dass Aktiv / Aktiv-Modus nur SKU HighPerformance funktioniert


## <a name ="aagateway"></a>Teil 1: Erstellen und konfigurieren Sie aktive VPN-Gateways

Die folgenden Schritte werden das Azure VPN-Gateway im Aktiv / Aktiv-Modus konfiguriert. Die wichtigsten Unterschiede zwischen aktiv / aktiv- und aktiv-Standby-Gateways:

- Sie müssen zwei IP-Gateway-Konfigurationen mit zwei öffentliche IP-Adressen erstellen
- Sie müssen das Flag EnableActiveActiveFeature festgelegt.
- Das Gateway SKU muss leistungsstarker

Andere Eigenschaften sind aktiv-aktiv-Gateways identisch. 

### <a name="before-you-begin"></a>Bevor Sie beginnen

- Überprüfen Sie die Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.
    
- Sie müssen den Azure-Ressourcen-Manager-PowerShell-Cmdlets installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.

### <a name="step-1---create-and-configure-vnet1"></a>Schritt 1: Erstellen und Konfigurieren von VNet1

#### <a name="1-declare-your-variables"></a>1. Deklarieren von Variablen

In dieser Übung beginnen wir durch das Deklarieren von Variablen. Im folgenden Beispiel werden die Werte für diese Übung mit Variablen deklariert. Werden Sie die Werte durch Ihren eigenen ersetzen Sie beim Konfigurieren der Produktion. Diese Variablen können Schritte, sich mit dieser Art von Konfiguration ausgeführt werden. Ändern Sie die Variablen kopieren Sie und dann fügen Sie die PowerShell-Konsole ein.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2 verbinden Sie 2 Ihr Abonnement, und erstellen Sie eine neue Ressourcengruppe

Stellen Sie sicher, dass PowerShell Modus Ressourcenmanager Cmdlets wechseln. Weitere Informationen finden Sie unter [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

Öffnen Sie die PowerShell-Konsole und Ihr Konto verbinden. Verwenden Sie Folgendes Beispiel zu verbinden:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. erstellen TestVNet1

Im folgenden Beispiel erstellt ein virtuelles Netzwerk mit dem Namen TestVNet1 und drei Subnetze, eine aufgerufene Subnetzname ein als Front-End und einem als Back-End. Werte ersetzen, es ist wichtig, immer gatewaysubnetz Namen speziell Subnetzname. Wenn Sie einen anderen Namen, scheitert Ihr Gateway erstellen. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Schritt 2 - Erstellen von VPN-Gateway für TestVNet1 mit Aktiv-Aktiv-Modus

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. erstellen Sie 1. öffentliche IP-Adressen und Gateway IP-Konfigurationen

Fordern Sie zwei öffentliche IP-Adressen zum Gateway für die VNet erstellen zugewiesen werden. Sie definieren auch die Subnetzmaske und IP-Konfigurationen erforderlich. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. erstellen Sie das VPN-Gateway mit Aktiv / Aktiv-Konfiguration

Erstellen Sie virtuelle Netzwerk-Gateway für TestVNet1. Beachten Sie, dass zwei GatewayIpConfig Einträge und das EnableActiveActiveFeature-Flag festgelegt ist. Aktiv / Aktiv-Modus erfordert eine Route-basierten VPN-Gateway HighPerformance SKU. Erstellen eines Gateways kann eine Weile dauern (30 Minuten oder länger).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. erhalten Sie Gateway öffentlichen IP-Adressen und BGP Peer-IP-Adresse

Nachdem das Gateway erstellt wurde, müssen Sie die BGP Peer-IP-Adresse auf dem Azure VPN-Gateway. Diese Adresse muss als Peer BGP für Ihre lokalen VPN-Geräte Azure VPN-Gateway konfigurieren.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Verwenden Sie die folgenden Cmdlets zwei öffentlichen IP-Adressen reserviert für das VPN-Gateway und die entsprechenden BGP Peer-IP-Adressen für jede Gatewayinstanz anzeigen:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

Die Reihenfolge der öffentlichen IP-Adressen für die Gateway-Instanzen und zugehörigen BGP Peering Adressen entsprechen. In diesem Beispiel das Gateway VM mit öffentlichen IP-40.112.190.5 verwenden 10.12.255.4 als seine BGP Peering Adresse und Gateway mit 138.91.156.129 verwenden 10.12.255.5. Diese Informationen werden benötigt, beim Einrichten der auf lokale VPN-Geräte aktiv-aktiv-Gateway. Das Gateway ist in der Abbildung unten mit Adressen dargestellt:

![aktive gateway](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Erstellte Gateway können dieses Gateway Sie aktive standortübergreifende oder VNet VNet Verbindung herstellen. In den folgenden Abschnitten werden die Schritte der Übung gehen.


## <a name ="aacrossprem"></a>Teil 2 – eine aktive standortübergreifende Verbindung

Um eine standortübergreifende Verbindung müssen Sie ein lokales Netzwerk-Gateway Darstellung des lokalen VPN-Geräts und eine Verbindung mit dem lokalen Netzwerk-Gateway die Verbindung Azure VPN-Gateway erstellen. In diesem Beispiel wird das Azure VPN-Gateway Aktiv / Aktiv-Modus Daher, obwohl nur ein VPN-Gerät (LAN Gateway) und eine Verbindungsressource lokal ist werden beide Instanzen Azure VPN-Gateway S2S VPN-Tunnel mit dem lokalen Gerät herstellen.

Stellen Sie bevor Sie fortfahren sicher, dass Sie [Teil 1](#aagateway) dieser Übung abgeschlossen haben.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Schritt 1: Erstellen und Konfigurieren des lokalen Netzwerk-Gateways

#### <a name="1-declare-your-variables"></a>1. Deklarieren von Variablen

In dieser Übung weiterhin im Diagramm dargestellte Konfiguration erstellen. Achten Sie darauf, dass die Werte ersetzen, die Sie für Ihre Konfiguration verwenden möchten.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Ein paar Dinge über das lokale Gateway Netzwerkparameter beachten:

- Das lokale Netzwerk-Gateway kann demselben oder einem anderen Standort und Ressourcengruppe VPN-Gateway. Dieses Beispiel zeigt ihnen in verschiedenen Ressourcengruppen jedoch in Azure Speicherort.

- Ist nur eine lokale VPN-Gerät wie oben gezeigt, kann die aktive Verbindung mit oder ohne BGP-Protokoll arbeiten. Dieses Beispiel verwendet BGP für die standortübergreifende Verbindung.

- Bei aktiviertem BGP ist das Präfix für das lokale Netzwerk-Gateway deklarieren müssen die Hostadresse Ihre BGP Peer-IP-Adresse auf dem VPN-Gerät. In diesem Fall ist es ein /32 Präfix "10.52.255.253/32".

- Erinnerung verwenden Sie unterschiedliche BGP ASNs zwischen lokalen Netzwerken und Azure VNet. Identisch sind, müssen Sie Ihr VNet ASN nutzt Ihre lokalen VPN-Gerät bereits das ASN zu anderen Nachbarn BGP ändern.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. erstellen Sie das lokale Netzwerk-Gateway für Site5
    
Bevor Sie fortfahren, stellen Sie sicher, dass Sie noch Abonnement 1 verbunden sind. Erstellen Sie die Ressourcengruppe, wenn es noch nicht erstellt wurde.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Schritt 2 - VNet-Gateway und lokale Netzwerk-Gateway herstellen

#### <a name="1-get-the-two-gateways"></a>1. erhalten Sie zwei gateways

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. erstellen Sie TestVNet1 Site5 Verbindung

In diesem Schritt erstellen Sie die Verbindung von TestVNet1 zu Site5_1 mit "EnableBGP" auf $True festgelegt.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN und BGP-Parameter für das lokale VPN-Gerät

Im folgenden Beispiel sind die Parameter eingegebene in BGP-Konfigurationsabschnitt auf dem lokalen VPN-Gerät für diese Übung wird:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.253
    - Präfixe anzukündigen: (Beispiel) 10.51.0.0/16 und 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 Tunnel 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 Tunnel 138.91.156.129
    - Statische Routen: Ziel 10.12.255.4/32, Nexthop VPN-Tunnel Schnittstelle für 40.112.190.5 Ziel 10.12.255.5/32 Nexthop-Schnittstelle des VPN-Tunnels für 138.91.156.129
    - eBGP Multihop: gewährleisten eBGP auf Ihrem Gerät aktiviert ist, bei Bedarf die Option "Multihop"

Die Verbindung sollte nach einigen Minuten und peering BGP-Sitzung wird gestartet, nachdem die IPSec-Verbindung. In diesem Beispiel wurde bislang nur eine lokale VPN-Gerät, was im Diagramm unten konfiguriert:

![aktiv-aktiv-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Schritt 3 - Geräte zwei lokalen VPN-aktive VPN-Gateway

Haben Sie zwei VPN-Geräte in demselben lokalen Netzwerk können Sie zwei Redundanz Azure VPN-Gateway, das zweite VPN-Gerät anschließen.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. erstellen Sie 1. das zweite LAN Gateway für Site5

Beachten Sie, dass die Gateway IP-Adresse Adresspräfix und BGP peering Adresse für den zweiten LAN Gateway frühere LAN Gateway für das lokale Netzwerk nicht überlappen müssen. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. Schließen Sie VNet-Gateway und das zweite LAN gateway

Erstellen Sie die Verbindung von TestVNet1 zu Site5_2 mit "EnableBGP" auf $True festgelegt

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN und BGP-Parameter für das zweite lokalen VPN-Gerät

Ebenso unter Listen die Parameter geben Sie in das zweite VPN-Gerät ein:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Präfixe anzukündigen: (Beispiel) 10.51.0.0/16 und 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 Tunnel 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 Tunnel 138.91.156.129
    - Statische Routen: Ziel 10.12.255.4/32, Nexthop VPN-Tunnel Schnittstelle für 40.112.190.5 Ziel 10.12.255.5/32 Nexthop-Schnittstelle des VPN-Tunnels für 138.91.156.129
    - eBGP Multihop: gewährleisten eBGP auf Ihrem Gerät aktiviert ist, bei Bedarf die Option "Multihop"

Sobald die Verbindung (Tunnel) hergestellt werden, haben Sie zwei redundante VPN-Geräte und Tunnel lokalen Netzwerk und Azure:

![Dual-Redundanz-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Teil 3 - Verbindung eine aktive VNet VNet

In diesem Abschnitt erstellt eine aktive VNet VNet Verbindung mit BGP. 

Hinweise weiter oben aufgeführten Schritte. Führen Sie zum Erstellen und Konfigurieren von TestVNet1 und VPN-Gateway mit BGP [Teil 1](#aagateway) . 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Schritt 1 - erstellen Sie TestVNet2 und VPN-gateway

Es ist wichtig sicherzustellen, dass IP-Adressbereich des neuen virtuellen Netzwerk, TestVNet2, mit einem VNet Bereiche nicht überlappen.

In diesem Beispiel gehören die virtuellen Netzwerke dieselbe Abonnement. Sie können VNet VNet Verbindungen zwischen verschiedenen Abonnements festlegen. [Konfigurieren einer Verbindung VNet VNet](./vpn-gateway-vnet-vnet-rm-ps.md) Weitere Informationen zu finden. Sicherstellen, dass Sie Hinzufügen der "-EnableBgp $True" beim Erstellen von Verbindungen zu BGP aktivieren.

#### <a name="1-declare-your-variables"></a>1. Deklarieren von Variablen

Achten Sie darauf, dass die Werte ersetzen, die Sie für Ihre Konfiguration verwenden möchten.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. erstellen Sie TestVNet2 in die neue Ressourcengruppe

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. erstellen Sie aktive VPN-Gateway für TestVNet2

Fordern Sie zwei öffentliche IP-Adressen zum Gateway für die VNet erstellen zugewiesen werden. Sie definieren auch die Subnetzmaske und IP-Konfigurationen erforderlich. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

VPN-Gateway als Zahl mit dem Flag "EnableActiveActiveFeature" erstellen. Beachten Sie, dass standardmäßig ASN für Ihren Azure VPN-Gateways überschrieben werden müssen. ASNs für verbundenen VNets müssen BGP und Übertragung routing unterscheiden.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Schritt 2 - TestVNet1 und TestVNet2-Gateways verbinden

In diesem Beispiel sind beide Gateways in dieselbe Abonnement. Sie können diesen Schritt in der PowerShell-Sitzung abschließen.

#### <a name="1-get-both-gateways"></a>1. erhalten Sie beide gateways

Vergewissern Sie sich anmelden und Abonnement 1 an.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. erstellen Sie beide Verbindungen

In diesem Schritt wird die Verbindung zwischen TestVNet1 und TestVNet2 und die Verbindung von TestVNet2, TestVNet1 erstellen.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Achten Sie darauf, dass BGP für beide Verbindungen aktiviert.

Nach Abschluss dieser Schritte Herstellen der Verbindung in wenigen Minuten und das BGP werden peering Sitzung Abschluss von VNet VNet Verbindung mit zwei Redundanz:

![aktiv-aktiv-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Teil 4: Update vorhandenen Gateway zwischen aktiv / aktiv- und aktiv-standby

Der letzte Abschnitt beschreibt ein vorhandenes Azure VPN-Gateways von aktiven Standby Aktiv / Aktiv-Modus (oder umgekehrt) konfigurieren.

>[AZURE.IMPORTANT] Beachten Sie, dass Aktiv / Aktiv-Modus nur SKU HighPerformance funktioniert

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Konfigurieren Sie einen aktiv-Standby-Gateway aktive Gateway

#### <a name="1-gateway-parameters"></a>1. Gateway-Parameter

Im folgende Beispiel konvertiert einen aktiv-Standby-Gateway in einer aktiv / aktiv-Gateway. Sie möchten erstellen eine andere öffentliche IP-Adresse und eine zweite Gateway-IP-Konfiguration hinzufügen. Folgende enthält Parameter:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. erstellen Sie öffentliche IP-Adresse und fügen die zweite Gateway IP-Konfiguration hinzu

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3 aktiv / aktiv-Modus aktivieren und das Gateway aktualisieren

Legen Sie in PowerShell die eigentliche Aktualisierung Auslösen des Gateway-Objekts. SKU des Gatewayobjekts muss auch, leistungsstarker geändert werden, da bereits als Standard erstellt wurde.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

Dieses Update kann 30 bis 45 Minuten dauern.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Konfigurieren Sie einen Gateway aktiv / aktiv-aktiv-Standby-Gateway

#### <a name="1-gateway-parameters"></a>1. Gateway-Parameter

Verwenden Sie dieselben Parameter wie oben, Namen Sie den der IP-Konfiguration zu entfernen.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. entfernen Sie die Gateway-IP-Konfiguration und deaktivieren Aktiv / Aktiv-Modus

Ebenso müssen Sie in PowerShell die eigentliche Aktualisierung Auslösen des Gateway-Objekts festlegen.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Diese Aktualisierung kann bis zu 30 bis 45 Minuten dauern.


## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss die Verbindung können Sie virtuelle Computer, virtuelle Netzwerke hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

