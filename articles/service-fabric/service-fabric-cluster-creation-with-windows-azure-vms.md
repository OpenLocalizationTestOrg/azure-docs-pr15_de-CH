<properties
   pageTitle="Erstellen Sie einen eigenständigen Cluster unter Windows Azure-VMs | Microsoft Azure"
   description="Informationen Sie zum Erstellen und Verwalten eines Clusters Azure Service Fabric Azure virtuelle Computer mit Windows Server."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Erstellen Sie einen Cluster mit drei Knoten eigenständige Service Fabric Azure virtuelle Computer unter Windows Server

In diesem Artikel wird das Erstellen eines Clusters auf Windows basierenden Azure virtuelle Maschinen (VMs) mit dem eigenständigen Service Fabric-Installer für Windows Server beschrieben. Dies ist ein Sonderfall eines [Erstellen und Verwalten von einem Cluster unter Windows Server](service-fabric-cluster-creation-for-windows-server.md) VMs sind [Azure VMs mit Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md), jedoch [keinen Azure Cloud-basierten Service Fabric-Cluster](service-fabric-cluster-creation-via-portal.md)erstellen. Der Unterschied ist, dass eigenständige Service Fabric-Cluster erstellt Schritte vollständig von Ihnen verwalteten während der Azure Cloud-Dienstfabric Cluster verwaltet und Service Fabric Ressourcenanbieter aktualisiert.


## <a name="steps-to-create-the-standalone-cluster"></a>Eigenständige Cluster erstellen

1. Azure-Portal anmelden und eine neue Windows Server 2012 R2 Datacenter VM in eine Ressourcengruppe erstellen. Lesen des Artikels [einen virtuellen Computer Windows Azure-Portal erstellen](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
2. Ein paar weitere Windows Server 2012 R2 Datacenter VMs auf derselben Ressourcengruppe hinzufügen Sicherstellen Sie, dass alle VMs gleichen Administrator-Benutzernamen und Kennwort erstellt. Nach der Erstellung alle drei virtuellen Computer im gleichen virtuellen Netzwerk sollte angezeigt werden.
3. Jedem virtuellen Computer verbinden und die Windows Firewall mithilfe der [Server-Manager, lokale Server-Dashboard](https://technet.microsoft.com/library/jj134147.aspx). Dadurch der Datenverkehr zwischen den Computern kommunizieren kann. Verbindung zu jedem Computer erhalten Sie die IP-Adresse, öffnen ein Eingabeaufforderungsfenster und geben `ipconfig`. Auch sehen Sie die IP-Adresse jedes Computers im Azure-Portal virtuellen Netzwerkressource für die Ressourcengruppe auswählen.
4. An eine der VMs und überprüfen Sie, ob Sie die zwei VMs anpingen können.
5. VMs und [eigenständige Service Fabric Paket für Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) in einem neuen Ordner auf dem Computer verbinden und das Paket zu extrahieren.
6. Öffnen Sie die Datei *ClusterConfig.Unsecure.MultiMachine.json* im Editor, und bearbeiten Sie jeder Knoten mit drei IP-Adressen der Computer. Ändern Sie der Clustername oben und speichern Sie die Datei.  Ein Teil des Manifests Cluster unter Beispiel.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. [PowerShell ISE Fenster](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise)öffnen. Navigieren Sie zum Ordner, extrahiert das Installationspaket heruntergeladen eigenständige und Cluster-Manifestdatei gespeichert. Führen Sie den folgenden PowerShell-Befehl.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Finden Sie unter PowerShell ausführen, jeder Computer verbinden und einen Cluster erstellen. Nach ungefähr einer Minute Sie überprüfen, ob der Cluster funktioniert z. B. mit Service Fabric-Explorer auf eine IP-Adresse des Computers anschließen `http://10.7.0.5:19080/Explorer/index.html`. Dies ist eine eigenständige Cluster mit Azure VMs, Sichern sie Sie bereitstellen [Zertifikate Azure VMs](service-fabric-windows-cluster-x509-security.md) oder richten Sie einen Computer als [Controller für Windows-Authentifizierung Windows Server Active Directory (AD)](service-fabric-windows-cluster-windows-security.md), wie Sie lokal.


## <a name="next-steps"></a>Nächste Schritte
- [Erstellen Sie eigenständige Service Fabric Cluster unter Windows Server oder Linux](service-fabric-deploy-anywhere.md)
- [Hinzufügen oder Entfernen von Knoten zu einem eigenständigen Service Fabric-cluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Konfigurationen für eigenständige Windows-cluster](service-fabric-cluster-manifest.md)
- [Sichern Sie eigenständige Cluster mit Windows-Sicherheit unter Windows](service-fabric-windows-cluster-windows-security.md)
- [Sichern ein Clusters eigenständige Windows X509 mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)
