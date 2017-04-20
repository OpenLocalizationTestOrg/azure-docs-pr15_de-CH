<properties
   pageTitle="Sichern ein Clusters unter Windows mit Windows-Sicherheit | Microsoft Azure"
   description="Informationen Sie zum Knoten- und Client-zu-Knoten-Sicherheit auf einem Standalone-Cluster unter Windows mit Windows-Sicherheit zu konfigurieren."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Sichern Sie eigenständige Cluster mit Windows-Sicherheit unter Windows

Um einen Service Fabric-Cluster vor nicht autorisierten Zugriffen müssen Sie, Sichern besonders wenn Produktionsarbeitslasten ausgeführt hat. Dieser Artikel beschreibt das Konfigurieren von Knoten zu Knoten und Client-zu-Knoten-Sicherheit mit Windows-Sicherheit in der *ClusterConfig.JSON* -Datei und Schritt Sicherheit konfigurieren, [eigenständige Cluster erstellen](service-fabric-cluster-creation-for-windows-server.md)unter Windows entspricht. Informationen wie Service Fabric Windows Security verwendet finden Sie unter [Cluster Sicherheitsszenarios](service-fabric-cluster-security.md).

>[AZURE.NOTE]
Sie sollten Ihre Sicherheit Auswahl-Knoten Sicherheit, sorgfältig gibt keine Cluster-Aktualisierung eine Auswahl in einen anderen. Sicherheit-Auswahl ändern erfordert eine vollständige Cluster neu erstellen.

## <a name="configure-windows-security"></a>Konfigurieren der Windows-Sicherheit
[Microsoft.Azure.ServiceFabric.WindowsServer *ClusterConfig.Windows.JSON* -Beispielkonfigurationsdatei heruntergeladen.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) eigenständige Cluster-Paket enthält eine Vorlage für Windows-Sicherheit konfigurieren.  Windows-Sicherheit wird im Abschnitt **Eigenschaften** konfiguriert:

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Einstellung**|**Beschreibung**|
|-----------------------|--------------------------|
|ClusterCredentialType|Windows-Sicherheit wird durch den **ClusterCredentialType** -Parameter auf *Windows*aktiviert.|
|ServerCredentialType|Windows-Sicherheit für Clients mithilfe der **ServerCredentialType** -Parameter auf *Windows*. Dies bedeutet, dass die Clients des Clusters und der Cluster in einer Active Directory-Domäne ausgeführt werden.|
|WindowsIdentities|Die Cluster und den Clientcomputern Identitäten enthält.|
|ClusterIdentity|Knoten-Sicherheit konfiguriert. Eine durch Kommas getrennte Liste der Gruppe verwaltete Dienstkonten oder Computernamen.|
|ClientIdentities|Client-zu-Knoten-Sicherheit konfiguriert. Ein Array von Client-Benutzerkonten.|
|Identität|Die Clientidentität Domänenbenutzer.|
|IsAdmin|True gibt an, dass der Domänenbenutzer Client Administratorzugriff für Client Benutzerzugriff.|

[Knoten Knoten Sicherheit](service-fabric-cluster-security.md#node-to-node-security) wird durch eine Einstellung mit **ClusterIdentity**konfiguriert. Um Vertrauensstellungen zwischen Knoten zu erstellen, müssen sie voneinander hingewiesen werden. Dies kann auf zwei Arten: Geben die Gruppe verwaltetes Dienstkonto, die alle Knoten im Cluster umfasst oder Domäne Knoten Identitäten aller Knoten im Cluster. Wir empfehlen Ansatz [Managed Service Gruppenkonto (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) besonders für größere Cluster (mehr als 10 Knoten) oder Cluster, die vergrößert oder verkleinert.
Dadurch kann Knoten hinzugefügt oder daraus gMSA, sich dem Cluster Manifest. Dieser Ansatz erfordert nicht das Erstellen einer Domänengruppe für die Clusterverwaltung erteilten Zugriffsrechte hinzufügen und Entfernen von Mitgliedern. Weitere Informationen finden Sie in der [Gruppe verwaltete Dienstkonten Einstieg](http://technet.microsoft.com/library/jj128431.aspx).

[Client Security Knoten](service-fabric-cluster-security.md#client-to-node-security) ist mit **ClientIdentities**konfiguriert. Um die Vertrauensstellung zwischen einem Client und dem Cluster einrichten, müssen Sie den Cluster Client Identitäten kennen, die sie vertrauen kann konfigurieren. Dies kann auf zwei Arten: Geben Sie der Gruppe Domänenbenutzer, die eine Verbindung herstellen oder Domäne Knoten Benutzer angeben, die eine Verbindung herstellen können. Fabric-Dienst unterstützt zwei verschiedenen Steuerelementtypen für Clients, die mit einem Service Fabric-Cluster verbunden sind: Administrator und Benutzer. Zugriffskontrolle bietet die Möglichkeit der Cluster-Administrator den Zugriff auf bestimmte Clustervorgänge für verschiedene Benutzergruppen, Sicherheit von Cluster beschränken.  Administratoren haben vollen Zugriff auf Management-Funktionen (einschließlich Schreib-/Lesezugriff). Benutzer haben standardmäßig nur Lesezugriff auf Management-Funktionen (z. B. Abfragefunktionen) und auch aufgelöst.

Im folgenden Beispielabschnitt **Sicherheit** konfiguriert Windows-Sicherheit und gibt an, ob die Computer im *ServiceFabric/clusterA.contoso.com* Teil des Clusters werden *CONTOSO\usera* Admin Client zugreifen:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren der Windows-Sicherheit in der Datei *ClusterConfig.JSON* , Fortsetzen der Clustererstellung in [eigenständigen Cluster erstellen, auf dem Windows](service-fabric-cluster-creation-for-windows-server.md).

Weitere Informationen zum Knoten-Sicherheit, Client-zu-Knoten-Sicherheit und rollenbasierte Zugriffskontrolle finden Sie unter [Cluster Sicherheitsszenarios](service-fabric-cluster-security.md).

Beispiele für die Verbindung mit PowerShell oder FabricClient finden Sie unter [Verbinden einer sicheren Cluster](service-fabric-connect-to-secure-cluster.md) .
