<properties
   pageTitle="Prüfbarkeit Aktion | Microsoft Azure"
   description="Dieser Artikel spricht über die Prüfbarkeit Aktionen in Microsoft Azure Service Fabric gefunden."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Prüfbarkeit Aktionen
Simulieren eine unzuverlässige Infrastruktur bietet Azure Service Fabric Entwickler mit verschiedenen realen Ausfällen und Zustand. Diese werden als Prüfbarkeit Aktionen verfügbar gemacht. Die Aktionen sind systemnahe APIs, die eine bestimmte Fehlerinjektion Statusübergang oder Validierung verursacht. Durch Kombinieren dieser Aktionen können Sie umfassende Testszenarios für Ihre Dienste schreiben.

Service Fabric enthält dazu einige allgemeine Testszenarien aus. Wir empfehlen, dass Sie diese integrierten Szenarien nutzen die häufige Statusübergänge und Fehlern testen sorgfältig ausgewählt werden. Aktionen können jedoch verwendet werden, benutzerdefinierte Szenarien beim hinzuzufügende Abdeckung für Szenarien, fallen nicht unter die integrierten Szenarien noch oder dem benutzerdefinierten speziell für Ihre Anwendung, erstellen.

C#-Implementierung von Aktionen sind in der Assembly System.Fabric.dll gefunden. Das System Fabric PowerShell-Modul befindet sich in der Microsoft.ServiceFabric.Powershell.dll-Assembly. Als Teil der Common Language Runtime-Installation wird ServiceFabric PowerShell-Modul installiert um Bedienung zu ermöglichen.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Ordnungsgemäßes oder fehlerhaftes Fehler Aktionen
Prüfbarkeit Aktionen werden in zwei große Gruppen eingeteilt:

* Fehlerhaftes Fehler: Diese Fehler Fehler wie Computer-Neustart simulieren und Abstürze zu verarbeiten. In solchen Fällen Fehler beendet der Ausführungskontext Prozess abrupt. Dies bedeutet, dass keine Bereinigung des Staates ausgeführt werden kann, bevor die Anwendung erneut gestartet wird.

* Ordnungsgemäßes Fehler: Diese Fehler simulieren ordnungsgemäßes Aktionen wie Replikat verschoben und Tropfen durch Netzwerklastenausgleich ausgelöst. In solchen Fällen wird der Dienst erhält eine Benachrichtigung schließen und kann Bereinigen der Zustand vor dem Beenden.

Bessere Qualität Validierung führen Sie Service und geschäftliche Arbeitslast bei verschiedenen ordnungsgemäßes fehlerhaftes Fehler veranlassen aus. Fehlerhaftes Fehler Übung Szenarien, in denen des Prozesses in einigen Workflow abrupt beendet. Dies überprüft der Wiederherstellungspfad nach Service Fabric Service Replikat wiederhergestellt. Dadurch testen Datenkonsistenz und ob der Dienststatus nach Fehlern richtig verwaltet wird. Anderen Satz Fehler (der ordnungsgemäßes) testen, Replikate von Service Fabric bewegt der Dienst ordnungsgemäß reagiert. Diese tests Behandlung des Abbruchs in der RunAsync-Methode. Der Dienst muss für das Abbruchtoken festgelegt, ordnungsgemäß Zustand gespeichert wird und beendet die RunAsync-Methode.

## <a name="testability-actions-list"></a>Aktionsliste Prüfbarkeit

| Aktion | Beschreibung | Verwaltete API | PowerShell-Cmdlets | Anmutig/fehlerhaftes Fehler |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| Entfernt den Testzustand aus dem Cluster bei einem fehlerhaften Herunterfahren des Testtreiber. | CleanTestStateAsync | ServiceFabricTestState entfernen | Nicht zutreffend |
| InvokeDataLoss | Führt zu Datenverlust in der Servicepartition. | InvokeDataLossAsync | Rufen Sie ServiceFabricPartitionDataLoss | Ordnungsgemäßes |
| InvokeQuorumLoss | Versetzt eine bestimmten statusbehaftete Servicepartition in Quorum verloren gehen. | InvokeQuorumLossAsync | Rufen Sie ServiceFabricQuorumLoss | Ordnungsgemäßes |
| Primäre verschieben | Verschiebt angegebene primäre Replikat der statusbehaftete Dienst angegebene Cluster-Knoten. | MovePrimaryAsync | ServiceFabricPrimaryReplica verschieben | Ordnungsgemäßes |
| Sekundäre verschieben | Verschiebt das aktuelle sekundäre Replikat statusbehaftete Dienst auf einen anderen Clusterknoten. | MoveSecondaryAsync | ServiceFabricSecondaryReplica verschieben | Ordnungsgemäßes |
| RemoveReplica | Simuliert einen Replikat-Fehler von einem Replikat aus einem Cluster entfernen. Dies schließt das Replikat und wird diese Rolle 'None', dessen Zustand vom Cluster entfernt. | RemoveReplicaAsync | ServiceFabricReplica entfernen | Ordnungsgemäßes |
| RestartDeployedCodePackage | Simuliert ein Paketfehler Prozess Code neu ein Paket auf einem Knoten in einem Cluster bereitgestellt. Dies bricht den Code Paket Prozess alle Benutzer Service Replikate der gehosteten dabei neu gestartet wird. | RestartDeployedCodePackageAsync | Neu starten ServiceFabricDeployedCodePackage | Fehlerhaftes |
| RestartNode | Simuliert ein Clusterfehler Knoten Service Fabric Knoten neu. | RestartNodeAsync | Neu starten ServiceFabricNode | Fehlerhaftes |
| RestartPartition | Simuliert ein Szenario Datacenter Stromausfälle oder Cluster Blackout einige oder alle Replikate einer Partition neu. | RestartPartitionAsync | Neu starten ServiceFabricPartition | Ordnungsgemäßes |
| RestartReplica | Simuliert einen Replikat Fehler neu beibehaltenen Replikat in einem Cluster, das Replikat schließen und erneut öffnen. | RestartReplicaAsync | Neu starten ServiceFabricReplica | Ordnungsgemäßes |
| StartNode | Startet einen Knoten in einem Cluster, der bereits beendet ist. | StartNodeAsync | ServiceFabricNode starten | Nicht zutreffend |
| StopNode | Simuliert einen Knotenausfall beenden einen Knoten in einem Cluster. Der Knoten bleibt nach unten bis StartNode aufgerufen wird. | StopNodeAsync | ServiceFabricNode beenden | Fehlerhaftes |
| ValidateApplication | Überprüft die Verfügbarkeit und den Zustand aller Service Fabric-Dienste in einer Anwendung nach veranlassen einige Fehler im System. | ValidateApplicationAsync | Test-ServiceFabricApplication | Nicht zutreffend |
| ValidateService | Überprüft die Verfügbarkeit und den Zustand eines Dienstes Service Fabric nach veranlassen einige Fehler im System. | ValidateServiceAsync | Test-ServiceFabricService | Nicht zutreffend |

## <a name="running-a-testability-action-using-powershell"></a>Eine mit PowerShell Prüfbarkeit Aktion ausführen

Dieses Lernprogramm veranschaulicht die Prüfbarkeit Aktion mithilfe von PowerShell ausführen. Sie lernen Prüfbarkeit Aktion gegen einen lokalen Cluster (ein Feld) oder eine Azure-Cluster ausführen. Microsoft.Fabric.Powershell.dll - Modul Service Fabric PowerShell - wird automatisch installiert, wenn Microsoft Service Fabric MSI installiert. Das Modul wird automatisch geladen, wenn Sie PowerShell Prompt öffnen.

Tutorial Segmente:

- [Ein-Feld-Cluster eine Aktion ausführen](#run-an-action-against-a-one-box-cluster)
- [Führen Sie eine Aktion für einen Azure-cluster](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Ein-Feld-Cluster eine Aktion ausführen

Zum Ausführen einer Aktion Prüfbarkeit gegen einen lokalen Cluster zuerst zum Cluster herstellen und PowerShell Prompt im Administratormodus öffnen. Lassen Sie uns die **Neustart-ServiceFabricNode** Aktion.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Hier wird die Aktion **ServiceFabricNode Neustart** auf einem Knoten mit dem Namen "Node1" ausgeführt. Die Beendigungsmodus gibt an, dass nicht überprüfen soll, ob die Neustart-Knoten Aktion tatsächlich erfolgreich. Angeben der Beendigungsmodus "Überprüfen" führen sie überprüfen, ob die neustartaktion tatsächlich erfolgreich. Anstatt direkt den Knoten mit dem Namen, können Sie es über Partitionsschlüssel und Replikat wie folgt angeben:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**ServiceFabricNode Neustart** sollte verwendet werden, einen Service Fabric-Knoten in einem Cluster neu starten. Dies beendet den Prozess Fabric.exe, der alle System und Service Replikate auf einem Knoten gehostet neu gestartet wird. Mit dieser API den Dienst testen kann Fehler auf Failover Wiederherstellungspfaden aufdecken. Sie können Knoten im Cluster simulieren.

Der folgende Screenshot zeigt Befehls Prüfbarkeit **Neustart ServiceFabricNode** in Aktion.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

Die Ausgabe des ersten **Get-ServiceFabricNode** (ein Cmdlet des Moduls Service Fabric PowerShell) zeigt an, dass der lokale Cluster mit fünf Knoten besitzt: Node.1, Node.5. Nach Ausführung der Aktion Prüfbarkeit (Cmdlet) **ServiceFabricNode Neustart** auf dem Knoten mit dem Namen Node.4, sehen wir, dass der Knoten Betriebszeit zurückgesetzt wurde.

### <a name="run-an-action-against-an-azure-cluster"></a>Führen Sie eine Aktion für einen Azure-cluster

Ausführen einer Aktion Prüfbarkeit (mit PowerShell) gegen einen Azure-Cluster ähnelt dem Ausführen der Aktion für einen lokalen Cluster. Der einzige Unterschied ist, dass Sie vor dem Ausführen der Aktion statt des lokalen Clusters Azure zuerst herstellen müssen.

## <a name="running-a-testability-action-using-c35"></a>Eine mit C & #35 Prüfbarkeit Aktion ausführen;

Um mit C# Prüfbarkeit Aktion ausführen, müssen zunächst FabricClient mit dem Cluster herstellen. Dann erhalten Sie die Parameter, die Aktion auszuführen. Andere Parameter können verwendet werden, dieselbe Aktion ausführen.
RestartServiceFabricNode Aktion ist, eine Möglichkeit für die Ausführung mit Knoteninformationen (Knotennamen und Knoten-Instanz-ID) im Cluster.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Erklärung der Parameter:

- **CompleteMode** gibt an, dass der Modus nicht überprüfen sollten, ob die neustartaktion tatsächlich erfolgreich. Angeben der Beendigungsmodus "Überprüfen" führen sie überprüfen, ob die neustartaktion tatsächlich erfolgreich.  
- **OperationTimeout** legt die Zeitspanne für den Vorgang zu beenden, bevor eine TimeoutException-Ausnahme ausgelöst wird.
- **CancellationToken** ermöglicht einen ausstehenden Aufruf abgebrochen werden.

Anstatt direkt den Knoten mit dem Namen, festlegen über Partitionsschlüssel und Replikat.

Weitere Informationen finden Sie unter [PartitionSelector und ReplicaSelector](#partition_replica_selector).


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector und ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector ist ein Hilfsprogramm Prüfbarkeit ausgesetzt und wird verwendet, um eine bestimmte Partition, die Prüfbarkeit Aktionen durchführen. Hiermit können Sie eine bestimmte Partition zu wählen, wenn die Partitions-ID bekannt ist. Den Partitionsschlüssel bereitstellen oder der Vorgang löst die Partitions-ID intern. Sie können auch eine zufällige Partition auswählen.

Um dieses Hilfsprogramm verwenden, erstellen Sie das PartitionSelector-Objekt und wählen Sie die Partition mithilfe einer Select * Methoden. Dann im PartitionSelector-Objekt-API, die es benötigt. Wenn keine Option ausgewählt ist, wird standardmäßig eine zufällige Partition.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector ist ein Hilfsprogramm Prüfbarkeit ausgesetzt und wird verwendet, um ein Replikat eines Prüfbarkeit Aktionen ausführen, zu markieren. Hiermit können Sie ein bestimmtes Replikat auswählen, wenn die Replikat-ID bekannt ist. Darüber hinaus können Sie ein primäres Replikat oder eine zufällige sekundäre. ReplicaSelector ist von PartitionSelector, müssen Sie wählen das Replikat und die Partition die Prüfbarkeit Operation ausgeführt werden soll.

Um dieses Hilfsprogramm verwenden, erstellen Sie ein ReplicaSelector-Objekt, und legen Sie das Replikat und die Partition auswählen möchten. Anschließend können Sie es in der API übergeben, die er benötigt. Wenn keine Option ausgewählt ist, wird standardmäßig ein zufälliger Replikat und zufällige Partition.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Nächste Schritte

- [Prüfbarkeit Szenarien](service-fabric-testability-scenarios.md)
- Wie Sie den Dienst testen
   - [Fehler beim Dienst Arbeitslasten simulieren](service-fabric-testability-workload-tests.md)
   - [Dienst-Kommunikationsfehler](service-fabric-testability-scenarios-service-communication.md)
