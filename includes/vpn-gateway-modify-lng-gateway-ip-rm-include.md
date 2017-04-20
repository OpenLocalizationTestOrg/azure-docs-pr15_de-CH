Ändern Sie die IP-Adresse des Gateways verwenden die `New-AzureRmVirtualNetworkGatewayConnection` Cmdlet. Solange Sie dem Namen des lokalen Gateways genau wie den vorhandenen Namen, überschreibt die Einstellung. Zurzeit unterstützt das Cmdlet "Set" nicht die Gateway-IP-Adresse ändern.

### <a name="gwipnoconnection"></a>Gateway IP-Adresse - Gateway-Verbindung ändern

Verwenden Sie, um die Gateway IP-Adresse für Ihr LAN Gateway aktualisieren, die eine Verbindung noch nicht im folgenden Beispiel. Sie können auch die Adresspräfixe gleichzeitig aktualisieren. Eingegebenen Einstellungen überschreibt die vorhandene Einstellung. Achten Sie darauf, dass der vorhandene Name Ihres lokalen Netzwerks Gateways verwendet. Andernfalls werden Sie ein neues LAN-Gateway erstellen nicht vorhandenen überschreiben.

Verwenden Sie beispielsweise die eigene Werte ersetzen.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Gateway IP-Adresse - vorhandene Gateway-Verbindung ändern

Existiert bereits eine gatewayverbindung müssen Sie zuerst die Verbindung zu entfernen. Dann können Sie die Gateway-IP-Adresse ändern und eine neue Verbindung erstellen. Dies führt in einigen Ausfallzeiten für die VPN-Verbindung.


>[AZURE.IMPORTANT] Löschen Sie das VPN-Gateway. Wenn Sie dies tun, müssen Sie zurück durch die Schritte zum neu erstellen sowie Konfigurieren des lokale Routers die IP-Adresse, die dem neu erstellten Gateway zugewiesen werden.
 

1. Entfernen Sie die Verbindung. Suchen Sie den Namen der Verbindung mit dem `Get-AzureRmVirtualNetworkGatewayConnection` Cmdlet.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Ändern Sie den Wert GatewayIpAddress. Sie können auch Ihre Adresspräfixe zu diesem Zeitpunkt bei Bedarf ändern. Beachten Sie, dass das vorhandene LAN gatewayeinstellungen überschrieben werden. Verwenden Sie den vorhandenen Namen Ihres lokalen Netzwerks Gateways, wenn ändern, sodass die Einstellung überschrieben. Andernfalls werden Sie ein neues LAN-Gateway erstellen nicht vorhandenen ändern.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Erstellen der Verbindung. In diesem Beispiel konfigurieren wir einen IPSec-Verbindungstyp. Wenn Sie die Verbindung neu erstellen, verwenden Sie den Verbindungstyp angegeben ist für Ihre Konfiguration. Finden Sie weitere Typen der [PowerShell-Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) .  Zum Abrufen des Namens VirtualNetworkGateway führen Sie das `Get-AzureRmVirtualNetworkGateway` Cmdlet.

    Legen Sie die Variablen:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Die Verbindung zu erstellen:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

