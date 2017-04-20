
Konfigurieren Sie das VPN-Gerät benötigen Sie öffentliche IP-Adresse des Gateways virtuelles Netzwerk für Ihre lokalen VPN-Gerät konfigurieren. Arbeiten mit Ihrem Gerätehersteller Konfigurationsinformationen und konfigurieren Sie das Gerät. Finden Sie weitere Informationen zu VPN-Geräte mit Azure [VPN-Geräte](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) .

Öffentliche IP-Adresse des virtuellen Netzwerkgateway mit PowerShell zu finden, verwenden Sie Folgendes Beispiel:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Sie können auch öffentliche IP-Adresse für Ihr virtuelles Netzwerk-Gateway mithilfe von Azure-Portal anzeigen. Navigieren Sie zum **virtuellen Netzwerk-Gateways**und dann auf den Namen Ihres Gateways.