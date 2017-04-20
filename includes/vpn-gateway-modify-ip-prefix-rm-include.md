### <a name="noconnection"></a>Hinzufügen oder Entfernen von Präfixen - Gateway-Verbindung

- **Hinzufügen** zusätzlicher Adresspräfixe zu einem lokalen Netzwerk-Gateway, das Sie erstellt haben, aber nicht noch Gateway verbunden, das Beispiel verwenden. Achten Sie darauf, Ihren ändern.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- Verwenden **Entfernen** ein Adresspräfix von einem lokalen Netzwerk-Gateway, die eine VPN-Verbindung nicht im folgenden Beispiel. Lassen Sie die Präfixe nicht mehr benötigen. In diesem Beispiel wir mehr benötigen Präfix 20.0.0.0/24 (aus dem vorherigen Beispiel), wir aktualisieren die lokale Netzwerk-Gateway und das Präfix ausschließen.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Hinzufügen oder Entfernen von Präfixen - vorhandene Gateway-Verbindung

Wenn Sie die gatewayverbindung erstellt haben, hinzufügen oder Entfernen von IP-Adresspräfixe in Ihrem lokalen Netzwerk-Gateway müssen Sie folgende Schritte in der Reihenfolge. Dies führt in einigen Ausfallzeiten für die VPN-Verbindung. Beim Aktualisieren der Präfixe Sie zuerst die Verbindung, die Präfixe ändern, und erstellen Sie eine neue Verbindung. Müssen Sie in den folgenden Beispielen eigene ändern.

>[AZURE.IMPORTANT] Löschen Sie das VPN-Gateway. Wenn Sie dies tun, müssen Sie die Schritte neu erstellen sowie Konfigurieren des lokalen Routers bei zurück.
 
1. Entfernen Sie die Verbindung.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Ändern Sie die Adresspräfixe für Ihr lokales Netzwerkgateway.

    Legen Sie die Variable für die LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Ändern Sie die Präfixe.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Erstellen der Verbindung. In diesem Beispiel konfigurieren wir einen IPSec-Verbindungstyp. Wenn Sie die Verbindung neu erstellen, verwenden Sie den Verbindungstyp angegeben ist für Ihre Konfiguration. Finden Sie weitere Typen der [PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) .

    Legen Sie die Variable für die VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Erstellen der Verbindung. Beachten Sie, dass in diesem Beispiel die Variable $local verwendet, die im vorherigen Schritt festgelegt.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
