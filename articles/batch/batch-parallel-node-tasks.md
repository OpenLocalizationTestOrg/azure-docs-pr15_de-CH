<properties
    pageTitle="Maximierung der Batch Knoten mit parallelen Aufgaben | Microsoft Azure"
    description="Über weniger Compute-Knoten und Ausführen paralleler Aufgaben auf jedem Knoten in einem Pool Azure Batch erhöhen Sie Effizienz und geringere Kosten"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Maximieren Sie Azure Batch Compute Ressourcenverbrauch bei gleichzeitiger Knoten Aufgaben

Maximieren Sie mehrere Aufgaben gleichzeitig auf jeder Compute-Knoten im Pool Azure Batch ausführen, Ressource: Einsatz auf einer geringeren Anzahl von Knoten im Pool. Für einige Arbeitslasten können schnellere Auftrag und niedrigere Kosten dadurch.

Einige Szenarien profitieren alle Ressourcen von einem Knoten zu einem einzelnen Vorgang widmen, profitieren Situationen mehrere Aufgaben Ressourcen freigeben:

 - **Minimieren Sie die Datenübertragung** Aufgaben Daten gemeinsam nutzen können. In diesem Szenario reduzieren erheblich datenübertragungsgebühren Sie freigegebene Daten wenige Knoten kopieren und Ausführen von Aufgaben auf jedem Knoten. Dies gilt insbesondere, wenn die Daten in jedem Knoten kopiert werden zwischen Regionen übertragen werden müssen.

 - **Speicherverwendung maximieren** Aufgaben benötigt viel Arbeitsspeicher, allerdings nur während kurzer Zeiträume und zu unterschiedlichen Zeiten während der Ausführung. Sie können weniger jedoch größer, Compute-Knoten mit mehr Speicher effizient behandelt solche Spitzen verwenden. Diese Knoten hätte mehrere Vorgänge parallel auf jedem Knoten ausgeführt, aber jede Aufgabe dauert die Knoten reichlich Speicher zu unterschiedlichen Zeiten.

 - **Verringerung der Anzahl Grenzwerte Knoten** bei der Kommunikation zwischen den Knoten in einem Pool erforderlich ist. Pools für die Kommunikation zwischen den Knoten konfiguriert sind derzeit 50 Compute-Knoten auf. Wenn jeder Knoten in einem Pool Aufgaben parallel ausgeführt werden kann, kann eine größere Anzahl von Aufgaben gleichzeitig ausgeführt werden.

 - **Für lokale Replikation compute Cluster**, wie wenn Sie zunächst Computing-Umgebung zu Azure wechseln. Wenn Ihre aktuelle lokale Lösung mehrere Vorgänge pro Compute-Knoten ausführt, steigern Sie die maximale Anzahl von Knoten Vorgänge genauer spiegeln diese Konfiguration.

## <a name="example-scenario"></a>Beispielszenario

Als Beispiel, die Vorteile paralleler Aufgaben angenommen, die Aufgabe Anwendung erforderliche CPU und Speicher, [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) Knoten reichen. Jedoch um die Arbeit innerhalb der erforderlichen Zeit beenden, 1.000 Knoten benötigt werden.

Anstatt Standard\_D1 Knoten mit 1 CPU-Kern, können [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) Knoten, die 16 Kerne und Ausführung paralleler Aufgaben ermöglichen. Aus diesem Grund *16 Mal weniger Knoten* genutzt werden – statt 1.000 Knoten, nur 63 erforderlich. Außerdem sind große Dateien oder Daten für jeden Knoten, sind Auftragsdauer und Effizienz erneut verbessert, da die Daten nur 16 Knoten.

## <a name="enable-parallel-task-execution"></a>Parallele Taskausführung aktivieren

Compute-Knoten für die Ausführung paralleler Aufgaben konfiguriert auf Poolebene. Legen Sie mit der Bibliothek Batch .NET [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] -Eigenschaft beim Erstellen eines Pools. Wenn Sie Batch REST-API verwenden, legen Sie [MaxTasksPerNode] [ rest_addpool] Element im Hauptteil Anforderung beim Pool erstellen.

Azure Batch können Sie maximale Aufgaben pro Knoten bis zu viermal (4 X) die Anzahl der Knoten Kerne. Z. B. wenn der Pool mit Knoten konfiguriert Größe "Groß" (vier Kerne) dann `maxTasksPerNode` kann 16 festgelegt werden. Ausführliche Informationen über die Anzahl der Kerne für jeden Knoten Größen anzeigen Sie [Größen für Cloud-Dienste](../cloud-services/cloud-services-sizes-specs.md) Weitere Informationen zu Service-Grenzwerte finden Sie unter [Kontingente und Grenzwerte für Azure Batch-Dienst](batch-quota-limit.md).

> [AZURE.TIP] Berücksichtigen Sie unbedingt die `maxTasksPerNode` Wert beim Erstellen einer [Formel automatisch skalieren] [ enable_autoscaling] für den Pool. Angenommen, eine Formel, berechnet `$RunningTasks` Zunahme Aufgaben pro Knoten könnte erheblich beeinträchtigt werden. Weitere Informationen finden Sie unter [automatisch skalieren compute-Knoten in einem Pool Azure Batch](batch-automatic-scaling.md) .

## <a name="distribution-of-tasks"></a>Aufgabenverteilung

Compute-Knoten in einem Pool Aufgaben gleichzeitig ausgeführt werden, ist es wichtig anzugeben, wie die Aufgaben über die Knoten im Pool verteilt werden.

[CloudPool.TaskSchedulingPolicy] mit[ task_schedule] -Eigenschaft können Sie angeben, dass Aufgaben gleichmäßig auf alle Knoten im Pool ("Zuweisung") zugewiesen werden soll. Oder Sie können angeben, dass so viele Vorgänge wie möglich jeder Knoten zugewiesen werden soll, bevor Sie Aufgaben auf einen anderen Knoten im Pool ("Verpackung").

Berücksichtigen Sie beispielsweise der dabei wertvolle Pool von [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) Knoten (im obigen Beispiel), die mit [CloudPool.MaxTasksPerComputeNode] konfiguriert[ maxtasks_net] Wert 16. Wenn [CloudPool.TaskSchedulingPolicy] [ task_schedule] mit [ComputeNodeFillType] konfiguriert[ fill_type] *Pack*es Verwendung alle 16 Kerne für die einzelnen Knoten zu maximieren und ermöglichen eine [Skalierung Pool](batch-automatic-scaling.md) nicht verwendete Knoten aus dem Pool (Knoten ohne Aufgaben) zu löschen. Dies minimiert die Ressourcenverwendung und spart Geld.

## <a name="batch-net-example"></a>Batch .NET Beispiel

Diese [Stapelverarbeitung .NET] [ api_net] API-Codeausschnitt zeigt eine Anforderung zum Erstellen eines Pools, die vier große Knoten mit maximal vier Aufgaben pro Knoten enthält. Es gibt eine Richtlinie, die jeden Knoten Aufgaben vor dem Zuweisen von Aufgaben zu einem anderen Knoten im Pool automatisch Planen von Tasks. Weitere Informationen zum Hinzufügen von Pools mit Batch .NET API finden Sie unter [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Batch REST-Beispiel

Diese [Stapelverarbeitung REST] [ api_rest] API-Codeausschnitt wird eine Anforderung zum Erstellen eines Pools, das zwei große Knoten mit maximal vier Aufgaben pro Knoten enthält. Weitere Informationen zum Hinzufügen von Pools mithilfe der REST-API finden Sie unter [Hinzufügen eines Pools mit einem Konto][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] Legen Sie die `maxTasksPerNode` Element und [MaxTasksPerComputeNode] [ maxtasks_net] -Eigenschaft nur zum Zeitpunkt der Pool-Erstellung. Sie können nicht geändert werden, nachdem ein Pool bereits erstellt wurde.

## <a name="code-sample"></a>Codebeispiel

[ParallelNodeTasks] [ parallel_tasks_sample] GitHub Projekt veranschaulicht die Verwendung der [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] Eigenschaft.

Dieser C#-Konsole Anwendung [Batch.NET] [ api_net] Bibliothek zum Erstellen eines Pools mit einem oder mehreren Computeknoten. Sie führt eine konfigurierbare Anzahl von Aufgaben auf diesen Knoten Variablen simulieren. Ausgabe der Anwendung gibt an, welche Knoten jede Aufgabe ausgeführt. Die Anwendung bietet außerdem eine Zusammenfassung der Parameter und Dauer. Zusammenfassung Teil der Ausgabe aus zwei verschiedenen derselben Anwendung wird unten angezeigt.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Die erste Ausführung derselben Anwendung zeigt, dass mit einem einzelnen Knoten und die Standardeinstellung jeweils eine Aufgabe pro Knoten Auftrag über 30 Minuten dauert.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Das zweite Beispiel zeigt einen beträchtlichen Rückgang Auftragsdauer ausführen. Ist der Pool mit vier Aufgaben pro Knoten konfiguriert wurde parallelen Taskausführung Mitarbeit in fast ein Viertel der Zeit ermöglicht.

> [AZURE.NOTE] Auftrag Dauer in der obigen Zusammenfassung enthalten keinen Pool-Erstellung. Jobs oben wurde zuvor erstellten Pools übermittelt, deren Compute-Knoten im *Leerlauf* Zustand Sendezeit waren.

## <a name="next-steps"></a>Nächste Schritte

### <a name="batch-explorer-heat-map"></a>Batch-Explorer Heatmap

[Azure Batch Explorer][batch_explorer], eines Azure Batch [Anwendungsbeispiele][github_samples], enthält eine *Heat Map* Funktion, Visualisierung der Taskausführung. Wenn Sie [ParallelTasks] ausgeführt haben[ parallel_tasks_sample] Anwendung können Sie die Heat Map-Funktion problemlos Parallele Aufgaben auf jedem Knoten darstellen.

![Batch-Explorer Heatmap][1]

*Batch-Explorer Heat Map zeigt einen Pool von vier Knoten mit jedem Knoten ausgeführten vier Aufgaben*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
