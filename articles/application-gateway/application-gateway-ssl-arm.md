<properties
   pageTitle="Ein Gateway für SSL-Verschiebung mithilfe von Azure-Ressourcen-Manager konfigurieren | Microsoft Azure"
   description="Diese Seite enthält auf ein Gateway mit SSL erstellen mithilfe von Azure-Ressourcen-Manager verschieben"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurieren Sie ein Gateway für SSL-Verschiebung mithilfe von Azure-Ressourcen-Manager

> [AZURE.SELECTOR]
-[Azure-Portal](application-gateway-ssl-portal.md)
-[Azure Ressourcenmanager PowerShell](application-gateway-ssl-arm.md)
-[Azure klassische PowerShell](application-gateway-ssl.md)

 Azure Application Gateway kann zum Beenden der Sitzung Secure Sockets Layer (SSL) am Gateway teure SSL-Entschlüsselung Aufgaben zu der Webserverfarm konfiguriert werden. SSL-Verschiebung vereinfacht auch die Front-End-Server-Setup und Verwaltung der Website.

## <a name="before-you-begin"></a>Bevor Sie beginnen

1. Installieren Sie die neueste Version von Azure PowerShell-Cmdlets mit dem Webplattform-Installer. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** Teil der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Erstellen Sie ein virtuelles Netzwerk und Subnetz für Application Gateway. Stellen Sie sicher, dass keine virtuellen Maschinen oder Cloud-Bereitstellungen im Subnetz verwenden. Application Gateway muss sich in einem virtuellen Netzwerksubnetz sein.
3. Sie konfigurieren das Application Gateway Server bestehen oder Endpunkten erstellt das virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist ein Gateway erstellen?


- **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. IP-Adressen sollten entweder das virtuelle Netzwerk-Subnetz angehören oder sollte eine öffentliche IP-Adresse/VIP.
- **Back-End-pooleinstellungen:** Jeder Pool hat wie Port, Protokoll und cookiebasierte Affinität. Diese sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
- **Front-End-Port:** Dieser Port ist der öffentliche Port, der auf dem Anwendungsgateway geöffnet wird. Datenverkehr trifft dieser Anschluss und dann auf einen Back-End-Server umgeleitet wird.
- **Listener:** Der Listener verfügt über einen Front-End-Port ein Protokoll (Http oder Https dazu sind Groß-/Kleinschreibung) und SSL-Zertifikat (wenn offload Konfigurieren von SSL).
- **Regel:** Bindet den Listener und Back-End-Serverpool und definiert die Back-End-Serverpool der Datenverkehr weitergeleitet werden soll, einen bestimmten Listener trifft. Derzeit wird *nur grundsätzlich* unterstützt. *Die Grundregel* ist Round-Robin-Verteilung.

**Zusätzliche Konfiguration Notizen**

Für SSL-Zertifikate Konfiguration sollte das Protokoll in **HttpListener** in *Https* (Groß-und Kleinschreibung) ändern. Das Element **geändert** wird durch den Variablenwert für das SSL-Zertifikat konfiguriert **HttpListener** hinzugefügt. Der Front-End-Port sollte auf 443 aktualisiert werden.

**Cookie-basierte Affinität aktivieren**: ein Gateway kann konfiguriert werden, um sicherzustellen, dass eine Anforderung einer Clientsitzung immer gleichen virtuellen Computer in der Webfarm weitergeleitet wird. Dabei erfolgt durch Injektion ein Sitzungscookie, das Gateway Datenverkehr entsprechend angewiesen ermöglicht. Um cookiebasierte Affinität zu aktivieren, legen Sie **CookieBasedAffinity** *aktiviert* im **BackendHttpSettings** -Element.


## <a name="create-an-application-gateway"></a>Ein Gateway erstellen

Der Unterschied zwischen der Azure-Bereitstellung klassisch und Azure-Ressourcen-Manager ist die Reihenfolge, die Sie erstellen, ein Gateway und Elemente, die konfiguriert werden müssen.

Mit Ressourcen-Manager alle Komponenten eines Gateways Anwendung individuell konfiguriert und dann zu einer Anwendungsressource Gateway.


Hier werden die Schritte ein Gateway erstellen:

1. Erstellen Sie eine Ressourcengruppe für Ressourcenmanager
2. Erstellen Sie virtuelles Netzwerk und Subnetz öffentliche IP-Adresse für das Anwendungsgateway
3. Gateway-Konfiguration Anwendungsobjekt erstellt
4. Erstellen Sie eine Ressource auf gateway


## <a name="create-a-resource-group-for-resource-manager"></a>Erstellen Sie eine Ressourcengruppe für Ressourcenmanager

Stellen Sie sicher, dass PowerShell Modus Azure-Ressourcen-Manager-Cmdlets verwenden, wechseln. Mehr steht unter [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto.

    Get-AzureRmSubscription

Sie werden aufgefordert, Ihre Anmeldeinformationen authentifizieren.<BR>

### <a name="step-3"></a>Schritt 3

Auswählen von Azure Abonnements verwenden. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe (überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Ressourcen-Manager muss alle Ressourcengruppen einen Speicherort angeben. Diese Einstellung wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle ein Gateway Erstellen derselben Ressourcengruppe verwendet.

Im obigen Beispiel haben wir eine Ressourcengruppe namens "Appgw-RG" und "West US" Position.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und Subnetz für Application gateway

Im folgenden Beispiel wird veranschaulicht, wie ein virtuelles Netzwerk mit Ressourcen-Manager erstellen:

### <a name="step-1"></a>Schritt 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

In diesem Beispiel weist 10.0.0.0/24 Bereich Adresse einer Variablen Subnetz zum Erstellen eines virtuellen Netzwerks verwendet werden.

### <a name="step-2"></a>Schritt 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

Dieses Beispiel erstellt ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in Ressource Gruppe "Appgw-Rg" Region West USA Subnetz 10.0.0.0/24 mit dem Präfix 10.0.0.0/16.

### <a name="step-3"></a>Schritt 3

    $subnet = $vnet.Subnets[0]

In diesem Beispiel weist das Subnetzobjekt Variablen $subnet für die nächsten Schritte.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

Dieses Beispiel erstellt eine öffentliche IP-Ressource in Ressource Gruppe "Appgw-Rg" für die Region West US "publicIP01".


## <a name="create-an-application-gateway-configuration-object"></a>Gateway-Konfiguration Anwendungsobjekt erstellt

### <a name="step-1"></a>Schritt 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Dieses Beispiel erstellt eine Application Gateway IP-Konfiguration mit dem Namen "gatewayIP01". Beim Start Application Gateway nimmt eine IP-Adresse aus dem Subnetz konfiguriert und IP-Adressen im Pool Back-End-IP-Netzwerk-Datenverkehr. Denken Sie daran, dass jede Instanz eine IP-Adresse hat.

### <a name="step-2"></a>Schritt 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

In diesem Beispiel wird den Back-End-IP-Adresspool mit dem Namen "pool01" mit IP-Adressen konfiguriert "134.170.185.46, 134.170.188.221,134.170.185.50." Diese Werte sind IP-Adressen, die den Netzwerkdatenverkehr aus Front-End-IP-Endpunkt. Die IP-Adressen der Web Anwendungsendpunkte ersetzen Sie IP-Adressen aus dem vorherigen Beispiel.

### <a name="step-3"></a>Schritt 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

In diesem Beispiel wird Application Gateway Einstellung "poolsetting01" Netzwerkverkehr mit Lastenausgleich im Back-End-Pool konfiguriert.

### <a name="step-4"></a>Schritt 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

In diesem Beispiel wird den Front-End-IP-Port mit dem Namen "frontendport01" für die öffentliche IP-Endpunkt konfiguriert.

### <a name="step-5"></a>Schritt 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

In diesem Beispiel wird das Zertifikat verwendet SSL-Verbindung konfiguriert. Das Zertifikat muss im PFX-Format und das Kennwort muss zwischen 4 und 12 Zeichen sein.

### <a name="step-6"></a>Schritt 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

In diesem Beispiel erstellt die Front-End-IP-Konfiguration mit dem Namen "fipconfig01" und ordnet die öffentliche IP-Adresse der Front-End-IP-Konfiguration.

### <a name="step-7"></a>Schritt 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


In diesem Beispiel wird der Listener "listener01" und den Front-End-Port und die Front-End-IP-Konfiguration.

### <a name="step-8"></a>Schritt 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Dieses Beispiel erstellt die Load Balancer routing Regel "rule01", die Load Balancer Verhalten konfiguriert.

### <a name="step-9"></a>Schritt 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

In diesem Beispiel konfiguriert die Instanz Application Gateway.

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* ist 2 mit dem Höchstwert 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können Standard_Small, Standard_Medium und Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Erstellen Sie ein Gateway mit AzureApplicationGateway neu

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

Dieses Beispiel erstellt ein Gateway mit alle Konfigurationselemente der vorangehenden Schritte. Im Beispiel wird das Application Gateway "Appgwtest" aufgerufen.

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

Wenn Sie einen Application Gateway mit einem internen Lastenausgleich (ILB) konfigurieren möchten, finden Sie unter [erstellen ein Gateway mit einer internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere Informationen über Ladeoptionen Lastenausgleich im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
