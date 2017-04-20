<properties
   pageTitle="Hinzufügen oder Entfernen von Knoten zu einem Cluster eigenständige Service Fabric | Microsoft Azure"
   description="Informationen Sie zum Hinzufügen oder Entfernen von Knoten zu einem Cluster Azure Service Fabric auf einer physikalischen oder virtuellen Computer mit Windows Server, die lokale oder eine Wolke."
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
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Hinzufügen oder Entfernen von Knoten zu einem eigenständigen Service Fabric-Cluster unter Windows Server

Nachdem Sie die [eigenständige Service Fabric Cluster auf Windows Server-Computern erstellt](service-fabric-cluster-creation-for-windows-server.md)haben, können Ihr Unternehmen braucht möglicherweise hinzufügen oder mehrere Knoten zum Cluster entfernen ändern. Dieser Artikel enthält detaillierte Schritte aus, um dieses Ziel zu erreichen.


## <a name="add-nodes-to-your-cluster"></a>Hinzufügen von Knoten zum cluster

1. Vorbereiten der VM/Computer im Abschnitt [Vorbereiten der Computer die erforderlichen Komponenten für die Bereitstellung zu](service-fabric-cluster-creation-for-windows-server.md#preparemachines) genannten Schritte zum Cluster hinzufügen möchten.
2. Planen der Fehlertoleranz und Upgrade Domäne diese VM/Computer hinzufügen soll.
3. Remotedesktop (RDP) in VM/Rechner zum Cluster hinzufügen möchten.
4. Kopieren oder auf diese VM-Maschine [Eigenständiges Paket für Fabric Service für Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) und das Paket entpacken.
5. Führen Sie Powershell als Administrator aus, und navigieren Sie zum Speicherort der entpackten Paket.
6. Führen Sie *AddNode.ps1* Powershell Parameter beschreiben den neuen Knoten hinzufügen. Im folgenden Beispiel wird einen neuen Knoten namens VM5, mit NodeType0, IP-Adresse 182.17.34.52 in UD1 und FD1 hinzugefügt. *ExistingClusterConnectionEndPoint* ist ein Verbindungsendpunkt für einen Knoten bereits im bestehenden Cluster. Für diesen Endpunkt können Sie die IP-Adresse eines *Knotens* im Cluster.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Knoten vom Cluster entfernen

1. Je nach Zuverlässigkeit für den Cluster ausgewählt können Sie die ersten n (3/5/7/9) Knoten vom Typ Primärknoten nicht entfernen
2. In einem Cluster Dev RemoveNode Befehl ausführen wird nicht unterstützt.
2. Remotedesktop (RDP) VM/Computer aus dem Cluster entfernen möchten.
2. Kopieren oder das [eigenständige Paket für Fabric Service für Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) und Entpacken Sie das Paket an diese VM-Maschine.
3. Führen Sie Powershell als Administrator aus, und navigieren Sie zum Speicherort der entpackten Paket.
4. *RemoveNode.ps1* in PowerShell ausführen. Im folgenden Beispiel löscht den aktuellen Knoten aus dem Cluster. *ExistingClientConnectionEndpoint* ist ein Clientendpunkt Verbindung für jeden Knoten im Cluster bleibt. Wählen Sie die IP-Adresse und Endpunktport von *allen* **anderen Knoten** im Cluster. Diesem **anderen Knoten** wird wiederum die Clusterkonfiguration für den entfernten Knoten aktualisiert. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Beachten Sie, dass auch nach dem Entfernen eines Knotens, sie zeigen kann, Abfragen und SFX. Dies ist ein bekannter Fehler und wird in einer zukünftigen Version behoben. 


## <a name="next-steps"></a>Nächste Schritte
- [Konfigurationen für eigenständige Windows-cluster](service-fabric-cluster-manifest.md)
- [Sichern Sie eigenständige Cluster mit Windows-Sicherheit unter Windows](service-fabric-windows-cluster-windows-security.md)
- [Sichern ein Clusters eigenständige Windows X509 mit Zertifikaten](service-fabric-windows-cluster-x509-security.md)
- [Erstellen Sie einen eigenständige Service Fabric-Cluster unter Windows Azure-VMs](service-fabric-cluster-creation-with-windows-azure-vms.md)
