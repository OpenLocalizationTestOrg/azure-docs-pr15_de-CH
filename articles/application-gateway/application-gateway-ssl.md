<properties
   pageTitle="Ein Gateway für SSL-Verschiebung mithilfe von klassischen Bereitstellung konfigurieren | Microsoft Azure"
   description="Dieser Artikel bietet auf ein Gateway mit SSL erstellen mithilfe von Azure klassischen Bereitstellungsmodell abnimmt."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Konfigurieren Sie ein Gateway für SSL-Verschiebung mithilfe des klassischen Bereitstellungsmodells

> [AZURE.SELECTOR]
-[Azure-Portal](application-gateway-ssl-portal.md)
-[Azure Ressourcenmanager PowerShell](application-gateway-ssl-arm.md)
-[Azure klassische PowerShell](application-gateway-ssl.md)

Azure Application Gateway kann zum Beenden der Sitzung Secure Sockets Layer (SSL) am Gateway teure SSL-Entschlüsselung Aufgaben zu der Webserverfarm konfiguriert werden. SSL-Verschiebung vereinfacht auch die Front-End-Server-Setup und Verwaltung der Website.

## <a name="before-you-begin"></a>Bevor Sie beginnen

1. Installieren Sie die neueste Version von Azure PowerShell-Cmdlets mit dem Webplattform-Installer. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** Teil der [Downloadseite](https://azure.microsoft.com/downloads/).
2. Überprüfen Sie, ob Sie ein virtuelles Netzwerk arbeiten mit einem gültigen Subnetz. Stellen Sie sicher, dass keine virtuellen Maschinen oder Cloud-Bereitstellungen im Subnetz verwenden. Application Gateway muss sich in einem virtuellen Netzwerksubnetz sein.
3. Sie konfigurieren das Application Gateway Server bestehen oder Endpunkten erstellt das virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen haben.

So konfigurieren Sie SSL-Verschiebung auf ein Gateway führen Sie die folgenden Schritte in der angegebenen Reihenfolge:

1. [Ein Gateway erstellen](#create-an-application-gateway)
2. [Hochladen von SSL-Zertifikaten](#upload-ssl-certificates)
3. [Konfigurieren des Gateways](#configure-the-gateway)
4. [Festlegen der Gatewaykonfiguration](#set-the-gateway-configuration)
5. [Starten des Gateways](#start-the-gateway)
6. [Überprüfen Sie den Gateway-status](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Ein Gateway erstellen

Verwenden Sie zum Erstellen des Gateways Cmdlet " **New-AzureApplicationGateway** " durch eigene Werte ersetzen. Abrechnung für das Gateway wird zu diesem Zeitpunkt nicht gestartet. Abrechnung beginnt in einem späteren Schritt das Gateway erfolgreich gestartet wurde.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Überprüfen der Gateway erstellt wurde, können Sie das Cmdlet " **Get-AzureApplicationGateway** ".

Im Beispiel, *Beschreibung*, *InstanceCount*und *GatewaySize* sind optionale Parameter. Der Standardwert für *InstanceCount* ist 2 mit dem Höchstwert 10. Der Standardwert für *GatewaySize* ist Mittel. Kleine und große anderen verfügbaren Werte. *VirtualIPs* und *DnsName* werden leer angezeigt, da das Gateway noch nicht gestartet wurde. Diese Werte werden erstellt, sobald das Gateway ausgeführt wird.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>Hochladen von SSL-Zertifikaten

Mit der Application Gateway das Serverzertifikat im *Pfx* -Format hinzufügen **Hinzufügen AzureApplicationGatewaySslCertificate** . Der Name ist ein Benutzer ausgewählt und muss innerhalb der Anwendung Gateways. Dieses Zertifikat wird mit diesem Namen an alle Zertifikat Application Gateway bezeichnet.

Das folgende Beispiel zeigt das Cmdlet Werte im Beispiel durch Ihren eigenen ersetzen.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Als Nächstes überprüfen Sie Zertifikat hochladen. Verwenden Sie das Cmdlet " **Get-AzureApplicationGatewayCertificate** ".

Dieses Beispiel zeigt das Cmdlet in der ersten Zeile der Ausgabe gefolgt.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Das Kennwort muss zwischen 4 und 12 Zeichen, Buchstaben oder Zahlen. Sonderzeichen sind nicht zulässig.

## <a name="configure-the-gateway"></a>Konfigurieren des Gateways

Eine Application Gateway-Konfiguration besteht von mehreren Werten. Die Werte gebunden werden zusammen, um die Konfiguration zu erstellen.

Die Werte sind:

- **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. IP-Adressen sollten entweder das virtuelle Netzwerk-Subnetz angehören oder sollte eine öffentliche IP-Adresse/VIP.
- **Back-End-pooleinstellungen:** Jeder Pool hat wie Port, Protokoll und cookiebasierte Affinität. Diese sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
- **Front-End-Port:** Dieser Port ist der öffentliche Port, der auf dem Anwendungsgateway geöffnet wird. Datenverkehr trifft dieser Anschluss und dann auf einen Back-End-Server umgeleitet wird.
- **Listener:** Der Listener verfügt über einen Front-End-Port ein Protokoll (Http oder Https, diese Werte sind Groß-/Kleinschreibung) und SSL-Zertifikat (wenn offload Konfigurieren von SSL).
- **Regel:** Bindet den Listener und Back-End-Serverpool und definiert die Back-End-Serverpool der Datenverkehr weitergeleitet werden soll, einen bestimmten Listener trifft. Derzeit wird *nur grundsätzlich* unterstützt. *Die Grundregel* ist Round-Robin-Verteilung.

**Zusätzliche Konfiguration Notizen**

Für SSL-Zertifikate Konfiguration sollte das Protokoll in **HttpListener** in *Https* (Groß-und Kleinschreibung) ändern. Das **SslCert** -Element wird mit dem Wert, den gleichen Namen wie der Upload des vorhergehenden Abschnitt für SSL-Zertifikate für **HttpListener** hinzugefügt. Der Front-End-Port sollte auf 443 aktualisiert werden.

**Cookie-basierte Affinität aktivieren**: ein Gateway kann konfiguriert werden, um sicherzustellen, dass eine Anforderung einer Clientsitzung immer gleichen virtuellen Computer in der Webfarm weitergeleitet wird. Dabei erfolgt durch Injektion ein Sitzungscookie, das Gateway Datenverkehr entsprechend angewiesen ermöglicht. Um cookiebasierte Affinität zu aktivieren, legen Sie **CookieBasedAffinity** *aktiviert* im **BackendHttpSettings** -Element.



Sie können die Konfiguration ein Konfigurationsobjekt erstellen oder mithilfe einer XML-Konfigurationsdatei erstellen.
Um die Konfiguration mithilfe einer XML-Konfigurationsdatei zu erstellen, verwenden Sie Folgendes Beispiel:

**XML-Konfigurationsbeispiel**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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


## <a name="set-the-gateway-configuration"></a>Festlegen der Gatewaykonfiguration

Richten Sie das Application Gateway. Das Cmdlet " **Set-AzureApplicationGatewayConfig** " können ein Konfigurationsobjekt oder einer XML-Konfigurationsdatei.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Starten des Gateways

Sobald das Gateway konfiguriert wurden, verwenden Sie das Cmdlet **Start AzureApplicationGateway** Gateway gestartet. Abrechnung für ein Gateway beginnt, nachdem das Gateway erfolgreich gestartet wurde.


**Hinweis:** **Start-AzureApplicationGateway** -Cmdlet kann bis zu 15-20 Minuten dauern.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Überprüfen Sie den Gateway-status

Verwenden Sie das Cmdlet " **Get-AzureApplicationGateway** " Überprüfen des Status des Gateways. **Start-AzureApplicationGateway** im vorherigen Schritt erfolgreich *Zustand* läuft und *VirtualIPs* und *DnsName* müssen gültige Einträge.

Dieses Beispiel zeigt ein Gateway, die, ausgeführt wird, kann der Verkehr.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Nächste Schritte


Weitere Informationen über Ladeoptionen Lastenausgleich im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)