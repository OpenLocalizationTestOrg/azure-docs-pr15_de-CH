<properties
   pageTitle="Erstellen und konfigurieren Sie ein Gateway mit einer internen Lastenausgleich (ILB) mit Azure Ressourcenmanager | Microsoft Azure"
   description="Diese Seite enthält eine Anleitung zum Erstellen, konfigurieren, starten und löschen eine Azure-Anwendungsgateway mit internen Lastenausgleich (ILB) für Azure-Ressourcen-Manager"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Mit Azure Ressource-Manager erstellen Sie ein Gateway mit einer internen Lastenausgleich (ILB)

> [AZURE.SELECTOR]
- [Azure klassische Schritte](application-gateway-ilb.md)
- [Ressourcenmanager PowerShell Schritte](application-gateway-ilb-arm.md)

Azure Application Gateway kann konfiguriert werden, mit einer VIP Internetzugriff oder einen internen Endpunkt, der nicht mit dem Internet auch einen internen Load Balancer (ILB) Endpunkt verfügbar gemacht wird. Konfigurieren das Gateway mit einem ILB eignet sich für interne LOB Anwendung, die nicht mit dem Internet verbunden sind. Es eignet sich auch für Dienste und Ebenen innerhalb einer Anwendung mit mehreren Ebenen, die sitzen in eine Sicherheitsgrenze, die nicht mit dem Internet verbunden ist, jedoch noch benötigen Round Robin laden Verteilung Sitzung Klebrigkeit oder Secure Sockets Layer (SSL) beendet.

Dieser Artikel führt Sie durch die Schritte zum Konfigurieren Sie ein Gateway mit einer ILB.

## <a name="before-you-begin"></a>Bevor Sie beginnen

1. Installieren Sie die neueste Version von Azure PowerShell-Cmdlets mit dem Webplattform-Installer. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** Teil der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Erstellen Sie ein virtuelles Netzwerk und ein Subnetz für Application Gateway an. Stellen Sie sicher, dass keine virtuellen Maschinen oder Cloud-Bereitstellungen im Subnetz verwenden. Application Gateway muss sich in einem virtuellen Netzwerksubnetz sein.
3. Sie konfigurieren das Application Gateway Server bestehen oder Endpunkten erstellt das virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist ein Gateway erstellen?


- **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. IP-Adressen müssen entweder an das virtuelle Netzwerk jedoch in einem anderen Subnetz für Application Gateway gehören oder sollte eine öffentliche IP-Adresse/VIP.
- **Back-End-pooleinstellungen:** Jeder Pool hat wie Port, Protokoll und cookiebasierte Affinität. Diese sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
- **Front-End-Port:** Dieser Port ist der öffentliche Port, der auf dem Anwendungsgateway geöffnet wird. Datenverkehr trifft dieser Anschluss und dann auf einen Back-End-Server umgeleitet wird.
- **Listener:** Der Listener verfügt über einen Front-End-Port ein Protokoll (Http oder Https sind Groß-/Kleinschreibung), und Name des SSL-Zertifikat (sofern offload Konfigurieren von SSL).
- **Regel:** Bindet den Listener und Back-End-Serverpool und definiert die Back-End-Serverpool der Datenverkehr weitergeleitet werden soll, einen bestimmten Listener trifft. Derzeit wird *nur grundsätzlich* unterstützt. *Die Grundregel* ist Round-Robin-Verteilung.



## <a name="create-an-application-gateway"></a>Ein Gateway erstellen

Der Unterschied zwischen Azure Classic und Azure-Ressourcen-Manager ist die Reihenfolge, in der erstellt Application Gateway und die Elemente, die konfiguriert werden müssen.
Mit Ressourcen-Manager alle Elemente, ein Gateway individuell konfiguriert und dann zu Gateway Anwendungsressource.


Hier sind die Schritte, die erforderlich sind, um ein Gateway zu erstellen:

1. Erstellen Sie eine Ressourcengruppe für Ressourcenmanager
2. Erstellen Sie ein virtuelles Netzwerk und Subnetz für Application gateway
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

Erstellen Sie eine neue Ressourcengruppe (überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Ressourcen-Manager muss alle Ressourcengruppen einen Speicherort angeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle ein Gateway Erstellen derselben Ressourcengruppe verwendet.

Im obigen Beispiel haben wir eine Ressourcengruppe namens "Appgw-Rg" und "West US" Position.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und Subnetz für Application gateway

Im folgenden Beispiel wird veranschaulicht, wie ein virtuelles Netzwerk mit Ressourcen-Manager erstellen:

### <a name="step-1"></a>Schritt 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Dies weist 10.0.0.0/24 Bereich Adresse einer Variablen Subnetz zum Erstellen eines virtuellen Netzwerks verwendet werden.

### <a name="step-2"></a>Schritt 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Dies erstellt ein virtuelles Netzwerk mit dem Namen "Appgwvnet" in Ressource Gruppe "Appgw-Rg" Region West USA Subnetz 10.0.0.0/24 mit dem Präfix 10.0.0.0/16.

### <a name="step-3"></a>Schritt 3

    $subnet = $vnet.subnets[0]

Dies weist das Subnetzobjekt Variablen $subnet für die nächsten Schritte.

## <a name="create-an-application-gateway-configuration-object"></a>Gateway-Konfiguration Anwendungsobjekt erstellt

### <a name="step-1"></a>Schritt 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Dadurch kann eine Anwendung Gateway IP-Konfiguration mit dem Namen "gatewayIP01". Beim Start Application Gateway nimmt eine IP-Adresse aus dem Subnetz konfiguriert und IP-Adressen im Pool Back-End-IP-Netzwerk-Datenverkehr. Denken Sie daran, dass jede Instanz eine IP-Adresse hat.


### <a name="step-2"></a>Schritt 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Dadurch wird den Back-End-IP-Adresspool mit dem Namen "pool01" mit IP-Adressen konfiguriert "134.170.185.46, 134.170.188.221,134.170.185.50". Dies sind die IP-Adressen, die den Netzwerkdatenverkehr aus Front-End-IP-Endpunkt. Ersetzen Sie die IP-Adressen über eine eigene IP-Adresse Anwendungsendpunkte hinzufügen.

### <a name="step-3"></a>Schritt 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Dadurch wird Application Gateway Einstellung "poolsetting01" für die Last ausgeglichen Netzwerkverkehr im Back-End-Pool konfiguriert.

### <a name="step-4"></a>Schritt 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Dadurch wird den Front-End-IP-Port mit dem Namen "frontendport01" für die ILB konfiguriert.

### <a name="step-5"></a>Schritt 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Dies erstellt die Front-End-IP-Konfiguration "fipconfig01" aufgerufen und eine Private IP-Adresse aus dem aktuellen virtuellen Netzwerksubnetz zugeordnet.

### <a name="step-6"></a>Schritt 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Dies wird den Listener mit der Bezeichnung "listener01" und den Front-End-Port der Front-End-IP-Konfiguration.

### <a name="step-7"></a>Schritt 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Dadurch kann die Load Balancer routing Regel namens "rule01", die Load Balancer Verhalten konfiguriert.

### <a name="step-8"></a>Schritt 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Größe der Instanz des Application Gateway wird konfiguriert.

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* ist 2 mit dem Höchstwert 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können Standard_Small, Standard_Medium und Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Erstellen Sie ein Gateway mit AzureApplicationGateway neu

Ein Gateway erstellt mit alle Konfigurationselemente aus den Schritten. In diesem Beispiel wird das Application Gateway "Appgwtest" aufgerufen.


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Dies erstellt ein Gateway mit alle Konfigurationselemente aus den Schritten. Im Beispiel wird das Application Gateway "Appgwtest" aufgerufen.


## <a name="delete-an-application-gateway"></a>Ein Gateway löschen

Um ein Gateway zu löschen, müssen Sie in der Reihenfolge Folgendes:

1. Das Cmdlet " **Stop AzureRmApplicationGateway** " Gateway zu verwenden.
2. Verwenden Sie das Cmdlet **Entfernen AzureRmApplicationGateway** Gateway entfernen.
3. Überprüfen Sie, ob das Gateway mit dem Cmdlet **Get-AzureApplicationGateway** entfernt wurde.


### <a name="step-1"></a>Schritt 1

Rufen Sie des Anwendungsobjekts Gateway ab und eine Variable "$getgw" zuzuordnen.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Schritt 2

**Stop AzureRmApplicationGateway** Application Gateway zu verwenden. Dieses Beispiel zeigt das Cmdlet " **Stop AzureRmApplicationGateway** " in der ersten Zeile der Ausgabe gefolgt.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Sobald das Application Gateway beendet ist, verwenden Sie das Cmdlet **Entfernen AzureRmApplicationGateway** den Dienst entfernen.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] Die **-force** Switch zu entfernen Bestätigungsnachricht verwendet werden.


Um sicherzustellen, dass der Dienst entfernt wurde, können Sie das Cmdlet " **Get-AzureRmApplicationGateway** ". Dieser Schritt ist nicht erforderlich.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Nächste Schritte

SSL-Verschiebung konfigurieren, finden Sie unter [konfigurieren ein Gateway für SSL-Verschiebung](application-gateway-ssl.md).

So konfigurieren Sie einen Gateway der mit einer ILB, finden Sie unter [erstellen ein Gateway mit einer internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere Informationen über Ladeoptionen Lastenausgleich im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
