<properties
   pageTitle="Immer auf Availability Group – Microsoft Azure konfigurieren"
   description="Konfigurieren Sie Verfügbarkeit Gruppe Listener Azure Ressourcenmanager Modell internen Lastenausgleich eine oder mehrere IP-Adressen."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Konfigurieren Sie mindestens immer auf Verfügbarkeit Gruppe Listener - Ressourcen-Manager 

Dieses Thema zeigt, wie zwei Dinge tun:

- Erstellen einer internen Lastenausgleich für SQL Server-Verfügbarkeitsgruppen mithilfe von PowerShell-Cmdlets.

- Ein Lastenausgleich unterstützt mehrere SQL Server-verfügbarkeitsgruppe fügen Sie weitere IP-Adressen hinzu. 

In SQL Server wird ein verfügbarkeitsgruppenlistener virtuelles Netzwerk Clients herstellen, um Zugriff auf eine Datenbank im primären oder sekundären Replikat. Auf Azure virtuelle Computer enthält einen Lastenausgleich die IP-Adresse für den Listener. Lastenausgleich leitet den Datenverkehr an die Instanz von SQL Server, die die Probe Port abhört. In den meisten Fällen wird eine verfügbarkeitsgruppe interner Lastenausgleich verwendet. Ein Azure internen Lastenausgleich kann eine oder mehrere IP-Adressen aufnehmen. Jede IP-Adresse verwendet einen bestimmten Prüfpunkt Port. Dieses Dokument veranschaulicht, wie mithilfe von PowerShell ein neues Lastenausgleich erstellen oder eine vorhandene Lastenausgleich für SQL Server-Verfügbarkeitsgruppen IP-Adressen hinzuzufügen. 

Mehrere IP-Adressen einer internen Lastenausgleich zuweisen ist in Azure neu und ist nur in Ressourcen-Manager-Modell. Um diese Aufgabe auszuführen, müssen Sie eine SQL Server-Verfügbarkeit Gruppe auf Azure VMs in Ressourcen-Manager-Modell bereitgestellt. Beide virtuelle SQL Server-Computer muss auf dieselben Verfügbarkeit gehören. [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können Sie automatisch die verfügbarkeitsgruppe in Azure-Ressourcen-Manager erstellen. Diese Vorlage erstellt automatisch die verfügbarkeitsgruppe interner Lastenausgleich für Sie. Alternativ können Sie [eine AlwaysOn Availability Group manuell konfigurieren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

In diesem Thema muss die Verfügbarkeitsgruppen bereits konfiguriert sind.  

Verwandte Themen:

- [Konfigurieren von AlwaysOn Availability Gruppen in Azure VM (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [Konfigurieren einer Verbindung VNet VNet mithilfe von Azure-Ressourcen-Manager und PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Windows-Firewall konfigurieren

Konfigurieren Sie Windows-Firewall für den SQL Server-Zugriff. Sie müssen den Firewall TCP-Verbindungen mit Ports der SQL Server-Instanz sowie mit den Listener verwendet Port konfigurieren. Ausführliche Informationen finden Sie unter [Konfigurieren einer Windows-Firewall für den Datenbankmodulzugriff](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Erstellen Sie eine eingehende Regel für den SQL Server-Port und Port Prüfpunkt.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Beispielskript: Erstellen einer internen Lastenausgleich mit PowerShell

> [AZURE.NOTE] Der Availability Group mit der [Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) Erstellen des Auslastungstests brauchen Sie diesen Schritt ausführen. 

PowerShell-Skript erstellt einen internen Lastenausgleich, Lastenausgleich Regeln konfiguriert und legt eine IP-Adresse für den Lastenausgleich. Um das Skript auszuführen, öffnen Sie Windows PowerShell ISE und fügen Sie das Skript im Skriptfenster. Mit `Login-AzureRMAccount` bei PowerShell. Haben Sie mehrere Azure-Abonnements verwenden `Select-AzureRmSubscription ` Abonnement festlegen. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Beispielskript: eine vorhandene Lastenausgleich mit PowerShell eine IP-Adresse hinzufügen

Verwendung mehrerer Verfügbarkeit Gruppen mithilfe von PowerShell vorhandenen Lastenausgleich eine zusätzliche IP-Adresse hinzufügen. Jede IP-Adresse erfordert eigene Lastenausgleich Regel Prüfpunkt Port und front-Port.

Front-End-Port ist der Port, den Anwendung verwenden, um die SQL Server-Instanz herstellen. IP-Adressen für verschiedene Verfügbarkeitsgruppen können demselben front-End-Port.

>[AZURE.NOTE] Für SQL Server-Verfügbarkeitsgruppen muss jede IP-Adresse einen Sonde Port. Beispielsweise können eine IP-Adresse auf einen Lastenausgleich Prüfpunkt Port 59999 verwendet, keine andere IP-Adressen, Lastenausgleich Prüfpunkt Anschluss 59999 verwenden. 

- Informationen zum Lastenausgleich finden Sie Grenzwerte unter [Netzwerk Grenzen - Ressourcenmanager Azure](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits) **Private Frontend-IP pro Lastenausgleich** .

- Informationen zur Verfügbarkeit finden Sie unter Gruppe wird [eingeschränkt (Verfügbarkeitsgruppen)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Das folgende Skript fügt eine neue IP-Adresse einer vorhandenen Lastenausgleich. Aktualisieren Sie die Variablen für Ihre Umgebung. Der ILB verwendet den Listenerport für Load balancing Frontend Port. Dieser Anschluss kann den Port, dem SQL Server überwacht. Für Standardinstanzen von SQL Server ist dies Port 1433. Load balancing-Regel für eine verfügbarkeitsgruppe erfordert eine schwebende IP (direct Server zurückgeben) Back-End-Port der front-End-Port identisch ist.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfigurieren Sie den Cluster Load Balancer IP-Adresse verwendet 

Im nächste Schritt ist konfigurieren Sie den Listener auf dem Cluster und den Listener online. Führen Sie dazu folgende Schritte aus: 

1. Erstellen Sie der verfügbarkeitsgruppenlistener im Failovercluster  

2. Den Listener online schalten

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. erstellen Sie 1. die verfügbarkeitsgruppenlistener im Failovercluster

In diesem Schritt des Failoverclusters mit Failovercluster-Clientzugriffspunkt hinzu, und verwenden Sie PowerShell Clusterressource zum Abhören der Prüfpunkt-Ports konfigurieren. 

1. Mit der Azure virtuellen Computer herstellen, der primäre Replikat hostet RDP. 

2. Failovercluster-Manager zu öffnen.

3. Wählen Sie den Knoten **Netzwerke** und beachten Sie Cluster-Netzwerkname. Verwenden Sie diesen Namen in der `$ClusterNetworkName` -Variable PowerShell-Skript.

4. Erweitern Sie den Clusternamen und klicken Sie auf **Rollen**.

5. Im Bereich **Rollen** die Verfügbarkeit Gruppe Namen, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt**.

6. Erstellen Sie im Feld **Name** einen Namen für diesen neuen Listener klicken Sie zweimal auf **Weiter** , und klicken Sie dann auf **Fertig stellen**. Schalten Sie den Listener oder die Ressource online zu diesem Zeitpunkt.
 
    Der Name für den neuen Listener wird der Netzwerkname Applikationen Verbindung zu Datenbanken in der SQL Server-verfügbarkeitsgruppe verwenden.

7. Klicken Sie auf die Registerkarte **Ressourcen** und anschließend Clientzugriffspunkt, die Sie gerade erstellt haben. Die IP-Adressressource und Eigenschaften. Beachten Sie die IP-Adresse ein. Verwenden Sie diesen Namen in der `$IPResourceName` -Variable PowerShell-Skript.

8. Unter **IP-Adresse** auf **Statische IP-Adresse** , und legen Sie die statische IP-Adresse dieselbe Adresse, die Sie beim Festlegen der Load-Balancer IP-Adresse der Azure-Portal verwendet. 

9. Deaktivieren von NetBIOS für diese Adresse, und klicken Sie auf **OK**. Wiederholen Sie diesen Schritt für jede IP-Ressource die Projektmappe mehrere Azure-VNets umfasst. 

10. Machen Sie SQL Server Availability Group-Ressource die IP-Adresse abhängig. Rechten Maustaste auf die Ressource im Clustermanager ist dies auf der Registerkarte **Ressourcen** unter **Andere Ressourcen**. 

11. Öffnen Sie auf dem Clusterknoten, der derzeit primäre Replikat hostet eine erhöhte PowerShell ISE, und fügen Sie die folgenden Befehle in ein neues Skript. Klicken Sie auf der Registerkarte **Abhängigkeiten** des Listeners.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Aktualisieren der Variablen und das PowerShell-Skript Konfigurieren von IP-Adresse und Port für den neuen Listener.

 >[AZURE.NOTE] Wenn SQL Server in separaten Bereichen befinden, müssen Sie PowerShell-Skript zweimal ausführen. Verwenden Sie beim ersten der Cluster-Netzwerkname Cluster IP-Ressourcenname und Laden Sie Lastenausgleich IP-Adresse aus der ersten Ressourcengruppe. Verwenden Sie das zweite Mal Clusternetzwerknamen Cluster IP-Ressourcenname und Laden Sie Balancer IP-Adresse aus der zweiten Ressourcengruppe.

Der Cluster verfügt jetzt über eine Ressource Listener Verfügbarkeit.

## <a name="2-bring-the-listener-online"></a>2. bringen Sie den Listener online

Der Availability Group Listener Ressource konfigurierten bringen Listener online Sie, dass Datenbanken in der Availability Group mit den Listener Anwendung herstellen können.

1. Wechseln Sie wieder zum Failovercluster-Manager. Erweitern Sie **Rollen** und markieren Sie Ihre Verfügbarkeit Gruppe. Auf der Registerkarte **Ressourcen** Listener Maustaste, und klicken Sie auf **Eigenschaften**.

1. Klicken Sie auf der Registerkarte **Dependencies** . Wenn mehrere Ressourcen aufgeführt sind, überprüfen Sie, ob die IP-Adressen oder nicht und Dependencies. Klicken Sie auf **OK**.

1. Listener Maustaste, und klicken Sie auf **Online schalten**.

1. Wenn der Listener aus **den Ressourcen** online ist, die Availability Group und **Eigenschaften**.

1. Erstellen einer Abhängigkeit Listener Name Ressource (nicht der IP-Adresse Ressourcenname). Klicken Sie auf **OK**.

1. Starten Sie SQL Server Management Studio und primäres Replikat an.

1. Navigieren Sie zu **AlwaysOn hohe Verfügbarkeit** | **Verfügbarkeitsgruppen** | **Availability Group-Listener**. 

1. Sie sollten den Listenernamen sehen, den Sie im Failovercluster-Manager erstellt. Listener Maustaste, und klicken Sie auf **Eigenschaften**.

1. Geben Sie im **Anschluss** die Anschlussnummer für den verfügbarkeitsgruppenlistener mit früheren verwendet $EndpointPort (1433 war der Standardwert), klicken Sie auf **OK**.

Sie haben jetzt eine SQL Server-verfügbarkeitsgruppe in Azure virtuellen Maschinen im Ressourcen-Manager ausgeführt. 

## <a name="test-the-connection-to-the-listener"></a>Testen Sie die Verbindung zum listener

Die Verbindung zu testen:

1. RDP auf einem SQL Server, die im gleichen virtuellen Netzwerk ist, aber nicht das Replikat. Dies ist der andere SQL Server-Cluster.

2. **Sqlcmd** -Dienstprogramm zum Testen der Verbindung. Das folgende Skript wird z. B. eine **Sqlcmd** Verbindung mit primäres Replikat über mit Windows-Authentifizierung:

    ```
    sqlmd -S <listenerName> -E
    ```

    Wenn der Listener nicht die Standardeinstellung verwenden (1433) port, Port in der Verbindungszeichenfolge angeben. Ein Listener am Port 1435 beispielsweise Verbindung folgenden Sqlcmd-Befehl: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

SQLCMD-Verbindung verbindet automatisch, welche SQL Server-Instanz primäre Replikat hostet. 

>[AZURE.NOTE] Sicherstellen Sie, dass der angegebene Anschluss auf beiden SQL Server-Firewall geöffnet ist. Beide Server müssen eine eingehende Regel für den TCP-Port, den Sie verwenden. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten von Firewallregeln](http://technet.microsoft.com/library/cc753558.aspx) . 

## <a name="guidelines-and-limitations"></a>Richtlinien

Beachten Sie Folgendes verfügbarkeitsgruppenlistener in Azure mit internen System zum Lastenausgleich geladen:

- Mit einer internen Lastenausgleich können Sie nur den Listener im gleichen virtuellen Netzwerk zugreifen.

## <a name="for-more-information"></a>Weitere Informationen

Weitere Informationen finden Sie unter [Konfigurieren immer Verfügbarkeit Gruppieren in Azure VM manuell](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>PowerShell-cmdlets

Verwenden Sie folgende PowerShell-Cmdlets internen Lastenausgleich für Azure virtuelle Computer erstellen.

- `New-AzureRmLoadBalancer`erstellt ein Lastenausgleich. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) . 

- `New-AzureRMLoadBalancerFrontendIpConfig`erstellt eine Front-End-IP-Konfiguration für einen Lastenausgleich. Weitere Informationen finden Sie unter [New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) .

- `New-AzureRmLoadBalancerRuleConfig`erstellt eine Konfiguration für einen Lastenausgleich. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) . 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`erstellt eine Back-End-Pool Adresskonfiguration für einen Lastenausgleich. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) . 

- `New-AzureRmLoadBalancerProbeConfig`erstellt eine Probe-Konfiguration für einen Lastenausgleich. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) .

Benötigen Sie ein Lastenausgleich von Azure Ressourcengruppe entfernen, `Remove-AzureRmLoadBalancer`. Weitere Informationen finden Sie unter [AzureRmLoadBalancer entfernen](http://msdn.microsoft.com/library/mt603862.aspx).
