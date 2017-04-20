<properties
   pageTitle="Erstellen Sie Listener für AlwaysOn Verfügbarkeit Gruppe für SQL Server in Azure Virtual Machines"
   description="Eine schrittweise Anleitung zum Erstellen eines Listeners für eine AlwaysOn Verfügbarkeit Gruppe für SQL Server in Azure Virtual Machines"
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
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Konfigurieren Sie ein interner Lastenausgleich für eine AlwaysOn Availability Group in Azure

Dieses Thema erläutert die internen Lastenausgleich für eine SQL Server AlwaysOn Availability Group in Azure virtuelle Maschinen in Ressourcen-Manager-Modell erstellen. Eine AlwaysOn Availability Group erfordert ein Lastenausgleich auf Azure virtuellen Computern wird die SQL Server-Instanzen. Lastenausgleich die IP-Adresse für den verfügbarkeitsgruppenlistener gespeichert. Wenn eine verfügbarkeitsgruppe mehrere Regionen erstreckt, benötigt jede Region ein Lastenausgleich.

Um diese Aufgabe auszuführen, müssen Sie eine SQL Server AlwaysOn Availability Gruppe auf Azure VMs in Ressourcen-Manager-Modell bereitgestellt. Beide virtuelle SQL Server-Computer muss auf dieselben Verfügbarkeit gehören. [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können die AlwaysOn Availability Group in Azure Ressourcen-Manager automatisch erstellt. Diese Vorlage erstellt automatisch interne Lastenausgleich. 

Alternativ können Sie [eine AlwaysOn Availability Group manuell konfigurieren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

In diesem Thema muss die Verfügbarkeit Gruppen bereits konfiguriert sind.  

Verwandte Themen:

 - [Konfigurieren von AlwaysOn Availability Gruppen in Azure VM (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [Konfigurieren einer Verbindung VNet VNet mithilfe von Azure-Ressourcen-Manager und PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Schritte

Wird dieses Dokument durchlaufen erstellt und ein Lastenausgleich in Azure-Portal konfigurieren. Nachdem dies abgeschlossen ist, konfigurieren Sie den Cluster zum Lastenausgleich die IP-Adresse für den AlwaysOn Availability Group Listener verwenden.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Erstellen und Konfigurieren von Lastenausgleich in Azure-portal

In diesem Teil des Vorgangs führen Sie folgende Schritte in der Azure-Portal:

1. Erstellen Sie Lastenausgleich und konfigurieren Sie die IP-Adresse

1. Konfigurieren Sie den Back-End-pool

1. Den Prüfpunkt erstellen 

1. Festlegen Sie den Lastenausgleich Regeln

>[AZURE.NOTE] Wenn SQL Server in verschiedenen Ressourcengruppen und Regionen sind, werden Sie alle Schritte, einmal in jede Ressourcengruppe tun.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. erstellen Sie 1. Lastenausgleich und konfigurieren Sie die IP-Adresse

Der erste Schritt ist die Erstellung von Lastenausgleich. Öffnen Sie in Azure-Portal Ressourcengruppe, die SQL Server virtuelle Computer enthält. Klicken Sie in der Ressourcengruppe auf **Hinzufügen**.

- **Lastenausgleich**suchen. Wählen Sie aus den Suchergebnissen **Lastenausgleich**, die von **Microsoft**veröffentlicht.

- Klicken Sie auf das Blade **Lastenausgleich** **Erstellen**.

- **Erstellen des Systems zum Lastenausgleich**Konfigurieren der Lastenausgleich wie folgt:

| Einstellung | Wert |
| ----- | ----- |
| **Name** | Text ein Lastenausgleich darstellt. Beispielsweise **SqlLB**. |
| **Schema** | **Interne** |
| **Virtuelles Netzwerk** | Wählen Sie das virtuelle Netzwerk, dem der SQL Server.   |
| **Subnetz**  | Wählen Sie das Subnetz, dem den SQL Server. |
| **Abonnement** | Wenn Sie mehrere Abonnements verfügen, kann dieses Feld angezeigt. Wählen Sie den Dauerauftrag aus, dem dieser Ressource zugeordnet werden soll. Es ist normalerweise dieselbe Abonnement als Ressourcen für die Availability Group.  |
| **Ressourcengruppe** | Wählen Sie die Ressourcengruppe, der der SQL Server. | 
| **Speicherort** | Wählen Sie Azure, die in SQL Server sind. |

- Klicken Sie auf **Erstellen**. 

Azure erstellt Lastenausgleich über konfiguriert. Lastenausgleich gehört zu einem bestimmten Netzwerk, Subnetz Ressourcengruppe und Speicherort. Nach Abschluss der Azure überprüfen Sie Load Balancer Azure. 

Konfigurieren Sie Load Balancer IP-Adresse jetzt.  

- Klicken Sie auf Load Balancer **Einstellungen** Blade auf **IP-Adresse**. Blade **-IP-Adresse** zeigt, dass das private Lastenausgleich im gleichen virtuellen Netzwerk als SQL Server. 

- Einstellen: 

| Einstellung | Wert |
| ----- | ----- |
| **Subnetz** | Wählen Sie das Subnetz, dem den SQL Server. |
| **Zuordnung** | **Statische** |
| **IP-Adresse** | Geben Sie eine nicht verwendete virtuelle IP-Adresse aus dem Subnetz.  |

- Speichern.

Das Lastenausgleichsmodul verfügt jetzt über eine IP-Adresse. Notieren Sie diese IP-Adresse. Diese IP-Adresse verwendet beim Erstellen Sie einen Listener im Cluster verwendet werden. In einem Powershellskript in diesem Artikel verwenden diese Adresse für die `$ILBIP` Variable.



## <a name="2-configure-the-backend-pool"></a>2. konfigurieren Sie den Back-End-pool

Im nächste Schritt wird einen Back-End-Adresspool erstellen. Azure wird der Back-End-Pool- *Back-End-Pool*. In diesem Fall ist der Back-End-Pool die Adressen der beiden SQL Server in der verfügbarkeitsgruppe. 

- Klicken Sie in der Ressourcengruppe auf Lastenausgleich erstellten. 

- Klicken Sie auf **Einstellungen**auf **Back-End-Pools**.

- **Backend-Adresspools**klicken Sie auf **Hinzufügen** , um ein Back-End-Adresspool erstellen. 

- **Back-End-Pool hinzufügen** unter **Name**Namen Sie einen für die Back-End-Pool.

- Klicken Sie unter **virtuelle Computer** **+ einen virtuellen Computer hinzufügen**. 

- Unter **virtuelle Computer auswählen** auf **Wählen Sie eine Verfügbarkeit** und festlegen Sie der Verfügbarkeit, dass SQL Server virtuelle Computer angehören.

- Nachdem die verfügbarkeitsgruppe ausgewählt haben, klicken Sie auf **Wählen Sie die virtuellen Computer**. Klicken Sie auf die zwei virtuelle Computer, die SQL Server-Instanzen in der Availability Group hosten. Klicken Sie auf **auswählen**. 

- Klicken Sie auf **OK** , um Blades für **virtuelle Computer auswählen**und **Back-End-Pool hinzufügen**zu schließen. 

Azure aktualisiert die Einstellungen für die Back-End-Adresspool. Jetzt hat die Verfügbarkeit einen Pool von zwei SQL-Server.

## <a name="3-create-a-probe"></a>3. erstellen Sie einen Prüfpunkt

Im nächste Schritt wird einen Prüfpunkt erstellt. Der Prüfpunkt definiert wie Azure überprüft der SQL Server derzeit die verfügbarkeitsgruppenlistener besitzt. Azure Prüfpunkt den Dienst anhand der IP-Adresse auf einen Anschluss, den Sie beim Erstellen des Prüfpunkts definieren.

- Klicken Sie auf Load Balancer **Einstellungen** Blade **untersucht**. 

- Blade **-Prüfpunkte** klicken Sie auf **Hinzufügen**.

- Konfigurieren des Prüfpunkts auf die **Probe hinzufügen** . Verwenden Sie die folgenden Werte den Prüfpunkt konfigurieren:

| Einstellung | Wert |
| ----- | ----- |
| **Name** | Text ein, den Prüfpunkt darstellt. Beispielsweise **SQLAlwaysOnEndPointProbe**. |
| **Protokoll** | **TCP** |
| **Anschluss** | Sie können einen beliebigen verfügbaren Anschluss verwenden. Beispielsweise *59999*.    |
| **Intervall**  | *5* | 
| **Fehlerhafte Schwellenwert**  | *2* | 

- Klicken Sie auf **OK**. 

>[AZURE.NOTE] Sicherstellen Sie, dass der angegebene Anschluss auf beiden SQL Server-Firewall geöffnet ist. Beide Server müssen eine eingehende Regel für den TCP-Port, den Sie verwenden. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten von Firewallregeln](http://technet.microsoft.com/library/cc753558.aspx) . 

Azure wird den Prüfpunkt erstellt. Azure verwendet Prüfpunkts zum Testen der SQL Server-Listener für die Availability Group hat.

## <a name="4-set-the-load-balancing-rules"></a>4. festlegen Sie den Lastenausgleich Regeln

Festlegen Sie den Lastenausgleich Regeln. Load balancing Regeln konfigurieren wie Lastenausgleich Datenverkehr an SQL Server geroutet. Für dieses System zum Lastenausgleich können Sie direkte Server zurück, da nur eine der beiden SQL Server immer die Verfügbarkeit Listener Ressource besitzen wird.

- Klicken Sie auf Load Balancer **Einstellungen** Blade **Lastenausgleich Regeln geladen**. 

- Das Blade **Lastenausgleich Regeln** klicken Sie auf **Hinzufügen**.

- Verwenden Sie Blade **Hinzufügen laden Lastenausgleich Regeln** Lastenausgleich Regel konfigurieren. Verwenden Sie Folgendes: 

| Einstellung | Wert |
| ----- | ----- |
| **Name** | Text ein Lastenausgleich Regeln darstellt. Beispielsweise **SQLAlwaysOnEndPointListener**. |
| **Protokoll** | **TCP** |
| **Anschluss** | *1433*   |
| **Back-End-Port** | *1433*. Beachten Sie, dass dieser deaktiviert wird, da diese Regel **Floating IP (direct Server zurück)**verwendet.   |
| **Prüfpunkt** | Verwenden Sie den Namen des Prüfpunkts erstellen für dieses System zum Lastenausgleich. |
| **Sitzung Persistenz**  | **Keine** | 
| **Leerlaufzeitlimit (Minuten)**  | *4* | 
| **Umlaufende IP-(direct Server zurück)**  | **Aktiviert** | 

 >[AZURE.NOTE] Möglicherweise wird auf dem Blatt an alle Bildlauf.

- Klicken Sie auf **OK**. 

- Azure wird den Regel für Lastenausgleich konfiguriert. Jetzt sieht Lastenausgleich Datenverkehr zu SQL Server, die Listener für die Availability Group hostet. 

An diesem Punkt hat die Ressourcengruppe ein Lastenausgleich auf beiden SQL Server-Computern. Lastenausgleich enthält eine IP-Adresse auch für SQL Server AlwaysOn Verfügbarkeit Gruppe Listener, sodass beide Computer auf die Verfügbarkeitsgruppen reagieren kann.

>[AZURE.NOTE] Wenn SQL Server in zwei separaten Bereichen sind, wiederholen Sie die Schritte in der Region. Jede Region benötigt ein Lastenausgleich. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfigurieren Sie den Cluster Load Balancer IP-Adresse verwendet 

Im nächste Schritt ist konfigurieren Sie den Listener auf dem Cluster und den Listener online. Führen Sie dazu folgende Schritte aus: 

1. Erstellen Sie Listener Gruppe Verfügbarkeit auf dem Failovercluster 

1. Den Listener online schalten

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. erstellen Sie 1. Listener Gruppe Verfügbarkeit auf dem Failovercluster

In diesem Schritt erstellen Sie manuell die verfügbarkeitsgruppenlistener im Failovercluster-Manager und SQL Server Management Studio (SSMS).

- Mit der Azure virtuellen Computer herstellen, der primäre Replikat hostet RDP. 

- Failovercluster-Manager zu öffnen.

- Wählen Sie den Knoten **Netzwerke** und beachten Sie Cluster-Netzwerkname. Diese Bezeichnung wird der `$ClusterNetworkName` -Variable PowerShell-Skript.

- Erweitern Sie den Clusternamen und klicken Sie auf **Rollen**.

- Im Bereich **Rollen** die Verfügbarkeit Gruppe Namen, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt**.

- Erstellen Sie im Feld **Name** einen Namen für diesen neuen Listener klicken Sie zweimal auf **Weiter** , und klicken Sie dann auf **Fertig stellen**. Schalten Sie den Listener oder die Ressource online zu diesem Zeitpunkt.

 >[AZURE.NOTE] Der Name für den neuen Listener wird der Netzwerkname Applikationen Verbindung zu Datenbanken in der SQL Server-verfügbarkeitsgruppe verwenden.

- Klicken Sie auf die Registerkarte **Ressourcen** und anschließend Clientzugriffspunkt, die Sie gerade erstellt haben. Die IP-Adressressource und Eigenschaften. Beachten Sie die IP-Adresse ein. Verwenden Sie diesen Namen in der `$IPResourceName` -Variable PowerShell-Skript.

- Unter **IP-Adresse** auf **Statische IP-Adresse** , und legen Sie die statische IP-Adresse dieselbe Adresse, die Sie beim Festlegen der Load-Balancer IP-Adresse der Azure-Portal verwendet. Aktivieren Sie NetBIOS für diese Adresse, und klicken Sie auf **OK**. Wiederholen Sie diesen Schritt für jede IP-Ressource die Projektmappe mehrere Azure-VNets umfasst. 

- Öffnen Sie auf dem Clusterknoten, der derzeit primäre Replikat hostet eine erhöhte PowerShell ISE, und fügen Sie die folgenden Befehle in ein neues Skript.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Aktualisieren der Variablen und das PowerShell-Skript Konfigurieren von IP-Adresse und Port für den neuen Listener.

 >[AZURE.NOTE] Wenn SQL Server in separaten Bereichen befinden, müssen Sie PowerShell-Skript zweimal ausführen. Verwenden Sie beim ersten der Cluster-Netzwerkname Cluster IP-Ressourcenname und Laden Sie Lastenausgleich IP-Adresse aus der ersten Ressourcengruppe. Verwenden Sie das zweite Mal Clusternetzwerknamen Cluster IP-Ressourcenname und Laden Sie Balancer IP-Adresse aus der zweiten Ressourcengruppe.

Der Cluster verfügt jetzt über eine Ressource Listener Verfügbarkeit.

## <a name="2-bring-the-listener-online"></a>2. bringen Sie den Listener online

Der Availability Group Listener Ressource konfigurierten bringen Listener online Sie, dass Datenbanken in der Availability Group mit den Listener Anwendung herstellen können.

- Wechseln Sie wieder zum Failovercluster-Manager. Erweitern Sie **Rollen** und markieren Sie Ihre Verfügbarkeit Gruppe. Auf der Registerkarte **Ressourcen** Listener Maustaste, und klicken Sie auf **Eigenschaften**.

- Klicken Sie auf der Registerkarte **Dependencies** . Wenn mehrere Ressourcen aufgeführt sind, überprüfen Sie, ob die IP-Adressen oder nicht und Dependencies. Klicken Sie auf **OK**.

- Listener Maustaste, und klicken Sie auf **Online schalten**.


- Wenn der Listener aus **den Ressourcen** online ist, die Availability Group und **Eigenschaften**.

- Erstellen einer Abhängigkeit Listener Name Ressource (nicht der IP-Adresse Ressourcenname). Klicken Sie auf **OK**.


- Starten Sie SQL Server Management Studio und primäres Replikat an.


- Navigieren Sie zu **AlwaysOn hohe Verfügbarkeit** | **Verfügbarkeitsgruppen** | **Availability Group-Listener**. 


- Sie sollten den Listenernamen sehen, den Sie im Failovercluster-Manager erstellt. Listener Maustaste, und klicken Sie auf **Eigenschaften**.


- Geben Sie im **Anschluss** die Anschlussnummer für den verfügbarkeitsgruppenlistener mit früheren verwendet $EndpointPort (1433 war der Standardwert), klicken Sie auf **OK**.

Sie haben jetzt eine SQL Server AlwaysOn Availability Group in Azure virtuelle Maschinen in Ressourcen-Manager-Modus. 

## <a name="test-the-connection-to-the-listener"></a>Testen Sie die Verbindung zum listener

Die Verbindung zu testen:

1. RDP auf einem SQL Server, die im gleichen virtuellen Netzwerk ist, aber nicht das Replikat. Dies ist der andere SQL Server-Cluster.

1. **Sqlcmd** -Dienstprogramm zum Testen der Verbindung. Das folgende Skript wird z. B. eine **Sqlcmd** Verbindung mit primäres Replikat über mit Windows-Authentifizierung:

        sqlcmd -S <listenerName> -E

SQLCMD-Verbindung automatisch herstellen, welche SQL Server-Instanz primäre Replikat hostet. 

## <a name="guidelines-and-limitations"></a>Richtlinien

Beachten Sie die folgenden Richtlinien auf Verfügbarkeit Gruppe in Azure mit internen System zum Lastenausgleich geladen:

- Pro Cloud-Dienst wird nur einen internen Verfügbarkeit Gruppe Listener unterstützt, da der Listener für Lastenausgleich konfiguriert ist, und nur eine interne Lastenausgleich. Es ist jedoch möglich, mehrere externe Listener erstellen. 

- Mit einer internen Lastenausgleich können Sie nur den Listener im gleichen virtuellen Netzwerk zugreifen.
 
