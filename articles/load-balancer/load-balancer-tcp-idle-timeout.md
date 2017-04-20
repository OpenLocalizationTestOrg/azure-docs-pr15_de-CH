<properties
   pageTitle="Load Balancer TCP-Leerlauftimeout konfigurieren | Microsoft Azure"
   description="Load Balancer TCP-Leerlauftimeout konfigurieren"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Konfigurieren Sie TCP-Leerlauftimeout für Azure-Lastenausgleich

In der Standardkonfiguration hat Azure Lastenausgleich ein Leerlaufzeitlimit 4 Minuten. Wenn ein Zeitraum der Inaktivität der Timeoutwert überschreitet, besteht keine Garantie, die TCP- oder HTTP-Sitzung zwischen dem Client und dem Cloud-Dienst verwaltet wird.

Wenn die Verbindung geschlossen wird, erhalten die Clientanwendung die folgende Fehlermeldung angezeigt: "die zugrunde liegende Verbindung wurde geschlossen: Verbindung beibehalten werden voraussichtlich vom Server geschlossen wurde."

Üblich ist mit einem TCP-Keep-alive. Dadurch hält die Verbindung für einen längeren Zeitraum aktiv. Weitere Informationen finden Sie in diesen [Beispiele für .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Keep-alive aktiviert ist, Pakete während Phasen der Inaktivität für die Verbindung. Diese Keep-alive-Pakete sicherstellen, dass der Leerlauftimeout Wert nicht erreicht und die Verbindung für einen längeren Zeitraum aufrechterhalten.

Diese Einstellung ist für eingehende Verbindungen. Zu verlieren die Verbindung müssen TCP Keep-alive-Intervall kleiner Leerlaufzeitlimit Einstellung konfigurieren oder erhöhen Sie den Leerlauf. Um solche Szenarien zu unterstützen, haben wir eine konfigurierbare Leerlaufzeitlimit unterstützt. Sie können nun eine Dauer von 4 bis 30 Minuten festlegen.

TCP-Keep-alive eignet sich für Szenarien, in denen Akkubetriebsdauer keine Einschränkung. Es empfiehlt sich nicht für Dienste. Mit TCP-Keep-alive in einer mobilen Anwendung kann die Geräte schneller Akkulebensdauer.

![TCP-timeout](./media/load-balancer-tcp-idle-timeout/image1.png)

In den folgenden Abschnitten beschrieben ändern Leerlaufzeitlimit in virtuellen Maschinen und cloud-Services.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Konfigurieren Sie TCP-Timeout für die Instanzebene öffentliche IP-Adresse auf 15 Minuten

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`ist optional. Wenn sie nicht festgelegt ist, beträgt das Standardtimeout 4 Minuten. Die zulässige Timeout reichen 4 bis 30 Minuten.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Das Leerlaufzeitlimit festgelegt, wenn einen Azure Endpunkt auf einem virtuellen Computer erstellen

Um die Timeouteinstellung für einen Endpunkt zu ändern, verwenden Sie Folgendes:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Rufen Sie Ihre Konfiguration Leerlaufzeitlimit Befehl:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Festlegen Sie TCP-Timeout auf Lastenausgleich Endpunkt

Endpunkte gehört zu einem Endpunkt mit Lastenausgleich sind, muss auf dem Endpunkt mit Lastenausgleich TCP-Timeout festgelegt. Zum Beispiel:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Zeitlimit für Cloud-Services ändern

Azure SDK können Sie den Cloud-Dienst aktualisieren. Sie stellen Endpunkt Einstellungen für Cloud-Services in der Datei .csdef. TCP-Timeout für die Bereitstellung eines Cloud-Diensts aktualisierende-Bereitstellung aktualisieren. Eine Ausnahme ist das TCP-Timeout für eine öffentliche IP-Adresse angegeben ist. Öffentliche IP-Einstellungen in der Datei .cscfg sind und durch Bereitstellung von Updates und Upgrades aktualisieren.

Die .csdef für die Endpunkt-Einstellung sind:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

Die .cscfg für die Timeouteinstellung öffentlicher IP-Adressen sind:

    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
        <InstanceAddress roleName="VMRolePersisted">
        <PublicIPs>
          <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
        </PublicIPs>
        </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="rest-api-example"></a>REST-API-Beispiel

Das TCP-Leerlauftimeout können mithilfe der Service Managements-API. Stellen Sie sicher, dass die `x-ms-version` Header soll Version `2014-06-01` oder höher. Aktualisieren Sie die Konfiguration der angegebenen Lastenausgleich input Endpunkte auf allen virtuellen Computern in einer Bereitstellung.

### <a name="request"></a>Anforderung

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Antwort

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Nächste Schritte

[Interne Load Balancer (Übersicht)](load-balancer-internal-overview.md)

[Beginnen Sie einen Lastenausgleich Internetzugriff konfigurieren](load-balancer-get-started-internet-arm-ps.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)
