<properties 
   pageTitle="Erstellen und konfigurieren Sie ein Gateway mit internen Lastenausgleich (ILB) in einem virtuellen Netzwerk | Microsoft Azure"
   description="Diese Seite beschreibt ein Gateway Azure mit einer internen Lastenausgleich konfigurieren"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Erstellen Sie ein Gateway mit einer internen Lastenausgleich (ILB)

> [AZURE.SELECTOR]
- [Azure klassische Schritte](application-gateway-ilb.md)
- [Ressourcenmanager Powershell Schritte](application-gateway-ilb-arm.md)

Application Gateway kann eine virtuelle IP-Adresse mit Internetzugriff oder einen internen Endpunkt nicht das Internet auch interne Load Balancer (ILB) Endpunkt zugänglich konfiguriert werden. Konfigurieren das Gateway mit einem ILB eignet sich für interne LOB Anwendung nicht Internet ausgesetzt. Es eignet sich auch für Services/Ebenen in einer Multi-Tier-Anwendung befindet sich im Internet nicht zugänglich Sicherheitsgrenze, aber dennoch Roundrobin Verteilung, Sitzung Klebrigkeit oder SSL-Beendigung erforderlich. Dieser Artikel führt Sie durch die Schritte zum Konfigurieren Sie ein Gateway mit einer ILB.

## <a name="before-you-begin"></a>Bevor Sie beginnen

1. Installieren Sie die neueste Version mit dem Webplattform-Installer Azure PowerShell-Cmdlets. Sie können herunterladen und Installieren der neuesten Version von **Windows PowerShell** Teil der [Download-Seite](https://azure.microsoft.com/downloads/).
2. Überprüfen Sie, ob Sie ein virtuelles Netzwerk arbeiten mit gültigen Subnetz.
3. Überprüfen Sie die Back-End-Server in das virtuelle Netzwerk oder mit einer öffentlichen IP-Adresse/VIP zugewiesen.


Erstellen Sie ein Gateway die folgenden Schritte in der angegebenen Reihenfolge. 

1. [Ein Gateway erstellen](#create-a-new-application-gateway)
2. [Konfigurieren des Gateways](#configure-the-gateway)
3. [Festlegen der Gatewaykonfiguration](#set-the-gateway-configuration)
4. [Starten des Gateways](#start-the-gateway)
4. [Überprüfen Sie das gateway](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Erstellen Sie ein Gateway:

**Das Gateway erstellen**, mit der `New-AzureApplicationGateway` Cmdlet durch eigene Werte ersetzen. Beachten Sie, dass die Abrechnung für das Gateway nicht zu diesem Zeitpunkt beginnt. Abrechnung beginnt in einem späteren Schritt das Gateway erfolgreich gestartet wurde.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Überprüft** das Gateway erstellt wurde, können Sie die `Get-AzureApplicationGateway` Cmdlet. 

Im Beispiel, *Beschreibung*, *InstanceCount*und *GatewaySize* sind optionale Parameter. Der Standardwert für *InstanceCount* ist 2 mit dem Höchstwert 10. Der Standardwert für *GatewaySize* ist Mittel. Kleine und große anderen verfügbaren Werte. *VIP* und *DnsName* werden leer angezeigt, da das Gateway noch nicht gestartet wurde. Diese werden erstellt, sobald das Gateway ausgeführt wird. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>Konfigurieren des Gateways

Eine Application Gateway-Konfiguration besteht von mehreren Werten. Die Werte gebunden werden zusammen, um die Konfiguration zu erstellen.
 
Die Werte sind:

- **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. Die IP-Adressen sollten entweder VNet Subnetz gehören oder sollte eine öffentliche IP-Adresse/VIP. 
- **Back-End-pooleinstellungen:** Jeder Pool hat wie Port, Protokoll und cookiebasierte Affinität. Diese sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
- **Frontend-Port:** Dieser Port ist der öffentliche Port auf dem Anwendungsgateway geöffnet. Datenverkehr trifft dieser Anschluss und dann auf einen Backend-Server umgeleitet wird.
- **Listener:** Der Listener verfügt über einen Front-End-Port ein Protokoll (Http oder Https sind Groß-/Kleinschreibung), und Name des SSL-Zertifikat (sofern offload Konfigurieren von SSL). 
- **Regel:** Bindet den Listener und Back-End-Serverpool und definiert die Back-End-Serverpool der Datenverkehr weitergeleitet werden soll, einen bestimmten Listener trifft. Derzeit wird *nur grundsätzlich* unterstützt. *Die Grundregel* ist Round-Robin-Verteilung.

Sie können die Konfiguration eine Konfiguration erstellen oder eine XML-Konfigurationsdatei erstellen. Ihre Konfiguration erstellen mithilfe einer XML-Konfigurationsdatei, verwenden Sie das folgenden Beispiel.



Beachten Sie Folgendes:


- Das *FrontendIPConfigurations* -Element beschreibt die ILB Informationen zum Konfigurieren von Application Gateway mit einem ILB. 

- Die Frontend-IP- *Typ* sollte 'Private' festgelegt werden

- *StaticIPAddress* auf die gewünschte interne IP fest auf dem Gateway Datenverkehr empfängt. Beachten Sie, dass das *StaticIPAddress* Element optional ist. Wenn nicht festgelegt, eine verfügbare interne IP bereitgestellte Subnetz ist. 

- Der Wert im *FrontendIPConfiguration* angegebene *Name* -Element sollte auf der FrontendIPConfiguration in die HTTPListener *FrontendIP* Element verwendet werden.

 **XML-Konfigurationsbeispiel**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>Festlegen der Gatewaykonfiguration

Anschließend legen Sie Application Gateway. Sie können die `Set-AzureApplicationGatewayConfig` Cmdlet ein Konfigurationsobjekt oder einer XML-Konfigurationsdatei. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Starten des Gateways

Sobald das Gateway konfiguriert wurde, können die `Start-AzureApplicationGateway` Cmdlet Gateway gestartet. Abrechnung für ein Gateway beginnt, nachdem das Gateway erfolgreich gestartet wurde. 


> [AZURE.NOTE] Die `Start-AzureApplicationGateway` Cmdlet kann bis zu 15-20 Minuten dauern. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Überprüfen Sie den Gateway-status

Verwenden der `Get-AzureApplicationGateway` -Cmdlet zum Überprüfen des Status der Gateway. *Start-AzureApplicationGateway* im vorherigen Schritt erfolgreich war, sollte der Zustand *ausgeführt*werden und Vip und DnsName müssen gültige Einträge. Dieses Beispiel zeigt das Cmdlet in der ersten Zeile der Ausgabe gefolgt. In diesem Beispiel wird das Gateway ausgeführt wird und kann Verkehr. 

> [AZURE.NOTE] Das Application Gateway konfiguriert Verkehr am konfigurierten ILB Endpunkt 10.0.0.10 in diesem Beispiel akzeptieren.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Nächste Schritte


Weitere Informationen über Ladeoptionen Lastenausgleich im Allgemeinen, finden Sie unter:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
