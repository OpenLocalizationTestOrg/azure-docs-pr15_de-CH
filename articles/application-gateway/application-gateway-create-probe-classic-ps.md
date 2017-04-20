<properties
   pageTitle="Mithilfe eine benutzerdefinierte Abfrage für Application Gateway PowerShell im klassischen Bereitstellungsmodell | Microsoft Azure"
   description="Erstellen Sie benutzerdefinierte Überprüfung für Application Gateway mithilfe von PowerShell im klassischen Bereitstellungsmodell"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Erstellen Sie eine benutzerdefinierte Überprüfung für Azure Application Gateway (classic) mithilfe von PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-probe-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-probe-ps.md)
- [Klassische Azure PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Ein Gateway erstellen

So erstellen Sie ein gateway

1. Erstellen Sie eine Ressource auf Gateway.
2. Erstellen einer XML-Konfigurationsdatei oder ein Konfigurationsobjekt.
3. Übernehmen Sie die Konfiguration der neu erstellten Anwendung Gateway-Ressource.

### <a name="create-an-application-gateway-resource"></a>Erstellen Sie eine Ressource auf gateway

Verwenden Sie zum Erstellen des Gateways Cmdlet " **New-AzureApplicationGateway** " durch eigene Werte ersetzen. Abrechnung für das Gateway wird zu diesem Zeitpunkt nicht gestartet. Abrechnung beginnt in einem späteren Schritt das Gateway erfolgreich gestartet wurde.

Das folgende Beispiel erstellt ein Gateway mit einem virtuellen Netzwerk namens "testvnet1" und ein Subnetz "Subnetz 1" bezeichnet.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Überprüfen der Gateway erstellt wurde, können Sie das Cmdlet " **Get-AzureApplicationGateway** ".

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  Der Standardwert für *InstanceCount* ist 2 mit dem Höchstwert 10. Der Standardwert für *GatewaySize* ist Mittel. Sie können zwischen Small, Medium und Large.

 *VirtualIPs* und *DnsName* werden leer angezeigt, da das Gateway noch nicht gestartet wurde. Diese werden erstellt, sobald das Gateway ausgeführt wird.

## <a name="configure-an-application-gateway"></a>Konfigurieren Sie ein gateway

Das Application Gateway können mit XML oder ein Konfigurationsobjekt.

## <a name="configure-an-application-gateway-by-using-xml"></a>Konfigurieren Sie ein Gateway mit XML

Im folgenden Beispiel wird eine XML-Datei konfigurieren alle Application Gateway und verpflichtet die Anwendungsressource Gateway verwenden.  

### <a name="step-1"></a>Schritt 1

Kopieren Sie folgenden Text in Editor.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Bearbeiten Sie die Werte zwischen den Klammern für die Konfigurationselemente. Speichern Sie die Datei mit der Erweiterung .xml.

Im folgenden Beispiel wird veranschaulicht, wie mithilfe eine Konfigurationsdatei Application Gateway für den Lastenausgleich HTTP-Verkehr auf öffentlichen Port 80 und Netzwerkverkehr an Back-End-Port 80 zwischen zwei IP-Adressen mithilfe einer benutzerdefinierten Abfrage einrichten.

>[AZURE.IMPORTANT] Das Element Protokoll Http oder Https beachtet werden.

Ein neues Konfigurationselement \<Prüfpunkt\> Benutzerdefinierte Probes konfigurieren hinzugefügt.

Die Konfigurationsparameter sind:

- **Name** - Verweisnamen für benutzerdefinierte Probe.
- **Protokoll** - Protokoll (mögliche Werte sind HTTP oder HTTPS).
- **Host** und **Pfad** – vollständige URL-Pfad, die vom Application Gateway bestimmt den Zustand der Instanz aufgerufen wird. Beispielsweise haben Sie eine Website http://contoso.com/ kann dann benutzerdefinierte Probe für "http://contoso.com/path/custompath.htm" Prüfpunkt Kontrollen eine erfolgreiche HTTP-Antwort konfiguriert werden.
- **Intervall** - konfiguriert Prüfpunkt Intervall überprüft in Sekunden.
- **Timeout** - definiert Prüfpunkt Timeout für eine HTTP-Antwort-Überprüfung.
- **UnhealthyThreshold** - die Anzahl der fehlgeschlagenen HTTP-Antworten kennzeichnen die Backend-Instanz als *fehlerhaft*.

Prüfpunktnamens bezieht sich auf die <BackendHttpSettings> Konfiguration der Back-End-Pool zuweisen benutzerdefinierte Probe Standardeinstellungen verwendet.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Ein Gateway der vorhandenen benutzerdefinierten Test-Konfiguration hinzufügen

Ändern der aktuellen Konfigurations ein Gateway erfordert drei Schritte: Abrufen die aktuelle XML-Konfigurationsdatei, um benutzerdefinierte Probe ändern und Konfigurieren des Gateways Anwendung bei XML.

### <a name="step-1"></a>Schritt 1

Rufen Sie die XML-Datei mithilfe von Get-AzureApplicationGatewayConfig ab. Exportiert die Konfigurations-XML zu Prüfpunkt Einstellung hinzufügen.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Schritt 2

Öffnen Sie die XML-Datei in einem Texteditor. Hinzufügen einer `<probe>` nach Abschnitt `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

Fügen Sie im Abschnitt BackendHttpSettings der XML Prüfpunktnamens wie im folgenden Beispiel gezeigt:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Speichern Sie die XML-Datei.

### <a name="step-3"></a>Schritt 3

Aktualisieren Sie Application Gateway-Konfiguration mit der neuen XML-Datei mithilfe von **Set AzureApplicationGatewayConfig**. Ihr Anwendungsgateway aktualisiert mit der neuen Konfiguration.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Nächste Schritte

Entlastung von Secure Sockets Layer (SSL) konfigurieren, finden Sie unter [konfigurieren ein Gateway für SSL-Verschiebung](application-gateway-ssl.md).

So konfigurieren Sie einen Gateway der internen Lastenausgleich verwendet, finden Sie unter [erstellen ein Gateway mit einer internen Lastenausgleich (ILB)](application-gateway-ilb.md).
