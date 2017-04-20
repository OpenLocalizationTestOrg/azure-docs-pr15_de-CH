### <a name="to-verify-your-connection-by-using-powershell"></a>Überprüfen die Verbindung mit PowerShell

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

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Überprüfen Sie die Verbindung mithilfe des Azure-Portals

In Azure-Portal können Sie den Status der Verbindung durch Navigieren zur Verbindung anzeigen. Es gibt mehrere Methoden zur Verfügung. Die folgenden Schritte zeigen eine Möglichkeit, navigieren Sie zu der Verbindung und überprüfen.

1. [Azure-Portal](http://portal.azure.com)auf **alle Ressourcen** und Ihrem virtuellen Netzwerk-Gateway navigieren.
2. Klicken Sie auf dem Blatt für Ihr virtuelles Netzwerk-Gateway auf **Verbindungen**. Sie können den Status jeder Verbindung anzeigen.
3. Klicken Sie auf den Namen der Verbindung, die Sie öffnen **Essentials**überprüfen möchten. In Essentials können Sie Informationen zu Ihrer Verbindung anzeigen. **Status** ist "Erfolgreich" und "Verbunden", wenn eine Verbindung hergestellt haben.

    ![Überprüfen der Verbindung](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)