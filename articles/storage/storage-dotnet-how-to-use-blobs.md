<properties
    pageTitle="Erste Schritte mit Azure BLOB-Speicher (Objektspeicher) mit .NET | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud Azure BLOB-Speicher (Storage Objekt)."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Erste Schritte mit Azure BLOB-Speicher mit .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

Azure BLOB-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte-Blobs in der Cloud gespeichert. Beliebigen Text oder Binärdaten, Dokument, Datei oder Anwendung Installer kann BLOB-Speicher gespeichert werden. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

### <a name="about-this-tutorial"></a>Zu diesem Lernprogramm

Dieses Lernprogramm zeigt .NET Programmieren für einige häufige Szenarien mit Azure BLOB-Speicher. Szenarios umfassen hochladen, auflisten, herunterladen und Löschen von Blobs.

**Geschätzte Dauer:** 45 Minuten

**Dabei:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure-Speicher-Clientbibliothek für .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Konfigurations-Manager für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Ein [Azure Storage-Konto](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Weitere Beispiele

Weitere Beispiele für die Verwendung von BLOB-Speicher finden Sie unter [Einstieg in Azure BLOB-Speicher in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Sie können die beispielanwendung herunterladen und ausführen oder Durchsuchen Sie den Code auf GitHub.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Namespacedeklarationen hinzuzufügen

Fügen Sie den folgenden `using` Aussagen am oberen Rand der `program.cs` Datei:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>Analysieren der Verbindungszeichenfolge

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>Erstellen des Blob-Clients

**CloudBlobClient** -Klasse können Sie zum Abrufen von Containern und Blobs im BLOB-Speicher gespeichert. Hier ist eine Möglichkeit, den Webdienstclient erstellen:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Jetzt können Sie Code schreiben, der Daten liest und schreibt Daten-BLOB-Speicher.

## <a name="create-a-container"></a>Erstellen eines Containers

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Dieses Beispiel zeigt, wie Sie einen Container erstellen, wenn es nicht bereits vorhanden ist:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Neue Container ist standardmäßig privat, d. h. den speicherzugriffsschlüssel zum Herunterladen von Blobs von diesem Container anzugeben. Wenn Sie für alle Dateien innerhalb des Containers verfügbar machen möchten, können Sie den Container öffentliche mit dem folgenden Code festlegen:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Jeder im Internet sehen Blobs in einem öffentlichen Container jedoch ändern oder löschen, wenn das entsprechende Konto Zugriffstaste oder eine SAS ist.

## <a name="upload-a-blob-into-a-container"></a>Ein Container einen Blob hochladen

Block-Blobs und Seitenblobs unterstützt Azure BLOB-Speicher.  Block-Blob ist in den meisten Fällen den empfohlenen Typ.

Zum Hochladen einer Datei auf ein Container bringen und einen Block BLOB-Verweis verwenden. Haben Sie eine BLOB-laden Stream von Daten, Sie durch Aufrufen der **UploadFromStream** -Methode. Dieser Vorgang erstellt das Blob nicht bereits vorhanden, oder überschreiben sie vorhanden ist.

Im folgenden Beispiel wird veranschaulicht, wie einen Blob in einem Container hochladen und geht davon aus, dass der Container bereits erstellt wurde.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Um die Blobs in einem Container sind, rufen Sie zunächst einen Containerverweis. **ListBlobs** -Methode des Containers können Sie Blobs bzw. Verzeichnisse innerhalb abzurufen. Zugriff auf den vielfältigen Eigenschaften und Methoden für eine zurückgegebene **IListBlobItem**muss **CloudBlockBlob**, **CloudPageBlob**oder **CloudBlobDirectory** -Objekt umgewandelt.  Wenn der Typ bekannt ist, können Sie eine Prüfung, fest, dass umgewandelt.  Der folgende Code veranschaulicht das Abrufen und ausgeben den URI jedes Elements in der `photos` Container:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

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

Wie oben gezeigt, können Sie Blobs mit Informationen in ihrem Namen benennen. Dadurch wird eine virtuelle Verzeichnisstruktur, die Sie organisieren und durchsuchen wie herkömmliche Dateisysteme erstellt. Beachten Sie, dass die Verzeichnisstruktur wird nur virtuelle nur Ressourcen-BLOB-Speicher für Container und Blobs. Die speicherclientbibliothek bietet jedoch ein **CloudBlobDirectory** -Objekt in einem virtuellen Verzeichnis und vereinfachen das Arbeiten mit Blobs, die so angeordnet sind.

Betrachten Sie z. B. den folgenden Satz von Block-Blobs in einem Container mit dem Namen `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Beim Aufruf von **ListBlobs** für den Container "Fotos" (wie im obigen Beispiel) ist eine hierarchische Auflistung zurückgegeben. Es enthält **CloudBlobDirectory** und **CloudBlockBlob** -Objekte, die Verzeichnisse und Blobs im Container darstellt. Die Ausgabe sieht folgendermaßen aus:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Optional kann der **UseFlatBlobListing** -Parameter der **ListBlobs** -Methode auf **true**festgelegt werden. In diesem Fall wird jedes Blob im Container als **CloudBlockBlob** Objekt zurückgegeben. Der Aufruf von **ListBlobs** eine unstrukturierte Liste zurückgegeben sieht folgendermaßen aus:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

und die Ergebnisse wie folgt aussehen:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>Herunterladen von blobs

Herunterladen von Blobs zunächst rufen Sie Blob Referenz ab und dann rufen Sie die Methode **DownloadToStream auf** . Das folgende Beispiel verwendet die Methode **DownloadToStream** BLOB-Inhalt in einem Stream-Objekt übertragen, die dann in eine lokale Datei beibehalten werden.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

**DownloadToStream** -Methode können Sie den Inhalt eines Blob als Textzeichenfolge herunterladen.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Löschen von blobs

Zum Löschen eines BLOBs zunächst eine Blob Referenz, und rufen die **Delete** -Methode auf.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Blobs in Seiten asynchron auflisten

Wenn Sie einer großen Anzahl von Blobs auflisten steuern die Anzahl der Ergebnisse in einem Auflistungsvorgang zurückgegeben werden soll, können Sie Blobs in Seiten Ergebnisse auflisten. Dieses Beispiel zeigt, wie Seiten asynchron Ergebnissen, Ausführung nicht blockiert, während auf eine große Menge von Ergebnissen zurück.

Dieses Beispiel zeigt einen flachen Blob auflisten, sondern Sie können auch eine hierarchische Liste durch Festlegen der `useFlatBlobListing` Parameter der Methode **ListBlobsSegmentedAsync** `false`.

Da die Beispielmethode eine asynchrone Methode aufruft, muss es mit vorangestelltem der `async` Schlüsselwort, und es muss ein **Task** -Objekt zurückgeben. Das Await-Schlüsselwort für die **ListBlobsSegmentedAsync** angegebene unterbricht die Ausführung der Beispielmethode bis Angebot Aufgabe abgeschlossen ist.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
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

## <a name="writing-to-an-append-blob"></a>Schreiben in ein Blob anhängen

Ein Blob Append ist ein neuer Typ BLOB eingeführt mit Version 5.x der Azure-Speicher-Clientbibliothek für .NET. Ein Blob Append ist für Append Operationen wie Protokollierung optimiert. Wie ein Blockblob ein Blob Append besteht aus Blöcken, aber wenn ein Blob Anhängen einen neuen Block hinzufügen, es ist immer am Ende angefügt des Blob. Nicht aktualisieren oder Löschen einen vorhandenen Block in ein Blob anhängen. Die Block-IDs für ein Blob Anhängen sind nicht verfügbar, wie für ein.

Jeder Block in ein Blob anhängen kann eine andere Größe maximal 4 MB und ein Append-Blob kann maximal 50.000 Blöcke enthalten. Die maximale Größe eines Append-BLOBs ist deshalb etwas mehr als 195 GB (4 MB X 50.000 Blöcke).

Im folgenden Beispiel erstellt ein neues Anhängen Blob und fügt einige Daten, simuliert eine einfache Protokollierung.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Weitere Informationen zu den Unterschieden zwischen den drei Arten von Blobs finden Sie unter [Understanding Block Blobs, Seitenblobs und Blobs anfügen](https://msdn.microsoft.com/library/azure/ee691964.aspx) .

## <a name="managing-security-for-blobs"></a>Verwalten der Sicherheit für blobs

Standardmäßig sichert Azure Storage Daten durch Einschränken des Zugriffs auf das Kontobesitzer Zugriffstasten Konto besitzt. Wenn Sie BLOB-Daten in das Speicherkonto müssen unbedingt ohne Beeinträchtigung der Sicherheit von Ihrem Konto Zugriffstasten. Darüber hinaus können Sie verschlüsseln BLOB-Daten über das Netzwerk und in Azure-Speicher ist.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Steuern des Zugriffs auf BLOB-Daten

BLOB-Daten in das Speicherkonto ist standardmäßig nur Kontobesitzer Speicher zugreifen. Authentifizierung gegen die BLOB-Speicher muss die Zugriffstaste Konto standardmäßig. Allerdings können Sie bestimmte BLOB-Daten für andere Benutzer verfügbar machen. Sie haben zwei Optionen:

- **Anonymen Zugriff:** Sie können den Container oder die Blobs öffentlich für den anonymen Zugriff zur Verfügung. Weitere Informationen finden Sie unter [Verwalten anonymen Lesezugriff auf Container und Blobs](storage-manage-access-to-resources.md) .
- **Shared Access Signaturen:** Kunden können mit SAS (SAS), Sie die delegierten Zugriff auf eine Ressource in das Speicherkonto mit Berechtigungen, die Sie angeben und über einen bestimmten Zeitraum, den Sie angeben. Weitere Informationen finden Sie unter [Verwenden gemeinsame Zugriff Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

### <a name="encrypting-blob-data"></a>Verschlüsseln von BLOB-Daten

Azure-Speicher unterstützt Verschlüsselung von BLOB-Daten auf dem Client und auf dem Server:

- **Clientseitige Verschlüsselung:** Speicher-Clientbibliothek für .NET unterstützt das Verschlüsseln von Daten in Clientanwendungen Azure-Speicher hochgeladen und Entschlüsseln von Daten beim Herunterladen auf den Client. Die Bibliothek unterstützt auch die Integration mit Azure Schlüssel für das Speichermanagement für Konto. Weitere Informationen finden Sie in der [Clientseitigen Verschlüsselung mit .NET für Microsoft Azure-Speicher](storage-client-side-encryption.md) . Siehe auch [Anleitung: Verschlüsseln und Entschlüsseln von Blobs in Azure Key Vault mit Microsoft Azure Storage](storage-encrypt-decrypt-blobs-key-vault.md).
- **Serverseitige Verschlüsselung**: Azure-Speicher unterstützt jetzt die serverseitige Verschlüsselung. Siehe [Azure Storage Service Verschlüsselung für Daten (Vorschau)](storage-service-encryption.md).

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der BLOB-Speicher bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure-Speicher-Explorer
- [Microsoft Azure Storage Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) ist kostenlos, eigenständige Anwendung von Microsoft, die Sie visuell mit Azure-Speicher unter Windows, OS X und Linux arbeiten kann.

### <a name="blob-storage-samples"></a>BLOB-Speicher-Beispiele

- [Erste Schritte mit Azure BLOB-Speicher in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>BLOB-Speicher Verweis

- [Speicher-Clientbibliothek für .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [REST-API-Referenz](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Grundlegende Hilfslinien

- [Datenübertragung mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)
- [Erste Schritte mit Dateispeicher für .NET](storage-dotnet-how-to-use-files.md)
- [Verwendung von Azure BLOB-Speicher mit WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
