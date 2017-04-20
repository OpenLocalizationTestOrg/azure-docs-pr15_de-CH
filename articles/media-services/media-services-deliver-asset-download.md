<properties 
    pageTitle="Herunterladen von Medien" 
    description="Informationen Sie zu Ihrem Computer herunterzuladen. Code-Beispiele sind in C# geschrieben und Media Services SDK für .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Gewusst wie: liefern eine Anlage per Download

Dieses Thema enthält Optionen für die Bereitstellung von Medien in Media Services hochgeladen. Media Services-Inhalte sind in zahlreichen Anwendungsszenarien liefern. Sie können Medien herunterladen oder mit einem Locator zugreifen. Sie können Medieninhalte zu einer anderen Anwendung oder einem anderen Inhaltsanbieter senden. Für verbesserte Leistung und Erweiterbarkeit können Sie Inhalte auch mit Content Delivery Network (CDN) bereitstellen.

Dieses Beispiel zeigt, wie Medien von Media Services auf Ihren lokalen Computer herunterladen. Der Code fragt Einzelvorgänge mit Media Services-Konto nach Auftrags-ID und greift auf die **OutputMediaAssets** -Auflistung (die mindestens Ausgabe Medien aus der Ausführung eines Auftrags ist der). Dieses Beispiel veranschaulicht die Ausgabe Medien aus einem Auftrag herunterladen, aber die Vorgehensweise zum Herunterladen anderer Vermögenswerte anzuwenden.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Siehe auch 

[Streaming-Inhalte](media-services-deliver-streaming-content.md)

