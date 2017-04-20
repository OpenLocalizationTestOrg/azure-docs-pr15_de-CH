<properties
    pageTitle="Lernprogramm - Erste Schritte mit der Bibliothek Azure Batch .NET | Microsoft Azure"
    description="Lernen Sie die grundlegenden Konzepte von Azure Batch und für den Batch-Dienst mit einem Beispielszenario entwickeln."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Erste Schritte mit Azure Batch-Bibliothek für .NET

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Grundlagen der [Azure Batch] [ azure_batch] und [Batch.NET] [ net_api] Bibliothek in diesem Artikel wie eine C#-Anwendung für Beispiel Schritt für Schritt erläutert. Wir werden wie diese Anwendung den Stapelverarbeitung Service zur Verarbeitung einer Arbeitslast parallelen in der Cloud sowie nutzt wie Interaktion mit [Azure Storage](../storage/storage-introduction.md) Datei Staging und abrufen. Batch Anwendung Workflow Techniken lernen. Sie Basis verstehen die Hauptkomponenten der Batch Jobs, Aufgaben, Pools und compute-Knoten.

![Batch-Lösung Workflow (Basic)][11]<br/>

## <a name="prerequisites"></a>Erforderliche Komponenten

Es wird vorausgesetzt, dass Kenntnisse in C# und Visual Studio verfügen. Es wird davon ausgegangen, dass Sie Konto erstellen Anforderungen, die für Azure und den Stapel und Speicher unten angegeben werden können.

### <a name="accounts"></a>Konten

- **Ein Azure-Konto**: haben Sie bereits ein Azure-Abonnement [erstellen ein kostenloses Azure-Konto][azure_free_account].
- **Batch-Konto**: haben Sie ein Azure-Abonnement [Azure Batch registrieren](batch-account-create-portal.md).
- **Konto**: Siehe [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) in [von Azure-Speicherkonten](../storage/storage-create-storage-account.md).

> [AZURE.IMPORTANT] Batch unterstützt derzeit *nur* Speichertyp Konto **Allgemeine** beschriebenen Schritt #5 in [von Azure-Speicherkonten](../storage/storage-create-storage-account.md) [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) .

### <a name="visual-studio"></a>Visual Studio

Sie müssen **Visual Studio 2015** des Beispielprojekts. Finden Sie in der [Übersicht über Visual Studio 2015 Produkte]freie und Testversion Versionen von Visual Studio[visual_studio].

### <a name="dotnettutorial-code-sample"></a>*DotNetTutorial* Beispiel

[DotNetTutorial] [ github_dotnettutorial] Beispiel ist vielen Codebeispielen in [Azure Batch Beispiele] gefunden[ github_samples] GitHub-Repository. Sie können auf die Schaltfläche **Download ZIP** auf Repository-Homepage oder indem [Azure Batch-Beispiele master.zip] im Beispiel[ github_samples_zip] direkten Download-Link. Nachdem Sie den Inhalt der ZIP-Datei extrahiert haben, finden Sie die Lösung in den folgenden Ordner:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure Batch Explorer (optional)

[Azure Batch Explorer] [ github_batchexplorer] ist ein kostenloses Hilfsprogramm, das in [Azure-Batch-Beispiele] enthalten[ github_samples] GitHub-Repository. Zum Bearbeiten dieses Lernprogramms nicht erforderlich, kann es hilfreich sein, beim Entwickeln und Debuggen von Batch-Projektmappen.

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial Beispiel Projektübersicht

Im *DotNetTutorial* Beispiel ist eine Visual Studio 2015-Lösung, die besteht aus zwei Projekten: **DotNetTutorial** und **TaskApplication**.

- **DotNetTutorial** ist die Client-Anwendung interagiert mit dem Stapel und Speicher Parallele Aufgaben ausführen compute-Knoten (virtuelle Maschinen). DotNetTutorial wird auf der lokalen Arbeitsstation ausgeführt.

- **TaskApplication** ist das Programm, das Computeknoten in Azure die tatsächliche Arbeit ausgeführt wird. Im Beispiel `TaskApplication.exe` analysiert der Text in eine Datei aus dem Azure-Speicher (Eingabedatei) heruntergeladen. Dann erzeugt eine Textdatei (Ausgabedatei) mit einer Liste der oberen drei Wörter, die in der Eingabedatei angezeigt. Nach Erstellung die Ausgabedatei hochgeladen TaskApplication die Datei auf Azure-Speicher. Dadurch die Clientanwendung zum Download zur Verfügung. TaskApplication läuft parallel auf mehreren Computeknoten im Batch-Dienst.

Das folgende Diagramm veranschaulicht den primären ausgeführte Vorgänge, die Client-Anwendung *DotNetTutorial*und die Anwendung von Aufgaben *TaskApplication*ausgeführt wird. Dieser grundlegende Workflow ist typisch für viele Compute-Lösungen, die mit erstellt werden. Während sie nicht jede Funktion in der Batch-Dienst zeigen, sind fast jeder Batch Szenario ähnliche Prozesse.

![Batch Beispielworkflow][8]<br/>

[**Schritt 1.**](#step-1-create-storage-containers) Erstellen Sie **Container** in Azure BLOB-Speicher.<br/>
[**Schritt 2.**](#step-2-upload-task-application-and-data-files) Hochladen Sie Anwendungsdateien und Dateien mit Containern.<br/>
[**Schritt 3.**](#step-3-create-batch-pool) Erstellen Sie Batch- **pool**<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Pool **StartTask** downloads die binäre Dateien (TaskApplication) Knoten, wie sie das Pool.<br/>
[**Schritt 4.**](#step-4-create-batch-job) Erstellen eines Batch **Job**.<br/>
[**Schritt 5.**](#step-5-add-tasks-to-job) Das Projekt **Vorgänge** hinzufügen.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Die Aufgaben sollen Knoten ausführen.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5 b.** Jede Aufgabe seiner Eingabedaten aus dem Azure-Speicher herunter und startet die Ausführung.<br/>
[**Schritt 6.**](#step-6-monitor-tasks) Überwachen Sie Aufgaben.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Aufgaben abgeschlossen sind, laden sie die Ausgabedaten in Azure-Speicher.<br/>
[**Schritt 7.**](#step-7-download-task-output) Taskausgabe vom Speicher herunterladen

Wie bereits erwähnt, nicht jeder Batch-Lösung führt diese Schritte und enthält möglicherweise viele, aber die *DotNetTutorial* Probe Anwendung veranschaulicht häufige Vorgänge in einem Batch-Lösung.

## <a name="build-the-dotnettutorial-sample-project"></a>Erstellen Sie das Beispielprojekt *DotNetTutorial*

Bevor Sie das Beispiel erfolgreich ausführen können, geben Sie Anmeldeinformationen Stapel und Speicher im *DotNetTutorial* Projekt `Program.cs` Datei. Wenn dies noch nicht getan haben, öffnen Sie die Projektmappe in Visual Studio durch Doppelklicken auf die `DotNetTutorial.sln` Projektmappendatei. Oder öffnen Sie sie in Visual Studio mithilfe der **Datei > Öffnen > Projekt/Projektmappe** Menü.

Open `Program.cs` *DotNetTutorial* Projekt. Fügen Sie Ihre Anmeldeinformationen im oberen Bereich der Datei angegeben:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Wie bereits erwähnt, müssen Sie die Anmeldeinformationen für ein Speicherkonto **Allgemeine** derzeit in Azure-Speicher angeben. Die Batch-Anwendung verwenden BLOB-Speicher innerhalb des **allgemeinen** Speicherkontos. Geben Sie die Anmeldeinformationen für ein Speicherkonto Auswählen der Kontenart *BLOB-Speicher* erstellt wurde.

Finden Sie Ihre Anmeldeinformationen Stapel und Speicher in das Konto Blade jedes Dienstes in [Azure-Portal][azure_portal]:

![Batch-Anmeldeinformationen im Portal][9]
![Speicher Anmeldeinformationen im Portal][10]<br/>

Nun Aktualisierung des Projekts mit der Maustaste der Projektmappe im Projektmappen-Explorer und klicken Sie auf **Projektmappe erstellen**. Bestätigen Sie Wiederherstellung von NuGet-Pakete aufgefordert werden.

> [AZURE.TIP] NuGet-Pakete nicht automatisch wiederhergestellt werden oder wenn Fehler zu einem Fehler der Pakete wiederherstellen, sicherzustellen, dass Sie [NuGet Paket-Manager] [ nuget_packagemgr] installiert. Aktivieren Sie den Download Packages fehlen. Finden Sie unter [Aktivieren von Paket Wiederherstellen während Build] [ nuget_restore] Download-Pakets aktivieren.

In den folgenden Abschnitten wir brechen Sample Application in die Schritte, die zur Verarbeitung einer Arbeitslast im Batch-Dienst ausführt und diese Schritte im Detail erläutern. Wir empfehlen Ihnen, finden Sie in der geöffneten Projektmappe in Visual Studio während der Arbeit Ihren Weg durch den Rest dieses Artikels, da nicht jede Codezeile im Beispiel erläutert wird.

Navigieren Sie zum Ende der `MainAsync` Methode des *DotNetTutorial* -Projekts `Program.cs` Datei mit Schritt 1 beginnen. Jeder Schritt unten dann etwa folgt den Fortschritt von Methodenaufrufen in `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Schritt 1: Speicher-Container erstellen

![Container in Azure-Speicher erstellen][1]
<br/>

Stapel enthält integrierten Unterstützung für die Interaktion mit Azure-Speicher. Container in das Speicherkonto stellen in Ihrem Konto Stapel ausgeführte Aufgaben erforderlichen Dateien. Der Container stellen auch die Ausgabedaten speichern, die die Aufgaben erstellen. Erstes *DotNetTutorial* Clientanwendung führt ist drei in [Azure BLOB-Speicher](../storage/storage-introduction.md)erstellen:

- **Anwendung**: Container speichert die Anwendung Aufgaben sowie einer Abhängigkeit, wie DLLs führen.
- **Eingabe**: Aufgaben downloadet Dateien aus der *Eingabe* verarbeitet.
- **Ausgabe**: Aufgaben Eingabedatei verarbeiten sie lädt die Ergebnisse dem *Ausgabe* -Container.

Um ein Speicherkonto interagieren und Container erstellen, verwenden wir [Azure Storage-Clientbibliothek für .NET][net_api_storage]. Wir erstellen einen Verweis auf das [CloudStorageAccount][net_cloudstorageaccount], und von der [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Wir verwenden die `blobClient` in der gesamten Anwendung verweisen und mehrere Methoden als Parameter übergeben. Ein Beispiel hierfür ist im Codeblock, der unmittelbar folgt, rufen wir die `CreateContainerIfNotExistAsync` der Container erstellen.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Nach die Containern erstellt wurden, kann die Anwendung jetzt Dateien hochladen, die von Aufgaben verwendet werden.

> [AZURE.TIP] [Wie BLOB-Speicher von .NET](../storage/storage-dotnet-how-to-use-blobs.md) bietet eine gute Übersicht über Azure Storage Container mit Blobs arbeiten. Es sollten oben Leseliste mit Batch arbeiten.

## <a name="step-2-upload-task-application-and-data-files"></a>Schritt 2: Hochladen Sie Anwendung und Dateien

![Upload Aufgabe Anwendung und Container den Eingabedateien (Daten)][2]
<br/>

In den Dateiupload definiert *DotNetTutorial* zuerst Sammlungen von **Anwendung** und **input** Dateipfade auf dem lokalen Computer vorhanden. Dann lädt es diese Dateien auf die Container, die Sie im vorherigen Schritt erstellt haben.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Es gibt zwei in `Program.cs` , der Ladevorgang beteiligt sind:

- `UploadFilesToContainerAsync`: Diese Methode gibt eine Auflistung von [ResourceFile] [ net_resourcefile] Objekte (siehe unten) und intern `UploadFileToContainerAsync` jeder hochladen *Dateipfade* Parameter übergeben wird.
- `UploadFileToContainerAsync`: Dies ist die Methode, der Datei-Upload führt [ResourceFile] erstellt[ net_resourcefile] Objekte. Nach dem Hochladen der Datei, ruft eine SAS (SAS) für die Datei und gibt ein ResourceFile-Objekt zurück, das es darstellt. Gemeinsamer Zugriff, Signaturen auch weiter unten beschrieben werden.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

[ResourceFile] [ net_resourcefile] enthält Aufgaben im Stapel mit URL zu einer Datei in Azure-Speicher, der einem Compute-Knoten vor diesem Task ausgeführt wird. [ResourceFile.BlobSource] [ net_resourcefile_blobsource] -Eigenschaft gibt die vollständige URL der Datei im Azure-Speicher vorhanden. Die URL kann auch SAS (SAS) enthalten den sicheren Zugriff auf die Datei. Die meisten Aufgaben in Batch .NET gehören eine Eigenschaft *ResourceFiles* einschließlich:

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

Die DotNetTutorial Probe Anwendung Vorgangsarten JobPreparationTask oder JobReleaseTask, jedoch nicht mehr über sie [Ausführen Vorbereitung und Durchführung Aufgaben in Azure Batch compute-Knoten](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>SAS (SAS)

Gemeinsamer Zugriff Signaturen sind Zeichenfolgen die – Wenn als Teil einer URL – bieten sicheren Zugriff auf Container und Blobs in Azure-Speicher. Blob und Container freigegebene Anwendung DotNetTutorial Signatur URLs zugreifen und veranschaulicht, wie diese gemeinsamen Zugriff Signaturzeichenfolgen von Storage Service erhalten.

- **BLOB shared Access Signaturen**: den Pool StartTask in DotNetTutorial verwendet Blob shared Access Signaturen beim Downloaden die Anwendungsbinärdateien und Daten-Dateien aus dem Speicher (siehe Schritt 3 weiter unten). Die `UploadFileToContainerAsync` -Methode in der DotNetTutorial `Program.cs` enthält Code, der jedes Blob SAS erhält. Dies geschieht durch Aufrufen von [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Container shared Access Signaturen**: er seine Ausgabedatei wie jede Aufgabe seine Arbeit auf Compute-Knoten beendet, *Ausgabe* Container in Azure-Speicher hochgeladen. Hierzu verwendet TaskApplication eine Container-SAS, die Schreibzugriff auf den Container als Teil des Pfades bereitstellt, wenn die Datei hochgeladen. Abrufen von Container-SAS erfolgt ähnlich wie beim Abrufen von Blob SAS freigegeben. In DotNetTutorial, finden Sie, die `GetContainerSasUrl` Hilfsmethode aufruft, [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] zu tun. Lesen Sie mehr darüber, wie TaskApplication Container SAS in freigegebenen "Schritt 6: Überwachen von Aufgaben."

> [AZURE.TIP] Auschecken der zweiteiligen Reihe SAS, [Teil 1: Grundlegendes zur gemeinsamen Zugriff Signatur (SAS) Modell](../storage/storage-dotnet-shared-access-signature-part-1.md) und [Teil2: Erstellen und Verwenden von SAS (SAS) mit BLOB-Speicher](../storage/storage-dotnet-shared-access-signature-part-2.md), sicheren Zugriff auf Daten in das Speicherkonto erfahren.

## <a name="step-3-create-batch-pool"></a>Schritt 3: Batch-Pool erstellen

![Erstellen eines Batch-Pools][3]
<br/>

Batch- **Pool** ist eine Sammlung von Compute-Knoten (virtuelle Maschinen) auf denen Stapel eine Aufgaben ausführt.

Nachdem er das Speicherkonto Anwendung und Dateien hochgeladen, startet *DotNetTutorial* seiner Interaktion mit der Batch-Dienst mithilfe der Stapelverarbeitung .NET Bibliothek. Hierzu [BatchClient] [ net_batchclient] erstellt:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Anschließend wird ein Pool von Compute-Knoten in Batch-Konto mit einem Aufruf erstellt `CreatePoolAsync`. `CreatePoolAsync`verwendet die [BatchClient.PoolOperations.CreatePool] [ net_pool_create] Methode tatsächlich einen Pool in der Batch-Dienst erstellen.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Bei der Erstellung eines Pools mit [CreatePool][net_pool_create], geben Sie mehrere Parameter wie die Anzahl der Compute-Knoten, die [Größe der Knoten](../cloud-services/cloud-services-sizes-specs.md)und den Knoten Betriebssystem. In *DotNetTutorial*verwenden wir [CloudServiceConfiguration] [ net_cloudserviceconfiguration] an Windows Server 2012 R2 von [Cloud-Diensten](../cloud-services/cloud-services-guestos-update-matrix.md). Durch Angabe einer [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] hingegen können Pools erstellt Marketplace Bilder beinhaltet sowohl Windows als auch Linux- [Bereitstellung Linux Computeknoten in Azure Batch Pools](batch-linux-nodes.md) Weitere Informationen finden Sie.

> [AZURE.IMPORTANT] Sie Zahlen für Serverressourcen im Stapel. Minimieren Sie Kosten senken Sie `targetDedicated` 1, bevor Sie das Beispiel ausführen.

Mit diesen Eigenschaften physischen Knoten kann auch ein [StartTask] angegeben[ net_pool_starttask] für den Pool. Der StartTask führt auf jedem Knoten dieses Knotens verknüpft Pool und jedem Knoten neu gestartet. Der StartTask eignet sich besonders für die Installation auf Compute-Knoten vor der Ausführung von Aufgaben. Beispielsweise wenn Ihre Aufgaben Daten verarbeiten mithilfe von Python-Skripts können einen StartTask Sie Python auf den Compute-Knoten installieren.

In dieser Anwendung der StartTask kopiert die Dateien aus dem Speicher heruntergeladen (die werden mithilfe der [StartTask]angegeben[net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] Eigenschaft) aus dem Arbeitsverzeichnis StartTask auf das freigegebene Verzeichnis *Alle* Vorgänge auf dem Knoten zugreifen können. Im Wesentlichen kopiert `TaskApplication.exe` und abhängigen auf das freigegebene Verzeichnis auf jedem Knoten wie der Knoten den Pool verknüpft, damit alle Tasks ausgeführt, die auf dem Knoten darauf zugreifen können.

> [AZURE.TIP] **Anwendungspakete** Feature von Azure Batch bietet eine weitere Möglichkeit, die Anwendung auf den Compute-Knoten in einem Pool. Einzelheiten finden Sie unter [Bereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md) .

Bemerkenswert im Codeausschnitt ist auch die Verwendung von zwei Umgebungsvariablen in der *Befehlszeile* -Eigenschaft die StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` und `%AZ_BATCH_NODE_SHARED_DIR%`. Jeder Compute-Knoten in einem Batch-Pool wird automatisch mit mehreren Variablen, die Stapel sind konfiguriert. Alle Prozesse, die durch eine Aufgabe ausgeführt hat Zugriff auf Umgebungsvariablen.

> [AZURE.TIP] Erfahren Sie mehr über die Umgebung Computeknoten Batch Pool und Informationen zum Vorgang Arbeitsverzeichnisse verfügbaren Variablen finden Sie unter Abschnitt [umgebungseinstellungen für Aufgaben](batch-api-basics.md#environment-settings-for-tasks) und [Dateien und Verzeichnisse](batch-api-basics.md#files-and-directories) im [Batch Übersicht über die Features für Entwickler](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Schritt 4: Stapelverarbeitungsauftrag erstellen

![Stapelverarbeitungsauftrag erstellen][4]<br/>

Ein **Auftrag** ist eine Sammlung von Aufgaben und einen Pool von Compute-Knoten zugeordnet ist. Die Aufgaben eines Projekts ausführen auf Compute-Knoten zugeordneten Pool.

Sie können ein Projekt organisieren und Nachverfolgen von Aufgaben in verwandte Arbeitslasten, sondern verwaltungsmäßigen bestimmte – wie die maximale Laufzeit für den Auftrag (und seine Aufgaben) sowie Priorität gegenüber anderen Aufträgen in Batch-Konto verwenden. In diesem Beispiel ist die Aufgabe jedoch nur in Schritt #3 erstellte Pool zugeordnet. Es sind keine zusätzlichen Eigenschaften konfiguriert.

Alle Stapelverarbeitungsaufträge werden einem bestimmten Pool zugeordnet. Diese Zuordnung gibt an, welche Knoten auf die Aufgaben ausgeführt werden. Sie geben mit [CloudJob.PoolInformation] [ net_job_poolinfo] -Eigenschaft, wie im folgenden Codeausschnitt gezeigt.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Ein Auftrag erstellt, werden Aufgaben verrichten hinzugefügt.

## <a name="step-5-add-tasks-to-job"></a>Schritt 5: Hinzufügen von Aufgaben zum Projekt

![Aufgaben zum Auftrag hinzufügen][5]<br/>
*(1) Aufgaben werden dem Projekt hinzugefügt und (2) die Aufgaben auf Knoten ausführen soll (3) die Aufgaben die Datendateien zu downloaden*

Batchverarbeitung von **Aufgaben** werden die einzelnen Arbeitseinheiten, die Compute-Knoten ausgeführt. Eine Aufgabe ist eine Befehlszeile und führt die Skripts oder ausführbare Dateien, die Sie in die Befehlszeile angeben.

Um tatsächlich Aufgaben müssen ein Auftrag Aufgaben hinzugefügt werden. Jedes [CloudTask] [ net_task] mit einem Befehlszeileneigenschaft und [ResourceFiles] konfiguriert[ net_task_resourcefiles] (wie dem Pool StartTask), die die Aufgabe downloadet zum Knoten vor die Befehlszeile automatisch ausgeführt wird. Im Beispielprojekt *DotNetTutorial* verarbeitet jeden Vorgang nur eine Datei. So enthält die ResourceFiles-Auflistung ein einzelnes Element.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Wenn sie Zugriff auf Umgebungsvariablen wie `%AZ_BATCH_NODE_SHARED_DIR%` oder eine Anwendung in des Knotens nicht gefunden `PATH`, Befehlszeilen mit Präfix `cmd /c`. Dies explizit den Befehlsinterpreter ausgeführt und weisen nach Durchführung des Befehls beendet. Dies ist unnötig, wenn Ihre Aufgaben einer Anwendung in des Knotens ausführen `PATH` (wie *robocopy.exe* oder *powershell.exe*) und keine Umgebungsvariablen verwendet werden.

In der `foreach` Schleife im Codeausschnitt, Sie sehen, dass die Befehlszeile für den Task erstellt wurde, drei Befehlszeilenargumente an *TaskApplication.exe*übergeben werden:

1. Das **erste Argument** ist der Pfad der zu verarbeitenden Datei. Der lokale Pfad zu der Datei ist auf dem Knoten vorhanden Wenn die ResourceFile Objekt `UploadFileToContainerAsync` wurde, der Dateiname (als Parameter an den Konstruktor ResourceFile) für diese Eigenschaft verwendet wurde. Dies bedeutet, dass die Datei in das Verzeichnis *TaskApplication.exe*finden.

2. Das **zweite Argument** gibt an, dass die Top *N* Wörter in die Ausgabedatei geschrieben werden soll. Im Beispiel ist dieses hartcodiert, damit die obersten drei Wörter in die Ausgabedatei geschrieben werden.

3. Das **dritte Argument** ist SAS (SAS), die Schreibzugriff auf den Container **Ausgabe** in Azure Storage bietet. *TaskApplication.exe* verwendet diese gemeinsamen Zugriff Signatur URL, wenn sie die Ausgabedatei in Azure-Speicher hochgeladen. Suchen Sie den Code dafür in den `UploadFileToContainer` Methode des TaskApplication-Projekts `Program.cs` Datei:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Schritt 6: Überwachen von Aufgaben

![Überwachen von Aufgaben][6]<br/>
*Die Clientanwendung (1) überwacht Aufgaben zur Beendigung und Erfolgsmeldung und (2) die Aufgaben Daten in Azure-Speicher hochladen*

Wenn Aufgaben mit einem Projekt hinzugefügt werden, automatisch in die Warteschlange gestellt und auf Compute-Knoten mit dem Auftrag assoziierten Pool für die Ausführung geplant. Basierend auf der angegebenen behandelt Batch alle Warteschlangen Tasks planen, Wiederholung und Abgaben Verwaltung Aufgabe für Sie. Es gibt viele Ansätze für die Überwachung der Ausführung der Aufgabe. DotNetTutorial ist ein einfaches Beispiel, das nur Erfolg oder Misserfolg Abschluss und Task Status meldet.

In der `MonitorTasks` -Methode in der DotNetTutorial `Program.cs`, gibt es drei Stapel .NET Konzepte, die Diskussion zu rechtfertigen. Sie sind in der Reihenfolge ihrer Darstellung unten aufgeführt:

1. **ODATADetailLevel**: Angabe [ODATADetailLevel] [ net_odatadetaillevel] in Liste (z. B. eine Liste der Aufgaben erhalten) Batch Anwendung Leistung wichtig ist. Fügen Sie [Azure Batch-Dienst effizient Abfragen](batch-efficient-list-queries.md) Leseliste auf jede Art von Status in Batch Anwendung soll hinzu.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] bietet Batch .NET Applications mit Hilfsprogramme für die Überwachung von Task-Status. In `MonitorTasks`, *DotNetTutorial* wartet, bis alle zu [TaskState.Completed] [ net_taskstate] Frist. Anschließend wird den Auftrag beendet.

3. **TerminateJobAsync**: Beenden eines Auftrags mit [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (oder blockierenden JobOperations.TerminateJob) diese Aufgabe als erledigt markiert. Unbedingt dazu Batch-Lösung nutzt eine [JobReleaseTask][net_jobreltask]. Dies ist eine besondere Art von Vorgang in [Vorbereitung und Durchführung Aufgaben](batch-job-prep-release.md)beschrieben.

Die `MonitorTasks` aus *DotNetTutorial* `Program.cs` wird unten angezeigt:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Schritt 7: Aufgabe Ausgabe herunterladen

![Speicher Aufgabe Ausgabe herunterladen][7]<br/>

Jetzt, dass der Auftrag abgeschlossen ist, kann die Ausgabe von Aufgaben Azure-Speicher heruntergeladen. Dies erfolgt mit einem Aufruf von `DownloadBlobsFromContainerAsync` in *DotNetTutorial* `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] Der Aufruf von `DownloadBlobsFromContainerAsync` *DotNetTutorial* Anwendung legt fest, dass die Dateien gedownloadet werden sollen die `%TEMP%` Ordner. Diese Ausgabespeicherort ändern können.

## <a name="step-8-delete-containers"></a>Schritt 8: Delete Container

Da Sie für Daten, die in Azure-Speicher befindet berechnet, empfiehlt es immer eine Blobs zu entfernen, die für die Stapelverarbeitungsaufträge nicht mehr benötigt werden. In der DotNetTutorial `Program.cs`, dazu drei Aufrufe an die Hilfsmethode `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Die Methode selbst nur einen Verweis auf den Container, und dann [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Schritt 9: Löschen Sie den Auftrag und pool

Im letzten Schritt wird der Benutzer aufgefordert, den Auftrag und DotNetTutorial Anwendung erstellte Pool löschen. Obwohl Sie nicht für Projekte und Aufgaben, die *Sie* berechnet Compute-Knoten berechnet. Daher wird empfohlen, nur nach Bedarf Knoten zuordnen. Löschen nicht verwendeter Pools kann Teil der Wartung.

Die BatchClient [JobOperations] [ net_joboperations] und [PoolOperations] [ net_pooloperations] beide verfügen über entsprechende Löschmethoden aufgerufen werden, wenn der Benutzer den Löschvorgang bestätigt:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Denken Sie daran, die Compute-Ressourcen geladen werden, Löschen nicht verwendeter Pools Kosten minimieren. Beachten Sie auch, dass einen Pool Löschen aller Computeknoten in diesem Pool und werden alle Daten auf dem Knoten nicht behebbarer gelöschten Pool.

## <a name="run-the-dotnettutorial-sample"></a>Führen Sie das *DotNetTutorial* -Beispiel

Beim Ausführen der beispielanwendung werden ähnelt der folgenden die Konsolenausgabe. Erleben Sie während der Ausführung eine Pause `Awaiting task completion, timeout in 00:30:00...` während des Pools Compute-Knoten gestartet werden. Verwenden der [Azure-Portal] [ azure_portal] zum Überwachen des Pools zu berechnen Knoten, Auftrag und Aufgaben während und nach der Ausführung. Verwenden der [Azure-Portal] [ azure_portal] oder den [Azure-Speicher-Explorer] [ storage_explorers] die Speicherressourcen (Container und Blobs) anzeigen, die von der Anwendung erstellt werden.

Normale dauert Ausführung **ca. 5 Minuten** beim Ausführen der Anwendungdes in der Standardkonfiguration.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Nächste Schritte

*DotNetTutorial* und *TaskApplication* mit unterschiedlichen berechnen Szenarien ändern können. Versuchen Sie beispielsweise, Hinzufügen einer Verzögerung Ausführung *TaskApplication*wie mit [Thread.Sleep][net_thread_sleep]simulieren zeitintensiven Aufgaben und im Portal überwachen. Versuchen Sie, weitere Aufgaben hinzufügen oder die Anzahl von Compute-Knoten. Fügen Sie Logik überprüfen und das Verwendung eines vorhandenen Geschwindigkeit Ausführungszeit (*Hinweis*: Auschecken `ArticleHelpers.cs` in [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] Projekt in [Azure-Batch-Beispiele][github_samples]).

Damit Sie grundlegenden Arbeitsablauf von Batch-Lösung kennen, ist es Zeit, auf zusätzliche Features von Batch-Dienst zu erhalten.

- Lesen Sie [Batch Übersicht über die Features für Entwickler](batch-api-basics.md), die für alle neuen Stapel Benutzer empfohlen.
- Starten auf anderen Stapel Entwicklung Artikel **Entwicklung detaillierte** [Batch Lernpfad][batch_learning_path].
- Auschecken einer anderen Implementierung verarbeitet "Top N Wörter" Arbeitslast mit Batch in [TopNWords] [ github_topnwords] Beispiel.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Container in Azure-Speicher erstellen"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Upload Aufgabe Anwendung und Container den Eingabedateien (Daten)"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Batch-Pool erstellen"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Stapelverarbeitungsauftrag erstellen"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Aufgaben zum Auftrag hinzufügen"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Überwachen von Aufgaben"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Speicher Aufgabe Ausgabe herunterladen"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Batch-Lösung Workflow (vollständiges Diagramm)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Batch-Anmeldeinformationen im Portal"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Speicherung von Anmeldeinformationen im Portal"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Batch-Lösung Workflow (minimale Diagramm)"
