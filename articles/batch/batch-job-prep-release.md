<properties
    pageTitle="Vorbereitung und Bereinigung in Batch Job | Microsoft Azure"
    description="Verwenden Sie Einzelvorgangsebene Vorbereitungsaufgaben Datentransfer Batch Azure Compute-Knoten zu, und lassen Sie Aufgaben für Knoten Bereinigung bei Beendigung des Auftrags."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Zur Vorbereitung und Durchführung Projektaufgaben Blattnamen Azure compute-Knoten

 Eine Azure-Stapelverarbeitung erforderlich häufig Setup seine Aufgaben ausgeführt und Wartung Abschluss die Aufgaben nach der Arbeit. Sie müssen gemeinsame Aufgabe Eingabedaten auf Compute-Knoten, oder nach Auftragsabschluss Ausgabedaten in Azure-Speicher hochgeladen. **Vorbereitung** und **Freigeben** Aufgaben können Sie diese Operationen ausführen.

## <a name="what-are-job-preparation-and-release-tasks"></a>Welche Vorbereitung und Aufgaben?

Vor dem Ausführen der Aufgaben führt Projektaufgabe Vorbereitung auf alle Computeknoten mindestens eine Aufgabe ausgeführt werden soll. Wenn der Auftrag abgeschlossen ist, wird Version Projektaufgabe auf jedem Knoten im Pool, der mindestens einen Vorgang ausgeführt. Wie bei normalen Stapelverarbeitungsaufgaben können eine Befehlszeile aufgerufen, wenn eine Projektaufgabe Zubereitung oder Version ausgeführt wird.

Aufgaben zur Vorbereitung und Veröffentlichung bieten vertraute Batch Aufgabe Dateidownload ([Ressourcendateien][net_job_prep_resourcefiles]), Ausführung, benutzerdefinierten Umgebungsvariablen maximale Dauer, Anzahl und Retentionszeit Datei erhöht.

In den folgenden Abschnitten erfahren Sie, wie [JobPreparationTask] [ net_job_prep] und [JobReleaseTask] [ net_job_release] Klassen gefunden in [Batch.NET] [ api_net] Bibliothek.

> [AZURE.TIP] Aufgaben zur Vorbereitung und Release sind besonders hilfreich in "shared Pool" Umgebung, in der Compute-Knoten bleibt zwischen ausgeführt und von vielen Aufträgen verwendet.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Wann Vorbereitung und Aufgaben freigeben

Vorbereitung und Veröffentlichung Projektaufgaben eignen sich für folgenden Situationen:

**Allgemeine Daten herunterladen**

Stapelverarbeitungen erfordern häufig einen gemeinsamen Satz von Daten als Eingabe für die Aufgaben. Beispielsweise ist täglich Risiken Analyse berechnet Daten bestimmte noch gelten für alle Aufgaben im Auftrag. Diese Marktdaten häufig mehrere GB groß, sollten für jeden Computeknoten einmal herunterladen, damit jede Aufgabe, die auf dem Knoten verwenden kann. Verwenden Sie eine **Projektaufgabe Vorbereitung** zum Herunterladen dieser Daten für jeden Knoten vor die Ausführung des Auftrags andere Aufgaben.

**Projekt und Ausgabe löschen**

In einer "shared Pool"-Umgebung, ein Pool Compute-Knoten nicht zwischen außer Betrieb gesetzt sind, müssen Sie Daten zwischen löschen. Möglicherweise müssen sparen Speicherplatz auf den Knoten oder Sicherheitsrichtlinien Ihrer Organisation erfüllt. Verwenden einer **Projektaufgabe Version** zum Löschen von Daten, die von einer Aufgabe zur Vorbereitung heruntergeladen oder während der Ausführung der Aufgabe generiert wurde.

**Protokoll-Aufbewahrung**

Möglicherweise möchten eine Kopie der Protokolldateien, die Ihre Aufgaben generieren oder vielleicht crash Dumpdateien fehlgeschlagene Anwendung generiert werden können. Verwenden einer **Projektaufgabe Version** in solchen Fällen zu komprimieren und diese Daten in eine [Azure-Speicher] [ azure_storage] Konto.

>[AZURE.TIP] Anders weiterhin Protokolle und andere Projekt und Ausgabe [Azure Batch Konventionen](batch-task-output.md) Bibliothek verwenden soll.

## <a name="job-preparation-task"></a>Vorbereitung der Projektaufgabe

Vor der Ausführung des Auftrags Aufgaben führt Batch Projektaufgabe Vorbereitung auf jeder Compute-Knoten, die zum Ausführen eines Tasks geplant ist. Standardmäßig wartet der Batch-Dienst für die Projektaufgabe Vorbereitung abgeschlossen werden vor dem Ausführen von Vorgängen auf dem Knoten ausgeführt. Allerdings können Sie den Dienst nicht konfigurieren. Wenn des Knotens Neustart Berichtsvorbereitungstask Auftrag erneut ausgeführt, aber Sie können dieses Verhalten deaktivieren.

Vorbereitung Projektaufgabe auf Knoten ausgeführt, die zum Ausführen eines Tasks geplant werden. Dies verhindert unnötigen Ausführung einer Aufgabe zur Vorbereitung für den Fall, dass ein Knoten nicht auf eine Aufgabe zugewiesen wird. Dies kann auftreten, wenn die Anzahl der Aufgaben für ein Projekt kleiner als die Anzahl der Knoten in einem Pool ist. Sie gilt auch bei [gleichzeitiger Taskausführung](batch-parallel-node-tasks.md) , die einige Knoten im Leerlauf bleibt aktiviert ist, ist die Anzahl der Aufgaben unter den insgesamt möglichen gleichzeitigen Aufgaben. Ausführen nicht die Projektaufgabe Vorbereitung auf Knoten im Leerlauf, verbringen Sie weniger Geld auf Daten übertragen.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] unterscheidet sich vom [CloudPool.StartTask] [ pool_starttask] JobPreparationTask am Anfang jeder Aufgabe ausführt, während StartTask nur bei Compute-Knoten führt zunächst verbindet einen Pool oder neu gestartet.

## <a name="job-release-task"></a>Release-Aufgabe

Sobald ein Auftrag als abgeschlossen gekennzeichnet ist, wird die Projektaufgabe Version auf jedem Knoten im Pool ausgeführt, die mindestens eine Aufgabe ausgeführt. Ein Projekt wird als eine Terminate Anforderung abgeschlossen markieren. Der Batch-Dienst dann wird der Jobstatus auf *beendet*mit dem Auftrag assoziierten aktiven oder laufenden Aufgaben beendet und führt die Projektaufgabe Version. Der Auftrag wird der Status *abgeschlossen* .

> [AZURE.NOTE] Auftrag löschen führt auch die Projektaufgabe Version. Jedoch wenn ein Projekt bereits beendet wurde, ist Task freigeben kein zweites Mal ausgeführt, wenn der Auftrag später gelöscht wird.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Auftrag vorbereiten und Aufgaben mit Batch lassen

Weisen Sie zum Verwenden einer Zubereitung Aufgabe [JobPreparationTask] [ net_job_prep] Objekt des Auftrags [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] Eigenschaft. Entsprechend initialisiert [JobReleaseTask] [ net_job_release] und Zuweisen des Auftrags [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] -Eigenschaft Projektaufgabe Version festgelegt.

In diesem Codeausschnitt `myBatchClient` eine Instanz von [BatchClient][net_batch_client], und `myPool` wird ein vorhandenen Pool in Batch-Konto.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Wie bereits erwähnt, wird Task freigeben ausgeführt, wenn ein Projekt beendet oder gelöscht wird. Beenden ein Auftrags mit [JobOperations.TerminateJobAsync][net_job_terminate]. Löschen eines Auftrags mit [JobOperations.DeleteJobAsync][net_job_delete]. In der Regel beenden oder Löschen eines Auftrags, wenn seine Aufgaben abgeschlossen sind oder die von Ihnen definierten Zeitlimit erreicht wurde.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Beispiel für GitHub

Vorbereitung und Aufgaben in Aktion freigeben, Auschecken [JobPrepRelease] [ job_prep_release_sample] -Beispielprojekt auf GitHub. Diese Konsolenanwendung führt Folgendes aus:

1. Erstellt einen Pool mit zwei Knoten "klein".
2. Erstellt ein Projekt mit Vorbereitung, Veröffentlichung und Standardkataloge.
3. Führt die Projektaufgabe Zubereitung, die zuerst die Knoten-ID in eine Textdatei in einem Knoten "shared" Verzeichnis schreibt.
4. Führt eine Aufgabe auf allen Knoten, die die Aufgaben-ID in derselben Textdatei schreibt.
5. Nachdem alle Aufgaben abgeschlossen sind oder das Zeitlimit erreicht ist, druckt den Inhalt der Textdatei in der Konsole des Knotens.
6. Wenn der Auftrag abgeschlossen ist, führt die Projektaufgabe Version zum Löschen der Datei vom Knoten.
6. Druckt die Exitcodes der Aufgaben zur Vorbereitung und Veröffentlichung für jeden Knoten, auf dem sie ausgeführt.
7. Hält die Ausführung Bestätigung des Auftrags oder Pool löschen können.

Ausgabe der beispielanwendung ähnelt der folgenden:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Aufgrund der Variablen erstellen und Starten der Knoten in einem neuen Pool (einige Knoten sind für andere Aufgaben bereit), sehen Sie möglicherweise unterschiedliche Ausgaben. Insbesondere weil schnell Aufgaben möglicherweise einen Knoten für den Pool alle die Aufgaben ausgeführt. In diesem Fall werden Sie feststellen, dass der Auftrag vorbereiten und Release-Aufgaben für die Knoten, die keine Aufgaben ausgeführt.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Prüfen Sie Vorbereitung und Aufgaben in Azure-Portal Version

Beim Ausführen der beispielanwendung können [Azure-Portal] [ portal] zeigen Sie die Eigenschaften des Auftrags und seine Aufgaben oder sogar herunterladen freigegebenen Textdatei, die von den Aufgaben geändert.

Das Bildschirmabbild unten zeigt die **Vorbereitung Aufgaben Blade** im Azure-Portal nach einer Beispiel-Anwendung. *JobPrepReleaseSampleJob* Eigenschaften nach Abschluss die Aufgaben (aber vor dem Löschen Ihrer Arbeit und Pool) navigieren Sie und auf **Vorbereitung** oder **Release Aufgaben** um ihre Eigenschaften anzuzeigen.

![Vorbereitung der Auftragseigenschaften in Azure-portal][1]

## <a name="next-steps"></a>Nächste Schritte

### <a name="application-packages"></a>Anwendungspakete

Neben Projektaufgabe Vorbereitung auch können [Anwendungspakete](batch-application-packages.md) Feature des Stapels Sie Compute-Knoten für die Ausführung der Aufgabe vorbereiten. Diese Funktion ist besonders nützlich für die Bereitstellung von Applikationen, die keine Installationsprogramme, Programme, die viele (100) Dateien oder Applikationen, die strenge Versionskontrolle benötigen ausgeführt.

### <a name="installing-applications-and-staging-data"></a>Installieren von Anwendun gen und staging-Daten

Diese MSDN-Forumsbeitrag Übersicht über verschiedene Methoden zur Vorbereitung der Knoten zum Ausführen von Aufgaben:

[Installieren von Anwendun gen und staging-Daten auf Batch compute-Knoten][forum_post]

Von einem Teammitglieder Azure Batch geschrieben werden verschiedene Techniken, mit denen Sie Programme und Daten zum Berechnen von Knoten bereitstellen.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
