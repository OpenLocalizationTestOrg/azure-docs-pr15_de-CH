<properties 
   pageTitle="VPN-Gateway-Einstellungen für virtuelle Netzwerkgateways | Microsoft Azure"
   description="Erfahren Sie mehr über VPN-Gateway-Einstellungen für Azure Virtual Network."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>VPN-Gateway-Einstellungen

Eine VPN-Gateway Verbindung Lösung basiert auf der Konfiguration mehrerer Ressourcen um Netzwerkverkehr zwischen virtuellen Netzwerken senden und lokalen Speicherorten. Jede Ressource enthält konfigurierbarer. Die Kombination der Ressourcen und bestimmt das Ergebnis Verbindung.

Den Abschnitten in diesem Artikel werden Ressourcen und Einstellungen auf einem VPN-Gateway in der **Ressourcen-Manager** -Bereitstellungsmodell. Sie können es die verfügbaren Varianten anzeigen Verbindung Topologiediagramme hilfreich. Beschreibung und Topologiediagramme für jede Verbindung Lösung im Artikel [Über VPN-Gateway](vpn-gateway-about-vpngateways.md) gefunden werden. 

## <a name="gwtype"></a>Gatewaytypen

Jedes virtuelles Netzwerk haben nur ein virtuelles Netzwerk-Gateway jedes Typs. Wenn Sie ein virtuelles Netzwerk-Gateway erstellen, müssen Sie sicherstellen, dass Gatewaytyp für Ihre Konfiguration korrekt ist.

Die verfügbaren Werte für - GatewayType sind: 

- VPN
- ExpressRoute

Eine VPN-Gateway muss das `-GatewayType` *Vpn*.  

Beispiel:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>Gateway-SKUs


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Konfigurieren von Gateway SKU

**Das Gateway SKU angeben in Azure-portal**

Wenn Sie Azure-Portal ein Ressourcen-Manager virtuelle Netzwerk-Gateway erstellen, können Sie das Gateway SKU mithilfe der Dropdown-Liste auswählen. Optionen mit präsentiert entsprechen Gatewaytyp und VPN-Typ, den Sie auswählen.

Beispielsweise bei Auswahl von Gatewaytyp "VPN" und den VPN-Typ "Policy-basierte' anzeigen 'Basic' SKU, die nur für Richtlinien VPNs SKU ist Bei Auswahl von "Route-basierte" können Sie Basic, Standard und leistungsstarker SKUs auswählen. 


**Angeben des Gateways SKU mit PowerShell**


PowerShell-Indexnummer gibt die `-GatewaySku` als *Standard*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Ein Gateway SKU ändern**

Möchten Sie Ihr Gateway SKU auf eine leistungsfähigere SKU (Basic/Standard leistungsstarker) aktualisieren können Sie die `Resize-AzureRmVirtualNetworkGateway` PowerShell-Cmdlet. Sie können das Gateway SKU Größe mit diesem Cmdlet heruntergestuft werden.

Folgende Beispiel PowerShell Gateway SKU leistungsstarker angepasst wird.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Geschätzte Gesamtdurchsatz Gateway SKU und Typ

Die folgende Tabelle zeigt die Gateways und geschätzte aggregierte Durchsatz. Diese Tabelle gilt für den Ressourcen-Manager und klassischen Bereitstellungsmodelle.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Verbindungstypen

In der Ressourcen-Manager-Bereitstellungsmodell erfordert jede Konfiguration einer bestimmten virtuellen Gateway Netzwerkverbindungstyp. Die verfügbaren Ressourcen-Manager PowerShell Werte für `-ConnectionType` sind:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

Im folgenden Beispiel PowerShell erstellen wir eine S2S Verbindung, die den Verbindungstyp *IPsec*erfordert.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>VPN-Typen

Beim Erstellen des virtuellen Netzwerk-Gateways für ein VPN-Gateway-Konfiguration müssen Sie einen VPN-Typ angeben. VPN-Typ, den Sie auswählen hängt die Verbindungstopologie, die Sie erstellen möchten. Beispielsweise erfordert eine P2S Verbindung RouteBased VPN-Typ. Ein VPN kann auch Hardware abhängig, die Sie verwenden. S2S Konfigurationen erfordern ein VPN-Gerät. Einige VPN-Geräte unterstützen nur einen bestimmte VPN-Typ.

Der VPN-Typ erfüllen muss alle Verbindung Vorschriften für die Projektmappe erstellen möchten. Beispielsweise möchten Sie erstellen eine Verbindung S2S VPN-Gateway und ein P2S VPN-Gateway für das virtuelle Netzwerk, verwenden VPN-Typ *RouteBased* Sie da P2S einen RouteBased VPN erfordert. Sie müssten auch überprüfen, ob das VPN-Gerät eine Verbindung RouteBased VPN unterstützt. 

Erstellte virtuelle Netzwerk-Gateway können Sie VPN-Typ nicht ändern. Sie müssen virtuelle Netzwerk-Gateway löschen und eine neue erstellen. Es gibt zwei VPN-Typen:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


PowerShell-Indexnummer gibt die `-VpnType` als *RouteBased*. Beim Erstellen eines Gateways müssen Sie sicherstellen, dass - VpnType für Ihre Konfiguration korrekt ist. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Gateway-Vorschriften

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Gatewaysubnetz

Um ein virtuelles Netzwerk-Gateway zu konfigurieren, müssen Sie zuerst ein für das VNet erstellen. Gatewaysubnetz muss *Subnetzname* ordnungsgemäß benannt werden. Diese Namen kann Azure, dass dieses Subnetz für das Gateway verwendet werden soll.

Die minimale Größe des gatewaysubnetz hängt ganz von der Konfiguration, die Sie erstellen möchten. Zwar kann ein so klein wie /29 erstellen empfiehlt ein gatewaysubnetz /28 oder größere erstellen (/ 28, /27, /26, etc..). 

Erstellen einer größeren Gateway verhindert, dass von gegen Gateway Größe ausführt. Angenommen, Sie möglicherweise ein virtuelles Netzwerk-Gateway mit Gateway Subnetz /29 für eine S2S-Verbindung erstellt. Sie wollen nun S2S/ExpressRoute konfigurieren koexistieren Konfiguration. Diese Konfiguration erfordert Gateway Subnetz Mindestgröße /28. Erstellen Sie Ihre Konfiguration müssten Sie gatewaysubnetz angepasst /28 ist die Mindestanforderung für die Verbindung ändern.

Folgende veranschaulicht Ressourcen-Manager PowerShell ein gatewaysubnetz mit dem Namen Subnetzname. Sie sehen die CIDR-Notation gibt eine /27 ermöglicht genügend IP-Adressen für die meisten Konfigurationen, die vorhanden.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>LAN-gateways

Beim Erstellen einer VPN-Gateway-Konfigurations stellt das lokale Netzwerk-Gateway häufig Ihren lokalen Standort. Im klassischen Bereitstellungsmodell wurde das lokale Netzwerk-Gateway eine lokale Website genannt. 

Sie benennen dem lokale Netzwerk-Gateway, öffentliche IP-Adresse des lokalen VPN-Gerät und Adresspräfixe, die auf den lokalen Speicherort angeben. Azure Ziel Adresspräfixe Netzwerkverkehr untersucht, konsultiert die Konfiguration, die Sie für Ihr lokales Netzwerkgateway und leitet Pakete entsprechend. Sie geben auch LAN-Gateways für VNet VNet Konfigurationen ein VPN-Gateway.

Im folgenden Beispiel wird PowerShell erstellt ein neues LAN Gateway:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Manchmal müssen Sie die LAN-Gateway ändern. Z. B. beim Hinzufügen oder ändern den Adressbereich und die IP-Adresse des VPN-Geräts ändert. Für klassische VNet können Sie diese Einstellung im Verwaltungsportal auf lokalen Netzwerken ändern. Für Ressourcen-Manager finden Sie unter [mit PowerShell Standardgatewayeinstellungen LAN ändern](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST-APIs und PowerShell-cmdlets

Für zusätzliche technische Ressourcen und spezifische Syntax bei Verwendung von REST-APIs und PowerShell-Cmdlets für VPN-Gateway-Konfigurationen Siehe:

|**Classic** | **Ressourcen-Manager**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST-API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST-API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über verfügbare verbindungskonfigurationen finden Sie unter [Über VPN-Gateway](vpn-gateway-about-vpngateways.md) . 







 
