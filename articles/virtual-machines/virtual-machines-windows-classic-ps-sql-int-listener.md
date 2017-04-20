<properties
    pageTitle="Einen ILB-Listener für immer auf Verfügbarkeit konfigurieren | Microsoft Azure"
    description="In diesem Lernprogramm verwendet Ressourcen mit dem klassischen Bereitstellungsmodell und erstellt eine immer auf Verfügbarkeitsgruppenlistener in Azure mit einer internen Load Balancer (ILB)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Konfigurieren Sie einen ILB-Listener für immer auf Verfügbarkeit in Azure

> [AZURE.SELECTOR]
- [Interne Listener](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externe Listener](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>Übersicht

In diesem Thema veranschaulicht die Listener für ein immer auf Availability Group mit einer **Internen Load Balancer (ILB)**konfigurieren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zum Konfigurieren eines ILB Listeners für eine verfügbarkeitsgruppe im Ressourcen-Manager-Modell finden Sie unter [Konfigurieren einer internen Lastenausgleich für eine Gruppe immer auf Verfügbarkeit in Azure](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


Der Availability Group können Replikate, die lokal, Azure, enthalten oder lokalen und Azure Hybrid-Konfigurationen umfassen. Azure Replikate können innerhalb derselben Region oder mehrere Regionen über mehrere virtuelle Netzwerke (VNets) befinden. Schritte angenommen haben bereits [eine verfügbarkeitsgruppe konfiguriert](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) aber keinen Listener konfiguriert haben.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Richtlinien und Einschränkungen für interne Listener
Beachten Sie die folgenden Richtlinien auf die Verfügbarkeit Gruppe in Azure mit ILB:

- Der verfügbarkeitsgruppenlistener wird unter Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2 unterstützt.

- Pro Cloud-Dienst wird nur eine interne verfügbarkeitsgruppenlistener unterstützt, da der Listener für den ILB konfiguriert ist, und gibt es nur eine ILB pro Cloud-Dienst. Es ist jedoch möglich, mehrere externe Listener zu erstellen. Weitere Informationen finden Sie unter [Konfigurieren einer externen Listener für immer auf Verfügbarkeit in Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

- Einen internen Listener im selben Cloud-Dienst erstellen, verfügen Sie über eine externe Listener mit Cloud-Dienst öffentliche VIP, wird nicht unterstützt.

## <a name="determine-the-accessibility-of-the-listener"></a>Den Zugriff des Listeners ermitteln

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Dieser Artikel konzentriert sich auf einen Listener, der eine **Interne Load Balancer (ILB)**verwendet. Benötigen Sie einen Public-externe Listener finden Sie in diesem Artikel, die Schritte zum Einrichten eines [externen Listener](virtual-machines-windows-classic-ps-sql-ext-listener.md) bereitstellt

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Erstellen Sie mit Lastenausgleich VM Endpunkte mit direct Server zurück

ILB legen Sie zunächst die internen Lastenausgleich. Dies erfolgt im folgenden Skript.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Für **ILB**sollten Sie eine statische IP-Adresse zuweisen. Untersuchen Sie zuerst die aktuelle VNet Konfiguration durch den folgenden Befehl ausführen:

        (Get-AzureVNetConfig).XMLConfiguration

1. Beachten Sie **den Teilnetznamen für das Subnetz mit VMs, die die Replikate enthalten** . Dies wird in der **$SubnetName** -Parameter im Skript verwendet.

1. Notieren Sie den Namen **VirtualNetworkSite** und der ersten **AddressPrefix** für das Subnetz mit VMs, die die Replikate enthalten. Suchen Sie eine verfügbare IP-Adresse beide Werte Befehl **Test-AzureStaticVNetIP** und **AvailableAddresses**überprüft. Beispielsweise wurde das VNet *MyVNet* und war Adresse liegen, gestartet am *172.16.0.128*: der folgende Befehl Liste Verfügbare Adressen

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Wählen Sie eine der verfügbaren Adressen und im **$ILBStaticIP** -Parameter das folgende Skript verwenden.

3. Kopieren Sie das folgende PowerShell-Skript in einem Text-Editor, und legen Sie die Werte entsprechend Ihrer Umgebung (Beachten Sie die Standardeinstellungen für einige Parameter bereitgestellten). Beachten Sie, dass vorhandene Installationen, die Gruppen mit ILB hinzugefügt werden können. Weitere Informationen zu ILB finden Sie unter [Interne Lastenausgleich](../load-balancer/load-balancer-internal-overview.md). Auch wenn Ihre Verfügbarkeit Azure Regionen erstreckt, führen Sie das Skript einmal in jedem Rechenzentrum für den Cloud-Dienst und Knoten im Datencenter befinden.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Nachdem Sie die Variablen festgelegt haben, kopieren Sie das Skript Texteditor in Azure PowerShell-Sitzung ausgeführt. Wenn die Meldung weiterhin angezeigt >>, geben Sie die EINGABETASTE erneut, um sicherzustellen, dass das Skript läuft. Hinweis

>[AZURE.NOTE] Azure-Verwaltungsportal unterstützt keine internen Lastenausgleich zu diesem Zeitpunkt die ILB oder die Endpunkte im klassischen Azure-Portal wird nicht angezeigt. **Get-AzureEndpoint** gibt jedoch eine interne IP-Adresse läuft Lastenausgleich auf. Andernfalls wird null zurückgegeben.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>KB2854082 bei Bedarf installiert ist

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Öffnen Sie die Firewall-Ports in Gruppenknoten Verfügbarkeit

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Erstellen der verfügbarkeitsgruppenlistener

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Für ILB verwenden Sie die IP-Adresse des zuvor erstellten internen Load Balancer (ILB). Verwenden Sie das folgende Skript diese IP-Adresse in PowerShell.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. Auf einem virtuellen Computer PowerShell-Skript für Ihr Betriebssystem in einen Texteditor kopieren und die Variablen auf die zuvor notierten Werte festgelegt.

    Verwenden Sie für Windows Server 2012 oder höher das folgende Skript:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    Verwenden Sie für Windows Server 2008 R2 das folgende Skript:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Einmal Sie Variablen festgelegt haben, öffnen Sie ein Fenster mit erweiterten Windows PowerShell kopieren Sie das Skript im Text-Editor und fügen Sie Ihre Azure PowerShell-Sitzung ausführen. Wenn die Meldung weiterhin angezeigt >>, geben Sie die EINGABETASTE erneut, um sicherzustellen, dass das Skript läuft.

2. Wiederholen Sie diesen Schritt für jeden virtuellen Computer. Dieses Skript wird die IP-Adressressource mit IP-Adresse des Cloud-Dienst und andere Parameter wie der Prüfpunkt-Port. Wenn die IP-Adressressource online geschaltet wird, kann es vom Endpunkt mit Lastenausgleich in diesem Lernprogramm erstellt dann zum Abrufen der Prüfpunkt-Port reagieren.

## <a name="bring-the-listener-online"></a>Den Listener online schalten

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Weiterverfolgung Elemente

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testen der verfügbarkeitsgruppenlistener (innerhalb der gleichen VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
