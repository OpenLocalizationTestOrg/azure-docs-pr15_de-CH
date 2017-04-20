<properties
    pageTitle="Projekt und Persistenz in Azure Batch Ausgabe | Microsoft Azure"
    description="Informationen Sie zum Azure-Speicher verwenden ein permanenten Speicher für Stapelverarbeitungsaufgaben und Auftrag ausgegeben und dauerhaften Ausgabe in Azure-Portal anzeigen aktivieren."
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
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Azure Batch Projekt und Ausgabe beibehalten

Die Aufgaben, die normalerweise im Batch ausgeführt erstellt Ausgabe, die gespeichert und später von anderen Aufgaben im Auftrag der Clientanwendung, die den Auftrag oder beide ausgeführt abgerufen werden muss. Diese Ausgabe möglicherweise Dateien von Eingabedaten verarbeitet oder Ausführung der Aufgabe zugeordneten Protokolldateien. Dieser Artikel stellt eine .NET Klassenbibliothek, die ein Übereinkommen Verfahren weiterhin diese Aufgabe Ausgabe Azure BLOB-Speicher verfügbar zu machen auch nach dem Löschen des Pools, Aufträge und-Knoten Compute verwendet.

Mithilfe des Verfahrens in diesem Artikel können Sie auch die Aufgabe Ausgabe anzeigen, **gespeicherte Dateien** und **Gespeicherte Protokolle** in [Azure-Portal][portal].

![Gespeicherte Dateien und gespeicherte Protokolle Auswahlen im portal][1]

>[AZURE.NOTE] [Azure Batch Konventionen] [ nuget_package] .NET in diesem Artikel beschriebenen ist zurzeit in der Vorschau. Einige der hier beschriebenen Funktionen können vor der allgemeinen Verfügbarkeit ändern.

## <a name="task-output-considerations"></a>Aufgabe Ausgabe Aspekte

Beim Entwerfen Ihrer Batch-Lösung müssen Sie einige Faktoren Projekt und Ausgaben berücksichtigen.

* **Compute-Knoten Lebensdauer**: Knoten sind besonders in Pools skalieren aktiviert häufig vorübergehend zu berechnen. Ausgaben auf einem Knoten ausgeführte Aufgaben stehen nur während der Knoten vorhanden ist, und nur innerhalb der Retentionszeit Datei Sie für den Vorgang festgelegt haben. Um sicherzustellen, dass die Aufgabe Ausgabe beibehalten wird, müssen Ihre Aufgaben daher die Ausgabedateien in einen permanenten Speicher, z. B. Azure-Speicher hochladen.

* **Ausgabedateien**: Ausgabedaten in permanenten Speicher bestehen, können Sie [Azure Storage SDK](../storage/storage-dotnet-how-to-use-blobs.md) in der Aufgabencode Aufgabe Ausgabe in einen Speichercontainer Blob hochladen. Bei der Implementierung von Container und Dateibenennungskonvention können der Clientanwendung oder andere Aufgaben im Auftrag dann suchen und laden diese Ausgabe der Konvention.

* **Ausgabe abrufen**: Sie können abrufen Aufgabe Ausgabe direkt von Compute-Knoten im Pool oder von Azure-Speicher Wenn Aufgaben Ausgabe beibehalten. Zum Abrufen einer Aufgabe Ausgabe direkt von Compute-Knoten benötigen Sie Namen und Standort Ausgabe auf dem Knoten. Ausgabe in Azure Storage bleiben bestehen, müssen downstream Vorgänge oder die Clientanwendung den vollständigen Pfad zur Datei in Azure Storage mit Azure Storage SDK herunterladen.

* **Ausgabe**: Wenn Sie Stapelverarbeitungsaufgabe im Azure-Portal navigieren und **auf Knoten**, werden alle Dateien mit der Aufgabe verknüpfte dargestellt, nicht nur die Ausgabedateien interessiert sind. In diesem Fall stehen auf Compute-Knoten nur und die Knoten innerhalb der Retentionszeit Datei Sie für den Vorgang festgelegt haben. Aufgabe Ausgabe anzeigen, die in Azure Storage in das Portal oder eine Anwendung wie [Azure Storage Explorer]beibehalten haben[storage_explorer], muss seine Lage und direkt zu der Datei navigieren.

## <a name="help-for-persisted-output"></a>Hilfe für beibehaltene Ausgabe

Hilfe mehr einfach beizubehalten, Projekt und Aufgabe Team Batch definiert und implementiert eine Reihe von Namenskonventionen sowie eine Klassenbibliothek .NET [Azure Batch Konventionen] [ nuget_package] Library, in der Batch-Anwendung verwendet werden können. Darüber hinaus ist Azure-Portal über diese Benennungskonventionen, damit Sie die Dateien finden, die Sie mithilfe der Bibliothek gespeichert haben.

## <a name="using-the-file-conventions-library"></a>Die Datei Konventionen Bibliothek

[Azure Batch Konventionen] [ nuget_package] ist eine, die Ihren Stapel problemlos speichern und Abrufen von Aufgabenausgaben zu und von Azure-Speicher verwenden können. Es soll im Task und Client - Task Code für persistente Dateien und im Clientcode aufzulisten und abrufen. Aufgaben können auch die Bibliothek für die Ausgaben von übergeordneten Aufgaben wie in einem Szenario mit [Vorgängern und Nachfolgern](batch-task-dependencies.md) abrufen.

Konventionen Bibliothek übernimmt sicherstellen, dass Speichercontainer und Task Ausgabedateien werden gemäß der Konvention benannt, und richtig Wenn Azure-Speicher hochgeladen. Beim Abrufen von Ausgaben finden Sie problemlos den Ausgaben für eine bestimmte Position oder Aufgabe auflisten oder die Ausgaben von ID und Zweck statt, Dateinamen oder besteht im Speicher abrufen.

Beispielsweise können Sie die Bibliothek "alle Zwischendateien für Aufgabe 7" oder "Hol mir die Miniaturansicht für Auftrag *Mymovie"*ohne den Dateinamen oder Speicherort innerhalb Ihres Speicherkontos.

### <a name="get-the-library"></a>Abrufen der Bibliothek

Bibliothek enthält neue Klassen und erweitert die [CloudJob] abrufen[ net_cloudjob] und [CloudTask] [ net_cloudtask] mit neuen Methoden von [NuGet][nuget_package]. Sie können das Visual Studio-Projekt mit [NuGet Bibliothek Paket-Manager]hinzufügen[nuget_manager].

>[AZURE.TIP] Suchen Sie den [Quellcode] [ github_file_conventions] für die Bibliothek Azure Batch Konventionen GitHub im Microsoft Azure SDK für .NET Repository.

## <a name="requirement-linked-storage-account"></a>Anforderung: verknüpftes Konto

Ausgaben in der Dateibibliothek Konventionen mit dauerhaften Speicher speichern und im Azure-Portal anzeigen, müssen Sie Ihr Konto Batch [Link Azure Storage-Konto](batch-application-packages.md#link-a-storage-account) . Wenn nicht bereits verknüpfen Sie ein Speicherkonto mit Ihrem Mitgliedskonto Stapel mithilfe des Azure-Portals:

**Batch-Konto** Blade > **Einstellungen** > **Speicherkonto** > **Konto** (keine) > Wählen Sie ein Speicherkonto in Ihrem Abonnement

Eine ausführliche Anleitung auf ein Speicherkonto Verknüpfen finden Sie unter [Bereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

## <a name="persist-output"></a>Ausgabe beibehalten

Gibt es zwei primäre Aktionen ausführen, wenn Projekt und Ausgabe mit Konventionen Dateibibliothek speichern: den Speichercontainer erstellen und Speichern der Ausgabe in den Container.

>[AZURE.WARNING] Da alle Projekt und Ausgaben im selben Container gespeichert sind, möglicherweise [Speicher Drosselung Grenzwerte](../storage/storage-performance-checklist.md#blobs) erzwungen, wenn eine große Anzahl von Aufgaben gleichzeitig Dateien weiterhin versuchen.

### <a name="create-storage-container"></a>Speichercontainer erstellen

Zunächst Ihre Aufgaben beibehalten Ausgabe Speicher, legen Sie einen Blob-Speichercontainer, sie ihre Ausgabe hochgeladen werden. Hierfür rufen [CloudJob][net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Diese Erweiterungsmethode hat [CloudStorageAccount] [ net_cloudstorageaccount] als Parameter-Objekt und einen Container namens so, dass der Inhalt sichtbar ist der Azure-Portal und der später in diesem Artikel beschriebenen Methoden zum Datenabruf erstellt.

Sie platzieren diesen Code in der Regel in der Clientanwendung - Anwendung, die Pools, Aufträgen und Aufgaben erstellt.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Speichern von Aufgabenausgaben

Da einen Container im BLOB-Speicher vorbereitet haben, Aufgaben sparen Ausgabe zum Container mit [TaskOutputStorage] [ net_taskoutputstorage] Klasse in der Datei Konventionen Bibliothek gefunden.

In der Aufgabencode zunächst [TaskOutputStorage] [ net_taskoutputstorage] -Objekt und der Vorgang seine Arbeit abgeschlossen hat, rufen die [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] -Methode die Ausgabe in Azure-Speicher zu speichern.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

Der Parameter "output Art" kategorisiert beibehaltenen Dateien. Es gibt vier vordefinierte [TaskOutputKind] [ net_taskoutputkind] Typen: "TaskOutput", "TaskPreview", "TaskLog" und "TaskIntermediate." Wenn sie den Workflow nützlich, können Sie auch benutzerdefinierte Arten definieren.

Diese Ausgabetypen können Sie angeben, welche Art von Ausgaben zum Auflisten der beibehaltenen Ausgaben eines bestimmten Vorgangs später Stapel abzufragen. Liste die Ausgaben für einen Vorgang können also eine der Ausgabe die Liste filtern. Beispielsweise "mir die *Vorschau* Ausgabe für Aufgabe *109*." Mehr werden zum Auflisten und Abrufen von Ausgaben in [Abrufen Ausgabe](#retrieve-output) später in diesem Artikel.

>[AZURE.TIP] Welche Ausgabe auch bestimmt, wo der Azure-Portal eine bestimmte Datei angezeigt wird: *TaskOutput*-kategorisierte Dateien in "Task Ausgabedateien" und *TaskLog* Dateien angezeigt in "Task-Protokolle"

### <a name="store-job-outputs"></a>Auftrag lagern gibt

Neben Aufgabenausgaben speichern, können Sie die Ausgaben für einen gesamten Auftrag speichern. In diesem Seriendruck einen Film Rendern Auftrag konnte Sie beispielsweise vollständig gerenderten Film als Auftrag Ausgang beibehalten. Wenn der Auftrag abgeschlossen ist, die Clientanwendung einfach auflisten und die Ausgaben für das Projekt abrufen und muss nicht die einzelnen Vorgänge Abfragen.

Speichern Auftragsausgabe durch Aufrufen von [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] -Methode, und geben Sie [JobOutputKind] [ net_joboutputkind] und Dateiname:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Wie mit TaskOutputKind für Aufgabenausgaben [JobOutputKind] [ net_joboutputkind] Parameter einen Auftrag Kategorisieren der Dateien beibehalten. Dieser Parameter können Sie eine spätere Abfrage für eine bestimmte Art von Ausgabe (Liste). Die JobOutputKind umfasst Ausgabe und Vorschau und Erstellen von benutzerdefinierten Typen unterstützt.

### <a name="store-task-logs"></a>Task-Protokolle speichern

Neben dem Speichern einer Datei in den dauerhaften Speicher nach Abschluss einer Aufgabe oder den Einzelvorgang, unter Umständen Dateien beizubehalten, die während der Ausführung eines Vorgangs - Dateien aktualisiert werden müssen oder `stdout.txt` und `stderr.txt`, beispielsweise. Zu diesem Zweck stellt die Bibliothek Azure Batch Konventionen [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] Methode. Mit [SaveTrackedAsync][net_savetrackedasync], verfolgen Sie Updates in einer Datei auf dem Knoten (in Zeitabständen, die Sie angeben) und die Updates in den Azure-Speicher beibehalten.

Im folgenden Codeausschnitt verwenden wir [SaveTrackedAsync] [ net_savetrackedasync] aktualisieren `stdout.txt` in Azure Storage alle 15 Sekunden während der Ausführung der Aufgabe:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`steht nur für Code, den die Aufgabe normalerweise ausführen würden. Beispielsweise müssen Sie Code, der Daten von Azure Storage and auf Transformation oder Berechnung durchführt. Bestandteil dieser Ausschnitt zeigt, wie dieser Code im umbrochen werden kann ein `using` Block regelmäßig aktualisieren eine Datei mit [SaveTrackedAsync][net_savetrackedasync].

Die `Task.Delay` muss am Ende der `using` Block, um sicherzustellen, dass der Knoten Zeit hat, den Inhalt der Standardausgabe in der Datei stdout.txt auf den Knoten löschen (Knoten-Agent ist ein Programm, das wird auf jedem Knoten im Pool und die Befehl Steuerelement Schnittstelle zwischen dem Knoten und Batch-Dienst). Ohne diese Verzögerung kann die Sekunden Ausgabe verpassen. Diese Verzögerung möglicherweise nicht für alle Dateien.

>[AZURE.NOTE] Beim Aktivieren der Datei mit SaveTrackedAsync werden nur überwachte Datei *angehängt* in Azure-Speicher beibehalten. Verwenden Sie diese Methode nur zum Nachverfolgen von Protokolldateien nicht drehen oder andere Dateien, ausgestattet sind, nur am Ende der Datei hinzugefügt wird, wenn diese aktualisiert wird.

## <a name="retrieve-output"></a>Abrufen der Ausgabe

Beim Abrufen der beibehaltenen Ausgabe mithilfe der Bibliothek Azure Batch Konventionen erfolgt auf eine Aufgabe und Projekt orientierte. Sie können die Ausgabe anfordern, für Aufgabe oder den Einzelvorgang ohne Pfadangabe im BLOB-Speicher oder sogar den Dateinamen kennen. Sie können einfach sagen, "Ausgabedateien für Aufgabe *109*mir."

Der folgende Codeausschnitt durchläuft alle Vorgänge des Projekts, gibt einige Informationen über die Ausgabedateien für die Aufgabe und downloadet die Dateien aus dem Speicher.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Aufgabenausgaben und Azure-portal

Azure-Portal zeigt Aufgabenausgaben und Protokolle, die beibehalten werden, eine verknüpfte Azure Storage-Konto mithilfe von Namenskonventionen in [Azure Batch Datei Konventionen README]gefunden[github_file_conventions_readme]. Diese Konventionen selbst implementieren in einer Sprache Ihrer Wahl und können Sie in Ihren Konventionen Dateibibliothek.

### <a name="enable-portal-display"></a>Portal anzeigen aktivieren

Aktivieren Sie die Anzeige der Ausgaben im Portal müssen folgenden Vorschriften entsprechen:

 1. [Link Azure Storage-Konto](#requirement-linked-storage-account) zu Ihrem Konto Batch.
 2. Halten Sie die vordefinierten Benennungskonventionen für Speichercontainer und Dateien Ausgaben beibehalten. Suchen Sie die Definition dieser Konventionen in der Dateibibliothek Konventionen [README][github_file_conventions_readme]. Verwenden Sie die [Azure Batch Konventionen] [ nuget_package] Bibliothek weiterhin die Ausgabe diese Anforderung erfüllt.

### <a name="view-outputs-in-the-portal"></a>Ausgaben im Portal anzeigen

Um Aufgabenausgaben und Protokolle im Azure-Portal anzuzeigen, navigieren Sie auf die Aufgabe, deren Ausgabe interessiert sind, dann klicken Sie auf **gespeicherte Dateien** oder **Gespeicherte Protokolle**. Dieses Bild zeigt die **gespeicherte Dateien** für den Vorgang mit der ID "007":

![Aufgabe gibt Blade im Azure-portal][2]

## <a name="code-sample"></a>Codebeispiel

[PersistOutputs] [ github_persistoutputs] Beispielprojekt ist eines der [Codebeispiele Azure Batch] [ github_samples] auf GitHub. Visual Studio 2015 Lösung veranschaulicht, wie die Bibliothek Azure Batch Konventionen Aufgabe Ausgabe dauerhaften Speicher beibehalten werden. Gehen folgendermaßen Sie vor um das Beispiel auszuführen:

1. Öffnen Sie das Projekt in **Visual Studio 2015**.
2. Hinzufügen der Stapelverarbeitung und Speicher **Anmeldeinformationen** **AccountSettings.settings** im Microsoft.Azure.Batch.Samples.Common-Projekt.
3. **Erstellen** (aber nicht) die Lösung. Wiederherstellen Sie NuGet-Pakete aufgefordert.
4. Verwenden des Azure-Portals Hochladen eines [Anwendungspakets](batch-application-packages.md) für **PersistOutputsTask**. Enthalten die `PersistOutputsTask.exe` und den abhängigen Assemblys im ZIP-Paket die Anwendung ID "PersistOutputsTask" die Anwendung Paketversion "1.0".
5. **Starten** () **PersistOutputs** Projekt.

## <a name="next-steps"></a>Nächste Schritte

### <a name="application-deployment"></a>Bereitstellung

[Anwendungspakete](batch-application-packages.md) Feature Charge erleichtert die beiden bereitstellen und die Programme, die Ihre Aufgaben ausgeführt compute-Knoten.

### <a name="installing-applications-and-staging-data"></a>Installieren von Anwendun gen und staging-Daten

[Installieren von Clientanwendungen und staging Daten Batch Computeknoten] Auschecken[ forum_post] stellen in Azure Batch Forum eine Übersicht über die verschiedenen Methoden der Knoten für die Ausführung von Aufgaben vorbereiten. Von einem Teammitglieder Azure Batch ist dieser Beitrag eine gute Einführung auf verschiedenen Arten (einschließlich Applikationen und eingegebenen Daten) zu auf Compute-Knoten und einige Besonderheiten für jede Methode.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Gespeicherte Dateien und gespeicherte Protokolle Auswahlen im portal"
[2]: ./media/batch-task-output/task-output-02.png "Aufgabe gibt Blade im Azure-portal"