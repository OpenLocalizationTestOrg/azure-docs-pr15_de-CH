<properties
   pageTitle="Erstellen, starten oder ein Gateway löschen | Microsoft Azure"
   description="Diese Seite beschreibt das Erstellen, konfigurieren, starten und Löschen eines Gateways Azure-Anwendung"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Erstellen, starten oder ein Gateway löschen

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-gateway-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-gateway-arm.md)
- [Klassische Azure PowerShell](application-gateway-create-gateway.md)
- [Azure-Ressourcen-Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist eine Layer 7-Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Application Gateway bietet viele Application Delivery Controller (ADC) Funktionen einschließlich HTTP-Lastenausgleich cookiebasierte sitzungsaffinität, Secure Sockets Layer (SSL) ausgelagert, benutzerdefinierten Zustand Prüfpunkte, Unterstützung für mehrere Standorte und viele andere. Eine vollständige Liste der unterstützten Funktionen finden Sie auf [Gateway Anwendungsübersicht](application-gateway-introduction.md)

Dieser Artikel führt Sie durch die Schritte zum Erstellen, konfigurieren, starten und löschen ein Gateway.

## <a name="before-you-begin"></a>Bevor Sie beginnen

1. Installieren Sie die neueste Version von Azure PowerShell-Cmdlets mit dem Webplattform-Installer. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** Teil der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Haben Sie ein vorhandenes virtuelles Netzwerk, wählen Sie ein vorhandenes leeres Subnetz oder Erstellen eines neuen Subnetzes im vorhandenen virtuellen Netzwerk ausschließlich für die Verwendung von Application Gateway. Application Gateway für ein anderes virtuelles Netzwerk als Ressourcen außer Vnet peering ist hinter dem Anwendungsgateway bereitstellen möchten, kann nicht bereitgestellt werden. Weitere Informationen finden Sie [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)
3. Überprüfen Sie, ob Sie ein virtuelles Netzwerk arbeiten mit einem gültigen Subnetz. Stellen Sie sicher, dass keine virtuellen Maschinen oder Cloud-Bereitstellungen im Subnetz verwenden. Application Gateway muss sich in einem virtuellen Netzwerksubnetz sein.
3. Sie konfigurieren das Application Gateway Server bestehen oder Endpunkten erstellt das virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

## <a name="what-is-required-to-create-an-application-gateway"></a>Was ist ein Gateway erstellen?

Wenn Sie den Befehl **Neu AzureApplicationGateway** Application Gateway zu verwenden, keine Konfiguration zu diesem Zeitpunkt festgelegt ist und die neu erstellte Ressource konfiguriert über XML oder ein Konfigurationsobjekt.

Die Werte sind:

- **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. IP-Adressen sollten entweder das virtuelle Netzwerk-Subnetz angehören oder sollte eine öffentliche IP-Adresse/VIP.
- **Back-End-pooleinstellungen:** Jeder Pool hat wie Port, Protokoll und cookiebasierte Affinität. Diese sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
- **Front-End-Port:** Dieser Port ist der öffentliche Port, der auf dem Anwendungsgateway geöffnet wird. Datenverkehr trifft dieser Anschluss und dann auf einen Back-End-Server umgeleitet wird.
- **Listener:** Der Listener verfügt über einen Front-End-Port ein Protokoll (Http oder Https, diese Werte sind Groß-/Kleinschreibung) und SSL-Zertifikat (wenn offload Konfigurieren von SSL).
- **Regel:** Bindet den Listener und Back-End-Serverpool und definiert die Back-End-Serverpool der Datenverkehr weitergeleitet werden soll, einen bestimmten Listener trifft.

## <a name="create-an-application-gateway"></a>Ein Gateway erstellen

So erstellen Sie ein gateway

1. Erstellen Sie eine Ressource auf Gateway.
2. Erstellen einer XML-Konfigurationsdatei oder ein Konfigurationsobjekt.
3. Übernehmen Sie die Konfiguration der neu erstellten Anwendung Gateway-Ressource.

>[AZURE.NOTE] Benutzerdefinierte Überprüfung für Ihr Anwendungsgateway konfigurieren, finden Sie unter [erstellen ein Gateway mit benutzerdefinierten Prüfpunkte mit PowerShell](application-gateway-create-probe-classic-ps.md). Checken Sie die [Benutzerdefinierte Probes und Überwachung](application-gateway-probe-overview.md) .

![Szenario-Beispiel][scenario]

### <a name="create-an-application-gateway-resource"></a>Erstellen Sie eine Ressource auf gateway

Verwenden Sie zum Erstellen des Gateways Cmdlet " **New-AzureApplicationGateway** " durch eigene Werte ersetzen. Abrechnung für das Gateway wird zu diesem Zeitpunkt nicht gestartet. Abrechnung beginnt in einem späteren Schritt das Gateway erfolgreich gestartet wurde.

Das folgende Beispiel erstellt ein Gateway mit einem virtuellen Netzwerk namens "testvnet1" und ein Subnetz "Subnetz 1" bezeichnet.


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Beschreibung*, *InstanceCount*und *GatewaySize* sind optionale Parameter.

Überprüfen der Gateway erstellt wurde, können Sie das Cmdlet " **Get-AzureApplicationGateway** ".

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* ist 2 mit dem Höchstwert 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können zwischen Small, Medium und Large.

*VirtualIPs* und *DnsName* werden leer angezeigt, da das Gateway noch nicht gestartet wurde. Diese werden erstellt, sobald das Gateway ausgeführt wird.

## <a name="configure-the-application-gateway"></a>Konfigurieren Sie das Application gateway

Das Application Gateway können mit XML oder ein Konfigurationsobjekt.

## <a name="configure-the-application-gateway-by-using-xml"></a>Konfigurieren Sie das Anwendungsgateway mit XML

Im folgenden Beispiel wird eine XML-Datei konfigurieren alle Application Gateway und verpflichtet die Anwendungsressource Gateway verwenden.  

### <a name="step-1"></a>Schritt 1  

Kopieren Sie folgenden Text in Editor.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Bearbeiten Sie die Werte zwischen den Klammern für die Konfigurationselemente. Speichern Sie die Datei mit der Erweiterung .xml.

>[AZURE.IMPORTANT] Das Element Protokoll Http oder Https beachtet werden.

Im folgenden Beispiel wird veranschaulicht, wie eine Konfigurationsdatei verwenden, um das Application Gateway einzurichten. Die Beispiel gleicht HTTP-Verkehr auf öffentlichen Port 80 und Netzwerkverkehr Back-End-Port 80 zwischen zwei IP-Adressen sendet.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Schritt 2

Richten Sie das Application Gateway an. Verwenden Sie das Cmdlet " **Set-AzureApplicationGatewayConfig** " mit einer XML-Konfigurationsdatei.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Mithilfe eines Konfigurationsobjekts konfigurieren Sie Application gateway

Im folgende Beispiel veranschaulicht mithilfe Objekte Anwendungsgateway konfigurieren. Alle Konfigurationselemente müssen individuell konfiguriert und Gateway-Konfiguration Anwendungsobjekt hinzugefügt. Nach der Konfiguration erstellen, verwenden Sie den Befehl **Set AzureApplicationGateway** , auf die zuvor erstellte Anwendung Gateway Ressource übernommen.

>[AZURE.NOTE] Vor jeder Configuration-Objekt einen Wert zuweisen, müssen Sie deklarieren, welche Art von Objekt PowerShell für Speicher verwendet. Die erste Zeile zum Erstellen einzelner Elemente definiert, welche Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (Objektname) verwendet werden.

### <a name="step-1"></a>Schritt 1

Erstellen Sie individuelle Konfigurationselemente.

Erstellen Sie die Front-End-IP-Adresse wie im folgenden Beispiel gezeigt.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Erstellen Sie den Front-End-Port, wie im folgenden Beispiel gezeigt.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Erstellen des Back-End-Server-Pools.

 Definieren Sie die IP-Adressen, die Back-End-Server wie im nächsten Beispiel aufgenommen werden.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Mit der $server-Objekt können die Werte für das Back-End-Pool-Objekt ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Erstellen Sie die Back-End-Pool-Einstellung.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Erstellen Sie den Listener.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Erstellen Sie die Regel.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Schritt 2

Gateway-Konfiguration Anwendungsobjekt ($appgwconfig) alle einzelnen Konfigurationselemente zuweisen.

Die Front-End-IP-Adresse zur Konfiguration hinzufügen.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Die Konfiguration den Front-End-Port hinzufügen.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

Die Konfiguration den Back-End-Server-Pool hinzufügen.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

Die Konfiguration die Back-End-Pool-Einstellung hinzufügen.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Die Konfiguration den Listener hinzufügen.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

Die Konfiguration die Regel hinzufügen.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Schritt 3

Verpflichten Sie das Konfigurationsobjekt Gateway Anwendungsressource mit **Set-AzureApplicationGatewayConfig**.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Starten des Gateways

Sobald das Gateway konfiguriert wurden, verwenden Sie das Cmdlet **Start AzureApplicationGateway** Gateway gestartet. Abrechnung für ein Gateway beginnt, nachdem das Gateway erfolgreich gestartet wurde.

> [AZURE.NOTE] **Start-AzureApplicationGateway** -Cmdlet kann bis zu 15-20 Minuten dauern.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Überprüfen Sie den Gateway-status

Verwenden Sie das Cmdlet " **Get-AzureApplicationGateway** " Überprüfen des Status des Gateways. **Start-AzureApplicationGateway** im vorherigen Schritt erfolgreich *Zustand* läuft und *Vip* und *DnsName* müssen gültige Einträge.

Das folgende Beispiel zeigt ein Gateway, die, Betrieb und bereit, Verkehr für bestimmte `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Ein Gateway löschen

So löschen Sie ein gateway

1. Das Cmdlet " **Stop AzureApplicationGateway** " Gateway zu verwenden.
2. Verwenden Sie das Cmdlet **Entfernen AzureApplicationGateway** Gateway entfernen.
3. Überprüfen Sie, ob das Gateway mit dem Cmdlet **Get-AzureApplicationGateway** entfernt wurde.

Das folgende Beispiel zeigt das Cmdlet " **Stop AzureApplicationGateway** " in der ersten Zeile, gefolgt von der Ausgabe.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Sobald das Application Gateway beendet ist, verwenden Sie das Cmdlet **Entfernen AzureApplicationGateway** den Dienst entfernen.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Um sicherzustellen, dass der Dienst entfernt wurde, können Sie das Cmdlet " **Get-AzureApplicationGateway** ". Dieser Schritt ist nicht erforderlich.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Nächste Schritte

SSL-Verschiebung konfigurieren, finden Sie unter [konfigurieren ein Gateway für SSL-Verschiebung](application-gateway-ssl.md).

So konfigurieren Sie einen Gateway der internen Lastenausgleich verwendet, finden Sie unter [erstellen ein Gateway mit einer internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Weitere Informationen über Ladeoptionen Lastenausgleich im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png