<properties
    pageTitle="Erste Schritte mit Blob speichern und Visual Studio verbunden Services (ASP.NET 5) | Microsoft Azure"
    description="Einstieg in Azure BLOB-Speicher in einem Visual Studio ASP.NET 5-Projekt nach der Erstellung ein Speicherkonto mit Visual Studio verbunden services"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Erste Schritte mit Azure Blob Storage und Visual Studio verbunden Services (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>Übersicht

Dieser Artikel beschreibt den Einstieg Azure BLOB-Speicher in Visual Studio nach erstellt oder referenziert Azure Speicherkonto in einem ASP.NET 5-Projekt über das Dialogfeld Visual Studio verbunden Dienste hinzufügen.

Azure BLOB-Speicher ist ein Dienst zum Speichern großer Mengen an unstrukturierten Daten von überall in der Welt über HTTP oder HTTPS zugegriffen werden können. Ein einzelnes Blob kann beliebig sein. BLOBs können z. B. Bilder, Audio-und Videodateien, Daten und Dokumente. Dieser Artikel beschreibt die BLOB-Speicher Einstieg nach Azure Speicherkonto erstellen mit Visual Studio **Verbunden Dienste hinzufügen** Dialogfeld in ASP.NET 5-Projekt.

Wie Dateien in Ordnern befinden, live Storage Blobs in Containern. Nachdem Sie einen Speicher erstellt haben, erstellen Sie eine oder mehrere Container im Speicher. Beispielsweise in einem Speicher namens "Album", können Container im Speicher "Bilder", um Bilder zu speichern, und andere Audiodateien speichern "audio" aufgerufen. Nach dem Erstellen der Container können Sie einzelne BLOB-Dateien Sie hochladen. Weitere Informationen zum programmgesteuerten Bearbeiten von Blobs finden Sie unter [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) .

##<a name="access-blob-containers-in-code"></a>Zugriff auf Blob-Container im code

Programmgesteuerten Zugriff auf Blobs in ASP.NET 5-Projekten müssen Sie folgende Elemente hinzufügen, wenn sie nicht bereits vorhanden sind.

1. Fügen Sie Code-Namespacedeklarationen am Anfang einer C#-Datei in der Azure-Speicher programmgesteuert zugreifen möchten.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Abzurufen Sie ein **CloudStorageAccount** -Objekt, Ihre speicherkontoinformationen darstellt. Verwenden Sie den folgenden Code zu dem Ihre Verbindungszeichenfolge Speicher und Speicher-Kontoinformationen von Azure-Dienstkonfiguration.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Hinweis:** Verwenden Sie den obigen Code vor dem Code in den folgenden Abschnitten.


3. Ein **CloudBlobClient** -Objekt in das Speicherkonto **CloudBlobContainer** auf einen vorhandenen Container zu verwenden.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Erstellen Sie einen Container im code

**CloudBlobClient** können einen Container Ihres Speicherkontos erstellen. Müssen Sie lediglich einen Aufruf an **CreateIfNotExistsAsync** wie im folgenden Code hinzufügen:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**Hinweis:** Die APIs, die Aufrufe von Azure-Speicher in ASP.NET 5 ausführen sind asynchron. Weitere Informationen finden Sie unter [asynchrone Programmierung mit Async und Await](http://msdn.microsoft.com/library/hh191443.aspx) . Im folgenden Code wird angenommen, dass asynchrone Programmierung Methoden verwendet werden.

Um für alle Dateien innerhalb des Containers verfügbar machen, können Sie den Container öffentliche werden mithilfe des folgenden Codes festlegen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Ein Container einen Blob hochladen

Hochladen eine BLOB-Datei in einen Container, einen Containerverweis und einen BLOB-Verweis verwenden. Nach dem Einrichten eine Blob Referenz laden jeder Datenstrom, Sie durch Aufrufen der **UploadFromStreamAsync** -Methode. Dieser Vorgang erstellt das Blob nicht bereits vorhanden ist oder es existiert überschreibt. Im folgenden Beispiel wird veranschaulicht, wie einen Blob in einem Container hochladen und geht davon aus, dass der Container bereits erstellt wurde.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container
Um die Blobs in einem Container sind, rufen Sie zunächst einen Containerverweis. Sie können dann **ListBlobsSegmentedAsync** -Methode zum Abrufen von Blobs oder Verzeichnisse innerhalb des Containers aufrufen. Zugriff auf den vielfältigen Eigenschaften und Methoden für eine zurückgegebene **IListBlobItem**muss **CloudBlockBlob**, **CloudPageBlob**oder **CloudBlobDirectory** -Objekt umgewandelt. Wenn Sie BLOB-Typ nicht kennen, können Sie eine Prüfung, festzustellen, dass umgewandelt. Der folgende Code veranschaulicht die abrufen und ausgeben den URI der einzelnen Elemente in einem Container.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);
            }

            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob pageBlob = (CloudPageBlob)item;

                Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);
            }

            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory directory = (CloudBlobDirectory)item;

                Console.WriteLine("Directory: {0}", directory.Uri);
            }
        }
    } while (token != null);

Es gibt andere Methoden zum Auflisten des Inhalts eines Containers Blob. Weitere Informationen finden Sie unter [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) .

##<a name="download-a-blob"></a>Einen Blob herunterladen
Einen Blob herunterladen, rufen Sie zunächst einen Verweis auf das Blob und dann die Methode **DownloadToStreamAsync** aufrufen. Das folgende Beispiel verwendet die Methode **DownloadToStreamAsync** den BLOB-Inhalt in einem Stream-Objekt übertragen, die dann als lokale Datei gespeichert werden kann.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Gibt es andere Weise Blobs als Dateien gespeichert. Weitere Informationen finden Sie unter [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) .

##<a name="delete-a-blob"></a>Löschen eines BLOBs
Um ein Blob zu löschen, rufen Sie zunächst einen Verweis auf das Blob und rufen Sie die Methode **DeleteAsync** auf.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
