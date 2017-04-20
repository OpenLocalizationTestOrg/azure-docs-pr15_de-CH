<properties
   pageTitle="BGP Azure Ressourcenmanager mit PowerShell Azure VPN-Gateways konfigurieren | Microsoft Azure"
   description="Dieser Artikel führt Sie durch Konfigurieren von BGP mit Azure-Ressourcen-Manager mit PowerShell Azure VPN-Gateways."
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
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>BGP Azure Ressourcenmanager mit PowerShell Azure VPN-Gateways konfigurieren

Dieser Artikel führt Sie schrittweise BGP eine standortübergreifende VPN-Standort-zu-Standort (S2S) Verbindung zu einem VNet VNet Verbindung mit Ressourcen-Manager-Bereitstellungsmodell und PowerShell aktivieren.


**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>Über BGP

BGP ist das standard Protokoll routing und Erreichbarkeit Informationsaustausch zwischen zwei oder mehr Netzwerke im Internet verbreitet. BGP ermöglicht Azure VPN-Gateways und die lokalen VPN-Geräte BGP Peers genannt Nachbarn austauschen "weitergeleitet", die beide Gateways zu Verfügbarkeit und Erreichbarkeit für diese Präfixe zu Gateways oder Router beteiligten informieren. BGP ermöglichen auch die Übertragung routing zwischen mehreren Netzwerken Routen lernt BGP Gateway von einem BGP Peer an alle anderen Peers BGP weitergegeben.

Weitere Informationen zu Vorteilen BGP und technischen Vorschriften und Aspekte der Verwendung von BGP finden Sie in der [Übersicht von BGP mit Azure VPN-Gateways](./vpn-gateway-bgp-overview.md) .

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Erste Schritte mit BGP Azure VPN-Gateways

Dieser Artikel führt Sie durch die Schritte für die folgenden Aufgaben:

- [Teil 1: Aktivieren BGP Azure VPN-Gateways](#enablebgp)

- [Teil 2 – Verbindung mit BGP standortübergreifende](#crossprembgp)

- [Teil 3: Verbindung mit BGP VNet VNet](#v2vbgp)

Jeder Teil der Anleitung bildet ein Grundbaustein für die Netzwerkkonnektivität BGP ermöglicht. Wenn Sie alle drei Teile abgeschlossen haben, erstellen Sie Topologie, wie im folgenden Diagramm dargestellt:

![BGP-Topologie](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Kombinieren Sie diese miteinander vernetzen komplexer, mehrere Hoffnung, Übertragung, die Ihren Bedürfnissen entsprechen.

## <a name ="enablebgp"></a>Teil 1 - BGP Azure VPN-Gateway konfigurieren

Die folgenden Konfigurationsschritte werden Parameter BGP Azure VPN-Gateway einrichten, wie im folgenden Diagramm dargestellt:

![BGP-Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Bevor Sie beginnen

- Überprüfen Sie die Azure-Abonnement. Wenn Sie bereits ein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnementvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder Zeichen, für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)aktivieren.
    
- Sie müssen den Azure-Ressourcen-Manager-PowerShell-Cmdlets installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.

### <a name="step-1---create-and-configure-vnet1"></a>Schritt 1: Erstellen und Konfigurieren von VNet1 

#### <a name="1-declare-your-variables"></a>1. Deklarieren von Variablen

In dieser Übung beginnen wir durch das Deklarieren von Variablen. Im folgenden Beispiel werden die Werte für diese Übung mit Variablen deklariert. Werden Sie die Werte durch Ihren eigenen ersetzen Sie beim Konfigurieren der Produktion. Diese Variablen können Schritte, sich mit dieser Art von Konfiguration ausgeführt werden. Ändern Sie die Variablen kopieren Sie und dann fügen Sie die PowerShell-Konsole ein.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
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
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

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

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Schritt 2 - VPN-Gateway BGP-Parameter für TestVNet1 erstellen

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. erstellen Sie 1. die IP- und Subnet-Konfigurationen

Fordern Sie eine öffentliche IP-Adresse Gateway zugeordnet werden, die Sie für das VNet erstellen. Sie definieren auch die Subnetzmaske und IP-Konfigurationen erforderlich. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. erstellen Sie VPN-Gateway mit der AS-Nummer

Erstellen Sie virtuelle Netzwerk-Gateway für TestVNet1. Beachten Sie, dass BGP Route-basierten VPN-Gateway und Parameters Zusatz - Asn, ASN (AS-Nummer) für TestVNet1 festlegen muss. Erstellen eines Gateways kann eine Weile dauern (30 Minuten oder länger).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. beziehen Sie Azure BGP Peer-IP-Adresse

Nachdem das Gateway erstellt wurde, müssen Sie die BGP Peer-IP-Adresse auf dem Azure VPN-Gateway. Diese Adresse muss als Peer BGP für Ihre lokalen VPN-Geräte Azure VPN-Gateway konfigurieren.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

Der letzte Befehl zeigt die entsprechenden BGP-Konfigurationen auf Azure VPN-Gateway; Zum Beispiel:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Erstellte Gateway können dieses Gateway Sie standortübergreifenden oder VNet VNet Verbindung mit BGP einrichten. In den folgenden Abschnitten werden die Schritte der Übung gehen.

## <a name ="crossprembbgp"></a>Teil 2 – Verbindung mit BGP standortübergreifende

Um eine standortübergreifende Verbindung müssen Sie ein lokales Netzwerk-Gateway Darstellung des lokalen VPN-Geräts und eine Verbindung mit dem lokalen Netzwerk-Gateway die Verbindung Azure VPN-Gateway erstellen. Dieser Artikel unterscheidet die zusätzlichen Eigenschaften BGP-Parameter angeben.

![BGP für standortübergreifende](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Stellen Sie bevor Sie fortfahren sicher, dass Sie [Teil 1](#enablebgp) dieser Übung abgeschlossen haben.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Schritt 1: Erstellen und Konfigurieren des lokalen Netzwerk-Gateways

#### <a name="1-declare-your-variables"></a>1. Deklarieren von Variablen

In dieser Übung weiterhin im Diagramm dargestellte Konfiguration erstellen. Achten Sie darauf, dass die Werte ersetzen, die Sie für Ihre Konfiguration verwenden möchten.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Ein paar Dinge über das lokale Gateway Netzwerkparameter beachten:

- Das lokale Netzwerk-Gateway kann demselben oder einem anderen Standort und Ressourcengruppe VPN-Gateway. Dieses Beispiel zeigt ihnen in verschiedenen Gruppen an verschiedenen Standorten.

- Minimale Präfix für das lokale Netzwerk-Gateway deklarieren müssen ist die Hostadresse Ihre BGP Peer-IP-Adresse auf dem VPN-Gerät. In diesem Fall ist es ein /32 Präfix "10.52.255.254/32".

- Erinnerung verwenden Sie unterschiedliche BGP ASNs zwischen lokalen Netzwerken und Azure VNet. Identisch sind, müssen Sie Ihr VNet ASN ändern Ihre lokalen VPN-Gerät bereits das ASN zu anderen BGP-Nachbarn.
    
Bevor Sie fortfahren, stellen Sie sicher, dass Sie noch Abonnement 1 verbunden sind.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. erstellen Sie das lokale Netzwerk-Gateway für Site5

Achten Sie darauf, dass Sie die Ressourcengruppe erstellen, wenn sie vor dem Erstellen des lokalen Netzwerk-Gateways nicht erstellt wird. Beachten Sie die zwei zusätzliche Parameter für das lokale Netzwerk-Gateway: Asn und BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Schritt 2 - VNet-Gateway und lokale Netzwerk-Gateway herstellen

#### <a name="1-get-the-two-gateways"></a>1. erhalten Sie zwei gateways

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. erstellen Sie TestVNet1 Site5 Verbindung

In diesem Schritt erstellen Sie die Verbindung von TestVNet1 auf Site5. Geben Sie "-EnableBGP $True" BGP für diese Verbindung zu aktivieren. Wie bereits erwähnt, kann sowohl BGP-BGP Anschlüsse für dieselbe Azure VPN-Gateway. Wenn BGP in die Connection-Eigenschaft aktiviert ist, wird Azure BGP für diese Verbindung aktivieren nicht, obwohl BGP Parameter bereits auf beiden Gateways konfiguriert sind.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


Im folgenden Beispiel sind die Parameter eingegebene in BGP-Konfigurationsabschnitt auf dem lokalen VPN-Gerät für diese Übung wird:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Präfixe anzukündigen: (Beispiel) 10.51.0.0/16 und 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP: 10.12.255.30
    - Statische Route: Hinzufügen einer Route für 10.12.255.30/32, mit Nexthop als VPN-Tunnel-Schnittstelle auf Ihrem Gerät
    - eBGP Multihop: gewährleisten eBGP auf Ihrem Gerät aktiviert ist, bei Bedarf die Option "Multihop"

Die Verbindung sollte nach einigen Minuten und peering BGP-Sitzung wird gestartet, nachdem die IPSec-Verbindung.
 
## <a name ="v2vbgp"></a>Teil 3: Verbindung mit BGP VNet VNet

Dieser Abschnitt fügt VNet VNet Verbindung mit BGP, wie im folgenden Diagramm dargestellt. 

![BGP für VNet VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Hinweise weiter oben aufgeführten Schritte. Sie müssen [Teil I](#enablebgp) erstellen und Konfigurieren von TestVNet1 und VPN-Gateway mit BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Schritt 1 - erstellen Sie TestVNet2 und VPN-gateway

Es ist wichtig sicherzustellen, dass IP-Adressbereich des neuen virtuellen Netzwerk, TestVNet2, mit einem VNet Bereiche nicht überlappen.

In diesem Beispiel gehören die virtuellen Netzwerke dieselbe Abonnement. Sie können VNet VNet Verbindung zwischen verschiedenen Abonnements einrichten; [Konfigurieren einer Verbindung VNet VNet](./vpn-gateway-vnet-vnet-rm-ps.md) Weitere Informationen zu finden. Sicherstellen, dass Sie Hinzufügen der "-EnableBgp $True" beim Erstellen von Verbindungen zu BGP aktivieren.

#### <a name="1-declare-your-variables"></a>1. Deklarieren von Variablen

Achten Sie darauf, dass die Werte ersetzen, die Sie für Ihre Konfiguration verwenden möchten.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
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
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. erstellen Sie TestVNet2 in die neue Ressourcengruppe

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. Erstellen des VPN-Gateways für TestVNet2 BGP-Parameter

Fordern Sie eine öffentliche IP-Adresse Gateway zugeordnet werden, die Sie für das VNet erstellen. Sie definieren auch die Subnetzmaske und IP-Konfigurationen erforderlich. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

Erstellen Sie VPN-Gateway mit der AS-Nummer. Beachten Sie, dass standardmäßig ASN für Ihren Azure VPN-Gateways überschrieben werden müssen. ASNs für verbundenen VNets müssen BGP und Übertragung routing unterscheiden.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

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

Nach Abschluss dieser Schritte Herstellen der Verbindung wird werden in wenigen Minuten, BGP peering Sitzung einmal ist die VNet VNet Verbindung hergestellt.

Wenn Sie alle drei Teile dieser Übung abgeschlossen haben, haben Sie werden eine Netzwerktopologie eingerichtet, wie unten dargestellt:

![BGP für VNet VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss die Verbindung können Sie virtuelle Computer, virtuelle Netzwerke hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

