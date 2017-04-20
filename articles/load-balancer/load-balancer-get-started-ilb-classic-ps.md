<properties
   pageTitle="Erstellen einer internen Lastenausgleich mithilfe von PowerShell im klassischen Bereitstellungsmodell | Microsoft Azure"
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

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Erste Schritte beim Erstellen einer internen Lastenausgleich (klassisch) mit PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Erstellen einer internen Lastenausgleich für virtuelle Computer festlegen

Erstellen einer internen System zum Lastenausgleich Lastsatz und Servern, die ihren Datenverkehr zu senden, müssen Sie Folgendes:

1. Erstellen Sie eine Instanz des internen Lastenausgleich, die den Endpunkt zu Lastenausgleich auf die Server einer Gruppe mit Lastenausgleich von eingehendem.

1. Hinzufügen von Endpunkten für den virtuellen Computer, die den eingehenden Datenverkehr empfangen.

1. Konfigurieren Sie die Server, die Datenverkehr Lastenausgleich um ihren Datenverkehr an die virtuelle IP-Adresse (VIP) Adresse der internen Lastenausgleich Instanz senden zu senden.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Schritt 1: Erstellen einer internen Lastenausgleich Instanz

Für einen vorhandenen Cloud-Dienst oder einen Cloud-Dienst unter einem regionalen virtuellen Netzwerk bereitgestellt können interne Lastenausgleich Erstellen einer Instanz mit folgenden Windows PowerShell-Befehlen:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Beachten Sie, dass diese Verwendung des [Add-AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx) Windows PowerShell-Cmdlets Parametersatz DefaultProbe. Finden Sie weitere Informationen auf zusätzliche Parameter [Hinzufügen AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Schritt 2: Hinzufügen von Endpunkten zu internen Lastenausgleich Instanz

Hier ist ein Beispiel:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>Schritt 3: Konfigurieren der Server den Datenverkehr an den neuen internen Lastenausgleich Endpunkt senden

Sie haben zum Konfigurieren des Servers, dessen Datenverkehr Lastenausgleich neue IP-Adresse (VIP) Instanz interne Netzwerklastenausgleich verwendet werden soll. Dies ist die Adresse, die internen Lastenausgleich Instanz überwacht. In den meisten Fällen müssen Sie nur hinzufügen oder Ändern von DNS-Datensatz für die VIP-Adresse der internen Lastenausgleich Instanz.

Wenn Sie während der Erstellung der Instanz internen Lastenausgleich die IP-Adresse angegeben, verfügen Sie bereits über die VIP-Adresse. Andernfalls sehen Sie die VIP-Adresse der folgenden Befehle aus:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Um diese Befehle zu verwenden, geben Sie die Werte, und entfernen Sie die < und >. Hier ist ein Beispiel:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


Notieren Sie aus der Anzeige des Befehls Get-AzureInternalLoadBalancer die IP-Adresse und ändern Sie die Server oder DNS-Datensätze, um sicherzustellen, dass Datenverkehr an die VIP-Adresse gesendet wird.

>[AZURE.NOTE]Microsoft Azure-Plattform verwendet eine statische, öffentlich routingfähige IPv4-Adresse für verschiedene administrative Szenarios. Die IP-Adresse ist 168.63.129.16. Diese IP-Adresse sollte nicht durch Firewalls, blockiert werden, da es zu unerwartetem Verhalten führen kann.
>Bei Azure internen Lastenausgleich diese IP-Adresse dient durch Überwachen von Lastenausgleich Prüfpunkte Zustand für virtuelle Computer in einer Lastenausgleich bestimmt. Sicherstellen Sie eine Netzwerk-Sicherheitsgruppe Azure virtuelle Computer in einem intern Lastenausgleich Verkehr eingeschränkt wird oder ein virtuelles Netzwerk-Subnetz zugewiesen wird, dass eine Netzwerkregel Sicherheit Datenverkehr von 168.63.129.16 hinzugefügt.


## <a name="example-of-internal-load-balancing"></a>Beispiel für interne Lastenausgleich

Um schrittweise den Ende zum Prozess erstellen Lastenausgleich für zwei Beispielkonfigurationen finden Sie in den folgenden Abschnitten.

### <a name="an-internet-facing-multi-tier-application"></a>Ein Internetzugriff Multi-Tier-Anwendung

Möchten Sie einen Lastenausgleich Datenbank Service für eine Reihe von Web-Servern mit Internetzugang. Beide Server werden in einem einzelnen Azure-Cloud-Dienst gehostet. Web-Verkehr auf TCP-Port 1433 muss zwei virtuelle Computer auf der Datenbankebene verteilt. Abbildung 1 zeigt die Konfiguration.

![Interne Lastenausgleich Satz für die Datenbankebene](./media/load-balancer-internal-getstarted/IC736321.png)


Die Konfiguration besteht aus:

- Vorhandenen Cloud-Dienst hosten virtueller Computer heißt Mytestcloud.

- Die beiden vorhandenen Datenbankserver heißen DB1, DB2.

- Webserver in der Webebene Verbinden mit Datenbankserver auf der Datenbankebene über private IP-Adresse. Eine weitere Option ist eigene DNS für das virtuelle Netzwerk und einen A-Eintrag für den internen Load Balancer Satz manuell registrieren.

Die folgenden Befehle eine neue interne Lastenausgleich-Instanz mit dem Namen **ILBset** konfigurieren und die virtuellen Computer für die zwei Datenbankserver Endpunkte hinzufügen:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Entfernen einer internen Netzwerklastenausgleich-Konfigurations

Um einen virtuellen Computer als Endpunkt aus einer internen Load Balancer Instanz zu entfernen, verwenden Sie die folgenden Befehle:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Um diese Befehle zu verwenden, geben Sie die Werte Entfernen der < und >.

Hier ist ein Beispiel:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Um eine interne Load Balancer Instanz einen Cloud-Dienst zu entfernen, verwenden Sie die folgenden Befehle:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

Um diese Befehle zu verwenden, geben Sie den Wert, und entfernen Sie die < und >.

Hier ist ein Beispiel:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Weitere Informationen über interne Load Balancer cmdlets


Weitere Informationen zu internen Lastenausgleich Cmdlets führen Sie die folgenden Befehle in Windows PowerShell eingeben:

- Hilfe neue AzureInternalLoadBalancerConfig-voll

- Hilfe hinzufügen AzureInternalLoadBalancer-voll

- Get-Help Get-AzureInternalLoadbalancer-voll

- Get-Help Remove-AzureInternalLoadBalancer-voll

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren Sie einen Quell-IP-Affinität mit Load Balancer Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)