Die Schritte für diese Aufgabe verwenden ein VNet anhand der nachfolgenden Werte. Zusätzliche Einstellungen und Namen werden auch in dieser Liste beschrieben. Wir verwenden nicht direkt in die Schritte dieser Liste, obwohl wir Variablen anhand der Werte in dieser Liste hinzufügen. Sie können die Liste als Referenz durch eigene Werte ersetzen.

Konfiguration aufgeführt:
    
- Virtuelles Netzwerkname = "TestVNet"
- Virtuelle Netzwerk-Adressraum = 192.168.0.0/16
- Ressourcengruppe = "TestRG"
- Subnet1 Name = "FrontEnd" 
- Adressraum Subnet1 = "192.168.0.0/16"
- Gateway Subnet-Name: "Subnetzname" müssen immer ein gatewaysubnetz *Subnetzname*benennen.
- Gateway Subnetz Adressraum = "192.168.200.0/26"
- Bereich = "East US"
- Gatewayname = "GW"
- Gateway IP-Name = "GWIP"
- Gateway-IP-Konfiguration Name = "Gwipconf"
-  Type = "ExpressRoute" dieses Typs ist für eine ExpressRoute-Konfiguration erforderlich.
- Gateway öffentlicher IP-Name = "Gwpip"


## <a name="add-a-gateway"></a>Hinzufügen eines Gateways

1. Verbinden Sie mit der Azure-Abonnement. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Deklarieren Sie die Variablen für diese Übung. In diesem Beispiel wird der Verwendung die Variablen im folgenden Beispiel verwendet. Achten Sie auf Bearbeiten, um die Einstellungen widerspiegeln, die Sie verwenden möchten. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Speichern Sie das virtuelle Netzwerkobjekt als Variable.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Ein virtuelles Netzwerk hinzufügen Gatewaysubnetz muss den Namen "Subnetzname". Möchten Sie ein Gateway erstellen, /27 oder (/ 26/25, etc..).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. Festlegen der Konfiguration.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. Speichern Sie gatewaysubnetz als Variable.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Eine öffentliche IP-Adresse anfordern. Vor dem Erstellen des Gateways ist die IP-Adresse angefordert. Sie können nicht die IP-Adresse angeben, die Sie verwenden möchten. Es wird dynamisch zugewiesen. Sie verwenden diese IP-Adresse im nächsten Konfigurationsabschnitt. Die AllocationMethod muss dynamisch sein.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. Erstellen Sie die Konfiguration für Ihr Gateway. Die Gateway-Konfiguration definiert das Subnetz und die öffentliche IP-Adresse verwenden. In diesem Schritt geben Sie die Konfiguration, die beim Erstellen der Gateways verwendet wird. Dieser Schritt erstellt tatsächlich keine Gateway-Objekts. Mit dem folgenden Beispiel können Ihre Gateway-Konfiguration erstellen. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. Erstellen Sie das Gateway. In diesem Schritt unbedingt **-GatewayType** . Verwenden Sie den Wert **ExpressRoute**. Beachten Sie, dass nach dem Ausführen dieser Cmdlets, das Gateway mindestens 20 Minuten erstellen kann.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Überprüfen Sie, ob das Gateway erstellt wurde

Verwenden Sie folgenden Befehl, ob das Gateway erstellt wurde.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Ändern der Größe eines Gateways

Es gibt eine Anzahl von [Gateway-SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Den folgenden Befehl können Gateway SKU jederzeit ändern.

>[AZURE.IMPORTANT] Dieser Befehl funktioniert nicht bei UltraPerformance Gateway. Ändern Sie Ihr Gateway auf ein Gateway UltraPerformance zuerst die vorhandenen ExpressRoute-Gateways und erstellen Sie ein neues UltraPerformance-Gateway. Um Ihr Gateway ein Gateway UltraPerformance heruntergestuft zuerst entfernen Sie UltraPerformance Gateway, und erstellen Sie ein neues Gateway.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Entfernen eines Gateways

Verwenden Sie geben Sie folgenden Befehl, um ein Gateway entfernen

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
