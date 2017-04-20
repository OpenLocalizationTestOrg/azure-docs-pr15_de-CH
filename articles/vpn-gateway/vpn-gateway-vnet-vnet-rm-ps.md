<properties
   pageTitle="Verbinden von Azure VNets VPN-Gateway mit PowerShell | Microsoft Azure"
   description="Dieser Artikel führt Sie durch virtuelle Netzwerke mithilfe von Azure-Ressourcen-Manager und PowerShell verbinden."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>Konfigurieren einer Verbindung VNet VNet für Ressourcen-Manager mit PowerShell

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Classic - Verwaltungsportal](virtual-networks-configure-vnet-to-vnet-connection.md)

Dieser Artikel führt Sie durch die Schritte zum Erstellen einer Verbindung zwischen VNets in Ressourcen-Manager-Bereitstellungsmodell mit VPN-Gateway. Virtuelle Netzwerke können im gleichen oder in verschiedenen Regionen und aus demselben oder einem anderen Abonnements sein.


![V2V-Diagramm](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Bereitstellungsmodelle und Methoden VNet VNet Verbindung

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle zeigt die derzeit Bereitstellungsmodelle und Methoden VNet VNet Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir direkt aus dieser Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>VNet VNet-Verbindungen

Ein virtuelles Netzwerk mit einem anderen virtuellen Netzwerk ähnelt (VNet VNet) ein VNet mit einem lokalen Standort. Beide Konnektivität verwenden eine Azure VPN-Gateway für einen sicheren Tunnel mit IPsec-IKE. VNets die Verbindung kann in verschiedenen Bereichen. Oder in verschiedenen Subskriptionen. Sie können auch VNet VNet Kommunikation mit standortübergreifende Konfigurationen kombinieren. Diese können Sie kombinieren Netzwerktopologien standortübergreifende Konnektivität zwischen virtuellen Netzwerkkonnektivität herstellen, wie im folgenden Diagramm dargestellt:


![Über Verbindungen](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Warum verbinden virtuelle Netzwerke?

Möglicherweise möchten virtuelle Netzwerke aus den folgenden Gründen:

- **Cross-Region Geo-Redundanz und Geo-Präsenz**
    - Geo-Replikation oder Synchronisierung lassen mit sicheren sich ohne Internetzugriff Endpunkte.
    - Azure Traffic Manager und Lastenausgleich können Sie hochverfügbare Arbeitslast Geo-Redundanz über mehrere Azure Bereiche festlegen. Ein wichtiges Beispiel soll SQL ständig mit Verfügbarkeit über mehrere Azure-Regionen.

- **Regionale Multi-Tier-Anwendung mit Isolation oder administrative Grenze**
    - Innerhalb desselben Bereichs können Sie mit mehreren virtuellen Netzwerken isoliert oder Verwaltungsaufwand verbunden Multi-Tier-Anwendung einrichten.


### <a name="vnet-to-vnet-faq"></a>VNet VNet-FAQ

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Welche Schritte sollte ich verwenden?

In diesem Artikel werden zwei unterschiedliche Schritte angezeigt. Bestimmte Schritte für [VNets, die sich in der gleichen Anmeldung befinden,](#samesub)und ein weiteres für [VNets, die sich in verschiedenen Subskriptionen befinden](#difsub). Der Hauptunterschied zwischen den ist, ob Sie erstellen und konfigurieren alle virtuellen Netzwerk und Gatewayressourcen innerhalb der gleichen PowerShell-Sitzung.

Die Schritte in diesem Artikel verwenden Sie am Anfang jedes Abschnitts deklarierte Variablen. Wenn Sie bereits vorhandene VNets verwenden, ändern Sie die Einstellung in Ihrer eigenen Umgebung angepasst. 

![Beide Verbindungen](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Verbindung in dieselbe Abonnement VNets

![V2V-Diagramm](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Bevor Sie beginnen
    
Vor Beginn müssen Sie Azure Ressourcenmanager PowerShell-Cmdlets installieren. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Weitere Informationen zum Installieren von PowerShell-Cmdlets.

### <a name="Step1"></a>Schritt 1 - IP-Adressbereiche planen


In den folgenden Schritten erstellen Sie zwei virtuelle Netzwerke und ihre jeweiligen Gateway Subnetze Konfigurationen. Wir erstellen Sie eine VPN-Verbindung zwischen zwei VNets. Es ist wichtig, die IP-Adressbereiche für Ihre Netzwerkkonfiguration planen. Denken Sie daran, die Sie sicherstellen müssen, dass Ihr VNet Bereiche oder lokalen Netzwerkbereiche in keiner Weise überlappen.

Wir verwenden die folgenden Werte in den Beispielen:

**Werte für TestVNet1:**

- VNet Name: TestVNet1
- Ressourcengruppe: TestRG1
- Ort: USA OST
- TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- Back-End: 10.12.0.0/24
- Subnetzname: 10.12.255.0/27
- DNS-Server: 8.8.8.8
- GatewayName: VNet1GW
- Öffentliche IP-Adresse: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**Werte für TestVNet4:**

- VNet Name: TestVNet4
- TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- Back-End: 10.42.0.0/24
- Subnetzname: 10.42.255.0/27
- Ressourcengruppe: TestRG4
- Lage: Westen der USA
- DNS-Server: 8.8.8.8
- GatewayName: VNet4GW
- Öffentliche IP-Adresse: VNet4GWIP
- VPNType: RouteBased
- Verbindung: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Schritt 2: Erstellen und Konfigurieren von TestVNet1

1. Deklarieren von Variablen

    Zunächst Deklarieren von Variablen. Dieses Beispiel deklariert die Variablen mit den Werten für diese Übung. In den meisten Fällen sollten Sie die Werte durch Ihren eigenen ersetzen. Allerdings können Sie diese Variablen ausführen Schritte mit dieser Art von Konfiguration vertraut. Ändern Sie die ggf. Kopieren Sie und fügen Sie die PowerShell-Konsole ein.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Verbinden mit Ihrem Abonnement

    Wechseln Sie in PowerShell-Modus, um die Ressourcen-Manager-Cmdlets verwenden. Öffnen Sie die PowerShell-Konsole und Ihr Konto verbinden. Verwenden Sie das folgende Beispiel zu verbinden:

        Login-AzureRmAccount

    Überprüfen Sie die Abonnements für das Konto.

        Get-AzureRmSubscription 

    Geben Sie das Abonnement, das Sie verwenden möchten.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Erstellen Sie eine neue Ressourcengruppe

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Subnetz Konfigurationen für TestVNet1 erstellen

    Dieses Beispiel erstellt ein virtuelles Netzwerk mit dem Namen TestVNet1 und drei Subnetze, eine aufgerufene Subnetzname ein als Front-End und einem als Back-End. Werte ersetzen, es ist wichtig, immer gatewaysubnetz Namen speziell Subnetzname. Wenn Sie einen anderen Namen, scheitert Ihr Gateway erstellen. 

    Das folgende Beispiel verwendet die Variablen, die Sie zuvor festgelegt. In diesem Beispiel wird das gatewaysubnetz ein /27 verwendet. Es ist, zwar möglich, ein /29 möglichst klein erstellen empfiehlt sich die größeren Subnetzes erstellen, die mehrere Adressen enthält mindestens /28 oder /27. Damit wird genügend Adressen, die in Zukunft möglicherweise zusätzliche Konfigurationen aufzunehmen. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. TestVNet1 erstellen

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Eine öffentliche IP-Adresse anfordern

    Fordern Sie eine öffentliche IP-Adresse Gateway zugeordnet werden, die Sie für das VNet erstellen. Beachten Sie, dass die AllocationMethod dynamisch. Sie können nicht die IP-Adresse angeben, die Sie verwenden möchten. Das Gateway wird dynamisch zugewiesen. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Erstellen der Gateway-Konfigurations

    Die Gateway-Konfiguration definiert das Subnetz und die öffentliche IP-Adresse verwenden. Anhand des Beispiels die Gateway-Konfiguration erstellen. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Das Gateway für TestVNet1 erstellen

    In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway für die TestVNet1. VNet VNet-Konfigurationen benötigt einen RouteBased VpnType. Erstellen eines Gateways kann eine Weile dauern (45 Minuten oder länger).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>Schritt 3: Erstellen und Konfigurieren von TestVNet4

Nachdem Sie TestVNet1 konfiguriert haben, erstellen Sie TestVNet4. Gehen Sie, ersetzen die Werte eigene bei Bedarf. Dieser Schritt in der PowerShell-Sitzung erfolgt ist dieselbe Abonnement.

1. Deklarieren von Variablen

    Achten Sie darauf, dass die Werte ersetzen, die Sie für Ihre Konfiguration verwenden möchten.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Bevor Sie fortfahren, sicherzustellen Sie, dass Sie noch Abonnement 1 verbunden sind.

2. Erstellen Sie eine neue Ressourcengruppe

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Subnetz Konfigurationen für TestVNet4 erstellen

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. TestVNet4 erstellen

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Eine öffentliche IP-Adresse anfordern

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Erstellen der Gateway-Konfigurations

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. TestVNet4-Gateway erstellen

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Schritt 4 - Gateways verbinden

1. Beide virtuelle Netzwerkgateways abrufen

    In diesem Beispiel denn beide Gateways in dieselbe Abonnement kann diesen Schritt in der PowerShell-Sitzung abgeschlossen werden.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. TestVNet1 TestVNet4 Verbindung erstellen

    In diesem Schritt erstellen Sie die Verbindung von TestVNet1 zu TestVNet4. Sie sehen einen gemeinsamen Schlüssel in den Beispielen verwiesen. Sie können eigene Werte für den freigegebenen Schlüssel. Wichtig ist, dass der gemeinsame Schlüssel für beide Verbindungen übereinstimmen muss. Herstellen einer Verbindung kann eine kurze Weile dauern.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. TestVNet4 TestVNet1 Verbindung erstellen

    Dieser Schritt ist ähnlich, außer die Verbindung von TestVNet4 auf TestVNet1 erstellen. Sicherstellen Sie, dass die freigegebenen Schlüssel überein.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    Die Verbindung sollte nach einigen Minuten.

4. Überprüfen Sie die Verbindung. Abschnitt [die Verbindung überprüfen](#verify).


## <a name="difsub"></a>Verbindung in verschiedenen Subskriptionen VNets


![V2V-Diagramm](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

In diesem Fall schließen Sie TestVNet1 und TestVNet5. TestVNet1 und TestVNet5 befinden sich in einem anderen Abonnement. Die Schritte für diese Konfiguration hinzufügen eine zusätzliche Verbindung VNet VNet TestVNet1 TestVNet5 Verbindung. 

Der Unterschied ist, dass einige Konfigurationsschritte in einem PowerShell-Sitzung im Kontext des zweiten Abonnements ausgeführt werden. Besonders wenn zwei Abonnements zu verschiedenen Organisationen gehören. 

Die Hinweise weiter oben aufgeführten Schritte. Führen Sie [Schritt 1](#Step1) und [Schritt2](#Step2) erstellen und Konfigurieren von TestVNet1 und VPN-Gateway für TestVNet1. Wenn Sie Schritt 1 und Schritt2 abgeschlossen haben, fahren Sie mit Schritt 5, um TestVNet5 zu erstellen.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Schritt 5 - überprüfen Sie die zusätzlichen IP-Adressbereiche

Es ist wichtig sicherzustellen, dass IP-Adressbereich des neuen virtuellen Netzwerk, TestVNet5, die VNet Bereiche oder LAN Gateway Bereiche nicht überlappen. 

In diesem Beispiel können virtuelle Netzwerke zu verschiedenen Organisationen gehören. In dieser Übung können Sie die folgenden Werte für die TestVNet5:

**Werte für TestVNet5:**

- VNet Name: TestVNet5
- Ressourcengruppe: TestRG5
- Ort: Japan OST
- TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- Back-End: 10.52.0.0/24
- Subnetzname: 10.52.255.0.0/27
- DNS-Server: 8.8.8.8
- GatewayName: VNet5GW
- Öffentliche IP-Adresse: VNet5GWIP
- VPNType: RouteBased
- Verbindung: VNet5toVNet1
- ConnectionType: VNet2VNet

**Weitere Werte für TestVNet1:**

- Verbindung: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Schritt 6: Erstellen und Konfigurieren von TestVNet5

Dieser Schritt muss im Kontext des neuen Abonnements. Dieses Teil kann vom Administrator in einer anderen Organisation ausgeführt werden, die das Abonnement besitzt.

1. Deklarieren von Variablen

    Achten Sie darauf, dass die Werte ersetzen, die Sie für Ihre Konfiguration verwenden möchten.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Verbinden mit Abonnement 5

    Öffnen Sie die PowerShell-Konsole und Ihr Konto verbinden. Verwenden Sie Folgendes Beispiel zu verbinden:

        Login-AzureRmAccount

    Überprüfen Sie die Abonnements für das Konto.

        Get-AzureRmSubscription 

    Geben Sie das Abonnement, das Sie verwenden möchten.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Erstellen Sie eine neue Ressourcengruppe

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Subnetz Konfigurationen für TestVNet4 erstellen
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. TestVNet5 erstellen

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Eine öffentliche IP-Adresse anfordern

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Erstellen der Gateway-Konfigurations

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. TestVNet5-Gateway erstellen

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>Schritt 7 - Gateways verbinden

In diesem Beispiel da Gateways in verschiedenen Subskriptionen sind haben wir diesen Schritt zwei PowerShell-Sitzung als [Abonnement 1] und [Abonnement 5] aufgeteilt.

1. **[Abonnement 1]** Virtuelle Netzwerk-Gateway für Abonnement 1 abrufen

    Vergewissern Sie sich anmelden und Abonnement 1 an.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Kopieren Sie die Ausgabe der folgenden Elemente und senden Sie diese an den Administrator des Abonnements 5 per e-Mail oder eine andere Methode.

        $vnet1gw.Name
        $vnet1gw.Id

    Diese beiden Elemente müssen Werte wie die folgende Ausgabe:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Abonnement 5]** Virtuelle Netzwerk-Gateway für Abonnement 5 abrufen

    Vergewissern Sie sich anmelden und Abonnement 5 an.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Kopieren Sie die Ausgabe der folgenden Elemente und senden Sie diese an den Administrator des Abonnements 1 per e-Mail oder eine andere Methode.

        $vnet5gw.Name
        $vnet5gw.Id

    Diese beiden Elemente müssen Werte wie die folgende Ausgabe:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Abonnement 1]** TestVNet1 TestVNet5 Verbindung erstellen

    In diesem Schritt erstellen Sie die Verbindung von TestVNet1 zu TestVNet5. Der Unterschied ist, dass $vnet5gw direkt abgerufen werden kann, ist in einem anderen Abonnement. Sie müssen ein neues PowerShell-Objekt mit den Werten in den obigen Schritten Abonnement 1 übermittelt erstellen. Verwenden Sie im folgenden Beispiel. Ersetzen Sie Name, Id und Schlüsseln durch eigene Werte. Wichtig ist, dass der gemeinsame Schlüssel für beide Verbindungen übereinstimmen muss. Herstellen einer Verbindung kann eine kurze Weile dauern.

    Stellen Sie sicher, dass Abonnements 1 Verbindung. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Abonnement 5]** TestVNet5 TestVNet1 Verbindung erstellen

    Dieser Schritt ist ähnlich, außer die Verbindung von TestVNet5 auf TestVNet1 erstellen. Denselben Vorgang zum Erstellen eines PowerShell-Objekts basierend auf Werten von Abonnement 1 auch hier. In diesem Schritt unbedingt freigegebenen Schlüssel überein.

    Stellen Sie sicher, dass Abonnements 5 herstellen.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Verbindung überprüfen


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Nächste Schritte

- Nach Abschluss die Verbindung können Sie virtuelle Computer, virtuelle Netzwerke hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Informationen über BGP finden Sie unter [Übersicht über BGP](vpn-gateway-bgp-overview.md) und [BGP konfigurieren](vpn-gateway-bgp-resource-manager-ps.md). 

