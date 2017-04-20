<properties
   pageTitle="Einen eigenständige Service Fabric-Cluster unter Windows Server aktualisieren | Microsoft Azure"
   description="Upgrade Service Fabric-Code und Konfiguration, die eine eigenständige Service Fabric-Cluster einschließlich Update Clustermodus ausgeführt wird"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Aktualisieren der eigenständigen Service Fabric-Cluster unter Windows Server

> [AZURE.SELECTOR]
- [Azure-Cluster](service-fabric-cluster-upgrade.md)
- [Eigenständige Cluster](service-fabric-cluster-upgrade-windows-server.md)

Entwerfen für Erweiterbarkeit ist für alle modernen System Schlüssel zur langfristigen Erfolg Ihres Produkts. Ein Cluster Service Fabric ist eine Ressource, die Sie besitzen. Dieser Artikel beschreibt, wie Sie sicherstellen können, dass der Cluster immer unterstützten Versionen von Fabric Service Code und Konfigurationen ausgeführt wird.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Steuerung der Fabric-Version, die im Cluster ausgeführt wird

Sie können Updates Fabric Service downloaden, wenn Microsoft neue Versionen oder wählen Sie eine unterstützte Fabric möchten Cluster zu Cluster festlegen. 

Hierzu die Clusterkonfiguration "FabricClusterAutoupgradeEnabled" auf true oder false festlegen.


>[AZURE.NOTE] Vergewissern Sie sich immer eine Version unterstützten Service Fabric Cluster zu. Wenn wir der Veröffentlichung einer neuen Version von Service Fabric ankündigen, wird die vorherige Version zum Ende mindestens 60 Tage ab dem markiert. Neuen Versionen werden angekündigten [Service Fabric-Teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Die neue Version steht wählen. 


Sie können Cluster auf die neue Version aktualisieren, nur bei Verwendung eine Knotenkonfiguration Produktions-Stil, wo jeder Service Fabric-Knoten auf einem separaten physischen oder virtuellen Computer reserviert. Haben Sie einen Cluster Entwicklung gibt mehrere Service Fabric-Knoten auf einem einzigen physikalischen oder virtuellen Computer, müssen Cluster abreißen und mit der neuen Version neu erstellen.


Es gibt zwei verschiedene Workflows für Cluster auf die neuesten oder unterstützten Service Fabric-Version aktualisieren. Für Cluster mit Konnektivität automatisch die neueste Version herunterladen und die zweite für Cluster, die keine Verbindung mit der neuesten Service Fabric herunterladen.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Aktualisieren von Clustern mit Downloads der neuesten Code und Konfiguration 

Gehen Sie folgendermaßen vor Cluster auf eine unterstützte Version aktualisieren, wenn die Clusterknoten die Internetkonnektivität zu [http://download.microsoft.com](http://download.microsoft.com) 

Für Cluster mit Verbindung zu [http://download.microsoft.com](http://download.microsoft.com), überprüfen wir regelmäßig die Verfügbarkeit neuer Service Fabric-Versionen.


Wenn eine neue Version von Service Fabric verfügbar ist, wird das Paket lokal auf dem Cluster heruntergeladen und Aktualisierung bereitgestellt. Darüber hinaus wird um informiert den Kunden über diese neue Version ein Warnhinweis explizite Cluster ähnlich der folgenden:

"Die aktuelle Cluster Version [Version #] Unterstützung endet am [Datum].", nachdem der Cluster die neueste Version ausgeführt wird, geht die Warnung entfernt.


#### <a name="cluster-upgrade-workflow"></a>Aktualisierungsworkflow des Clusters.
 
Nachdem die Cluster Health-Warnung angezeigt wird, müssen Sie Folgendes:

1. Verbinden Sie mit Cluster von einem Computer mit Administratorzugriff auf alle Computer, die als Knoten im Cluster aufgeführt sind. Computer, den dieses Skript auf keinen Teil des Clusters

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Ruft die Liste der Service Fabric-Versionen, denen Sie aktualisieren können

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Sie sollten eine Ausgabe ähnlich erhalten:

    ![Fabric-Versionen abrufen][getfabversions]

3. Starten einer Aktualisierung Cluster für eine Version, die mit [ServiceFabricClusterUpgrade Start PowerShell cmd](https://msdn.microsoft.com/library/mt125872.aspx)

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Sie können den Fortschritt der Aktualisierung auf Service Fabric-Explorer oder Power Shellbefehl überwachen.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nachdem Sie die Probleme, die Rollback geführt haben behoben, müssen Sie das Upgrade erneut anhand derselben Schritte einleiten.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Aktualisieren von Clustern mit <U>keine Verbindung</u> zum Downloaden der neuesten Code und Konfiguration

Anhand dieser Schritte Cluster auf eine unterstützte Version aktualisieren, wenn der Cluster Knoten **haben keine** Verbindung mit [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Wenn Sie einen Cluster, der nicht Internet verbunden ist ausführen, müssen Sie den Service Fabric-Teamblog einer neuen Version benachrichtigt überwachen. System **nicht** setzen Cluster Health Warnung Sie davon benachrichtigt.  

1. Ändern Sie die Clusterkonfiguration, die folgende Eigenschaft auf False festgelegt.

        "fabricClusterAutoupgradeEnabled": false,

und Konfiguration Upgrade. Einzelheiten finden Sie in [ServiceFabricClusterUpgrade Start PS Cmd](https://msdn.microsoft.com/library/mt125872.aspx) . Cluster manifest Version ist die Version, die Sie in der clusterConfig.JSON. Vergewissern Sie sich vor dem Projektstart Konfiguration aktualisieren aktualisieren.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Aktualisierungsworkflow des Clusters.
 


1. Die neueste Version des Pakets [Erstellen Service Fabric-Cluster für WindowsServer](service-fabric-cluster-creation-for-windows-server.md) -Dokument herunterladen 


1. Verbinden Sie mit Cluster von einem Computer mit Administratorzugriff auf alle Computer, die als Knoten im Cluster aufgeführt sind. Computer, den dieses Skript auf keinen Teil des Clusters 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Kopieren Sie das heruntergeladene Paket in Abbildspeicher Cluster.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Registrieren des kopierten Pakets 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Starten Sie eine Aktualisierung Cluster auf eine der Versionen, die verfügbar ist. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Sie können den Fortschritt der Aktualisierung auf Service Fabric-Explorer oder Power Shellbefehl überwachen.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nachdem Sie die Probleme, die Rollback geführt haben behoben, müssen Sie das Upgrade erneut anhand derselben Schritte einleiten.



## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zum [Service Fabric Fabric Clustereinstellungen](service-fabric-cluster-fabric-settings.md) anpassen
- Erfahren Sie, wie [Ihr Cluster und](service-fabric-cluster-scale-up-down.md)
- Erfahren Sie mehr über [Upgrades der Anwendung](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
