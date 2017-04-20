<properties
    pageTitle="Indizierung von Mediendateien mit Azure Media Indexer 2 Vorschau | Microsoft Azure"
    description="Azure Media Indexer können Sie Mediendateien durchsuchbar machen und eine Volltext-Protokoll für geschlossene Untertitelung und Schlüsselwörter erstellen. Dieses Thema beschreibt, wie Sie Media Indexer 2 Vorschau."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indizierung von Mediendateien mit Azure Media Indexer 2 Vorschau

##<a name="overview"></a>Übersicht

**Azure Media Indexer 2 Vorschau** Media Prozessor (MP) können Sie Mediendateien und Inhalte durchsucht werden, als auch geschlossene Untertitel-Spuren erstellen. Gegenüber der Vorgängerversion von [Azure Media Indexer](media-services-index-content.md) **Azure Media Indexer 2 Vorschau** führt schneller Indizierung und weiteren Sprachen unterstützt. Den unterstützten Sprachen gehören Englisch Spanisch Französisch Deutsch Italienisch Chinesisch Portugiesisch und Arabisch.

**Azure Media Indexer 2 Vorschau** MP ist zurzeit in der Vorschau.

In diesem Thema veranschaulicht die Indizierung Arbeitsplätze mit **Azure Media Indexer 2 Vorschau**.

>[AZURE.NOTE]Folgendes gilt:
>
>Indexer 2 wird in Azure China und Azure Regierung nicht unterstützt.
>
>Vorschau auf ca. 10 Minuten verarbeitet, jedoch alle Kunden.
>
>Beim Indizieren von Inhalt müssen Sie Mediendateien mit sehr klare Sprache (ohne Musik, Rauschen, Effekte oder Mikrofon Rauschen) verwendet werden. Beispiele für geeignete Inhalte: Meetings, Vorträge oder Präsentationen aufgezeichnet. Der folgende Inhalt ist möglicherweise nicht für die Indizierung: Filme, Fernsehsendungen, nichts mit gemischtem Audio und Soundeffekte, schlecht mit Hintergrundgeräusche (Rauschen) aufgezeichnet.


In diesem Thema finden Sie Einzelheiten über **Azure Media Indexer 2 Vorschau** und zeigt, wie sie mit Media Services SDK für .NET

##<a name="input-and-output-files"></a>Eingabe-und Ausgabedateien

###<a name="input-files"></a>Dateien

Audio-oder Videodateien

###<a name="output-files"></a>Ausgabedateien

Ein Indizierung Auftrag erzeugen Untertiteldateien in folgenden Formaten:  

- **Samisch**
- **TTML**
- **WebVTT**

Beschriftung (CC) Dateien in diesen Formaten verwendet werden können, um Audio-und Videodateien Anhörung Behinderte zugänglich geschlossen.

##<a name="task-configuration-preset"></a>Taskkonfiguration (Voreinstellung)

Beim Erstellen einer Volltextindizierung Tasks **Azure Media Indexer 2**Vorschau müssen Sie eine Voreinstellung Konfiguration angeben.

Die folgende JSON legt Parameter fest.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>Unterstützte Sprachen  

Azure Media Indexer 2 Preview unterstützt Sprache Text für die folgenden Sprachen (beim Namen der Sprache in der Taskkonfiguration mit 4-Zeichencode in Klammern angeben, wie unten dargestellt):

- Englisch [enüs]
- Spanisch [EsEs]
- Chinesisch [ZhCn]
- Französisch [FrFr]
- Deutsch [DeDe]
- Italienisch [ItIt]
- Portugiesische [PtBr]
- Arabisch (ägyptisch) [ArEg]


## <a name="sample-code"></a>Beispielcode

Das folgende Programm veranschaulicht, wie:

1. Eine Anlage erstellen und Laden einer Mediendatei in der Anlage.
1. Erstellt ein Projekt mit einer Volltextindizierung basierend auf eine Konfigurationsdatei mit den folgenden Json-Vorgabe.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. Die Ausgabedateien herunter. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                static void Main(string[] args)
                {
        
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        
                    progressJobTask.Wait();
        
                    // If job state is Error, the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                        Console.WriteLine(string.Format("Error: {0}. {1}",
                                                        error.Code,
                                                        error.Message));
                        return null;
                    }
        
                    return job.OutputMediaAssets[0];
                }
        
                static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
                {
                    IAsset asset = _context.Assets.Create(assetName, options);
        
                    var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                    assetFile.Upload(filePath);
        
                    return asset;
                }
        
                static void DownloadAsset(IAsset asset, string outputDirectory)
                {
                    foreach (IAssetFile file in asset.AssetFiles)
                    {
                        file.Download(Path.Combine(outputDirectory, file.Name));
                    }
                }
        
                static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors
                        .Where(p => p.Name == mediaProcessorName)
                        .ToList()
                        .OrderBy(p => new Version(p.Version))
                        .LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor",
                                                                   mediaProcessorName));
        
                    return processor;
                }
        
                static private void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine();
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
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
            }
        }


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Verwandte links

[Analytics Übersicht über Services Azure Media](media-services-analytics-overview.md)

[Azure Media Analytics-demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)