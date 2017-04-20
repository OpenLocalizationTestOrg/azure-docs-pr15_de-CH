<properties
    pageTitle="Redaktion Azure Media Analytics stellen | Microsoft Azure"
    description="In diesem Thema veranschaulicht, wie mit Azure Media Analytics Schwärzen."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Face Schwärzen Azure Media Analytics

##<a name="overview"></a>Übersicht

**Azure Media Korrespondent** ist ein [Azure Media Analytics](media-services-analytics-overview.md) Media-Prozessor (MP), der skalierbare Gesicht Schwärzen in die Cloud bietet. Gesicht Schwärzen können Sie das Video um Flächen der ausgewählten Personen Weichzeichnen ändern. Gesicht Schwärzen Service in öffentliche Sicherheit und Medien Szenarien verwenden möchten. Einige Minuten lang, die mehrere Flächen enthält können Stunden manuell Schwärzen, aber mit diesem Dienst Gesicht Schwärzen Prozess nur ein paar einfache Schritte erforderlich. Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/azure-media-redactor/) Blog.

Dieses Thema enthält Details zu **Azure Media Redakteur** und veranschaulicht, wie sie mit Media Services SDK für .NET.

**Azure Media Redakteur** MP ist derzeit in der Vorschau.

## <a name="face-redaction-modes"></a>Face Schwärzen Modi

Gesichtsausdruck Schwärzen arbeitet Flächen in jedem Frame des Videos und Verfolgungsobjekt das Gesicht sowohl vorwärts und rückwärts rechtzeitig die gleiche Person aus anderen Winkeln sowie weichgezeichnet werden. Automatisierte Schwärzen-Prozess ist sehr komplex und ist nicht immer Produkte 100 % der gewünschten Ausgabe daher Media Analytics bietet Ihnen auf verschiedene Weise auf die endgültige Ausgabe ändern.

Neben vollautomatischen Modus ist ein zweistufige Workflow ermöglicht die Auswahl/de-selection gefundenen Flächen über eine Liste von IDs. Um willkürlichen pro Frame Korrekturen der VA verwendet eine Metadatendatei im JSON-Format. Dieser Workflow ist in **Analysieren** und **Schwärzen** Modi unterteilt. Sie können zwei Modi in einem kombinieren, die beide Aufgaben in einem Job ausgeführt wird; Dieser Modus heißt **kombiniert**.

###<a name="combined-mode"></a>Kombinationsmodus

Diese erzeugt geschwärzten mp4 automatisch ohne manuelle Eingabe.

Stufe|Dateiname|Notizen
---|---|---
Input Anlage|foo.Bar|Videos im WMV, MOV oder MP4-format
Input-Konfiguration|Voreingestellte Konfiguration|{'Version':'1.0 ', 'Optionen': {'Mode': 'kombiniert'}}
Ausgabe-Anlage|foo_redacted.mp4|Video mit Unschärfe angewendet

####<a name="input-example"></a>Input-Beispiel:

[Dieses Video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Beispielausgabe:

[Dieses Video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Analysemodus

Die **Analysieren** des Workflows zweistufige nimmt einen Videoeingang und erzeugt eine JSON Gesicht Speicherorte und Jpg-Bilder jeder erkannte Fläche.

Stufe|Dateiname|Notizen
---|---|----
Input Anlage|foo.Bar|Videos im WMV, MPV oder MP4-format
Input-Konfiguration|Voreingestellte Konfiguration|{'Version':'1.0 ', 'Optionen': {'Mode': 'analysieren'}}
Ausgabe-Anlage|foo_annotations.JSON|Daten der Fläche Speicherorte im JSON-Format. Dadurch kann der Benutzer zum Ändern des Weichzeichners Begrenzungsrahmen bearbeitet werden. Siehe Beispiel unten.
Ausgabe-Anlage|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Zugeschnittene Jpg jedes Gesicht, wo die Anzahl LabelId der Fläche gibt erkannt

####<a name="output-example"></a>Beispielausgabe:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… abgeschnitten


###<a name="redact-mode"></a>Modus Schwärzen

Beim zweiten Durchgang des Workflows wird eine größere Anzahl von Eingaben in einer einzelnen Ressource kombiniert werden müssen.

Diese enthält eine Liste der IDs Weichzeichnen, das Originalvideo und Strukturänderungen JSON. Dieser Modus verwendet die Strukturänderungen video Input Weichzeichnen anwenden.

Die Ausgabe aus dem Analysedurchlauf umfasst nicht das Originalvideo. Das Video muss input Anlage dafür Modus Schwärzen hochgeladen und als die primäre Datei.

Stufe|Dateiname|Notizen
---|---|---
Input Anlage|foo.Bar|Videos im WMV, MPV oder MP4. Dasselbe video wie in Schritt 1.
Input Anlage|foo_annotations.JSON|Strukturänderungen Metadatendatei aus Phase 1 mit optional.
Input Anlage|foo_IDList.txt (Optional)|Optionale Zeilenumbruch getrennte Liste von IDs Schwärzen Gesicht. Wenn leer, verwischt dieser alle Flächen.
Input-Konfiguration|Voreingestellte Konfiguration|{'Version':'1.0 ', 'Optionen': {'Mode': Schwärzen"}}
Ausgabe-Anlage|foo_redacted.mp4|Video mit Unschärfe angewendet basierend auf Kommentare

####<a name="example-output"></a>Beispiel für die Ausgabe

Dies ist die Ausgabe von Zonen mit einer ID ausgewählt.

[Dieses Video](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Attribut Beschreibung

Redaktion MP bietet hohe Präzision Gesicht Speicherort und tracking kann bis zu 64 Gesichter in eines. Frontale Flächen bieten die besten Ergebnisse, Seitenflächen und klein (kleiner oder gleich 24 x 24 Pixel) sind eine Herausforderung.

Flächen erkannten und verfolgten werden mit Koordinaten die Position der Flächen ein Gesicht der ID-Nummer, die angibt, der Überwachung der einzelnen zurückgegeben. Face ID-Nummern sind anfällig für unter Umständen zurückgesetzt, wenn die Stirnseite verloren oder überlappende im Rahmen einzelner immer mehrere IDs zugewiesen.

Eine ausführliche Erklärung der Attribute finden Sie [erkennen und Emotionen Azure Media Analytics](media-services-face-and-emotion-detection.md) .

## <a name="sample-code"></a>Beispielcode

Das folgende Programm veranschaulicht, wie:

1. Eine Anlage erstellen und Laden einer Mediendatei in der Anlage.
1. Erstellen eines Auftrags Gesicht Schwärzen Aufgabe basierend auf eine Konfigurationsdatei mit den folgenden Json-Vorgabe. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Die JSON-Ausgabedateien herunterladen 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Nächstes

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Verwandte links

[Analytics Übersicht über Services Azure Media](media-services-analytics-overview.md)

[Azure Media Analytics-demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
