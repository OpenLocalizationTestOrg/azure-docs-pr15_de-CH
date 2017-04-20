<properties
   pageTitle="Erste Schritte beim Erstellen ein Lastenausgleich mit klassischen Bereitstellung Modell für Cloud-Services mit Internetzugriff | Microsoft Azure"
   description="Erstellen Sie ein System zum Lastenausgleich im klassischen Bereitstellungsmodell für Cloud-Services mit Internetzugriff"
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
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Erste Schritte beim Erstellen einer Internetschnittstelle Lastenausgleich für Cloud-services

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell. Sie können auch [ein Lastenausgleich mithilfe von Azure Resource Manager mit Internetzugriff erstellen](load-balancer-get-started-internet-arm-cli.md).

Cloud-Dienste werden automatisch mit Lastenausgleichsfunktion konfiguriert und können über das Service-Modell angepasst werden.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Erstellen Sie eine Service-Definitionsdatei mit Lastenausgleich

Sie können das Azure SDK für .NET 2.5 aktualisieren Ihre Cloud-Dienst nutzen. Endpunkt sind für Cloud-Dienste in der [Dienstdefinition](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef Datei eingestellt.

Das folgende Beispiel zeigt, wie eine servicedefinition.csdef für eine Cloudbereitstellung konfiguriert ist:

Überprüfen der Snipet für die .csdef-Datei von einer Cloudbereitstellung generiert, den externen Endpunkt konfiguriert Anschlüsse HTTP Port 10000 und 10001, 10002 angezeigt.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Überprüfen Sie Load Balancer Zustand für Cloud-services


Folgendes ist ein Beispiel eines Prüfpunkts Health:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

Lastenausgleich kombiniert die Informationen des Endpunkts und die Informationen des Prüfpunkts URL in Form von http://{DIP von VM}:80/Probe.aspx erstellen, mit der den Zustand des Dienstes abzufragen.

Der Dienst erkennt regelmäßige Prüfpunkte von derselben IP-Adresse. Dies ist Gesundheit Probe-Anforderung aus der Host des Knotens, auf dem der virtuelle Computer ausgeführt.
Der Dienst hat Antworten mit einem HTTP 200 Status des Lastenausgleichs davon ausgehen, dass der Dienst fehlerfrei ist. Alle anderen HTTP-Statuscode (z. B. 503) direkt nimmt den virtuellen Computer aus der Rotation.

Definition der Prüfpunkt steuert auch die Häufigkeit des Prüfpunkts. In unserem Fall Sondieren Lastenausgleich Endpunkt alle 5 Sekunden. 10 Sekunden (zwei Prüfpunkt Intervalle) keine positive Antwort empfangen, der Prüfpunkt wird unten und den virtuellen Computer aus der Rotation genommen. Ebenso wird, wenn der Dienst aus der Rotation und positive Antwort empfangen wird, der Dienst wieder Drehung sofort gesetzt. Wenn der Dienst zwischen fehlerfreie und fehlerhafte schwankt, können Lastenausgleich Wiedereinführung des Dienstes, Drehung verzögern, bis er für eine Anzahl von Prüfpunkten wurde.

Überprüfen Sie Service Definitionsschema für den [Integritätstest](https://msdn.microsoft.com/library/azure/jj151530.aspx) für Weitere Informationen.

## <a name="next-steps"></a>Nächste Schritte

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-ps.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)

