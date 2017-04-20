<properties
    pageTitle="Externen Listener für immer auf Verfügbarkeit konfigurieren | Microsoft Azure"
    description="Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer immer auf Verfügbarkeitsgruppenlistener in Azure extern zugänglich sind, die öffentliche virtuelle IP-Adresse zugeordneten Cloud-Dienst mit."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Konfigurieren Sie externen Listener für immer auf Verfügbarkeit in Azure

> [AZURE.SELECTOR]
- [Interne Listener](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externe Listener](virtual-machines-windows-classic-ps-sql-ext-listener.md)

Dieses Thema veranschaulicht einen Listener für ein immer auf Availability Group konfigurieren, die extern auf das Internet zugegriffen werden kann. Dies erfolgt durch den Listener Cloud-Dienst **Öffentliche VIP (Virtual IP)** -Adresse zuordnen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Der Availability Group können Replikate, die lokal, Azure, enthalten oder lokalen und Azure Hybrid-Konfigurationen umfassen. Azure Replikate können innerhalb derselben Region oder mehrere Regionen über mehrere virtuelle Netzwerke (VNets) befinden. Schritte angenommen haben bereits [eine verfügbarkeitsgruppe konfiguriert](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) aber keinen Listener konfiguriert haben.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Richtlinien und Einschränkungen für externe Listener

Beachten Sie die folgenden Richtlinien zum verfügbarkeitsgruppenlistener in Azure beim Bereitstellen mit öffentlichen Cloud Service-VIP-Adresse:

- Der verfügbarkeitsgruppenlistener wird unter Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2 unterstützt.

- Die Clientanwendung muss auf eine andere Cloud-Dienst als befinden, das die Verfügbarkeit VMs enthält. Azure unterstützt keine direkte Server-Retoure mit Client und Server im selben Cloud-Dienst.

- Standardmäßig an die Schritte in diesem Artikel einen Listener Verwendung die Cloud Service VIP (Virtual IP)-Adresse konfigurieren. Es ist jedoch möglich, reservieren und mehrere VIP-Adressen für den Clouddienst erstellen. Dadurch können Sie mit den Schritten in diesem Artikel mehrere Listener erstellen, die verschiedene VIP zugeordnet sind. Informationen zu mehreren VIP-Adressen finden Sie unter [Mehrere VIPs pro Cloud-Dienst](../load-balancer/load-balancer-multivip.md).

- Wenn Sie einen Listener für eine hybridumgebung erstellen, müssen im lokalen Netzwerk mit dem öffentlichen Internet neben Standort-zu-Standort-VPN mit Azure virtual Network. Der verfügbarkeitsgruppenlistener ist in Azure Subnetz nur öffentliche IP-Adresse des jeweiligen Cloud-Dienst erreichbar.

- Externen Listener im selben Cloud-Dienst erstellen, verfügen Sie über einen internen Listener mit internen Load Balancer (ILB) wird nicht unterstützt.

## <a name="determine-the-accessibility-of-the-listener"></a>Den Zugriff des Listeners ermitteln

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Dieser Artikel konzentriert sich auf einen Listener, der **externen Lastenausgleich**verwendet. Gegebenenfalls einen Listener, die privaten virtuellen Netzwerks finden Sie in diesem Artikel, die Schritte zum Einrichten eines [mit ILB](virtual-machines-windows-classic-ps-sql-int-listener.md) bereitstellt

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Erstellen Sie mit Lastenausgleich VM Endpunkte mit direct Server zurück

Externe verwendet das virtuelle öffentliche virtuelle IP-Adresse des Cloud-Dienst, der die virtuellen Computer hostet. Damit brauchen Sie erstellen oder konfigurieren den Lastenausgleich bei.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Kopieren Sie das folgende PowerShell-Skript in einem Text-Editor, und legen Sie die Variablenwerte an Ihre Umgebung an (Standardwerte sind einige Parameter vorgesehen). Beachten Sie, dass wenn Ihre Verfügbarkeit Azure Regionen erstreckt, Sie müssen das Skript einmal in jedem Rechenzentrum für den Cloud-Dienst und Knoten im Datencenter befinden.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Nachdem Sie die Variablen festgelegt haben, kopieren Sie das Skript Texteditor in Azure PowerShell-Sitzung ausgeführt. Wenn die Meldung weiterhin angezeigt >>, geben Sie die EINGABETASTE erneut, um sicherzustellen, dass das Skript läuft.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>KB2854082 bei Bedarf installiert ist

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Öffnen Sie die Firewall-Ports in Gruppenknoten Verfügbarkeit

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Erstellen der verfügbarkeitsgruppenlistener

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Für externe Lastenausgleich müssen öffentliche virtuelle IP-Adresse des Cloud-Dienst erhalten, die Replikate enthält. Klassischen Azure-Portal anmelden. Navigieren Sie zum Cloud-Dienst, der die Verfügbarkeit VM enthält. **Dashboard** -Ansicht zu öffnen.

3. Beachten Sie die Adresse unter **Öffentliche VIP (Virtual IP)-Adresse**. Die Lösung umfasst die VNets, wiederholen Sie diesen Schritt für jeden Clouddienst mit einer VM, die ein Replikat befindet.

4. Auf einem virtuellen Computer das folgende PowerShell-Skript in einen Texteditor kopieren und Variablen auf die zuvor notierten Werte festgelegt.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Einmal Sie Variablen festgelegt haben, öffnen Sie ein Fenster mit erweiterten Windows PowerShell kopieren Sie das Skript im Text-Editor und fügen Sie Ihre Azure PowerShell-Sitzung ausführen. Wenn die Meldung weiterhin angezeigt >>, geben Sie die EINGABETASTE erneut, um sicherzustellen, dass das Skript läuft.

1. Wiederholen Sie diesen Schritt für jeden virtuellen Computer. Dieses Skript wird die IP-Adressressource mit IP-Adresse des Cloud-Dienst und andere Parameter wie der Prüfpunkt-Port. Wenn die IP-Adressressource online geschaltet wird, kann es vom Endpunkt mit Lastenausgleich in diesem Lernprogramm erstellt dann zum Abrufen der Prüfpunkt-Port reagieren.

## <a name="bring-the-listener-online"></a>Den Listener online schalten

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Weiterverfolgung Elemente

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testen der verfügbarkeitsgruppenlistener (innerhalb der gleichen VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Testen der verfügbarkeitsgruppenlistener (über Internet)

Um den Listener außerhalb des virtuellen Netzwerks zuzugreifen, müssen Sie verwenden externe öffentlichem Lastenausgleich (in diesem Thema beschrieben) ILB, ist nur zugänglich innerhalb derselben VNet. In der Verbindungszeichenfolge Geben Sie den Namen Cloud. Wäre einen Cloud-Dienst mit dem Namen *Mycloudservice*würde z. B. Sqlcmd-Anweisung folgendermaßen aussehen:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Im Gegensatz zum vorherigen Beispiel muss SQL-Authentifizierung verwendet werden, da der Aufrufer Windows-Authentifizierung über das Internet verwenden kann. Weitere Informationen finden Sie unter [immer auf Availability Group in Azure VM: Client Connectivity Szenarios](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Bei SQL-Authentifizierung stellen Sie sicher, dass derselbe Benutzername auf beide Replikate erstellen. Weitere Informationen zur Problembehandlung bei Benutzernamen mit Verfügbarkeit finden Sie unter [wie Benutzernamen zuordnen oder enthaltenen SQL Datenbankbenutzer mit anderen Replikaten und verfügbarkeitsdatenbanken zuordnen](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Sind Replikate ständig in verschiedenen Subnetzen, müssen Clients angeben **MultisubnetFailover = True** in der Verbindungszeichenfolge. Dadurch parallelen Verbindungsversuche zu Replikaten in verschiedenen Subnetzen. Beachten Sie, dass dieses Szenario eine Cross-Region immer auf Verfügbarkeit Bereitstellung.

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
