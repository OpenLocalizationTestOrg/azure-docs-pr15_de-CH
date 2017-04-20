<properties
    pageTitle="Visual Studio-Vorlagen für Azure | Microsoft Azure"
    description="Erfahren Sie, wie diese Visual Studio-Projektvorlagen können Sie implementieren und rechenintensive Workloads in Azure Batch ausführen"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
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

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio-Projektvorlagen für Azure

Der **Auftrags-Manager** und **Aufgabe Prozessor Visual Studio-Vorlagen** für Stapel stellen Code um implementieren und Ihre rechenintensiven Arbeitslasten auf Stapel mit geringstem Aufwand. Dieses Dokument beschreibt diese Vorlagen und Anleitung für deren Verwendung.

>[AZURE.IMPORTANT] Dieser Artikel beschreibt nur Informationen für diese zwei Vorlagen und setzt voraus, dass Sie Stapel und wichtige Konzepte zu kennen: Pools Knoten, Aufträgen und Aufgaben, Aufgaben-Manager, Umgebungsvariablen und Informationen zu berechnen. Weitere Informationen finden in [Azure Batch Grundlagen](batch-technical-overview.md) [Batch Übersicht über die Features für Entwickler](batch-api-basics.md)und [Erste Schritte mit der Bibliothek Azure Batch für .NET](batch-dotnet-get-started.md).

## <a name="high-level-overview"></a>Übersicht

Die Auftrags-Manager und Prozessor Vorlagen können verwendet werden, erstellen Sie zwei Komponenten:

* Ein Manager Projektaufgabe, die einen auftragssplitter implementiert, der einen Auftrag in mehrere Tasks brechen, die unabhängig voneinander parallel ausgeführt werden können.

* Ein Prozessor, der vorverarbeitet und Nachbearbeitung in einer Befehlszeile verwendet werden kann.

Beispielsweise in einem Film Rendern Szenario aktivieren auftragssplitter ein Auftrags Film in Hunderte oder Tausende von separaten Aufgaben, die einzelne Frames separat verarbeitet. Entsprechend der Prozessor würde die Rendering-Anwendung aufrufen und alle abhängigen Prozesse, die pro render-Rahmen sowie zusätzliche Aktionen (z. B. gerenderten Frame an einen Speicherort kopieren) durchzuführen.

>[AZURE.NOTE] Der Auftrags-Manager und Prozessor ist unabhängig von anderen, damit Sie wählen können, eine oder beide von ihnen, je nach Ihrem Compute und Ihren Vorlieben.

Wie in der folgenden Abbildung gezeigt wird, geht ein Compute-Projekt, das diese Vorlagen verwendet in drei Phasen:

1. Der Client-Code (z. B. Anwendung, Webdienst usw.) führt einen Einzelvorgang Batch-Dienst in Azure als seinen JobManager task Job Manager Programm.

2. Der Stapel Dienst-Manager Projektaufgabe auf Compute-Knoten und Splitter Einzelvorgang startet die angegebene Anzahl von Aufgabe Prozessor Aufgaben wie viele Knoten Bedarf basierend auf den Parametern und Spezifikationen im Auftrag Splitter Code berechnet.

3. Die Aufgabe Prozessor Aufgaben ausgeführt, parallele Verarbeitung der Eingabedaten und Ausgabedaten.

![Diagramm Interaktion Clientcode und Batch-Dienst][diagram01]

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die Batch-Vorlagen verwenden, benötigen Sie Folgendes:

* Ein Computer mit Visual Studio 2015 oder neuer, bereits installiert ist.

* Batch-Vorlagen, die [Visual Studio Gallery] [ vs_gallery] als Visual Studio Extensions. Es gibt zwei Arten der Vorlagen:

  * Installieren Sie die **Erweiterungen und Updates** im Dialogfeld in Visual Studio (Weitere Informationen finden Sie unter [Suchen und mit Visual Studio Extensions][vs_find_use_ext]). Klicken Sie im Dialogfeld **Erweiterungen und Updates** suchen und die folgenden beiden Erweiterungen herunterladen:

    * Azure Batch Job Manager mit Auftragssplitter
    * Azure Batch-Prozessor

  * Vorlagen aus dem Onlinekatalog für Visual Studio: [Microsoft Azure Batch-Projektvorlagen][vs_gallery_templates]

* [Anwendungspakete](batch-application-packages.md) -Funktion verwendet den Auftrags-Manager bereitgestellt und Prozessor zum Stapel compute-Knoten müssen Sie ein Speicherkonto Batch-Konto verknüpfen.

## <a name="preparation"></a>Vorbereitung

Es wird empfohlen, erstellen eine Lösung, die Ihren JobManager sowie der Aufgabenprozessor enthalten kann, da dies zur Freigabe von Code zwischen der Auftrags-Manager und Aufgabe Prozessor Programmen erleichtern. Um diese Lösung zu erstellen, gehen Sie folgendermaßen vor:

1. Öffnen Sie Visual Studio 2015 und wählen **Datei** > **neu** > **Projekt**.

2. Klicken Sie unter **Vorlagen**erweitern Sie **Andere Projekttypen**, **Visual Studio-Projektmappen**und wählen Sie dann **Leere Projektmappe**.

3. Geben Sie einen aussagekräftigen Namen für die Anwendung und den Zweck dieser Lösung (z. B. "LitwareBatchTaskPrograms").

4. Klicken Sie auf **OK**, um die neue Lösung erstellt.

## <a name="job-manager-template"></a>Auftrags-Manager-Vorlage

Die Auftrags-Manager-Vorlage können Sie eine Projektaufgabe Manager implementieren, die die folgenden Aktionen ausführen können:

* Einen Auftrag in mehrere Vorgänge aufteilen.
* Senden Sie Vorgänge auf Batch ausgeführt.

>[AZURE.NOTE] Weitere Informationen zum Aufgaben-Manager finden Sie unter [Batch Übersicht über die Features für Entwickler](batch-api-basics.md#job-manager-task).

### <a name="create-a-job-manager-using-the-template"></a>Erstellen Sie eine Auftrags-Manager mithilfe der Vorlage

Auftrags-Manager der Lösung hinzuzufügen, die Sie zuvor erstellt haben, gehen Sie folgendermaßen vor:

1. Öffnen Sie die vorhandene Projektmappe in Visual Studio 2015.

2. Im Projektmappen-Explorer mit der rechten Maustaste der Projektmappe, klicken Sie auf **Hinzufügen** > **Neues Projekt**.

3. Unter **Visual C#**auf **Cloud**und klicken Sie dann auf **Azure Batch Job Manager mit Auftragssplitter**.

4. Geben Sie einen Namen, der die Anwendung beschreibt und dieses Projekt als Projekt-Manager (z. B. "LitwareJobManager") bezeichnet.

5. Klicken Sie auf **OK**, um das Projekt zu erstellen.

6. Schließlich erstellen Sie das Projekt erzwingen Visual Studio alle referenzierten NuGet-Pakete laden und überprüfen, ob das Projekt vor der Änderung gilt.

### <a name="job-manager-template-files-and-their-purpose"></a>Auftrags-Manager Vorlagendateien und Verwendungszweck

Beim Erstellen eines Projekts mit der Auftrags-Manager-Vorlage erzeugt drei Gruppen von Dateien:

* Die Programm-Datei ("Program.cs"). Der Einstiegspunkt des Programms und Behandlung der obersten Ebene enthält. Normalerweise sollte nicht müssen diese ändern.

* Der Frameworkverzeichnis. Enthält Dateien für die Arbeit "Textbausteine" Job Manager-Programm – Entpacken Parameter hinzufügen von Aufgaben, die Stapelverarbeitung usw. verantwortlich. Sie sollte nicht normalerweise ändern müssen.

* Splitter Auftragsdatei (JobSplitter.cs). Dies ist dort legen Sie eine anwendungsspezifische Logik für einen Auftrag in Aufgaben aufteilen.

Natürlich können Sie zusätzliche Dateien als unterstützen Auftrag Splitter Code, je nach Komplexität des Auftrags Teilen Logik hinzufügen.

Die Vorlage erstellt auch standard-Projektdateien wie einer CSPROJ-Datei app.config, packages.config usw..

Der Rest dieses Abschnitts beschreibt die verschiedenen Dateien und die Codestruktur und erläutert, was jeder Klasse.

![Visual Studio-Projektmappen-Explorer mit der Auftrags-Manager-Vorlage für Projektmappen][solution_explorer01]

**Framework-Dateien**

* `Configuration.cs`: Das Laden des Auftrags Konfigurationsdaten Kontodetails Batch verknüpften Speichergruppe Anmeldeinformationen, Auftrag und Informationen und Parameter kapselt. Darüber hinaus Zugriff auf Stapel definierten Umgebungsvariablen (Siehe umgebungseinstellungen für Vorgänge in der Batch-Dokumentation) über die Configuration.EnvironmentVariable-Klasse.

* `IConfiguration.cs`: Abstrahiert die Implementierung der Klasse Konfiguration können Komponententests der auftragssplitter mit gefälschten oder simulierten Konfigurationsobjekt.

* `JobManager.cs`: Die Komponenten der Programm-Manager Job koordiniert. Er ist zuständig für das Initialisieren des Splitters Auftrag auftragssplitter aufrufen und Versand zurückgegebenen auftragssplitter an dem Einreichenden Aufgabe Aufgaben.

* `JobManagerException.cs`: Stellt einen Fehler, der Auftrags-Manager beenden. JobManagerException wird zum erwarteten' Fehler umschließen, wo diagnostische Informationen als Teil der Beendigung bereitgestellt werden.

* `TaskSubmitter.cs`: Diese Klasse ist für Aufgaben zurückgegebener auftragssplitter der Stapelverarbeitung. JobManager Klasse Aggregate die Abfolge der Aufgaben in Batches auf effiziente aber rechtzeitig den Auftrag ruft dann TaskSubmitter.SubmitTasks auf einem Hintergrund-Thread für jeden Batch.

**Job-Splitter**

`JobSplitter.cs`Diese Klasse enthält anwendungsspezifische Logik für den Auftrag in Aufgaben aufteilen. Das Framework Ruft die JobSplitter.Split-Methode zu eine Abfolge von Aufgaben, die dem Projekt hinzugefügt, die Methode zurück. Dies ist die Klasse, wird die Logik des Projekts einfügen. Implementieren Sie die Split-Methode zum Zurückgeben einer Sequenz von CloudTask Instanzen, in dem Sie den Auftrag partitionieren möchten, Vorgänge darstellen.

**Standard Befehlszeile Project-Dateien**

* `App.config`: Standard Konfigurationsdatei.

* `Packages.config`: Standard NuGet-Paket-Abhängigkeitsdatei.

* `Program.cs`Enthält den Einstiegspunkt des Programms und der obersten Ebene Ausnahmebehandlung.

### <a name="implementing-the-job-splitter"></a>Implementieren von Job-splitter

Beim Öffnen des Auftrags-Manager-Vorlagenprojekts müssen das Projekt JobSplitter.cs Datei standardmäßig geöffnet. Split-Logik für die Aufgaben realisieren Ihrer Arbeit mit den Split() Methode unten:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] Im Abschnitt kommentierte die `Split()` ist der einzige Teil des Auftrags-Manager Vorlagencode, der durch Hinzufügen von Logik ändern Ihre Aufträge in Aufgaben unterteilt soll. Wenn Sie einen anderen Abschnitt der Vorlage ändern möchten, überprüfen Sie mit der Stapelverarbeitung Funktionsweise vertraut sind und wenige [Stapel Codebeispiele][github_samples].

Die Implementierung Split() hat Zugriff auf:

* Die Auftragsparameter über die `_parameters` Feld.
* Das CloudJob-Objekt, das Projekt über die `_job` Feld.
* Das CloudTask Objekt, Projektaufgabe Manager über die `_jobManagerTask` Feld.

Die `Split()` Umsetzung braucht Aufgaben direkt dem Projekt hinzuzufügen. Stattdessen Code sollte eine Sequenz von CloudTask-Objekten zurück, und diese werden dem Projekt automatisch hinzugefügt durch die Framework-Klassen, die den auftragssplitter aufrufen. Häufig wird mit C# Iterator (`yield return`) Funktion Auftrag Bereichen wie diese Aufgaben so bald wie möglich anstatt warten auf alle Vorgänge berechnet werden kann.

**Auftragsfehler splitter**

Wenn der auftragssplitter ein Fehler auftritt, sollten sie:

* Beenden die Sequenz mit C# `yield break` Anweisung in dem Fall der Auftrags-Manager als erfolgreich behandelt werden; oder

* Eine Ausnahme, in der der Auftrags-Manager behandelt wird, als Fehler und kann je nach Client Konfiguration wurde wiederholt).

In beiden Fällen werden Aufgaben bereits zurückgegebener auftragssplitter und die Stapelverarbeitung hinzugefügt werden. Wenn Sie dies möchten, man dann:

* Beendet den Auftrag vor der Rückgabe von Job-splitter

* Formulieren Sie die gesamte Auflistung der vor der Rückgabe (zurückgegeben eine `ICollection<CloudTask>` oder `IList<CloudTask>` anstatt Ihr auftragssplitter mit C#-Iterator)

* Verwenden Sie verzögert alle Aufgaben abhängig von den erfolgreichen Abschluss des Auftrags-manager

**Auftrags-Manager Wiederholungsversuche**

Fällt der Auftrags-Manager kann vom Batch-Dienst je nach Clienteinstellungen wiederholen wiederholt werden. Im Allgemeinen ist dies sicher, da das Framework Aufgaben dem Projekt hinzugefügt, er Aufgaben ignoriert, die bereits vorhanden. Aber wenn Berechnung Aufgaben teuer, können nicht Sie die Kosten der Aufgaben, die dem Projekt hinzugefügt wurden neu; umgekehrt Wenn erneut ausführen nicht unbedingt die gleiche Aufgabe IDs wird dann das Verhalten "Duplikate ignorieren" nicht eintreten. In diesen Fällen sollten Sie entwerfen Ihre Arbeit Splitter erkennen die Arbeit, die bereits abgeschlossen und nicht wiederholen, z. B. durch einen CloudJob.ListTasks vor zu Aufgaben ausführen.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Beendigungscodes und Ausnahmen in der Auftrags-Manager-Vorlage

Beendigungscodes und Ausnahmen bieten einen Mechanismus, um das Ergebnis der Ausführung des Programms zu ermitteln, und sie können um Probleme mit der Ausführung des Programms zu identifizieren. Die Auftrags-Manager-Vorlage implementiert die Exitcodes und Ausnahmen in diesem Abschnitt beschrieben.

Eine Projektaufgabe Manager, die mit der Auftrags-Manager implementiert kann drei möglichen Rückgabecodes:

| Code | Beschreibung |
|------|-------------|
| 0    | Der Auftrags-Manager wurde erfolgreich abgeschlossen. Auftrag Splitter Code vollständig ausgeführt, und alle dem Projekt hinzugefügt wurden. |
| 1    | Task Manager Auftrag Fehler mit einer Ausnahme in einem "erwarteten" das Programm. Die Ausnahme wurde ein JobManagerException mit Diagnoseinformationen übersetzt und gegebenenfalls Vorschläge zur Behebung des Fehlers. |
| 2    | Die Projektaufgabe Manager Fehler mit Ausnahme "Unerwartetes". Die Ausnahme an die Standardausgabe protokolliert wurde, aber der Auftrags-Manager konnte keine zusätzliche Diagnose oder Behebung Informationen. |

Bei Job Manager Task einige Aufgaben möglicherweise noch mit dem Dienst hinzugefügt wurden vor des Fehlers auftreten. Diese Aufgaben werden normal ausgeführt. Erläuterung dieser Codepfad finden Sie unter "Job Splitter Failure" über.

Alle Ausnahmen zurückgegebene Informationen werden in stdout.txt und stderr.txt-Dateien geschrieben. Weitere Informationen finden Sie unter [Fehlerbehandlung](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Clientinformationen

Dieser Abschnitt beschreibt einige Clientanforderungen Implementierung einen auf dieser Vorlage basierenden Auftrags-Manager aufrufen. Details zum Übergeben von Parametern und umgebungseinstellungen finden Sie unter [Parameter und Umgebungsvariablen im Clientcode übergeben](#pass-environment-settings) .

**Obligatorische Anmeldeinformationen**

Azure Stapelverarbeitung Aufgaben hinzu, erfordert die Projektaufgabe Manager Ihre Azure Batch Konto URL und Schlüssel. Sie müssen diese Umgebungsvariablen mit dem Namen YOUR_BATCH_URL und YOUR_BATCH_KEY übergeben. Sie können diese im Job Manager Task Umgebung eingestellt. In C# Client:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Speicherung von Anmeldeinformationen**

Normalerweise muss der Client nicht verknüpfte Speicher Kontoanmeldeinformationen zur Projektaufgabe Manager bereitstellen, weil (a) die Projekt-Manager nicht explizit auf das Speicherkonto verknüpfte zugreifen müssen und (b) das Speicherkonto verknüpfte wird häufig auf alle Aufgaben als umgebungseinstellung für den Auftrag. Wenn Sie nicht verknüpfte Speicherkonto allgemeine Umgebung Einstellungen, und der Auftrags-Manager Zugriff auf verknüpfte Speicher benötigt, sollten Sie Anmeldeinformationen verknüpfte Speicher wie folgt angeben:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Task-Manager-Einstellungen**

Der Client sollte Job Manager *KillJobOnCompletion* -Flag auf **false**festgelegt.

Es ist gewöhnlich sicherer Client *RunExclusive* auf **false**festgelegt.

Der Client sollte *ResourceFiles* oder *ApplicationPackageReferences* -Auflistung verwenden, um den Job Manager ausführbare (und die erforderlichen DLLs) auf den Compute-Knoten bereitgestellt.

Standardmäßig wird der Auftrags-Manager nicht wiederholt schlägt. Je nach der Auftragslogik Manager Client eventuell Versuche über *Einschränkungen*aktivieren/*MaxTaskRetryCount*.

**Einstellungen**

Job-Splitter mit Abhängigkeiten ausgibt, muss der Client den Auftrag UsesTaskDependencies auf True festgelegt.

Im Auftrag Splitter-Modell ist für Kunden Aufträge über was Job Splitter erstellt Aufgaben hinzufügen möchten. Der Client sollte daher normalerweise den Auftrag *OnAllTasksComplete* auf **Terminatejob**festgelegt.

## <a name="task-processor-template"></a>Prozessor Aufgabenvorlage

Eine Prozessor-Vorlage können Sie einen Aufgabenprozessor implementieren, der die folgenden Aktionen ausführen können:

* Richten Sie die Angaben von einzelnen Stapelverarbeitungsaufgaben ausgeführt.
* Führen Sie alle Aktionen von jeder Stapelverarbeitungsaufgabe.
* Speichern Sie Aufgabenausgaben dauerhaft.

Zwar ein Aufgabenprozessor auszuführenden Aufgaben im Batch nicht erforderlich ist, ist der wichtigste Vorteil einen Aufgabenprozessor Wrapper implementieren alle Ausführung Aktionen an einem Ort bereit. Wenn beispielsweise mehrere Programme im Kontext der einzelnen Aufgaben ausgeführt werden müssen oder wenn Sie kopieren Daten dauerhaft nach Abschluss der jeweiligen Aufgabe.

Die Aktionen der Prozessor kann als einfach oder komplex, und viele oder wenige gemäß der Arbeitslast. Darüber hinaus implementieren alle Aktionen in einem Prozessor, können Sie leicht aktualisieren oder hinzufügen Änderungen an Applikationen oder Arbeitslast Aktionen beruhen. Jedoch in einigen Fällen ein Aufgabenprozessor die optimale Lösung für Ihre Implementierung möglicherweise nicht hinzufügen unnötigen Komplexität, beispielsweise beim Ausführen Aufträge schnell über eine einfache Befehlszeile gestartet werden können.

### <a name="create-a-task-processor-using-the-template"></a>Erstellen Sie ein Prozessor mit der Vorlage

Um einen Aufgabenprozessor zur Projektmappe hinzufügen, die Sie zuvor erstellt haben, gehen Sie folgendermaßen vor:

1. Öffnen Sie die vorhandene Projektmappe in Visual Studio 2015.

2. Im Projektmappen-Explorer mit der rechten Maustaste der Lösung, klicken Sie auf **Hinzufügen**und klicken Sie dann auf **Neues Projekt**.

3. Unter **Visual C#**auf **Cloud**und klicken Sie dann auf **Azure Batch-Prozessor**.

4. Geben Sie einen Namen, der die Anwendung beschreibt und identifiziert dabei als der Prozessor (z. B. "LitwareTaskProcessor").

5. Klicken Sie auf **OK**, um das Projekt zu erstellen.

6. Schließlich erstellen Sie das Projekt erzwingen Visual Studio alle referenzierten NuGet-Pakete laden und überprüfen, ob das Projekt vor der Änderung gilt.

### <a name="task-processor-template-files-and-their-purpose"></a>Prozessor Vorlagendateien und deren Zweck

Beim Erstellen eines Projekts mit Prozessor Aufgabenvorlage erstellt drei Gruppen von Dateien:

* Die Programm-Datei ("Program.cs"). Der Einstiegspunkt des Programms und Behandlung der obersten Ebene enthält. Normalerweise sollte nicht müssen diese ändern.

* Der Frameworkverzeichnis. Enthält Dateien für die Arbeit "Textbausteine" Job Manager-Programm – Entpacken Parameter hinzufügen von Aufgaben, die Stapelverarbeitung usw. verantwortlich. Sie sollte nicht normalerweise ändern müssen.

* Der Prozessor Aufgabendatei (TaskProcessor.cs). Dies ist dort legen Sie die anwendungsspezifische Logik zum Ausführen einer Aufgabe (normalerweise durch Aufrufen um eine vorhandene ausführbare Datei). Vor- und Nachbearbeitung Code wie zusätzliche Daten herunterladen oder Hochladen von Ergebnisdateien gilt auch hier.

Natürlich können Sie als Aufgabe Prozessor Code, je nach Komplexität des Auftrags Teilen Logik unterstützen zusätzliche Dateien hinzufügen.

Die Vorlage erstellt auch standard-Projektdateien wie einer CSPROJ-Datei app.config, packages.config usw..

Der Rest dieses Abschnitts beschreibt die verschiedenen Dateien und die Codestruktur und erläutert, was jeder Klasse.

![Visual Studio-Projektmappen-Explorer mit der Prozessor-Vorlage für Projektmappen][solution_explorer02]

**Framework-Dateien**

* `Configuration.cs`: Das Laden des Auftrags Konfigurationsdaten Kontodetails Batch verknüpften Speichergruppe Anmeldeinformationen, Auftrag und Informationen und Parameter kapselt. Darüber hinaus Zugriff auf Stapel definierten Umgebungsvariablen (Siehe umgebungseinstellungen für Vorgänge in der Batch-Dokumentation) über die Configuration.EnvironmentVariable-Klasse.

* `IConfiguration.cs`: Abstrahiert die Implementierung der Klasse Konfiguration können Komponententests der auftragssplitter mit gefälschten oder simulierten Konfigurationsobjekt.

* `TaskProcessorException.cs`: Stellt einen Fehler, der Auftrags-Manager beenden. TaskProcessorException wird zum erwarteten' Fehler umschließen, wo diagnostische Informationen als Teil der Beendigung bereitgestellt werden.

**Prozessor**

* `TaskProcessor.cs`: Führt den Task. Das Framework Ruft die Methode TaskProcessor.Run. Dies ist die Klasse, wird die anwendungsspezifische Logik der Aufgabe einfügen. Die Run-Methode zu implementieren:
  * Analysieren und alle Aufgabenparameter überprüfen
  * Verfassen Sie die Befehlszeile für externe Programme, die Sie aufrufen möchten
  * Melden Sie sich für das Debuggen benötigten Diagnoseinformationen
  * Starten Sie über die Befehlszeile
  * Warten Sie, bis des Prozess beendet
  * Erfassen Sie den Exitcode des Prozesses bestimmen, ob es erfolgreich war oder fehlgeschlagen ist
  * Speichern Sie alle Ausgabedateien möchten Sie dauerhaft

**Standard Befehlszeile Project-Dateien**

* `App.config`: Standard Konfigurationsdatei.
* `Packages.config`: Standard NuGet-Paket-Abhängigkeitsdatei.
* `Program.cs`Enthält den Einstiegspunkt des Programms und der obersten Ebene Ausnahmebehandlung.

## <a name="implementing-the-task-processor"></a>Implementieren des Prozessors

Beim Öffnen des Aufgabenprozessor Vorlagenprojekts müssen das Projekt TaskProcessor.cs Datei standardmäßig geöffnet. Sie können ausführen Implementieren der Logik für Aufgaben Ihrer Arbeit mit Run()-Methode unten:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] Der kommentierte Abschnitt Run()-Methode ist nur Teil Aufgabenprozessor Vorlagencode, der durch Hinzufügen zur Logik für die Aufgaben Ihrer Arbeit ändern soll. Wenn Sie einen anderen Abschnitt der Vorlage ändern möchten, bitte zuerst machen Sie sich mit der Funktionsweise der Stapelverarbeitung Batch Dokumentation überprüfen und wenige Stapel Codebeispiele auszuprobieren.

Run()-Methode obliegt der Befehlszeile starten eines oder mehrerer Prozesse auf allen Prozess abgeschlossen ist, starten die Ergebnisse speichern und schließlich mit Exitcode zurückgeben. Run()-Methode ist, die Verarbeitungslogik für Ihre Aufgaben implementieren. Aufgabe Prozessor Framework ruft Run()-Methode für Sie. Sie müssen nicht selbst aufrufen.

Die Implementierung Run() hat Zugriff auf:

* Die Aufgabenparameter über die `_parameters` Feld.
* Die Ids Projekt und über die `_jobId` und `_taskId` Felder.
* Taskkonfiguration über die `_configuration` Feld.

**Taskfehler**

Bei einem Ausfall beenden Run()-Methode durch Auslösen einer Ausnahme, aber dadurch des obersten Ebene ausnahmehandlers Kontrolle über den Code der Aufgabe beenden. Möchten Sie den Exitcode steuern, damit Sie verschiedene Fehlertypen beispielsweise für Diagnosezwecke unterscheiden können, oder einige Fehlermodi sollte den Auftrag beenden und andere nicht, sollten Sie Run()-Methode verlassen, durch einen Exitcode ungleich NULL zurückgeben. Dies wird den Exitcode Aufgabe.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Beendigungscodes und Ausnahmen in der Prozessor-Vorlage

Beendigungscodes und Ausnahmen bieten einen Mechanismus, um das Ergebnis der Ausführung des Programms zu ermitteln und sie können dabei helfen, Probleme mit der Ausführung des Programms zu identifizieren. Die Aufgabenprozessor Vorlage implementiert die Exitcodes und in diesem Abschnitt beschriebenen Ausnahmen.

Eine Prozessor-Aufgabe, die mit der Prozessor implementiert kann drei möglichen Rückgabecodes:

| Code | Beschreibung |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | Der Prozessor wurde vollständig ausgeführt. Hinweis dies nicht bedeutet, dass das aufgerufene Programm nur erfolgreich war, dass der Prozessor erfolgreich aufgerufen und Nachbearbeitung ohne Ausnahmen ausgeführt. Die Bedeutung des Exitcodes hängt das aufgerufene Programm – normalerweise Exitcode 0 bedeutet das Programm erfolgreich war und andere Exitcode Programm fehlgeschlagen. |
| 1    | Der Prozessor Fehler mit einer Ausnahme in einem "erwarteten" Teil des Programms. Die Ausnahme übersetzt war ein `TaskProcessorException` mit Diagnoseinformationen und gegebenenfalls Vorschläge zur Behebung des Fehlers. |
| 2    | Der Prozessor ist mit Ausnahme "Unerwartetes" fehlgeschlagen. Die Ausnahme an die Standardausgabe protokolliert wurde, aber der Prozessor konnte zusätzliche Diagnose oder Behebung Informationen. |

>[AZURE.NOTE] Wenn das Programm, das Sie aufrufen an bestimmten Fehlermodi Beendigungscodes 1 und 2 verwendet, ist Beendigungscodes 1 und 2 für Aufgabe Prozessorfehler mehrdeutig. Sie können diese Aufgabe Prozessor Fehlercodes auf besondere Beendigungscodes ändern Ausnahmefälle in der Datei Program.cs.

Alle Ausnahmen zurückgegebene Informationen werden in stdout.txt und stderr.txt-Dateien geschrieben. Weitere Informationen finden Sie in der Dokumentation zu Batch Error Handling.

### <a name="client-considerations"></a>Clientinformationen

**Speicherung von Anmeldeinformationen**

Nutzt der Aufgabenprozessor Azure BLOB-Speicher weiterhin Ausgaben, z. B. mit der Datei Konventionen Hilfsbibliothek ist dann Zugriff auf *entweder* die Cloud Storage Konto Anmeldeinformationen *oder* eine BLOB-Container URL mit SAS (SAS). Die Vorlage umfasst Unterstützung für Anmeldeinformationen über gemeinsame Umgebungsvariablen. Der Client kann Speicher Anmeldeinformationen übergeben:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Das Speicherkonto steht in der TaskProcessor über die `_configuration.StorageAccount` Eigenschaft.

Wenn eine URL Container mit SAS verwenden möchten, Prozessor Aufgabenvorlage enthält derzeit keine integrierte Unterstützung Sie können dies auch über Auftrag üblich Umgebung übergeben

**Remotespeicher-setup**

Es wird empfohlen, Task Manager Client oder Auftrag erforderlichen Aufgaben vor dem Hinzufügen von Aufgaben zum Auftrag Container erstellen. Dies ist bei Verwendung eine URL Container mit SAS so eine URL keine Berechtigung zum Erstellen des Containers umfasst erforderlich. Es empfiehlt sich auch übergeben von Anmeldeinformationen speichern als speichert jede Aufgabe im Container CloudBlobContainer.CreateIfNotExistsAsync aufzurufen.

## <a name="pass-parameters-and-environment-variables"></a>Übergeben Sie Parameter und Umgebungsvariablen

### <a name="pass-environment-settings"></a>Umgebungseinstellungen übergeben

Ein Client kann an Projektaufgabe in Form von Umgebung Settings Manager übergeben. Diese Informationen vom Manager Projektaufgabe lässt dann beim Aufgabe Prozessor Aufgaben generieren, die in der Compute-Auftrag ausgeführt wird. Informationen, die Sie als Standardeinstellungen Umgebung übergeben werden:

* Und speicherkontoschlüssel
* Batch-Konto URL
* Batch-kontoschlüssel

Der Batch-Dienst hat einen einfachen Mechanismus übergeben umgebungseinstellungen für eine Projektaufgabe Manager mithilfe der `EnvironmentSettings` Eigenschaft im [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Beispiel zu den `BatchClient` Instanz für Batch-Konto können Sie als Umgebungsvariablen aus der Client-URL und freigegebenen Schlüssel Anmeldeinformationen für das Konto Batch code übergeben. Ebenso können um das Speicherkonto zuzugreifen, das Batch-Konto verknüpft ist, Sie Storage-Kontonamen und Konto Speicherschlüssel als Umgebungsvariablen übergeben.

### <a name="pass-parameters-to-the-job-manager-template"></a>Der Auftrags-Manager-Vorlage Parameter übergeben

In vielen Fällen ist es für den Auftrag Task Manager, Teilen Prozess Auftrag steuern oder dafür konfigurieren pro-Parameter übergeben. Hierzu eine JSON-Datei namens parameters.json als Ressourcendatei für die Projektaufgabe Manager hochladen. Die Parameter werden dann für die `JobSplitter._parameters` Feld Auftrags-Manager-Vorlage.

>[AZURE.NOTE] Der integrierte Parameter Handler unterstützt nur Zeichenfolge-Wörterbücher. Wenn Sie komplexe JSON-Werte als Parameterwerte übergeben möchten, müssen als Zeichenfolgen übergeben und Splitter Projekt analysieren oder Ändern der Rahmen `Configuration.GetJobParameters` Methode.

### <a name="pass-parameters-to-the-task-processor-template"></a>Übergeben von Parametern an die Vorlage Prozessor

Sie können einzelne Aufgaben mithilfe der Vorlage Prozessor implementiert auch Parameter übergeben. Wie sucht mit Stellenvorlage Manager Prozessor Aufgabenvorlage Ressourcendatei

Parameters.JSON und wenn es gefunden als das Wörterbuch lädt. Es gibt verschiedene Optionen zum Task Prozessor Aufgaben Parameter übergeben:

* Verwenden Sie die Auftragsparameter JSON. Dies funktioniert gut, wenn nur Parameter Auftrag Wide (z. B. eine Render-Höhe und Breite) sind. Hierzu müssen Sie beim Erstellen einer CloudTask Job Splitter hinzufügen einen Verweis auf das Objekt parameters.json Resource File aus Manager Projektaufgabe des ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) der CloudTask ResourceFiles-Auflistung.

* Generieren und als Teil des Splitters Auftragsausführungsergebnisse aufgabenspezifischen parameters.json Dokument hochladen und dieses Blob in die Aufgabe Resource Files-Auflistung verweisen. Dies ist erforderlich, wenn unterschiedliche Aufgaben verschiedene Parameter. Ein Beispiel hierfür wäre ein Szenario-3D-Rendering, Frame-Index der Aufgabe als Parameter übergeben wird.

>[AZURE.NOTE] Der integrierte Parameter Handler unterstützt nur Zeichenfolge-Wörterbücher. Wenn Sie komplexe JSON-Werte als Parameterwerte übergeben möchten, müssen diese Zeichenfolgen übergeben und in der Prozessor analysieren oder Ändern des Frameworks `Configuration.GetTaskParameters` Methode.

## <a name="next-steps"></a>Nächste Schritte

### <a name="persist-job-and-task-output-to-azure-storage"></a>Projekt und Ausgabe in Azure-Speicher beibehalten

Ein weiteres hilfreiches Werkzeug in Batch-Lösungsentwicklung ist [Azure Batch Konventionen][nuget_package]. Verwenden Sie diese Klassenbibliothek .NET (in Vorschau) in Ihren Stapel problemlos speichern und Abrufen von Aufgabenausgaben zu und von Azure-Speicher. [Azure-Stapelverarbeitung beibehalten und Aufgabe Ausgabe](batch-task-output.md) enthält eine vollständige Erörterung der Bibliothek und ihre Verwendung.

### <a name="batch-forum"></a>Batch-Forum

[Azure Batch Forum] [ forum] auf MSDN eignet sich Batch und Fragen zu dem Dienst. Gehen Sie für hilfreiche Beiträge "dauerhaft", und stellen Sie Ihre Fragen, wie sie beim Erstellen von Batch-Projektmappen auftreten.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
