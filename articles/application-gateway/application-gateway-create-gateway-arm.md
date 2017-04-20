<properties
   pageTitle="Erstellen, starten oder ein Gateway mit Azure Ressource-Manager löschen | Microsoft Azure"
   description="Diese Seite beschreibt das Erstellen, konfigurieren, starten und löschen ein Azure Gateway Azure-Ressourcen-Manager"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Erstellen, starten oder ein Gateway mit Azure Ressource-Manager löschen

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-gateway-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-gateway-arm.md)
- [Klassische Azure PowerShell](application-gateway-create-gateway.md)
- [Azure-Ressourcen-Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist eine Layer 7-Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Application Gateway bietet viele Application Delivery Controller (ADC) Funktionen einschließlich HTTP-Lastenausgleich cookiebasierte sitzungsaffinität, Secure Sockets Layer (SSL) ausgelagert, benutzerdefinierten Zustand Prüfpunkte, Unterstützung für mehrere Standorte und viele andere. Eine vollständige Liste der unterstützten Funktionen finden Sie auf [Gateway Anwendungsübersicht](application-gateway-introduction.md)

Dieser Artikel führt Sie durch die Schritte zum Erstellen, konfigurieren, starten und löschen ein Gateway.

>[AZURE.IMPORTANT] Vor der Verwendung von Azure-Ressourcen ist zu verstehen, dass Azure derzeit zwei Bereitstellungsmodelle: Ressourcen-Manager und Classic. Stellen Sie sicher, dass [Bereitstellungsmodelle und Tools](../azure-classic-rm.md) vor Azure Ressourcen verstehen. Die Registerkarten oben in diesem Artikel können Sie die Dokumentation für verschiedene Tools anzeigen. Dieses Dokument beschreibt ein Gateway mit Azure Ressource-Manager erstellen. Um die klassische Version zu verwenden, gehen Sie [eine klassische Anwendung Gateway-Bereitstellung mithilfe von PowerShell erstellen](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Bevor Sie beginnen

1. Installieren Sie die neueste Version von Azure PowerShell-Cmdlets mit dem Webplattform-Installer. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** Teil der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Haben Sie ein vorhandenes virtuelles Netzwerk, wählen Sie ein vorhandenes leeres Subnetz oder Erstellen eines Subnetzes im vorhandenen virtuellen Netzwerk ausschließlich für die Verwendung von Application Gateway. Application Gateway für ein anderes virtuelles Netzwerk als Ressourcen hinter dem Anwendungsgateway bereitstellen möchten, kann nicht bereitgestellt werden.
3. Sie konfigurieren das Application Gateway Server bestehen oder Endpunkten erstellt das virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist ein Gateway erstellen?

- **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. IP-Adressen sollten entweder das virtuelle Netzwerk-Subnetz angehören oder sollte eine öffentliche IP-Adresse/VIP.
- **Back-End-pooleinstellungen:** Jeder Pool hat wie Port, Protokoll und cookiebasierte Affinität. Diese sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
- **Front-End-Port:** Dieser Port ist der öffentliche Port, der auf dem Anwendungsgateway geöffnet wird. Datenverkehr trifft dieser Anschluss und dann auf einen Back-End-Server umgeleitet wird.
- **Listener:** Der Listener verfügt über einen Front-End-Port ein Protokoll (Http oder Https, diese Werte sind Groß-/Kleinschreibung) und SSL-Zertifikat (wenn offload Konfigurieren von SSL).
- **Regel:** Bindet den Listener Back-End-Serverpool und definiert die Back-End-Serverpool der Datenverkehr weitergeleitet werden soll, einen bestimmten Listener trifft.

## <a name="create-an-application-gateway"></a>Ein Gateway erstellen

Der Unterschied zwischen Azure Classic und Azure-Ressourcen-Manager ist die Reihenfolge, in der erstellt Application Gateway und die Elemente, die konfiguriert werden müssen.

Mit Ressourcen-Manager alle Elemente, ein Gateway individuell konfiguriert und dann zu Gateway Anwendungsressource.

Es folgen die Schritte, die erforderlich sind, um ein Gateway erstellen.

## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen Sie eine Ressourcengruppe für Ressourcenmanager

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Mehr steht unter [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Melden Sie sich bei Azure

    Login-AzureRmAccount

Sie werden aufgefordert, Ihre Anmeldeinformationen authentifizieren.

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto.

    Get-AzureRmSubscription

### <a name="step-3"></a>Schritt 3

Auswählen von Azure Abonnements verwenden.

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe (überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Ressourcen-Manager muss alle Ressourcengruppen einen Speicherort angeben. Dieser Speicherort wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle ein Gateway Erstellen derselben Ressourcengruppe verwendet.

Im obigen Beispiel haben wir eine Ressourcengruppe namens "Appgw-RG" und "West US" Position.

>[AZURE.NOTE] Benutzerdefinierte Überprüfung für Ihr Anwendungsgateway konfigurieren, finden Sie unter [erstellen ein Gateway mit benutzerdefinierten Prüfpunkte mit PowerShell](application-gateway-create-probe-ps.md). Checken Sie die [Benutzerdefinierte Probes und Überwachung](application-gateway-probe-overview.md) .

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und Subnetz für Application gateway

Im folgenden Beispiel wird veranschaulicht, wie ein virtuelles Netzwerk mit Ressourcen-Manager erstellen.

### <a name="step-1"></a>Schritt 1

Die Subnetz-Variable ein virtuelles Netzwerk erstellt weisen Sie 10.0.0.0/24 Bereich Adresse zu.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Schritt 2

Erstellen Sie ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in Ressource Gruppe "Appgw-Rg" Region West USA Subnetz 10.0.0.0/24 mit dem Präfix 10.0.0.0/16.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Schritt 3

Weisen Sie eine Subnetz Variable für die nächsten Schritte, die ein Gateway erstellen.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration

Erstellen einer öffentlichen IP-Ressource in Ressource Gruppe "Appgw-Rg" für die Region West US "publicIP01".

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Gateway-Konfiguration Anwendungsobjekt erstellt

Sie müssen alle Konfigurationselemente einrichten, bevor Sie Application Gateway erstellen. Die folgenden Schritte erstellen Konfigurationselemente, die für eine Ressource auf Gateway.

### <a name="step-1"></a>Schritt 1

Erstellen einer Application Gateway IP-Konfigurations mit dem Namen "gatewayIP01". Beim Start Application Gateway nimmt eine IP-Adresse aus dem Subnetz konfiguriert und IP-Adressen in der Back-End-IP-Adresspool Netzwerkverkehr weiterleitet. Denken Sie daran, dass jede Instanz eine IP-Adresse hat.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Schritt 2

Konfigurieren den Back-End-IP-Adresspool mit dem Namen "pool01" mit IP-Adressen "134.170.185.46, 134.170.188.221,134.170.185.50". Diese Adressen sind die IP-Adressen, die den Netzwerkdatenverkehr aus Front-End-IP-Endpunkt. Sie ersetzen die vorherigen Adressen eigene IP-Adresse Anwendungsendpunkte hinzufügen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Schritt 3

Konfigurieren Sie Application Gateway Einstellung "poolsetting01" für den Netzwerkverkehr mit Lastenausgleich im Back-End-Pool.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie den Front-End-IP-Port mit dem Namen "frontendport01" für die öffentliche IP-Endpunkt.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Schritt 5

Die Front-End-IP-Konfiguration mit dem Namen "fipconfig01" und die Front-End-IP-Konfiguration die öffentliche IP-Adresse zugeordnet.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Schritt 6

Erstellen Sie den Listener namens "listener01" und ordnen Sie des Front-End-Ports der Front-End-IP-Konfiguration.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Schritt 7

Erstellen Sie die Load Balancer routing Regel "rule01", die Load Balancer Verhalten konfiguriert.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Schritt 8

Konfigurieren Sie Größe der Instanz des Application-Gateways.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* ist 2 mit dem Höchstwert 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können Standard_Small, Standard_Medium und Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Erstellen Sie ein Gateway mit AzureRmApplicationGateway neu

Erstellen Sie ein Gateway mit alle Konfigurationselemente der vorangehenden Schritte. In diesem Beispiel wird das Application Gateway "Appgwtest" aufgerufen.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Schritt 9

Rufen Sie DNS und VIP Application Gateway Details aus der öffentlichen IP-Ressource an der Application Gateway ab

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Ein Gateway löschen

Gehen Sie folgendermaßen vor, um ein Gateway zu löschen:

### <a name="step-1"></a>Schritt 1

Rufen Sie des Anwendungsobjekts Gateway ab und eine Variable "$getgw" zuzuordnen.

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Schritt 2

**Stop AzureRmApplicationGateway** Application Gateway zu verwenden.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Sobald das Application Gateway beendet ist, verwenden Sie das Cmdlet **Entfernen AzureRmApplicationGateway** den Dienst entfernen.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] Die **-force** Switch zu entfernen Bestätigungsnachricht verwendet werden.

Um sicherzustellen, dass der Dienst entfernt wurde, können Sie das Cmdlet " **Get-AzureRmApplicationGateway** ". Dieser Schritt ist nicht erforderlich.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

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

SSL-Verschiebung konfigurieren, finden Sie unter [konfigurieren ein Gateway für SSL-Verschiebung](application-gateway-ssl.md).

So konfigurieren Sie einen Gateway der internen Lastenausgleich verwendet, finden Sie unter [erstellen ein Gateway mit einer internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere Informationen über Ladeoptionen Lastenausgleich im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
