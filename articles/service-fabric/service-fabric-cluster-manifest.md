<properties
   pageTitle="Eigenständige Cluster konfigurieren | Microsoft Azure"
   description="Dieser Artikel beschreibt die eigenständigen oder private Service Fabric-Cluster konfigurieren."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Konfigurationen für eigenständige Windows-cluster

Dieser Artikel beschreibt, wie einen eigenständige Service Fabric-Cluster mithilfe der Datei _**ClusterConfig.JSON**_ konfigurieren. Diese Datei können Sie Informationen wie Service Fabric-Knoten und IP-Adressen, verschiedene Typen von Knoten des Clusters die Sicherheitskonfigurationen sowie der Netzwerktopologie in Fault-Aktualisierung Domänen für eigenständige Cluster angeben.

Wenn Sie [eigenständige Service Fabric Paket](service-fabric-cluster-creation-for-windows-server.md#downloadpackage)Beispiele der Datei ClusterConfig.JSON werden heruntergeladen Computers arbeiten. Die Prüfmuster mit *DevCluster* im Namen helfen ein Clusters mit allen drei Knoten auf dem gleichen Computer wie logische Knoten Ihnen. Davon muss mindestens einen Knoten als primären Knoten markiert. Diese Cluster eignet sich für eine Entwicklung oder Tests und nicht als einen Produktionscluster unterstützt. Die Prüfmuster mit *MultiMachine* in ihrem Namen können Sie einen Produktionscluster Qualität mit jeder Knoten auf einem separaten Computer erstellen. Die Anzahl der primäre Knoten für diesen Cluster basiert auf der [Zuverlässigkeit](#reliability).

1. *ClusterConfig.Unsecure.DevCluster.JSON* und *ClusterConfig.Unsecure.MultiMachine.JSON* zeigen, wie einen unsicheren Test- oder Cluster erstellen. 
    
2. *ClusterConfig.Windows.DevCluster.JSON* und *ClusterConfig.Windows.MultiMachine.JSON* veranschaulichen Test- oder Cluster gesicherte [Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)zu erstellen.

3. *ClusterConfig.X509.DevCluster.JSON* und *ClusterConfig.X509.MultiMachine.JSON* zeigen, wie zu Test- oder Cluster gesicherte [X509 Zertifikaten basierende Sicherheit](service-fabric-windows-cluster-x509-security.md). 


Nun untersuchen wir die verschiedenen Abschnitten der Datei _**ClusterConfig.JSON**_ als unten.

## <a name="general-cluster-configurations"></a>Allgemeine Clusterkonfigurationen
Dies umfasst die Breite bestimmten Clusterkonfigurationen, wie im folgenden Codeausschnitt JSON.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

Die Variable **Name** zuweisen können Cluster Service Fabric angezeigten Name zugewiesen werden. **ClusterConfigurationVersion** ist die Versionsnummer des Clusters. Erhöhen Sie die es jedem Cluster Service Fabric aktualisieren. Sie sollten jedoch **Anforderungsparameter** den Standardwert belassen.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>Knoten im cluster
Die Knoten können im Cluster Service Fabric mithilfe des Abschnitts **Knoten** wie im folgenden Codeausschnitt dargestellt.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Service Fabric-Cluster muss mindestens 3 Knoten enthalten. Sie können in diesem Abschnitt nach Ihrem Setup weitere Knoten hinzufügen. Die folgende Tabelle erläutert die Konfigurationsdateien für jeden Knoten.

|**Knotenkonfiguration**|**Beschreibung**|
|-----------------------|--------------------------|
|Knotenname|Angezeigten Name erhalten auf den Knoten.|
|IP-Adresse|Ermitteln der IP-Adresse der Knoten, öffnen ein Eingabeaufforderungsfenster und geben `ipconfig`. Beachten Sie die IPv4-Adresse und der **IP-Adresse** Variablen zuweisen.|
|nodeTypeRef|Jeder Knoten kann einen anderen Knotentyp zugewiesen werden. Die [Knotentypen](#nodetypes) werden unten definiert.|
|faultDomain|Fehlerdomänen aktivieren Clusteradministratoren physikalischen Knoten definieren, die gleichzeitig aufgrund von freigegebenen physischen Abhängigkeiten fehlschlagen.|
|upgradeDomain|Upgrade Domänen beschreiben Knotengruppen Service Fabric Upgrades zur selben Zeit heruntergefahren werden. Welche Knoten die Aktualisierung Domänen zuweisen können als sie nicht durch alle Anforderungen begrenzt werden.| 


## <a name="cluster-properties"></a>Clustereigenschaften

Im Abschnitt **Eigenschaften** in der ClusterConfig.JSON wird die Clusterkonfiguration wie folgt verwendet.

<a id="reliability"></a>
### <a name="reliability"></a>Zuverlässigkeit 
Abschnitt **ReliabilityLevel** definiert die Anzahl der Kopien der Systemdienste, die auf dem primären Knoten des Clusters ausgeführt werden können. Dies erhöht die Zuverlässigkeit dieser Dienste und der Cluster. Sie können diese Variable *Bronze*, *Silber*, *Gold* oder *Platin* für 3, 5, 7 oder 9 Kopien dieser Dienste festgelegt. Siehe Beispiel unten.

    "reliabilityLevel": "Bronze",
    
Beachten Sie, dass seit der primärer Knoten ein Exemplar der Systemdienste ausgeführt wird, mindestens 3 primären Knoten für *Bronze*, *Silber*7 für *Gold* und *Platin* -Zuverlässigkeit 9 5 möchten.


### <a name="diagnostics"></a>Diagnose
Abschnitt **DiagnosticsStore** können Sie zum Aktivieren von Diagnosen und Problembehandlung bei Fehlern Knoten oder Cluster, wie im folgenden Codeausschnitt gezeigt konfigurieren. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**Metadaten** ist eine Cluster-Diagnose und kann nach der Installation festgelegt werden. Diese Variablen können ETW-Ablaufverfolgungsprotokolle Speicherabbilder sowie Leistungsindikatoren sammeln. Lesen Sie weitere ETW-Ablaufverfolgungsprotokolle [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) und [ETW-Tracing](https://msdn.microsoft.com/library/ms751538.aspx) . Alle Protokolle auch [Absturzabbilder](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) [Leistungsindikatoren](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) können in **ConnectionString** -Ordner auf Ihrem Computer geleitet werden. *AzureStorage* können zum Speichern von Diagnose. Nachfolgend finden Sie ein Beispiel-Ausschnitt.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Sicherheit 
**Abschnitt** ist für eine sichere eigenständige Service Fabric-Cluster erforderlich. Die folgende Ausschnitt zeigt einen Teil dieses Abschnitts.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

**Metadaten** ist eine sichere Cluster und kann nach der Installation festgelegt werden. **ClusterCredentialType** und **ServerCredentialType** bestimmen die Sicherheit, die Cluster und Knoten implementiert. Sie können auf Zertifikaten basierende Sicherheit entweder *X509* oder *Windows* für eine Azure Active Directory-Sicherheit festlegen. Die restlichen Bereich **Sicherheit** basiert auf den Typ der Sicherheit. Informationen zum Ausfüllen der restlichen **Abschnitt** finden Sie [Zertifikaten basierende Sicherheit in einem Standalone-Cluster](service-fabric-windows-cluster-x509-security.md) oder [Windows-Sicherheit in eine eigenständige Cluster](service-fabric-windows-cluster-windows-security.md) .


<a id="nodetypes"></a>
### <a name="node-types"></a>Knotentypen
Abschnitt **NodeTypes** beschreibt den Knoten des Clusters ist. Mindestens ein Knoten muss für einen Cluster angegeben werden, wie im folgenden Codeausschnitt gezeigt. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

Der **Name** ist der angezeigte Name für diesen bestimmten Knoten. [Einen Knoten dieses Knotentyps erstellen, weisen Sie den Anzeigenamen der Variablen **NodeTypeRef** für diesen Knoten als erwähnt.](#clusternodes) Definieren Sie für jeden Knotentyp Verbindungsendpunkte, die verwendet wird. Sie können eine beliebige Portnummer für diese Verbindungsendpunkte, solange sie nicht mit anderen Endpunkten im Cluster in Konflikt stehen. In einem Cluster mit mehreren Knoten werden ein oder mehrere primäre Knoten (d. h. **IsPrimary** auf *true*festgelegt), je nach [**ReliabilityLevel**](#reliability). Finden Sie [Service Fabric-Cluster Kapazität Hinweise zur Planung](service-fabric-cluster-capacity.md) Informationen für **NodeTypes** und **ReliabilityLevel** und wissen, welche primären und nicht primäre Knotentypen. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Endpunkte konfiguriert die Knotentypen

- *ClientConnectionEndpointPort* wird vom Client zum Cluster herstellen, wenn des Clients APIs verwendet. 
- *ClusterConnectionEndpointPort* ist der Port, an dem Knoten miteinander kommunizieren.
- *LeaseDriverEndpointPort* wird zur Treiber Lease Cluster Knoten aktiv sind. 
- *ServiceConnectionEndpointPort* ist der Port, Programme und Dienste auf einem Knoten bereitgestellt zum Kommunizieren mit dem Service Fabric-Client auf einem bestimmten Knoten.
- *HttpGatewayEndpointPort* wird zur Explorer Fabric-Dienst herstellen.
- *EphemeralPorts* sind die [dynamischen Ports, die vom Betriebssystem verwendet](https://support.microsoft.com/kb/929851). Service Fabric verwenden diese Anwendungsports Teil und die übrigen werden für das Betriebssystem. Es wird auch dieses Bereichs an den vorhandenen Bereich des Betriebssystems zuordnen, damit für alle Zwecke in JSON-Beispieldateien angegebenen Bereiche verwenden können. Sie müssen sicherstellen, dass der Unterschied zwischen den Start- und End-Ports mindestens 255 ist. 
- *ApplicationPorts* sind die Ports, die von der Fabric Service verwendet werden. Dies sollte ein Teil der *EphemeralPorts*, dass der Endpunkt Anwendung decken. Fabric-Dienst verwendet diese immer neue Ports erforderlich sind sowie übernehmen die Firewall für diese Ports öffnen. 
- *ReverseProxyEndpointPort* ist eine optionale reverse Proxyendpunkt. Weitere Informationen finden Sie unter [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) . 


### <a name="other-settings"></a>Weitere
Abschnitt **FabricSettings** können Sie die Stammverzeichnisse für Service Fabric Daten und Protokolle festlegen. Sie können nur während der ersten Clustererstellung. Nachfolgend finden Sie ein Beispiel-Ausschnitt dieses Abschnitts.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Es wird empfohlen, mit einem BS-Laufwerk als FabricDataRoot und FabricLogRoot bietet mehr Sicherheit gegen Betriebssystem stürzt ab. Beachten Sie, dass wenn Sie nur den Stamm Daten anpassen, dann Protokoll Stamm eine Ebene unterhalb des Stammverzeichnisses Daten platziert wird.


## <a name="next-steps"></a>Nächste Schritte

Haben Sie eine vollständige ClusterConfig.JSON-Datei nach der eigenständigen Cluster Setup konfiguriert, Sie Cluster bereitstellen, gemäß Artikel [lokalen einen Azure Service Fabric-Cluster erstellen oder in der Cloud](service-fabric-cluster-creation-for-windows-server.md) und [Visualisierung Cluster mit Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md).


