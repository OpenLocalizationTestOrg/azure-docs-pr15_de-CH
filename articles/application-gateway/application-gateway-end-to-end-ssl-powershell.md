<properties
    pageTitle="Konfigurieren von SSL-Richtlinien und Ende SSL mit Application Gateway | Microsoft Azure"
    description="Dieser Artikel beschreibt das Ende SSL mit Application Gateway mit Azure Ressourcenmanager PowerShell konfigurieren"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>Konfigurieren von SSL-Richtlinien und Ende SSL mit Application Gateway mit PowerShell

## <a name="overview"></a>Übersicht

Application Gateway unterstützt durchgehende Verschlüsselung des Datenverkehrs. Application Gateway wird am Anwendungsgateway SSL-Verbindung beendet. Gateway dann gilt die Routingregeln für den Datenverkehr neu verschlüsselt das Paket und übermittelt das Datenpaket an den entsprechenden Back-End-Grundlage die definiert. Keine Antwort vom Webserver durchläuft der Vorgehensweise für den Endbenutzer.

Ein anderes feature, Application Gateway unterstützt bestimmte Protokollversionen SSL deaktivieren. Application Gateway unterstützt die folgenden Protokollversion deaktivieren. TLSv1.0, TLSv1.1 und TLSv1.2.

> [AZURE.NOTE] SSL 2.0 und SSL 3.0 sind standardmäßig deaktiviert und kann nicht aktiviert werden. Sie gelten nicht gesichert und können nicht mit Application Gateway

![Szenario-Bild][scenario]

## <a name="scenario"></a>Szenario

In diesem Szenario erfahren Sie, wie ein Gateway mit Ende SSL mit PowerShell erstellen.

Dieses Szenario wird:

- Erstellen Sie eine Ressourcengruppe mit dem Namen "Appgw-Rg"
- Erstellen Sie ein virtuelles Netzwerk mit dem Namen "Appgwvnet" mit einem 10.0.0.0/16 reservierten CIDR-Block.
- Erstellen Sie zwei Subnetzen namens "Appgwsubnet" und "Appsubnet".
- Erstellen Sie eine kleine Anwendungsgateway unterstützen Ende SSL-Verschlüsselung, die bestimmte SSL-Protokolle deaktiviert.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Zum Konfigurieren von SSL Ende mit ein Gateway ist ein Zertifikat erforderlich, das Gateway und Zertifikate für die Back-End-Server erforderlich. Das Gateway-Zertifikat wird zum Verschlüsseln und Entschlüsseln von Datenverkehr mithilfe von SSL. Das Gateway-Zertifikat muss im Format Persönlicher Informationsaustausch (Pfx). Dieses Dateiformat kann für den privaten Schlüssel exportieren die vom Application Gateway zu ver- und Entschlüsselung von Datenverkehr führen.

Ende-Ssl-Verschlüsselung muß Backend White Application Gateway. Dies ist das öffentliche Zertifikat der Backends Application Gateway hochladen. Dadurch das Application Gateway nur mit bekannten Backend kommuniziert. Dies sichert Weitere End-to-End-Kommunikation.

Dieser Prozess wird in den folgenden Schritten beschrieben:

## <a name="create-the-resource-group"></a>Die Ressourcengruppe erstellen

Dieser Abschnitt führt Sie durch die Erstellung einer Ressourcengruppe, die Application Gateway enthält.

### <a name="step-1"></a>Schritt 1

Ein Azure-Konto anmelden.

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Wählen Sie das Abonnement für dieses Szenario verwendet.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Schritt 3

Erstellen Sie eine Ressourcengruppe (überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Erstellen Sie ein virtuelles Netzwerk und Subnetz für Application gateway

Das folgende Beispiel erstellt ein virtuelles Netzwerk und zwei Subnetzen. Ein Subnetz wird verwendet, um das Application Gateway halten. Das Subnetz wird für das Hosten der Anwendung Backends verwendet.

### <a name="step-1"></a>Schritt 1

Weisen Sie einen Adressbereich für das Subnetz für Application Gateway selbst verwendet werden.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Subnetze für Application Gateway konfiguriert sollten so bemessen werden. Ein Gateway kann bis zu 10 Instanzen konfiguriert werden. Jede Instanz hat 1 IP-Adresse im Subnetz. Zu kleine eines Subnetzes kann das scaling-out einer Anwendungsgateway beeinträchtigen.

### <a name="step-2"></a>Schritt 2

Weisen Sie einen Adressbereich für die Back-End-Adresspool verwendet werden.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>Schritt 3

Erstellen Sie ein virtuelles Netzwerk mit den in den vorhergehenden Schritten definiert.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Schritt 4

Abrufen der virtuellen Netzwerkressource und Subnetzressourcen in den folgenden Schritten:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Erstellen Sie eine öffentliche IP-Adresse für die Front-End-Konfiguration

Erstellen Sie eine öffentliche IP-Adressressource für Application Gateway verwendet werden. Diese öffentliche IP-Adresse folgenden Schritt.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Application Gateway unterstützt nicht die Verwendung einer öffentlichen IP-Adresse mit einer definierten Domäne erstellt. Nur eine öffentliche IP-Adresse mit einer dynamisch erstellten Domäne wird unterstützt. Benötigen Sie einen benutzerfreundliche DNS-Namen für Application Gateway wird empfohlen einen Cname-Eintrag als Alias verwendet.

## <a name="create-an-application-gateway-configuration-object"></a>Gateway-Konfiguration Anwendungsobjekt erstellt

Sie müssen alle Konfigurationselemente einrichten, bevor Sie Application Gateway erstellen. Die folgenden Schritte erstellen Konfigurationselemente, die für eine Ressource auf Gateway.

### <a name="step-1"></a>Schritt 1

Erstellen einer Application Gateway IP-Konfigurations, mit dieser Einstellung konfigurieren, welche Subnetz Application Gateway verwendet. Beim Start Application Gateway nimmt eine IP-Adresse aus dem Subnetz konfiguriert und IP-Adressen in der Back-End-IP-Adresspool Netzwerkverkehr weiterleitet. Denken Sie daran, dass jede Instanz eine IP-Adresse hat.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Schritt 2

Front-End-IP-Konfiguration erstellen, diese Einstellung ordnet eine private oder öffentliche IP-Adresse der Front-End der Anwendungsgateway. Im folgende Schritt ordnet öffentliche IP-Adresse im vorherigen Schritt die Front-End-IP-Konfiguration.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>Schritt 3

Konfigurieren des Back-End-IP-Adresspools mit der IP-Adressen der Back-End-Webserver. Diese Adressen sind die IP-Adressen, die den Netzwerkdatenverkehr aus Front-End-IP-Endpunkt. Ersetzen Sie die folgenden IP-Adressen um eigene IP-Adresse Anwendungsendpunkte hinzufügen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Ein vollqualifizierten Domänennamen (FQDN) steht ein gültiger Wert anstelle einer IP-Adresse für die Back-End-Server mit der Option - BackendFqdns.

### <a name="step-4"></a>Schritt 4

Konfigurieren Sie den Front-End-IP-Port für die öffentliche IP-Endpunkt. Dieser Port ist der Port, dem Benutzer zu verbinden.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>Schritt 5

Das Zertifikat für das Application Gateway zu konfigurieren. Dieses Zertifikat wird verwendet, zu entschlüsseln und den Datenverkehr auf dem Anwendungsgateway erneut verschlüsseln.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] In diesem Beispiel wird das Zertifikat verwendet SSL-Verbindung konfiguriert. Das Zertifikat muss im PFX-Format und das Kennwort muss zwischen 4 und 12 Zeichen sein.

### <a name="step-6"></a>Schritt 6

Erstellen Sie den HTTP-Listener für das Anwendungsgateway. Weisen Sie das Front-End-IP-Konfiguration, Anschluss und SSL-Zertifikat.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Schritt 7

Laden Sie das Zertifikat für die Ssl-fähiger Back-End-Ressourcen verwendet werden.

> [AZURE.NOTE] Standard-Prüfpunkt Ruft den öffentlichen Schlüssel von **SSL Bindungsregel auf Back-End-IP-Adresse** und den Wert des öffentlichen Schlüssels, den öffentlichen Schlüsselwert erhält, die Sie hier verglichen. Der abgerufene öffentliche Schlüssel unbedingt Standort möglicherweise nicht, Verkehrsfluss wird **Wenn** Sie Hostheader und SNI am Back-End verwenden. Besuchen Sie im Zweifelsfall https://127.0.0.1/ Back-Ends bestätigen, welches Zertifikat für **Standard** -SSL-Bindung verwendet wird. Verwenden Sie den öffentlichen Schlüssel von der Anforderung in diesem Abschnitt. Wenn Sie Host-Header und SNI für HTTPS-Bindung und Sie eine Antwort und ein Zertifikat aus einer manuellen Browseranforderung zu https://127.0.0.1/ an der Rückseite erhalten, müssen Sie eine Bindungsregel SSL auf Back-Ends einrichten. Wenn Sie dies tun, Prüfpunkte fehl und Back-End werden nicht auf der weißen Liste.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] In diesem Schritt bereitgestellte Zertifikat sollte den öffentlichen Schlüssel der Pfx-Zertifikat auf dem Back-End. Exportieren Sie das Zertifikat (nicht das Stammzertifikat) auf dem Back-End-Server installiert. CER formatieren und in diesem Schritt verwenden. Dieser Schritt weiße Backend mit Application Gateway.

### <a name="step-8"></a>Schritt 8

Konfigurieren der Anwendung Gateway HTTP-Back-End. Zuweisen des Zertifikats im vorherigen Schritt auf den HTTP-hochgeladen.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Schritt 9

Erstellen Sie eine Load Balancer Routingregel, die Load Balancer Verhalten konfiguriert. In diesem Beispiel wird eine einfache Round-Robin-Regel erstellt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>Schritt 10

Konfigurieren Sie Größe der Instanz des Application-Gateways.  Die Größen sind **Standard\_kleine**, **Standard\_Mittel**, und **Standard\_große**.  Kapazität sind die verfügbaren Werte 1 bis 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Zu Testzwecken kann ein Instanzenzahl von 1 ausgewählt werden. Es ist wichtig, dass jede Instanz unter zwei Instanzen fällt nicht unter die SLA und werden daher nicht empfohlen. Kleine Gateways für Test- und nicht für die Produktion verwendet werden sollen.

### <a name="step-11"></a>Schritt 11

Konfigurieren der SSL-Richtlinie auf Application Gateway verwendet werden. Application Gateway unterstützt die Möglichkeit, bestimmte Protokollversionen SSL deaktivieren.

Die folgenden Werte sind eine Liste von Protokollversionen deaktiviert werden kann.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

Das folgende Beispiel deaktiviert TLSv1\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>Application Gateway erstellen

Erstellen Sie die vorhergehenden Schritte mit Application Gateway. Die Erstellung des Gateways ist ein langer Prozess.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>Deaktivieren Sie SSL Protokollversionen auf ein vorhandenes Gateway

Die vorangehenden Schritte durch Erstellen einer Anwendung mit Ssl Ende und deaktivieren bestimmte Versionen der SSL-Protokoll. Das folgende Beispiel deaktiviert bestimmte Richtlinien SSL ein vorhandenes Gateway.

### <a name="step-1"></a>Schritt 1

Abrufen des Application Gateways aktualisieren.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Schritt 2

Definieren Sie SSL-Richtlinien Im folgenden Beispiel werden TLSv1.0 und TLSv1.1 deaktiviert.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>Schritt 3

Schließlich aktualisieren Sie das Gateway. Es ist wichtig zu beachten, dass der letzte Schritt eine zeitintensive Aufgabe. Wenn es abgeschlossen ist, wird Ssl Ende auf Application Gateway konfiguriert.

    $gw | Set-AzureRmApplicationGateway

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

Erfahren Sie mehr über die Sicherheit einer Anwendung mit Web Application Firewall über Application Gateway auf [Web Application Firewall Overview](application-gateway-webapplicationfirewall-overview.md) Absichern

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
