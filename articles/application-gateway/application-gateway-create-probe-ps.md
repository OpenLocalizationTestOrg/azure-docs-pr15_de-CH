<properties
   pageTitle="Mithilfe eine benutzerdefinierte Abfrage für Application Gateway PowerShell im Ressourcenmanager | Microsoft Azure"
   description="Erstellen Sie benutzerdefinierte Überprüfung für Application Gateway mithilfe von PowerShell in Ressourcen-Manager"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Mithilfe einer benutzerdefinierten Abfrage für Azure Application Gateway PowerShell für Azure-Manager

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-probe-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-probe-ps.md)
- [Klassische Azure PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Schritt 1

Verwenden Sie Login-AzureRmAccount zur Authentifizierung.

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto.

    Get-AzureRmSubscription

### <a name="step-3"></a>Schritt 3

Auswählen von Azure Abonnements verwenden. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe (überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Ressourcen-Manager muss alle Ressourcengruppen einen Speicherort angeben. Dieser Speicherort wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle ein Gateway Erstellen derselben Ressourcengruppe verwenden.

Im obigen Beispiel haben wir eine Ressourcengruppe namens "Appgw-RG" und "West US" Position.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und Subnetz für Application gateway

Folgendermaßen erstellen Sie ein virtuelles Netzwerk und Subnetz für Application Gateway.

### <a name="step-1"></a>Schritt 1


Eine Subnetz-Variable, die ein virtuelles Netzwerk erstellt weisen Sie 10.0.0.0/24 Bereich Adresse zu.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Schritt 2

Erstellen Sie ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in Ressource Gruppe "Appgw-Rg" Region West USA Subnetz 10.0.0.0/24 mit dem Präfix 10.0.0.0/16.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Schritt 3

Weisen Sie eine Subnetz Variable für die nächsten Schritte, die ein Gateway erstellen.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration


Erstellen einer öffentlichen IP-Ressource in Ressource Gruppe "Appgw-Rg" für die Region West US "publicIP01".

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Anwendungsobjekt Gateway-Konfiguration mit einem benutzerdefinierten erstellt

Alle Konfigurationselemente vor dem Erstellen des Anwendung Gateways eingerichtet. Die folgenden Schritte erstellen Konfigurationselemente, die für eine Ressource auf Gateway.

### <a name="step-1"></a>Schritt 1

Erstellen einer Application Gateway IP-Konfigurations mit dem Namen "gatewayIP01". Beim Start Application Gateway nimmt eine IP-Adresse aus dem Subnetz konfiguriert und IP-Adressen im Pool Back-End-IP-Netzwerk-Datenverkehr. Denken Sie daran, dass jede Instanz eine IP-Adresse hat.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Schritt 2


Konfigurieren den Back-End-IP-Adresspool mit dem Namen "pool01" mit IP-Adressen "134.170.185.46, 134.170.188.221,134.170.185.50". Diese Werte sind IP-Adressen, die den Netzwerkdatenverkehr aus Front-End-IP-Endpunkt. Ersetzen Sie die IP-Adressen über eine eigene IP-Adresse Anwendungsendpunkte hinzufügen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Schritt 3


Benutzerdefinierte Probe wird in diesem Schritt konfiguriert.

Der Parameter verwendet werden:

- **Intervall** - konfiguriert Prüfpunkt Intervall überprüft in Sekunden.
- **Timeout** - definiert Prüfpunkt Timeout für eine HTTP-Antwort-Überprüfung.
- **-Hostname und Pfad** vollständige URL-Pfad von Application Gateway bestimmt den Zustand der Instanz aufgerufen wird. Beispielsweise haben Sie eine Website http://contoso.com/ kann dann benutzerdefinierte Probe für "http://contoso.com/path/custompath.htm" Prüfpunkt Kontrollen eine erfolgreiche HTTP-Antwort konfiguriert werden.
- **UnhealthyThreshold** - die Anzahl der fehlgeschlagenen HTTP-Antworten kennzeichnen die Backend-Instanz als *fehlerhaft*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie Application Gateway Einstellung "poolsetting01" für den Datenverkehr in den Back-End-Pool. Dieser Schritt ist auch eine Timeout Konfiguration für die Back-End-Pool Antwort auf eine Anforderung Application Gateway. Trifft eine Back-End-Antwort ein Zeitlimit, bricht Application Gateway die Anforderung ab. Dieser Wert ist ein Prüfpunkt Timeout, die nur für die Back-End-Prüfpunkt überprüft auf anders.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Schritt 5

Konfigurieren Sie den Front-End-IP-Port mit dem Namen "frontendport01" für die öffentliche IP-Endpunkt.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Schritt 6

Die Front-End-IP-Konfiguration mit dem Namen "fipconfig01" und die Front-End-IP-Konfiguration die öffentliche IP-Adresse zugeordnet.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Schritt 7

Erstellen Sie den Listener namens "listener01" und ordnen Sie des Front-End-Ports der Front-End-IP-Konfiguration.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Schritt 8

Erstellen Sie die Load Balancer routing Regel "rule01", die Load Balancer Verhalten konfiguriert.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Schritt 9

Konfigurieren Sie Größe der Instanz des Application-Gateways.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  Der Standardwert für *InstanceCount* ist 2 mit dem Höchstwert 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können Standard_Small, Standard_Medium und Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Erstellen Sie ein Gateway mit AzureRmApplicationGateway neu

Erstellen Sie ein Gateway mit alle Konfigurationselemente aus den Schritten. In diesem Beispiel wird das Application Gateway "Appgwtest" aufgerufen.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Ein vorhandenes Gateway einen Prüfpunkt hinzufügen

Sie haben vier Schritte ein Gateway der vorhandenen benutzerdefinierten Test hinzu.

### <a name="step-1"></a>Schritt 1

Laden Sie Gateway-Anwendungsressource in PowerShell-Variable mithilfe von **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Schritt 2

Der Gateway-Konfiguration einen Prüfpunkt hinzufügen.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


Im Beispiel ist die benutzerdefinierte Probe konfiguriert für URL-Pfad contoso.com/path/custompath.htm alle 30 Sekunden überprüfen. Ein Schwellenwert Timeout von 120 Sekunden wird mit maximal 8 fehlgeschlagenen Suchanfragen konfiguriert.

### <a name="step-3"></a>Schritt 3

Hinzufügen den Prüfpunkt Back-End-Pool Einstellungskonfiguration und Timeout mit **-festgelegt-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Schritt 4

Speichern Sie die Konfiguration zum Application Gateway mit **Set-AzureRmApplicationGateway**.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Ein vorhandenes Gateway einen Prüfpunkt entfernen

Hier sind die Schritte zum Entfernen von benutzerdefinierten Test von einer vorhandenen Anwendungsgateway.

### <a name="step-1"></a>Schritt 1

Laden Sie Gateway-Anwendungsressource in PowerShell-Variable mithilfe von **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Schritt 2

Entfernen der Prüfpunkt Konfiguration Application Gateway mit **AzureRmApplicationGatewayProbeConfig entfernen**.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Schritt 3

Aktualisieren der Back-End-Pool-Einstellung den Prüfpunkt entfernen und Timeout-Einstellung mit **-festgelegt-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Schritt 4

Speichern Sie die Konfiguration zum Application Gateway mit **Set-AzureRmApplicationGateway**. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="get-application-gateway-dns-name"></a>Application Gateway DNS-Namen

Nachdem das Gateway erstellt wurde, besteht der nächste Schritt zum Konfigurieren von front-End für die Kommunikation. Wenn eine öffentliche IP-Adresse verwenden, erfordert Application Gateway einen dynamisch zugewiesenen DNS-Namen nicht ist. Um sicherzustellen, dass Endbenutzer das Application Gateway einen CNAME-Eintrag treffen können auf den öffentlichen Endpunkt Application Gateway verwendet werden. [Einen benutzerdefinierten Domänennamen für in Azure konfigurieren](../cloud-services/cloud-services-custom-domain-name-portal.md). Rufen Sie dazu Details Application Gateway und den zugeordneten IP/DNS-Namen mithilfe des Öffentl.IP-Elements Application Gateway zugeordnet. Application Gateway DNS-Namen zum einen CNAME-Eintrag erstellen, der zwei ASP.NET-Webanwendungen auf dem DNS-Namen verweist. Die Verwendung von A-Datensätze wird nicht empfohlen, da die VIP Neustart des Application Gateway ändern kann.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Konfigurieren SSL-Abladung auf [Konfigurieren SSL-Verschiebung](application-gateway-ssl-arm.md)

