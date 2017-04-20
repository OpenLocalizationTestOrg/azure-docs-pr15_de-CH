<properties 
    pageTitle="Hochladen von Dateien in Media Services-Konto mit .NET | Microsoft Azure" 
    description="Informationen Sie zu Medieninhalte Media Services erstellen und Hochladen von Ressourcen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Media Services-Konto mit .NET hochladen Sie Dateien

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [REST](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

Media Services Sie hochladen oder nehmen Ihre digitalen Dateien zu. Die **Aktivposten** -Entität darf Video, Audio, Bilder, Miniaturansichten Sammlungen, Text Titel und Untertitel Dateien (und Metadaten Dateien.)  Sobald die Dateien hochgeladen werden, wird der Inhalt in der Cloud zur weiteren Verarbeitung und streaming sicher.

Die Dateien in der Anlage heißen **Objektdateien**. Die **AssetFile** -Instanz und die aktuelle Mediendatei sind zwei verschiedene Objekte. Die AssetFile-Instanz enthält Metadaten über die Mediendatei Mediendatei tatsächlichen Medien.

>[AZURE.NOTE]Beim Auswählen eines Namens für die Anlage ist Folgendes zu berücksichtigen:
>
>- Media Services verwendet den Wert der Eigenschaft IAssetFile.Name beim Erstellen von URLs für das streaming von Inhalten (z. B. http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Aus diesem Grund ist die Prozent-Codierung nicht zulässig. Der Wert der **Name** -Eigenschaft keinen der folgenden [%-Codierung-reservierten Zeichen](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Es können nur auch einen "." für die Erweiterung.
>
>- Die Länge des Namens sollte nicht länger als 260 Zeichen sein.

Wenn Sie Elemente erstellen, können Sie die folgenden Verschlüsselungsoptionen angeben. 

- **Keine** - keine Verschlüsselung verwendet wird. Dies ist der Standardwert. Beachten Sie, dass bei Verwendung dieser Option der Inhalt nicht während der Übertragung oder im Speicher geschützt ist.
Möchten Sie ein MP4 liefern progressives Herunterladen verwendet diese Option. 
- **CommonEncryption** - verwenden Sie diese Option, wenn Sie Inhalte hochladen, die bereits verschlüsselt und mit gemeinsamen Verschlüsselung oder PlayReady-DRM (z. B. Smooth Streaming geschützt mit PlayReady-DRM) geschützt.
- **EnvelopeEncrypted** – verwenden Sie diese Option, wenn Sie mit AES verschlüsselt HLS hochladen. Beachten Sie, dass Dateien müssen codiert und Transformieren Manager verschlüsselt.
- **StorageEncrypted** - verschlüsselt löschen Inhalte lokal statisch mit AES-256-bit-Verschlüsselung und Uploads es Azure Storage Speicherort verschlüsselt. Mit Storage Verschlüsselung geschützt werden automatisch entschlüsselt ein verschlüsseltes Dateisystem vor Codierung platziert und optional vor dem Hochladen als eine neue Ausgabe Anlage erneut verschlüsselt. Primäre Nutzung bei Speicher-Verschlüsselung wird Sie hochwertige input Mediendateien mit starker Verschlüsselung ruhender auf Festplatte sichern möchten.

    Media Services bietet Verschlüsselung auf Datenträger für Ihre Anlagen nicht über das Netzwerk wie Digital Rights Manager (DRM).

    Ist die Anlage Speicher verschlüsselt, müssen Anlagen Lieferung Richtlinie konfigurieren. Informationen finden Sie unter [Konfigurieren von Anlage Lieferung Richtlinie](media-services-dotnet-configure-asset-delivery-policy.md).

Wenn Sie die Anlage mit einem **CommonEncrypted** , oder eine **EnvelopeEncypted** Option Verschlüsselung angeben, müssen Sie **ContentKey**Ihrer Ressource zuordnen. Weitere Informationen finden Sie unter [Erstellen einer ContentKey](media-services-dotnet-create-contentkey.md). 

Bei Angabe der Anlage mit der Option **StorageEncrypted** verschlüsselt erstellt Media Services SDK für .NET einen **StorateEncrypted** **ContentKey** für die Anlage.


In diesem Thema veranschaulicht, wie mit Media Services .NET SDK sowie Media Services .NET SDK Extensions zum Hochladen von Dateien in einen Media-Dienste.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Hochladen einer einzelnen Datei mit Media Services .NET SDK 

Der Beispielcode verwendet .NET SDK für die folgenden Aufgaben: 

- Erstellt eine leere Anlage.
- Erstellt eine AssetFile-Instanz, die der Anlage zugeordnet werden soll.
- Erstellt eine AccessPolicy-Instanz, die die Berechtigungen und Zugriff auf die Anlage definiert.
- Erstellt eine Instanz der Locator, die auf die Anlage zugreifen.
- Wird eine einzelne Mediendatei in Media Services hochgeladen. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Hochladen Sie mehrerer Dateien mit Media Services .NET SDK 

Der folgende Code veranschaulicht eine Anlage und mehrere Dateien uploaden.

Der Code führt Folgendes aus:
    
-   Erstellt eine leere Anlage mithilfe der CreateEmptyAsset-Methode im vorherigen Schritt definiert.
    
-   Erstellt eine **AccessPolicy** -Instanz, die die Berechtigungen und Zugriff auf die Anlage definiert.
    
-   Erstellt eine Instanz der **Locator** , die auf die Anlage zugreifen.
    
-   Erstellt eine **BlobTransferClient** -Instanz. Dieser Typ stellt einen Client, der auf den Azure-Blobs. In diesem Beispiel verwenden wir den Client den Upload-Fortschritt überwachen. 
    
-   Listet Dateien im angegebenen Verzeichnis und erstellt eine **AssetFile** -Instanz für jede Datei.
    
-   Lädt die Dateien in Media Services mithilfe der **UploadAsync** -Methode. 
    
>[AZURE.NOTE] Verwenden Sie die UploadAsync-Methode Aufrufe nicht blockiert und hochgeladen parallel.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Beachten Sie Folgendes, eine große Anzahl von Anlagen übertragen.

- Erstellen Sie ein neues **CloudMediaContext** -Objekt pro Thread. Die **CloudMediaContext** -Klasse ist nicht threadsicher.
 
- Erhöhen Sie NumberOfConcurrentTransfers vom Standardwert 2 höher wie 5. Diese Einstellung wirkt sich auf alle Instanzen **CloudMediaContext**. 
 
- Behalten Sie den Standardwert von 10 ParallelTransferThreadCount.
 
##<a id="ingest_in_bulk"></a>Technologische Ressourcen gebündelt mit Media Services .NET SDK 

Große Anlagen Upload kann einen Engpass während der Erstellung von Ressourcen. Einnahme Anlagen in Massen oder "Bulk Einnahme", umfasst die Erstellung von Ressourcen aus den Uploadprozess entkoppeln. Erstellen Sie eine Einnahme Ansatz Massen ein Manifest (IngestManifest), die die Anlage und die zugehörigen Dateien beschreibt. Können Sie die Upload-Methode Ihrer Wahl das Manifest BLOB-Container zugeordneten Dateien hochladen. Microsoft Azure Media Services überwacht zugeordneten Manifest blobcontainer. Nach dem Hochladen einer Datei in den Blob-Container schließt Microsoft Azure Media Services die Erstellung von Ressourcen basierend auf der Konfiguration der Anlage im Manifest (IngestManifestAsset).


Erstellen einer neuen IngestManifest Aufrufen die Create-Methode der Auflistung IngestManifests das CloudMediaContext ausgesetzt. Diese Methode erstellt ein neues IngestManifest mit den Manifestnamen bereitgestellten.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Erstellen Sie die Ressourcen, die den größten IngestManifest zugeordnet werden. Konfigurieren Sie die gewünschten Verschlüsselungsoptionen für den Vermögenswert für Massenkopieren Einnahme.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Ein IngestManifestAsset ordnet eine Anlage mit einem IngestManifest für Massen Einnahme. Es ordnet auch AssetFiles, aus denen jede Anlage zusammensetzt. Erstellen Sie ein IngestManifestAsset verwenden Sie die Erstellungsmethode auf den Serverkontext.

Das folgende Beispiel veranschaulicht das Hinzufügen von zwei neuen IngestManifestAssets, die zwei Anlagen erzeugten das gleichzeitige zuordnen Manifest aufnehmen. Jede IngestManifestAsset ordnet einen Satz von Dateien, die hochgeladen werden für jede Anlage auch beim Massenkopieren Einnahme.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
Zur Anlage Upload BLOB-Speicher Container von der **IIngestManifest.BlobStorageUriForUpload** -Eigenschaft der IngestManifest bereitgestellte URI Hochgeschwindigkeits Clientanwendungen können. Eine wichtige Hochgeschwindigkeits-Upload-Dienst wird [Aspera auf Azure-Anwendung](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Sie können auch Code zum Hochladen von Anlagen Dateien wie im folgenden Codebeispiel gezeigt schreiben.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

Der Code für das Hochladen von Objektdateien in diesem Thema verwendete Beispiel ist im folgenden Codebeispiel dargestellt.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

Sie können den Fortschritt der Bulk-Einnahme für alle Anlagen mit einer **IngestManifest** durch Abrufen der Statistik-Eigenschaft des **IngestManifest**bestimmen. Um Informationen zu aktualisieren, verwenden neue **CloudMediaContext** jedes Mal Sie Statistik-Eigenschaft abrufen.

Das folgende Beispiel veranschaulicht eine IngestManifest anhand dessen **Id**abrufen.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Hochladen von Dateien mit .NET SDK Extensions 

Im folgenden Beispiel wird das Hochladen einer einzelnen Datei mit .NET SDK Extensions veranschaulicht. In diesem Fall die **CreateFromFile** -Methode verwendet, aber die asynchrone Version ist auch verfügbar (**CreateFromFileAsync**). **CreateFromFile** -Methode können Sie den Dateinamen Verschlüsselungsoption und einen Rückruf um Fortschritte Upload der Datei angeben.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

Im folgende Beispiel UploadFile Funktion und gibt speicherverschlüsselung optional Erstellung Anlage.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Nächstes

Jetzt hochladen eine Anlage Media Services gehen Sie zum Thema [wie ein Media Prozessor][] .

[Wie man einen Media-Prozessor]: media-services-get-media-processor.md
 
