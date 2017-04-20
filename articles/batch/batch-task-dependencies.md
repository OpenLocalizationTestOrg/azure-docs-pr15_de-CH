<properties
    pageTitle="Aufgabe Abhängigkeiten in Azure Batch | Microsoft Azure"
    description="Erstellen von Aufgaben, die nach erfolgreichem Abschluss anderer Aufgaben MapReduce Stil und ähnliche große Daten abhängen Arbeitslasten in Azure Batch."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Vorgängern und Nachfolgern in Azure Batch

Aufgabe Abhängigkeiten Feature von Azure Batch ist gut verarbeitet werden soll:

- MapReduce-Stil Arbeitslasten in der Cloud.
- Aufträge, deren Verarbeitung Vorgänge als gesteuerte azyklische Diagramm (so) ausgedrückt werden können.
- Anderen Projekten in der untergeordnete Aufgaben auf die übergeordneten Aufgaben abhängen.

Batch dienen Sie Aufgaben erstellen, die für die Ausführung auf Compute-Knoten nur nach Abschluss anderer Aufgaben geplant werden. Beispielsweise können Sie einen Auftrag erstellen, der jeden Frame eines Films mit separaten, parallele Aufgaben 3D rendert. Die endgültige Aufgabe – die "Merge Aufgabe" – führt die gerenderten Frames in der Film erst erfolgreich alle Frames gerendert wurden.

Sie können Tasks erstellen, die andere Aufgaben in einer 1: 1- oder 1: n-Beziehung abhängig. Sie können sogar Bereich Abhängigkeit erstellen den erfolgreichen Abschluss einer Gruppe von Aufgaben innerhalb eines bestimmten Bereichs der Vorgangsnummern, eine Aufgabe abhängig. Drei grundlegenden Szenarien erstellen viele-zu-viele-Beziehungen können kombiniert werden.

## <a name="task-dependencies-with-batch-net"></a>Mit Batch verzögert

In diesem Artikel verzögert konfigurieren mit [Batch.NET] besprochen[ net_msdn] Bibliothek. Zuerst erfahren Sie für Ihre Projekte [Abhängigkeitsverhältnis aktivieren](#enable-task-dependencies) und dann [eine Aufgabe Abhängigkeiten](#create-dependent-tasks)konfigurieren. Schließlich besprechen wir die [Abhängigkeit Szenarien](#dependency-scenarios) , die Stapelverarbeitung unterstützt.

## <a name="enable-task-dependencies"></a>Anordnungsbeziehungen aktivieren

Um Vorgängern und Nachfolgern in der Batch-Anwendung verwenden, müssen Sie die Batch-Dienst anweisen, das Projekt verzögert verwendet wird. In Batch .NET aktivieren sie die [CloudJob] [ net_cloudjob] durch Festlegen der [UsesTaskDependencies] [ net_usestaskdependencies] -Eigenschaft auf `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Im vorhergehenden Codeausschnitt wird "BatchClient" eine Instanz von [BatchClient] [ net_batchclient] Klasse.

## <a name="create-dependent-tasks"></a>Erstellen von Aufgaben

Weisen Sie eine Aufgabe erstellen, die den erfolgreichen Abschluss anderer Aufgaben abhängig ist, Batch, der Vorgang "andere Aufgaben abhängig". Konfigurieren Sie Batch .NET [CloudTask][net_cloudtask]. [DependsOn] [net_dependson] Eigenschaft mit einer Instanz der [TaskDependencies] [ net_taskdependencies] Klasse:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Dieser Codeausschnitt erstellt eine Aufgabe mit der ID "Blumen", die voraussichtlich auf Compute-Knoten ausgeführt wird, nur, wenn die Aufgaben-IDs "" und "Sun" erfolgreich ausgeführt haben.

 > [AZURE.NOTE] Ein Vorgang gilt als abgeschlossen, wenn der Status **abgeschlossen** ist und der **Exitcode** ist `0`. Batch .NET bedeutet [CloudTask][net_cloudtask]. [Zustand] [net_taskstate] Eigenschaftswert des `Completed` und die CloudTask [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] Eigenschaftenwert ist `0`.

## <a name="dependency-scenarios"></a>Abhängigkeit Szenarien

Einfache Aufgabe Abhängigkeit Szenarien, mit denen Sie in Azure Batch sind: 1: 1-, n- und Task-ID Bereich Abhängigkeit. Diese können zu vierten Szenario n kombiniert werden.

 Szenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Beispiel | |
 :-------------------: | ------------------- | -------------------
 [1](#one-to-one) | *TaskB* hängt von *taskA* <p/> *TaskB* wird erst zur Ausführung geplant werden, wenn *TaskA* erfolgreich abgeschlossen wurde | ![Diagramm: eins Abhängigkeitsverhältnis][1]
 [N-](#one-to-many) | *TaskC* hängt von *TaskA* und *taskB* <p/> *TaskC* wird erst zur Ausführung geplant werden, wenn *TaskA* und *TaskB* erfolgreich abgeschlossen haben | ![Diagramm: Aufgabe 1: n-Beziehung][2]
 [Task-ID-Bereich](#task-id-range) | *TaskD* hängt von einer Reihe von Aufgaben <p/> *TaskD* wird nicht für die Ausführung geplant werden, bis die Aufgaben mit IDs *1* bis *10* abgeschlossen haben | ![Diagramm: Abhängigkeitsverhältnis Id Bereich][3]

>[AZURE.TIP] **M: n -** Beziehungen wie Aufgaben A und b, C, D, E und F jedes Aufgaben abhängen erstellen Dies empfiehlt sich beispielsweise in parallelisierte Präprozessorlauf Szenarien die Ausgabe von mehreren übergeordneten Aufgaben, Ihre untergeordneten Aufgaben abhängig.

### <a name="one-to-one"></a>1

Eine Aufgabe erstellen, die den erfolgreichen Abschluss eines anderen Vorgangs abhängig ist, geben Sie eine einzelne Aufgabe-ID [TaskDependencies][net_taskdependencies]. [OnId] [net_onid] beim Füllen der [DependsOn] statische[ net_dependson] -Eigenschaft des [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>N-

Zum Erstellen einer Aufgabe die Abhängigkeit auf den erfolgreichen Abschluss der mehrere Aufgaben geben eine Auflistung von Task-IDs [TaskDependencies][net_taskdependencies]. [OnIds] [net_onids] beim Füllen der [DependsOn] statische[ net_dependson] -Eigenschaft des [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Task-ID-Bereich

Zum Erstellen einer Aufgabe die Abhängigkeit auf den erfolgreichen Abschluss einer Gruppe von Aufgaben, deren IDs liegen in einem Bereich geben Sie die erste und letzte Aufgabe IDs und [TaskDependencies][net_taskdependencies]. [OnIdRange] [net_onidrange] beim Füllen der [DependsOn] statische[ net_dependson] -Eigenschaft des [CloudTask][net_cloudtask].

>[AZURE.IMPORTANT] Bei Verwendung von Task-ID-Bereiche für die Abhängigkeiten sein Vorgangsnummern im Bereich *muss* Stringdarstellung ganzzahlige Werte. Außerdem muss jede Aufgabe im Bereich für die abhängige Aufgabe zur Ausführung geplant werden erfolgreich abgeschlossen.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Codebeispiel

[TaskDependencies] [ github_taskdependencies] Beispielprojekt ist eines der [Codebeispiele Azure Batch] [ github_samples] auf GitHub. Visual Studio 2015 Lösung veranschaulicht das Abhängigkeitsverhältnis für ein Projekt aktivieren, Aufgaben, die von anderen Aufgaben abhängen erstellen und Ausführen von Aufgaben in einem Pool von Compute-Knoten.

## <a name="next-steps"></a>Nächste Schritte

### <a name="application-deployment"></a>Bereitstellung

[Anwendungspakete](batch-application-packages.md) Feature Charge erleichtert die beiden bereitstellen und die Programme, die Ihre Aufgaben ausgeführt compute-Knoten.

### <a name="installing-applications-and-staging-data"></a>Installieren von Anwendun gen und staging-Daten

[Installieren von Clientanwendungen und staging Daten Batch Computeknoten] Auschecken[ forum_post] stellen in Azure Batch Forum eine Übersicht über die verschiedenen Knoten auszuführende Aufgaben vorbereiten. Von einem Teammitglieder Azure Batch ist dieser Beitrag eine gute Einführung auf verschiedenen Arten (einschließlich Applikationen und eingegebenen Daten) zu auf Compute-Knoten.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagramm: 1: 1-Beziehung"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagramm: n-Beziehung"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagramm: Abhängigkeitsverhältnis Id Bereich"
