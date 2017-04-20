<properties
   pageTitle="Lastenausgleich Verteilung Modus konfigurieren | Microsoft Azure"
   description="Konfigurieren von Azure Load Balancer Verteilung Modus Source IP-Affinität unterstützen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>Konfigurieren des Verteilung Modus für Lastenausgleich

## <a name="hash-based-distribution-mode"></a>Hash-basierte Verteilung Modus

Der Standardwert ist ein 5-Tupel (Quell-IP Quellport IP-, Zielport Protokolltyp) Hash verfügbaren Server Datenverkehr zuzuordnen. Klebrigkeit wird nur in eine transportsitzung bereitstellt. Pakete in derselben Sitzung werden auf die gleiche Instanz der Datacenter IP (DIP) hinter den Endpunkt Lastenausgleich weitergeleitet. Beginnt der Client eine neue Sitzung von der gleichen Quell-IP, ändert der Quellport und bewirkt, dass den Datenverkehr zu einem anderen DIP-Endpunkt.

![Hashwert zum Lastenausgleich](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Abbildung 1: 5-Tupel-Verteilung

## <a name="source-ip-affinity-mode"></a>Quell-IP-Affinität Modus

Wir haben eine andere Verteilung Modus Source IP-Affinität (auch sitzungsaffinität oder IP-Clientaffinität). Azure Lastenausgleich kann konfiguriert werden ein 2-Tupel (IP-Quelladresse, IP-Zieladresse) oder 3-Tupel (Quell-IP Ziel-IP-Protokoll) verfügbaren Server Datenverkehr zuzuordnen. Mithilfe von Quell-IP-Affinität geht Verbindungen vom gleichen Client-Computer mit dem gleichen DIP-Endpunkt.

Das folgende Diagramm zeigt eine 2-Tupel-Konfiguration. Beachten Sie, wie das 2-Tupel Lastenausgleich virtuellen Computer 1 (VM1) durchläuft dann VM2 und VM3 gesichert.

![sitzungsaffinität](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Abbildung 2: 2-Tupel-Verteilung

Quell-IP-Affinität löst eine Inkompatibilität zwischen Azure-Lastenausgleich und Gateway Desktop (RD). Jetzt können Sie eine RD-Gateway Farm in einem einzigen Clouddienst erstellen.

Eine andere Verwendung Fall ist Medien hochladen Datenupload geschieht über UDP wobei Steuerebene erfolgt über TCP:

- Ein Client initiiert eine TCP-Sitzung an öffentlichen Lastenausgleich Erstens wird angewiesen, sich bestimmte diesen Kanal Verbindung überwachen aktiv bleibt
- Eine neue UDP-Sitzung vom selben Clientcomputer wird initiiert, dieselbe Lastenausgleich öffentliche IP-Adresse, der Annahme, dass diese Verbindung auch mit dem gleichen DIP-Endpunkt geleitet wird, vorherige TCP-Verbindung, dass Medien hochladen bei hoher Durchsatz ausgeführt werden kann gleichzeitig einen Kanal über TCP.

>[AZURE.NOTE] Wenn eine Gruppe mit Lastenausgleich ändert ist entfernen oder Hinzufügen eines virtuellen Computers, die Verteilung der Clientanforderungen berechnet. Sie können nicht neue Verbindungen von vorhandenen Clients auf demselben Server abhängen. Darüber hinaus verursachen mit Quell-IP-Affinität Verteilung Modus eine ungleiche Verteilung von Datenverkehr. Clients hinter Proxys können als eine eindeutige Client-Anwendung angezeigt.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigurieren von Quell-IP-affinitätseinstellungen für Lastenausgleich

Für virtuelle Maschinen können Sie PowerShell Timeout ändern:

Einen Azure Endpunkt einer virtuellen Maschine hinzufügen und Load Balancer Verteilung Modus

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

LoadBalancerDistribution kann festgelegt werden, um SourceIP für 2-Tupel (IP-Quelladresse, IP-Zieladresse) Lastenausgleich, SourceIPProtocol Lastenausgleich 3-Tupel (Quell-IP Ziel-IP-Protokoll) oder keine ggf. das Standardverhalten des Lastenausgleichs 5-Tupel.

Anhand der folgenden eine Load Balancer Verteilung Modus Endpunktkonfiguration abzurufen:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

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
    LoadBalancerDistribution : sourceIP

Wenn das LoadBalancerDistribution-Element nicht vorhanden ist verwendet Azure Lastenausgleich den Standardalgorithmus 5-Tupel.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Festlegen Sie Vertrieb-Modus auf Lastenausgleich Endpunkt

Endpunkte ein Teil Lastenausgleich Endpunkt hingegen muss Verteilung Modus auf dem Lastenausgleich Endpunkt festlegen:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Cloud-Dienstkonfiguration Verteilung ändern

Nutzen Sie Azure SDK für .NET 2.5 (veröffentlicht im November) dem Cloud-Dienst aktualisieren. Endpunkt werden für Cloud-Dienste in die .csdef eingestellt. Um Load Balancer Verteilung Modus für eine Bereitstellung Cloud-Dienste aktualisieren, ist ein Upgrade Bereitstellung erforderlich.
Hier ist ein Beispiel für Endpunkt Einstellungen .csdef ändert:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
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

## <a name="api-example"></a>API-Beispiel

Sie können Load Balancer Verteilung mit dem Servicemanagement API. Fügen Sie auf der `x-ms-version` Header soll Version `2014-09-01` oder höher.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Aktualisieren Sie die Konfiguration für die angegebene Lastenausgleich in einer Bereitstellung

#### <a name="request-example"></a>Anforderung-Beispiel

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

Der Wert des LoadBalancerDistribution kann SourceIP für 2-Tupel Affinität, SourceIPProtocol für 3-Tupel Affinität oder keine (keine Affinität. d. h. 5-Tupel)

#### <a name="response"></a>Antwort

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Nächste Schritte

[Interne Load Balancer (Übersicht)](load-balancer-internal-overview.md)

[Erste Schritte mit Lastenausgleich ein Internetzugriff konfigurieren](load-balancer-get-started-internet-arm-ps.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)
