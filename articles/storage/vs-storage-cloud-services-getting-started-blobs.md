<properties
    pageTitle="Erste Schritte mit Blob speichern und Visual Studio verbunden Dienste (Wolke) | Microsoft Azure"
    description="Einstieg in Azure BLOB-Speicher in einem Cloud Service-Projekt in Visual Studio nach dem Anschließen an ein Speicherkonto mit Visual Studio verbunden services"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Erste Schritte mit Azure BLOB-Speicher und Visual Studio verbunden Services (Cloud services Projekte)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie Einstieg mit Azure BLOB-Speicher erstellt oder Azure Storage-Konto über das Dialogfeld Visual Studio **Verbunden Dienste hinzufügen** in einem Visual Studio Cloud Services-Projekt verwiesen. Wir zeigen Ihnen, wie zu BLOB-Container erstellen und wie häufig vorkommende Aufgaben auflisten, hochladen und herunterladen Blobs. Die Beispiele sind in C# geschrieben\# und [Microsoft Azure Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure BLOB-Speicher ist ein Dienst zum Speichern großer Mengen an unstrukturierten Daten von überall in der Welt über HTTP oder HTTPS zugegriffen werden können. Ein einzelnes Blob kann beliebig sein. BLOBs können z. B. Bilder, Audio-und Videodateien, Daten und Dokumente.

Wie Dateien in Ordnern befinden, live Storage Blobs in Containern. Nachdem Sie einen Speicher erstellt haben, erstellen Sie eine oder mehrere Container im Speicher. Beispielsweise in einem Speicher namens "Album", können Container im Speicher "Bilder", um Bilder zu speichern, und andere Audiodateien speichern "audio" aufgerufen. Nach dem Erstellen der Container können Sie einzelne BLOB-Dateien Sie hochladen.

- Informationen zum programmgesteuerten Bearbeiten von Blobs finden Sie unter [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md).
- Allgemeine Informationen zum Azure-Speicher finden Sie unter [Dokumentation](https://azure.microsoft.com/documentation/services/storage/).
- Allgemeine Informationen zu Azure Cloud Services finden Sie in der [Dokumentation Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/).
- Weitere Informationen über ASP.NET Applications programming anzeigen Sie [ASP.NET](http://www.asp.net)

## <a name="access-blob-containers-in-code"></a>Zugriff auf Blob-Container im code

Programmgesteuerten Zugriff auf Blobs in Cloud Projekte müssen Sie folgende Elemente hinzufügen, wenn sie nicht bereits vorhanden sind.

1. Fügen Sie Code-Namespacedeklarationen an den Anfang einer C#-Datei in der Azure-Speicher programmgesteuert zugreifen möchten.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Abzurufen Sie ein **CloudStorageAccount** -Objekt, Ihre speicherkontoinformationen darstellt. Verwenden Sie den folgenden Code zu dem Ihre Verbindungszeichenfolge Speicher und Speicher-Kontoinformationen von Azure-Dienstkonfiguration.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Ein **CloudBlobClient** -Objekt auf einen vorhandenen Container das Speicherkonto zu erhalten.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Rufen Sie ein **CloudBlobContainer** -Objekt auf ein bestimmtes BLOB-Container.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Verwenden Sie den Code im vorherigen Verfahren vor den Code in den folgenden Abschnitten.

## <a name="create-a-container-in-code"></a>Erstellen Sie einen Container im code

> [AZURE.NOTE] Einige APIs, die Aufrufe Azure Storage in ASP.NET ausführen sind asynchron. Weitere Informationen finden Sie unter [asynchrone Programmierung mit Async und Await](http://msdn.microsoft.com/library/hh191443.aspx) . Der Code im folgenden Beispiel wird vorausgesetzt, dass Sie asynchrone Programmiermethoden verwenden.

Um einen Container in das Speicherkonto erstellen, müssen Sie lediglich einen Aufruf **CreateIfNotExistsAsync** wie im folgenden Code hinzufügen:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Um für alle Dateien innerhalb des Containers verfügbar machen, können Sie den Container öffentliche werden mithilfe des folgenden Codes festlegen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Jeder im Internet sehen Blobs in einem öffentlichen Container jedoch ändern oder löschen, nur, wenn die entsprechende Taste.

## <a name="upload-a-blob-into-a-container"></a>Ein Container einen Blob hochladen

Block-Blobs und Seitenblobs unterstützt Azure-Speicher. Block-Blob ist in den meisten Fällen den empfohlenen Typ.

Zum Hochladen einer Datei auf ein Container bringen und einen Block BLOB-Verweis verwenden. Haben Sie eine BLOB-laden Stream von Daten, Sie durch Aufrufen der **UploadFromStream** -Methode. Dieser Vorgang erstellt das Blob bereits vorhanden sein oder überschreibt vorhanden ist. Im folgenden Beispiel wird veranschaulicht, wie einen Blob in einem Container hochladen und geht davon aus, dass der Container bereits erstellt wurde.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Um die Blobs in einem Container sind, rufen Sie zunächst einen Containerverweis. **ListBlobs** -Methode des Containers können Sie Blobs bzw. Verzeichnisse innerhalb abzurufen. Zugriff auf den vielfältigen Eigenschaften und Methoden für eine zurückgegebene **IListBlobItem**muss **CloudBlockBlob**, **CloudPageBlob**oder **CloudBlobDirectory** -Objekt umgewandelt. Wenn der Typ bekannt ist, können Sie eine Prüfung, fest, dass umgewandelt. Der folgende Code veranschaulicht das Abrufen und ausgeben den URI der einzelnen Elemente im Container **Fotos** :

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
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

Wie im vorherigen Beispiel dargestellt, hat der BLOB-Dienst das Konzept der Verzeichnisse im Container, Dies ist die Blobs in eine weitere Ordner-Struktur zu organisieren. Betrachten Sie z. B. den folgenden Satz von Block-Blobs in einem Container mit dem Namen **Fotos**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Beim Aufruf von **ListBlobs** für den Container (wie im vorherigen Beispiel) enthält die zurückgegebene Auflistung **CloudBlobDirectory** und **CloudBlockBlob** Objekte darstellen der Verzeichnisse und Blobs auf der obersten Ebene enthalten. Hier ist die Ausgabe:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Optional kann der **UseFlatBlobListing** -Parameter der **ListBlobs** -Methode auf **true**festgelegt werden. Das Ergebnis jedes Blob als eine **CloudBlockBlob**, unabhängig vom Verzeichnis zurückgegeben. Hier ist der **ListBlobs**:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

und hier sind die Ergebnisse:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Weitere Informationen finden Sie unter [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Herunterladen von blobs

Herunterladen von Blobs zunächst rufen Sie Blob Referenz ab und dann rufen Sie die Methode **DownloadToStream auf** . Das folgende Beispiel verwendet die Methode **DownloadToStream** BLOB-Inhalt in einem Stream-Objekt übertragen, die dann in eine lokale Datei beibehalten werden.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

**DownloadToStream** -Methode können Sie den Inhalt eines Blob als Textzeichenfolge herunterladen.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Löschen von blobs

Zum Löschen eines BLOBs zunächst eine Blob Referenz, und rufen Sie die **Delete** -Methode.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Blobs in Seiten asynchron auflisten

Wenn Sie einer großen Anzahl von Blobs auflisten steuern die Anzahl der Ergebnisse in einem Auflistungsvorgang zurückgegeben werden soll, können Sie Blobs in Seiten Ergebnisse auflisten. Dieses Beispiel zeigt, wie Seiten asynchron Ergebnissen, Ausführung nicht blockiert, während auf eine große Menge von Ergebnissen zurück.

Dieses Beispiel zeigt einen flachen Blob auflisten, aber auch durchführbaren eine hierarchische Liste den **UseFlatBlobListing** -Parameter der **ListBlobsSegmentedAsync** -Methode auf **false**festlegen.

Da die Beispielmethode eine asynchrone Methode aufruft, muss mit dem Schlüsselwort **Async** vorangestellt sein und muss ein **Task** -Objekt zurück. Das Await-Schlüsselwort für die **ListBlobsSegmentedAsync** angegebene unterbricht die Ausführung der Beispielmethode bis Angebot Aufgabe abgeschlossen ist.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
