<properties
   pageTitle="Erstellen einer internen Lastenausgleich für Cloud-Services im klassischen Bereitstellungsmodell | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer internen Lastenausgleich mithilfe von PowerShell im klassischen Bereitstellungsmodell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Erste Schritte erstellen eine interne Lastenausgleich für Cloud-Services (klassisch)

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Interne Lastenausgleich für Cloud-Dienste konfigurieren

Interne Lastenausgleich wird für virtuelle Computer und Cloud-Dienste unterstützt. Ein interne Load Balancer Endpunkt in einem Clouddienst außerhalb einer regionalen virtuelles Netzwerk erstellt werden nur im Cloud-Dienst.

Interne Load Balancer Konfiguration muss während der Erstellung der ersten Bereitstellung Cloud-Dienst eingestellt werden wie im folgenden Beispiel gezeigt.

>[AZURE.IMPORTANT] Voraussetzung auszuführenden Schritte ist ein virtuelles Netzwerk für die Cloudbereitstellung bereits erstellt haben. Sie benötigen virtuellen Netzwerk- und Subnet-Name der internen Lastenausgleich erstellen.

### <a name="step-1"></a>Schritt 1

Service-Konfigurationsdatei (.cscfg) für die Cloudbereitstellung in Visual Studio öffnen und folgenden internen Lastenausgleich unter dem letzten erstellen hinzufügen "`</Role>`" Artikel für die Netzwerkkonfiguration.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Fügen Sie die Werte für die Netzwerk-Konfigurationsdatei zeigen, wie es aussieht. Beispiel: Angenommen Sie, ein Subnetz namens "Test_vnet" mit einem Subnetz 10.0.0.0/24 namens Test_subnet und eine statische IP-Adresse 10.0.0.4 erstellt. Der Lastenausgleich wird TestLB genannt.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Weitere Informationen zum Load Balancer Schema finden Sie unter [Hinzufügen Balancer laden](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Schritt 2


Ändern der Definitionsdatei Service (.csdef) der internen Lastenausgleich Endpunkte hinzufügen. Eine Instanz der Rolle erstellt Moment verleihen Service-Definitionsdatei die Rolleninstanzen internen Lastenausgleich.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Fügen Sie nach dieselben Werte aus dem obigen Beispiel die Werte der Service-Definitionsdatei.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Der Netzwerkverkehr wird Lastenausgleich TestLB Lastenausgleich über Port 80 für eingehende Anfragen senden Rolleninstanzen Worker auch auf Port 80.


## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren Sie einen Quell-IP-Affinität mit Load Balancer Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)