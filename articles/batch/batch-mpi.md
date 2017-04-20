<properties
    pageTitle="MPI-Anwendung mit mehreren Instanzen Aufgaben in Azure Batch ausführen | Microsoft Azure"
    description="Informationen Sie zum Message Passing Interface (MPI) Programme verwenden die Vorgangsart mit mehreren Instanzen in Azure Batch ausgeführt."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Verwenden Sie mehrere Instanzen Aufgaben Message Passing Interface (MPI) Anwendung in Azure Batch ausgeführt

Mehrere Instanzen Aufgaben ermöglichen Ihnen ein Azure Stapelverarbeitungsaufgaben gleichzeitig auf mehreren Computeknoten ausgeführt. Diese Aufgaben können Hochleistungscomputing Szenarien wie Message Passing Interface (MPI) im Stapel. In diesem Artikel lernen Sie mit mehreren Instanzen Aufgaben mit der [Stapelverarbeitung .NET] ausführen[ api_net] Bibliothek.

>[AZURE.NOTE] Und Beispiele in diesem Artikel beziehen sich auf Batch .NET MS MPI Windows compute-Knoten, gelten die hier erläuterten Mehrfachinstanz Aufgabe Konzepte für andere Plattformen und Technologien (Python und Intel MPI auf Linux-Knoten beispielsweise).

## <a name="multi-instance-task-overview"></a>Mehrere Instanzen Aufgabenübersicht

Im Batch ist jede Aufgabe normalerweise mehrere Aufgaben für einen Auftrag Absenden auf einzelnen Compute-Knoten – ausgeführt, und Batch-Service plant jeden Task für die Ausführung auf einem Knoten. Einen Vorgang **mit mehreren Instanzen Einstellungen**konfigurieren, erfahren Sie jedoch Batch erstellen eine primäre Aufgabe und mehrere Teilvorgänge, die dann auf mehreren Knoten ausgeführt werden.

![Mehrere Instanzen Aufgabenübersicht][1]

Beim Senden einer Aufgabe mit einem Projekt mit mehreren Instanzen führt Batch mehrere Schritte eindeutige Mehrfachinstanz-Aufgaben:

1. Der Batch-Dienst erstellt eine **primäre** und mehrere **Teilvorgänge** auf der Basis mit mehreren Instanzen. Die Gesamtzahl der Aufgaben (primäre sowie alle Teilvorgänge) entspricht der Anzahl von **Instanzen** (Compute-Knoten) Sie die Multi-Instanz angegeben.
1. Stapel weist eine Compute-Knoten als **master**und der primäre Vorgang auf dem Master ausgeführt. Es plant Teilvorgänge auszuführende auf den verbleibenden Computeknoten Mehrfachinstanz-Aufgabe eine Unteraufgabe pro Knoten zugeordnet.
1. Der primäre und alle Teilvorgänge herunterladen **Allgemeine Ressourcendateien** , die Sie den Multi-Instanz angegeben.
1. Nach gemeinsamen Ressourcen Dateien heruntergeladen, die primäre und Teilvorgänge in den Einstellungen mehrere Instanzen geben **Koordinierung Befehl** ausführen. Der Befehl Koordinierung dient normalerweise Knoten Ausführen der vorbereiten. Dazu gehören Hintergrunddienste starten (wie [Microsoft MPI][msmpi_msdn] `smpd.exe`) und überprüfen, dass die Knoten zwischen den Knoten Nachrichten verarbeiten können.
1. Die primäre Aufgabe führt den **Befehl** auf der master-Knoten *nach dem* Befehl Koordinierung der Haupt-und alle Teilvorgänge erfolgreich abgeschlossen wurde. Anwendungsbefehl ist die Befehlszeile der Aufgabe mit mehreren Instanzen und nur durch die Hauptaufgabe ausgeführt. In einem [MS MPI][msmpi_msdn]-Lösung, dies ist, wo die MPI-fähigen Anwendung ausführen `mpiexec.exe`.

> [AZURE.NOTE] Funktionell unterschiedlichen ist "mehrere Instanzen Task" ist nicht eindeutig Vorgangsart wie [StartTask] [ net_starttask] oder [JobPreparationTask][net_jobprep]. Aufgabe mit mehreren Instanzen ist einfach eine Standardaufgabe Batch ([CloudTask] [ net_task] in Batch .NET), deren Einstellungen mit mehreren Instanzen konfiguriert wurden. In diesem Artikel verwenden wir Sie **Mehrfachinstanz-Aufgabe**.

## <a name="requirements-for-multi-instance-tasks"></a>Vorschriften für Vorgänge mit mehreren Instanzen

Mehrere Instanzen Aufgaben erfordern einen Pool mit **Kommunikation zwischen den Knoten aktiviert**und **deaktiviert die gleichzeitige Taskausführung**. Versuch zum Ausführen eines Tasks mit mehreren Instanzen in einem Pool mit Netzwerk-Kommunikation deaktiviert oder mit einem *MaxTasksPerNode* -Wert größer als 1, wird nie Vorgängen – bleibt auf unbestimmte Zeit im Zustand "aktiv". Dieser Codeausschnitt zeigt die Erstellung eines Pools, die von der Stapelverarbeitung .NET Bibliothek.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Darüber hinaus können mit mehreren Instanzen Aufgaben *nur* auf **Pools erstellt nach 14 Dezember 2015**ausführen.

### <a name="use-a-starttask-to-install-mpi"></a>Verwenden Sie ein StartTask MPI installieren

Um MPI-Anwendung mit mehreren Instanzen auszuführen, müssen Sie eine MPI-Implementierung (MS MPI oder Intel MPI Beispiel) auf den Compute-Knoten im Pool installiert. Dies ist ein guter Zeitpunkt einen [StartTask]mit[net_starttask], führt die Knoten verbindet einen Pool oder wird neu gestartet. Dieser Codeausschnitt erstellt ein StartTask, das MS MPI-Setup-Paket als [Ressourcendatei]gibt[net_resourcefile]. Die Startaufgabe Befehlszeile wird nach dem Download der Datei auf den Knoten ausgeführt. In diesem Fall führt die Befehlszeile eine unbeaufsichtigte Installation von MS MPI.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Remote direct Memory Access (RDMA)

Wenn Sie eine [RDMA-fähig Größe](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) wie A9 für den Compute-Knoten im Batch-Pool wählen, profitieren die MPI-Anwendung Azure hohe Leistung, niedriger Latenz remote direct Memory Access (RDMA) Netzwerk.

Suchen Sie die Größenangaben als "RDMA-fähig" in den folgenden Artikeln:

* **CloudServiceConfiguration** -pools

  * [Größen für Cloud-Services](../cloud-services/cloud-services-sizes-specs.md) (Nur Windows)

* **VirtualMachineConfiguration** -pools

  * [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Um RDMA auf [Linux compute-Knoten](batch-linux-nodes.md)nutzen zu können, müssen Sie auf den Knoten **Intel MPI** verwenden. Weitere Informationen zu CloudServiceConfiguration und VirtualMachineConfiguration-Pools finden Sie in Abschnitt [Pool](batch-api-basics.md#Pool) Überblick Funktion Batch.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Erstellen Sie eine Aufgabe mit mehreren Instanzen mit Batch

Nun, wir Pool Vorschriften und MPI-Paketinstallation gesprochen haben, erstellen die Aufgabe mit mehreren Instanzen. In diesem Ausschnitt erstellen wir eine standard- [CloudTask][net_task], konfigurieren Sie die [MultiInstanceSettings] [ net_multiinstance_prop] Eigenschaft. Wie bereits erwähnt, ist die Aufgabe mit mehreren Instanzen nicht ein unterscheidet jedoch ein standard Stapelverarbeitungsaufgabe mit mehreren Instanzen konfiguriert.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Hauptaufgabe und Teilvorgänge

Beim Erstellen von Mehrfachinstanz-Einstellungen für einen Vorgang angeben die Anzahl von Compute-Knoten, die die Aufgabe ausführen. Beim Absenden des Tasks an einen Auftrag erstellt der Batch-Dienst **eine Hauptaufgabe** und genügend **Teilvorgänge** , die angegebene Anzahl Knoten miteinander entsprechen.

Diese Aufgaben werden *NumberOfInstances* - 1 eine ganzzahlige Id im Bereich von 0 zugewiesen. Die Aufgabe mit der Id 0 ist die primäre und alle anderen Ids sind Teilvorgänge. Beispielsweise wenn Sie Folgendes mit mehreren Instanzen für eine Aufgabe erstellen, die Hauptaufgabe hat eine Id von 0 und Teilvorgänge müsste Ids 1 bis 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Master-Knoten

Wenn Sie einen Vorgang mit mehreren Instanzen senden, Batch-Dienst bezeichnet man den Compute-Knoten als "master" Knoten und plant die Hauptaufgabe der master-Knoten ausführen. Der Teilvorgänge werden ausgeführt auf den verbleibenden Knoten mit mehreren Instanzen Vorgang zugeordnet.

## <a name="coordination-command"></a>Koordinierung Befehl

**Koordinierung Befehl** wird ausgeführt, indem der Primär- und Teilvorgänge.

Der Aufruf des Befehls Koordinierung blockiert - Stapel nicht Befehls Anwendung ausgeführt, bis Koordinierung Befehl für alle Teilvorgänge zurückgegeben hat. Befehls Koordinierung sollte daher starten alle Hintergrunddienste erforderlichen sicherzustellen, dass sie einsatzbereit und beenden. Z. B. beendet diesen Befehl Koordinierung für eine Lösung mit MS MPI Version 7 SMPD-Dienst auf dem Knoten gestartet:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Beachten Sie die Verwendung des `start` in diesem Befehl Koordinierung. Dies ist erforderlich, da die `smpd.exe` Anwendung nicht unmittelbar nach der Ausführung zurück. [Starten] ohne[ cmd_start] Befehl dieser Koordinierung Befehl nicht zurück und wird daher den Anwendungsbefehl blockiert.

## <a name="application-command"></a>Befehl

Nach primäre Aufgabe und alle Teilvorgänge Befehl Koordinierung Befehlszeile die Aufgabe mehrere Instanzen der Aufgabe *nur*erfolgt. Wir nennen dies den **Befehl** Befehls Koordinierung unterscheiden.

Für MS MPI-Applikationen Befehl Anwendung auszuführende die MPI-fähigen Anwendung mit `mpiexec.exe`. Hier ist beispielsweise ein Befehl für eine Lösung mit MS MPI Version 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Da MS-MPI `mpiexec.exe` verwendet die `CCP_NODES` Variable standardmäßig (siehe [Umgebungsvariablen](#environment-variables)) im Beispiel Befehlszeile oben ausgeschlossen.

## <a name="environment-variables"></a>Umgebungsvariablen

Batch erstellt mehrere [Umgebungsvariablen] [ msdn_env_var] für mehrere Instanzen Aufgaben auf den Compute-Knoten einen Vorgang mit mehreren Instanzen zugeordnet. Die Koordinierung und die Anwendung Zeilen können diese Umgebungsvariablen verweisen, wie Skripts und Programme sie ausführen können.

Die folgenden Umgebungsvariablen werden vom Batch-Dienst für die Verwendung mit mehreren Instanzen Aufgaben erzeugt:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Ausführliche Informationen zu diesen und anderen Stapel Compute Knoten Umgebungsvariablen einschließlich Inhalt und Sichtbarkeit finden Sie unter [berechnen Knoten Umgebungsvariablen][msdn_env_var].

>[AZURE.TIP] Batch Linux MPI-Codebeispiel enthält ein Beispiel wie mehrere Umgebungsvariablen verwendet werden können. [Koordinierung Cmd] [ coord_cmd_example] Bash-Skript liest allgemeine Anwendung und Eingabe von Azure Storage-Dateien, eine Freigabe (NFS = Network File System) auf dem Masterknoten aktiviert und konfiguriert die anderen Knoten als NFS-Clients mit mehreren Instanzen Vorgang zugeordnet.

## <a name="resource-files"></a>Ressourcendateien

Es gibt zwei Ressourcendateien für mehrere Instanzen Aufgaben berücksichtigen: **Allgemeine Ressourcendateien** , *Alle* herunterladen (Primär- und Teilvorgänge), und die **Ressourcendateien** für mehrere Instanzen angegeben, welche *die primäre* Aufgabe downloads Aufgabe.

Sie können eine oder mehrere **Allgemeine Ressourcendateien** den mit mehreren Instanzen für einen Vorgang angegeben. Diese gemeinsamen Ressourcendateien werden von [Azure Storage](./../storage/storage-introduction.md) in jedem Knoten **Aufgabe freigegebenen Verzeichnis** von primären und alle Teilvorgänge heruntergeladen. Anwendung und Koordinierung Befehlszeilen Aufgabe freigegebenes Verzeichnis zugreifen möchten, mit der `AZ_BATCH_TASK_SHARED_DIR` -Umgebungsvariable. Die `AZ_BATCH_TASK_SHARED_DIR` Pfad auf jedem Knoten mit mehreren Instanzen Vorgang zugeordnet ist, damit Sie können einen Befehl einzelne Koordinierung zwischen dem primären und alle Teilvorgänge. Batch "teilt" Directory insofern RAS jedoch als Halterung verwenden oder Teilen wie bereits zuvor in auf Umgebungsvariablen.

Resource Dateien für mehrere Instanzen Aufgabe in der Aufgabe Arbeitsverzeichnis heruntergeladen `AZ_BATCH_TASK_WORKING_DIR`, standardmäßig. Wie im Gegensatz zu gemeinsamen Ressourcendateien downloads nur die Hauptaufgabe der Ressourcendateien für die Aufgabe mehrere Instanzen angegeben.

> [AZURE.IMPORTANT] Verwenden Sie immer die Umgebungsvariablen `AZ_BATCH_TASK_SHARED_DIR` und `AZ_BATCH_TASK_WORKING_DIR` auf diese Verzeichnisse in die Zeilen. Führen Sie die Pfade manuell erstellen.

## <a name="task-lifetime"></a>Task-Lebensdauer

Die Lebensdauer der primäre Aufgabe steuert die Lebensdauer der gesamten Mehrfachinstanz-Aufgabe. Beim primären beenden, werden alle Teilvorgänge beendet. Der Exitcode des primären ist der Exitcode des Vorgangs und dient daher den Erfolg oder Misserfolg des Vorgangs zu wiederholen.

Eines Teilvorgänge fehlschlagen scheitert mit einem Rückgabecode ungleich Null beendet z. B. die gesamte mit mehreren Instanzen Aufgabe. Aufgabe mit mehreren Instanzen wird beendet und wiederholt, bis die maximale Anzahl der erneuten.

Beim Löschen einer Aufgabe mehrere Instanzen werden die primäre und alle Teilvorgänge auch vom Batch-Dienst gelöscht. Alle Unteraufgaben Verzeichnisse und Dateien von Compute-Knoten wie einen Standardkatalog gelöscht.

[TaskConstraints] [ net_taskconstraints] für einen Vorgang mit mehreren Instanzen wie [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], und [RetentionTime] [ net_taskconstraint_retention] Eigenschaften werden für eine Tätigkeit, und gelten für die primäre und alle Teilvorgänge berücksichtigt. Allerdings ändert das [RetentionTime] [ net_taskconstraint_retention] -Eigenschaft nach Auftrag dazu mehrere Instanzen Aufgabe hinzufügen wird nur auf die Hauptaufgabe angewendet. Alle Teilvorgänge weiterhin den ursprünglichen [RetentionTime][net_taskconstraint_retention].

Aktuelle Aufgabenliste ein Compute-Knoten gibt die Id eines Teilvorgangs war der letzte Vorgang Teil des Vorgangs mit mehreren Instanzen.

## <a name="obtain-information-about-subtasks"></a>Informationen über Teilvorgänge

Teilvorgänge Informationen mithilfe der Stapelverarbeitung .NET Bibliothek aufrufen, [CloudTask.ListSubtasks] [ net_task_listsubtasks] Methode. Diese Methode gibt Informationen über alle Teilvorgänge und Informationen über den Compute-Knoten, der die Aufgaben ausgeführt. Anhand dieser Informationen bestimmen Sie das Stammverzeichnis jeder Unteraufgabe, die Pool-Id des aktuellen Zustands, Exitcode und mehr. Können diese Informationen zusammen mit [PoolOperations.GetNodeFile] [ poolops_getnodefile] -Methode des Teilvorgangs Dateien abrufen. Beachten Sie, dass diese Methode nicht die primäre Aufgabe (Id 0) zurück.

> [AZURE.NOTE] Sofern nicht anders angegeben, Batch .NET Methoden, die Arbeiten mit mehreren Instanzen [CloudTask] [ net_task] selbst gelten *nur* für die primäre Aufgabe. Beispielsweise beim Aufruf von [CloudTask.ListNodeFiles] [ net_task_listnodefiles] Methode für einen Vorgang mit mehreren Instanzen nur die Hauptaufgabe Dateien zurückgegeben werden.

Der folgende Codeausschnitt veranschaulicht die Unteraufgabe Informationen sowie Knoten, auf denen sie ausgeführt Inhalt anfordern.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Codebeispiel

[MultiInstanceTasks] [ github_mpi] Codebeispiel auf GitHub veranschaulicht, wie eine Aufgabe mehrere Instanzen einer [MS MPI] ausführen[ msmpi_msdn] Anwendung im Batch compute-Knoten. Führen Sie die Schritte [zur Vorbereitung](#preparation) und [Ausführung](#execution) zum Ausführen des Beispiels.

### <a name="preparation"></a>Vorbereitung

1. Die ersten beiden Schritte wie [einfaches MS MPI-Programm kompilieren und Ausführen][msmpi_howto]. Dies entspricht der Prerequesites für den folgenden Schritt.
1. Erstellen Sie *eine Version von [MPIHelloWorld] * [ helloworld_proj] MPI-Beispielprogramm. Dies ist das Programm, das vom Task mehrere Instanzen auf Compute-Knoten ausgeführt wird.
1. Erstellen einer Zip-Datei mit `MPIHelloWorld.exe` (die Sie Schritt 2 erstellte) und `MSMpiSetup.exe` (die heruntergeladene Schritt 1). Sie werden als Nächstes Anwendungspaket diese Zip-Datei hochladen.
1. Verwenden der [Azure-Portal] [ portal] zum Erstellen einer Batch- [Anwendung](batch-application-packages.md) namens "MPIHelloWorld", und geben Sie die Zip-Datei erstellen im vorherigen Schritt als Version "1.0" des Anwendungspakets. Weitere Informationen finden Sie unter [hochzuladen und Applikationen verwalten](batch-application-packages.md#upload-and-manage-applications) .

>[AZURE.TIP] Erstellen Sie *eine Version von* `MPIHelloWorld.exe` , damit nicht zusätzlichen Abhängigkeiten enthalten (z. B. `msvcp140d.dll` oder `vcruntime140d.dll`) in das Anwendungspaket.

### <a name="execution"></a>Ausführung

1. [Azure-Batch-Beispiele] downloaden[ github_samples_zip] von GitHub.
1. Öffnen Sie die MultiInstanceTasks **Lösung** in Visual Studio 2015. Die `MultiInstanceTasks.sln` Projektmappendatei befindet sich in:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Geben Sie Ihre Anmeldeinformationen Stapel und Speicher in `AccountSettings.settings` im **Microsoft.Azure.Batch.Samples.Common** -Projekt.
1. **Erstellen und führen** MultiInstanceTasks Lösung MPI auszuführende Anwendung auf compute-Knoten in einem Batch.
1. *Optional*: Verwenden der [Azure-Portal] [ portal] oder [Batch-Explorer] [ batch_explorer] zu Beispiel Pool, Projekt und Aufgabe ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask"), bevor Sie Ressourcen löschen.

>[AZURE.TIP] Sie können [Visual Studio Community] [ visual_studio] kostenlos, wenn Sie nicht über Visual Studio verfügen.

Ausgabe von `MultiInstanceTasks.exe` ist wie folgt:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Nächste Schritte

- Microsoft HPC & Azure Batch Team Blog erläutert [MPI-Unterstützung für Linux auf Azure Batch][blog_mpi_linux], und enthält Informationen über [OpenFOAM] [ openfoam] mit. Finden Sie Codebeispiele Python [OpenFOAM Beispiel GitHub][github_mpi].

- Erfahren Sie, wie [Linux Compute-Knoten erstellen](batch-linux-nodes.md) , für die Verwendung in Azure Batch MPI-Projektmappen.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Mehrere Instanzen (Übersicht)"
