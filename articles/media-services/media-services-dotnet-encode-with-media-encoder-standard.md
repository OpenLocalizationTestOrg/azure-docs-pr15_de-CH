<properties 
    pageTitle="Codieren eine Anlage mit Media Encoder Standard mit .NET | Microsoft Azure" 
    description="In diesem Thema veranschaulicht, wie mit .NET zu eine Anlage mit Media Encoder Strandard codieren." 
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
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Codieren einer Anlage mit Media Encoder Standard mit .NET

Codierung Aufträge sind eine der häufigsten Verarbeitungsvorgänge in Media Services. Sie Arbeitsplätze Codierung, um Mediendateien aus einer Codierung in eine andere zu konvertieren. Beim kodieren können Sie Media Services integrierte Media Encoder. Sie können auch einen Encoder von Media Services Partner bereitgestellt. Drittanbieter-Encoder sind Azure Marketplace erhältlich. 

Dieses Thema beschreibt, wie Sie .NET Datenbestände mit Media Encoder Standard (MES) codiert. Media Encoder Standard sieht eine der Encoder-Vorgaben beschrieben [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Es wird empfohlen, Kodieren Sie die Aufsteckkarte Dateien immer in eine adaptive Bitrate MP4 festlegen und den Satz in das gewünschte Format der [Dynamischen Verpackung](media-services-dynamic-packaging-overview.md)konvertieren. Um dynamische Verpackung nutzen zu können, müssen Sie zunächst mindestens bei Bedarf Streaming-Einheit für Streaming-Endpunkt abrufen, aus denen Lieferung des Inhalts werden soll. Weitere Informationen finden Sie unter [Skalierung Media Services](media-services-portal-manage-streaming-endpoints.md).

Ist die Ausgabe Anlage Speicher verschlüsselt, müssen Anlagen Lieferung Richtlinie konfigurieren. Informationen finden Sie unter [Konfigurieren von Anlage Lieferung Richtlinie](media-services-dotnet-configure-asset-delivery-policy.md).

>[AZURE.NOTE]MWS erzeugt eine Ausgabedatei mit einem Namen, der die ersten 32 Zeichen des Eingabedatei enthält. Der Name basiert auf der Angabe in der Voreinstellungsdatei. Beispielsweise "Dateiname": "{Basisname} _ {Index} {Erweiterung}". {Basisname} wird durch die ersten 32 Zeichen des Eingabedatei ersetzt.

###<a name="mes-formats"></a>MWS-Formate

[Formate und codecs](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>MWS-Voreinstellungen

Media Encoder Standard sieht eine der Encoder-Vorgaben beschrieben [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Eingabe- und Metadaten

Beim Codieren input Vermögenswert (oder Anlagen) MES können Vermögenswert Ausgabe bei der erfolgreichen Abschluss dieser Aufgabe codieren. Die Ausgabe-Anlage enthält Video, Audio, Miniaturansichten, Manifest usw. basierend auf die Codierung Voreinstellung, die Sie verwenden.

Die Anlage Ausgabe enthält auch eine Datei mit Metadaten zur Eingabe Anlage. Der Name der XML-Metadatendatei hat das folgende Format: < Asset_id > _metadata.xml (z. B. 41114ad3-eb5e - 4c 57 8d 92-5354e2b7d4a4_metadata.xml), wobei < Asset_id > AssetId input Anlage wird. Das Schema dieser Eingabe Metadaten XML beschrieben [hier](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Die Anlage Ausgabe enthält auch eine Datei mit Metadaten zur Ausgabe Anlage. Der Name der XML-Metadatendatei hat das folgende Format: < Source_file_name > _manifest.xml (z. B. BigBuckBunny_manifest.xml). Das Schema dieser Ausgabe Metadaten XML beschrieben [hier](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Wenn Sie die Metadatendateien untersuchen möchten, können Sie SAS-Locator erstellen und downloaden Sie die Datei auf Ihrem lokalen Computer. Finden Sie ein Beispiel für eine SAS-Locator erstellen und Herunterladen einer Datei mit Media Services .NET SDK Extensions.

##<a name="download-sample"></a>Beispiel herunterladen

Abrufen und ein Beispiel [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/)ausführen.

##<a name="example"></a>Beispiel

Im folgenden Codebeispiel Media Services .NET SDK folgende Aufgaben:

- Erstellen einer Codierung.
- Abrufen eines Verweises auf die Media Encoder Standard Encoder.
- Gibt an, dass mithilfe der "H264 mehrere Bitrate 720p" voreingestellte. Sie sehen alle Presets [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Sie können auch überprüfen, diese Vorgaben müssen, Schema [hier](https://msdn.microsoft.com/library/mt269962.aspx) Thema.
- Einen einzelnen codieren Vorgang dem Projekt hinzufügen. 
- Geben Sie input Anlage codiert werden.
- Erstellen Sie Vermögenswert Ausgabe, die die codierte Anlage enthalten.
- Fügen Sie einen Ereignishandler, um den Job-Status überprüfen.
- Senden Sie den Auftrag.
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
                TaskOptions.None);
        
            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);
        
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
            return job.OutputMediaAssets[0];
        }
        
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
                    break;
                case JobState.Canceling:
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    Console.WriteLine("Please wait...\n");
                    break;
                case JobState.Canceled:
                case JobState.Error:
        
                    // Cast sender as a job.
                    IJob job = (IJob)sender;
        
                    // Display or log error details as needed.
                    break;
                default:
                    break;
            }
        }
        
        
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
            return processor;
        }


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch 

[Wie Miniaturansicht .NET mit Media Encoder Standard](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media-Codierung (Übersicht)](media-services-encode-asset.md)
