Sie können überprüfen, daß Ihre Verbindung mit der `Get-AzureRmVirtualNetworkGatewayConnection` -Cmdlet, mit oder ohne `-Debug`. 

1. Verwenden Sie Cmdlet beispielsweise Werte eigene entsprechend konfigurieren. Wählen Sie ggf. 'A', 'All' ausführen. Im Beispiel `-Name` verweist auf den Namen der Verbindung, die Sie erstellt und getestet.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Nach Abschluss der Cmdlets zeigen Sie Werte an Im folgenden Beispiel zeigt den Status der Verbindung als "Verbunden" und Ingress- und Egress Bytes angezeigt.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }