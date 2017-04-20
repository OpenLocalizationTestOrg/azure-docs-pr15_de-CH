<properties
   pageTitle="Web Application Firewall auf neuen oder vorhandenen Application Gateway konfigurieren | Microsoft Azure"
   description="Dieser Artikel enthält Hinweise verwenden Web Application Firewall auf ein vorhandenes oder neues Gateway."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurieren Sie Web Application Firewall auf neuen oder vorhandenen Application Gateway

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-web-application-firewall-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-web-application-firewall-powershell.md)

Die Web Application Firewall (AAF) in Azure Application Gateway verhindert allgemeine webbasierten Angriffen wie SQL Injection, Cross-Site scripting-Angriffe und Session Hijacking ASP.NET-Webanwendungen.

Azure Application Gateway ist eine Layer 7-Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Anwendung bietet viele Application Delivery Controller (ADC)-Features einschließlich HTTP-Lastenausgleich, cookiebasierte sitzungsaffinität, Secure Sockets Layer (SSL) ausgelagert, benutzerdefinierten Zustand Prüfpunkte, Unterstützung für mehrere Standorte und viele andere. Eine vollständige Liste der unterstützten Funktionen finden Sie auf Gateway Anwendungsübersicht

Der folgende Artikel zeigt [Web Application Firewall ein vorhandenes Gateway hinzufügen](#add-web-application-firewall-to-an-existing-application-gateway) und [erstellen ein Gateway, das Web Application Firewall verwendet](#create-an-application-gateway-with-web-application-firewall).

![Szenario-Bild][scenario]

## <a name="waf-configuration-differences"></a>WAF Konfigurationsunterschiede

Liest [ein Gateway mit PowerShell erstellen](application-gateway-create-gateway-arm.md)verstehen Sie die SKU konfigurieren, wenn ein Gateway erstellen. WAF bietet zusätzliche Optionen definieren, wenn die SKU ein Gateway konfigurieren. Es sind keine zusätzlichen Änderungen, die Sie auf das Application Gateway.

**SKU** - Gateway Anwendung ohne WAF **Standard\_kleine**, **Standard\_Mittel**, und **Standard\_große** Größen. Mit der Einführung von WAF werden zwei zusätzliche SKUs **WAF\_Mittel** und **WAF\_große**. WAF ist auf kleine Anwendungsgateways nicht unterstützt.

**Tier** - mögliche Werte sind **Standard** oder **WAF**. Wenn Web Application Firewall verwenden, müssen **WAF** ausgewählt.

**Modus** - diese Einstellung ist der WAF. Zulässige Werte sind **Erkennung** und **Abwehr**. Wenn im Erkennungsmodus WAF festgelegt ist, werden sämtliche Gefahren in einer Protokolldatei gespeichert. Im Schutzmodus Ereignisse weiterhin protokolliert jedoch der Angreifer das Application Gateway 403 nicht autorisierte Antwort empfängt.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Ein Gateway der vorhandenen Web Application Firewall hinzufügen

Stellen Sie sicher, dass Sie die neueste Version von Azure PowerShell verwenden. Mehr steht unter [Verwendung von Windows PowerShell mit Ressourcen-Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Schritt 1

Ein Azure-Konto anmelden.

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Wählen Sie das Abonnement für dieses Szenario verwendet.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Schritt 3

Rufen Sie das Gateway ab, dem Hinzufügen von Web Application Firewall.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Schritt 4

Konfigurieren der Web Application Firewall SKUs. Die verfügbaren Größen sind **WAF\_große** und **WAF\_Mittel**. Bei Web Application Firewall muss die Ebene **WAF**, die Kapazität muss bestätigt werden, bei der Sku.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Schritt 5

Konfigurieren der WAF wie folgt definiert:

Für die Einstellung **WafMode** werden die verfügbaren Werte Verhütung und

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Schritt 6

Aktualisieren Sie Application-Gateway mit der im vorherigen Schritt definierten Eigenschaften.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Dieser Befehl aktualisiert das Application Gateway mit Web Application Firewall. Es wird empfohlen, [Application Gateway Diagnose](application-gateway-diagnostics.md) zu verstehen, wie Protokolle für Ihr Anwendungsgateway anzeigen. Protokolle müssen aufgrund der Sicherheit WAF regelmäßig überprüft, um die Sicherheitslage einer Anwendung verstehen.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen Sie ein Gateway mit Web Application Firewall

Die folgenden Schritte führen Sie durch den gesamten Prozess von Anfang bis Ende für ein Gateway mit Web Application Firewall.

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

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Schritt 4

Erstellen Sie eine Ressourcengruppe (überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Ressourcen-Manager muss alle Ressourcengruppen einen Speicherort angeben. Dieser Speicherort wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle ein Gateway Erstellen derselben Ressourcengruppe verwendet.

Im vorherigen Beispiel haben wir eine Ressourcengruppe namens "Appgw-RG" und "West US" Position.

>[AZURE.NOTE] Benutzerdefinierte Überprüfung für Ihr Anwendungsgateway konfigurieren, finden Sie unter [erstellen ein Gateway mit benutzerdefinierten Prüfpunkte mit PowerShell](application-gateway-create-probe-ps.md). Checken Sie die [Benutzerdefinierte Probes und Überwachung](application-gateway-probe-overview.md) .

### <a name="step-5"></a>Schritt 5

Weisen Sie einen Adressbereich für das Subnetz für Application Gateway selbst verwendet werden.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Für eine Anwendung ein Subnetz müssen mindestens 28 Maskenbits. Dieser Wert lässt 10 verfügbare Adressen im Subnetz Application Gateway Instanzen. Mit einem kleineren Subnetz in der Zukunft hinzufügen weitere Instanz des Application Gateway möglicherweise nicht.

### <a name="step-6"></a>Schritt 6

Weisen Sie einen Adressbereich für die Back-End-Adresspool verwendet werden.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Schritt 7

Erstellen Sie ein virtuelles Netzwerk mit der vorhergehenden in der Ressourcengruppe in Schritt erstellt: [Erstellen der Ressourcengruppe](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Schritt 8

Abrufen der virtuellen Netzwerkressource und Subnetzressourcen in den folgenden Schritten:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Schritt 9

Erstellen Sie eine öffentliche IP-Adressressource für Application Gateway verwendet werden. Diese öffentliche IP-Adresse wird in einer folgendermaßen verwendet:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Application Gateway unterstützt nicht die Verwendung einer öffentlichen IP-Adresse mit einer definierten Domäne erstellt. Nur eine öffentliche IP-Adresse mit einer dynamisch erstellten Domäne wird unterstützt. Benötigen Sie einen benutzerfreundliche DNS-Namen für Application Gateway wird empfohlen einen Cname-Eintrag als Alias verwendet.

### <a name="step-10"></a>Schritt 10

Sie müssen alle Konfigurationselemente einrichten, bevor Sie Application Gateway erstellen. Die folgenden Schritte erstellen Konfigurationselemente, die für eine Ressource auf Gateway.

Erstellen einer Application Gateway IP-Konfigurations, dies was Application Gateway verwendet Subnetz konfiguriert. Beim Start Application Gateway nimmt eine IP-Adresse aus dem Subnetz konfiguriert und IP-Adressen in der Back-End-IP-Adresspool Netzwerkverkehr weiterleitet. Denken Sie daran, dass jede Instanz eine IP-Adresse hat.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Schritt 11

Konfigurieren des Back-End-IP-Adresspools mit der IP-Adressen der Back-End-Webserver. Diese Adressen sind die IP-Adressen, die den Netzwerkdatenverkehr aus Front-End-IP-Endpunkt. Ersetzen Sie die folgenden IP-Adressen um eigene IP-Adresse Anwendungsendpunkte hinzufügen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Schritt 12

Laden Sie das Zertifikat für die Ssl-fähiger Back-End-Ressourcen verwendet werden.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Schritt 13

Konfigurieren der Anwendung Gateway HTTP-Back-End. Zuweisen des Zertifikats im vorherigen Schritt auf den HTTP-hochgeladen.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Schritt 14

Konfigurieren Sie den Front-End-IP-Port für die öffentliche IP-Endpunkt. Dieser Port ist der Port, dem Benutzer zu verbinden.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Schritt 15

Front-End-IP-Konfiguration erstellen, diese Einstellung ordnet eine private oder öffentliche IP-Adresse der Front-End der Anwendungsgateway. Im folgende Schritt ordnet öffentliche IP-Adresse im vorherigen Schritt die Front-End-IP-Konfiguration.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Schritt 16

Das Zertifikat für das Application Gateway zu konfigurieren. Dieses Zertifikat wird verwendet, zu entschlüsseln und den Datenverkehr auf dem Anwendungsgateway erneut verschlüsseln.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Schritt 17

Erstellen Sie den HTTP-Listener für das Anwendungsgateway. Weisen Sie das Front-End-IP-Konfiguration, Anschluss und SSL-Zertifikat.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Schritt 18

Erstellen Sie eine Load Balancer Routingregel, die Load Balancer Verhalten konfiguriert. In diesem Beispiel wird eine einfache Round-Robin-Regel erstellt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Schritt 19

Konfigurieren Sie Größe der Instanz des Application-Gateways.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Sie können **WAF\_Mittel** und **WAF\_große**, Ebene wird mit WAF immer **WAF**. Kapazität ist eine Zahl zwischen 1 und 10.

### <a name="step-20"></a>Schritt 20

Konfigurieren den Modus für WAF, ** **Verhütung** und**zulässige Werte sind.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Schritt 21

Erstellen Sie ein Gateway mit alle Konfigurationselemente der vorangehenden Schritte. In diesem Beispiel wird das Application Gateway "Appgwtest" aufgerufen.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

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

Informationen Sie zum Konfigurieren des Diagnoseprotokolls erkannt oder verhindert werden Ereignisse mit Web Application Firewall auf [Application Gateway Diagnose](application-gateway-diagnostics.md) protokollieren


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png