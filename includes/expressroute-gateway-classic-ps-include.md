Ein VNet und ein müssen zunächst erstellen, bevor die folgenden Aufgaben. Finden Sie weitere Informationen im Artikel [konfigurieren ein virtuelles Netzwerk mit dem Verwaltungsportal](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) .   

## <a name="add-a-gateway"></a>Hinzufügen eines Gateways

Verwenden Sie folgenden Befehl zum Erstellen eines Gateways. Achten Sie darauf, dass Sie für Ihre eigenen Werte ersetzen.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Überprüfen Sie, ob das Gateway erstellt wurde

Verwenden Sie folgenden Befehl, ob das Gateway erstellt wurde. Dieser Befehl ruft auch die Gateway-ID, die für andere Vorgänge ab.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Ändern der Größe eines Gateways

Es gibt eine Anzahl von [Gateway-SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Den folgenden Befehl können Gateway SKU jederzeit ändern.

>[AZURE.IMPORTANT] Dieser Befehl funktioniert nicht bei UltraPerformance Gateway. Ändern Sie Ihr Gateway auf ein Gateway UltraPerformance zuerst die vorhandenen ExpressRoute-Gateways und erstellen Sie ein neues UltraPerformance-Gateway. Um Ihr Gateway ein Gateway UltraPerformance heruntergestuft zuerst entfernen Sie UltraPerformance Gateway, und erstellen Sie ein neues Gateway. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Entfernen eines Gateways

Verwenden Sie geben Sie folgenden Befehl, um ein Gateway entfernen

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>