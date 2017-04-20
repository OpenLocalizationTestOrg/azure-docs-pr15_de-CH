<properties
    pageTitle="Fläche und Emotionen Azure Media Analytics erkennen | Microsoft Azure"
    description="In diesem Thema veranschaulicht, wie Flächen und mit Azure Media Analytics."
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
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Fläche und Emotionen Azure Media Analytics erkennen

##<a name="overview"></a>Übersicht

**Azure Media Gesicht Detektor** Media Prozessor (MP) können Sie zu zählen, Bewegung, Publikum und Reaktion über Gesichtsausdrücke selbst beurteilen. Dieser Service umfasst zwei Funktionen: 

- **Face-Erkennung**

    Face-Erkennung findet und verfolgt Gesichter in einem Video. Mehrere Flächen erkannt werden und wie sie mit der JSON-Datei zurückgegebenen Metadaten und Position bewegen, anschließend nachverfolgt werden. Während der Überwachung versucht es geben eine konsistente ID zur gleichen Fläche während Person Bildschirm bewegen wird auch wenn sie blockiert werden oder lassen Sie kurz den Frame.

    >[AZURE.NOTE]Dieser Dienst führt keine gesichtserkennung. Eine Person, die bewirkt, dass den Rahmen oder behindert wird, für lange erhalten eine neue ID Rückkehr.

- **Emotionen Erkennung**
    
    Emotionen Erkennung ist eine optionale Komponente nach Erkennung Media Prozessor, die Analyse mehrere emotionale Attribute aus den erkannt, einschließlich Glück, Trauer, Angst, Wut und zurückgibt. 

**Azure Media Gesicht Detektor** MP ist derzeit in der Vorschau.

Dieses Thema bietet Details zu **Azure Media Gesicht Detektor** und veranschaulicht, wie sie mit Media Services SDK für .NET.

##<a name="face-detector-input-files"></a>Detektor Eingabedateien sind

Videodateien. Derzeit werden folgende Formate unterstützt: MP4, MOV und WMV.

##<a name="face-detector-output-files"></a>Stellen Sie Ausgabedateien Detektors

Die Fläche Erkennung und Überwachung-API bietet präzise Gesicht Speicherort erkennen und nachverfolgen, die in einem Video bis zu 64 Gesichter erkennen kann. Frontale Flächen bieten die besten Ergebnisse, Seitenflächen und kleine Flächen (kleiner oder gleich 24 x 24 Pixel) möglicherweise nicht so genau.

Die Flächen erkannten und verfolgten werden mit Koordinaten (links, oben, Breite und Höhe) zurückgegeben, die die Position des Bildes in Pixel sowie eine Face ID Zahl die Überwachung der einzelnen Flächen. Face ID-Nummern sind anfällig für unter Umständen zurückgesetzt, wenn die Stirnseite verloren oder überlappende im Rahmen einzelner immer mehrere IDs zugewiesen.

###<a id="output_elements"></a>Elemente der JSON-Ausgabedatei

Face-Erkennung und tracking-Vorgang enthält das Ergebnis die Metadaten aus der angegebenen Datei im JSON-Format.

Face-Erkennung und Überwachung JSON enthält die folgenden Attribute:

Element|Beschreibung
---|---
Version|Dies bezieht sich auf die Version des Video-API.
Zeitskala|"Ticks" pro Sekunde des Videos.
Offset|Dies ist das Zeitoffset Zeitstempel. In Version 1.0 von Video-APIs werden dies immer 0. In Zukunft können Szenarios unterstützt diesen Wert ändern.
Bildrate|Bilder pro Sekunde des Videos.
Fragmente|Die Metadaten ist in Segmente als Fragmente von chunked. Fragment enthält ein Anfang, Dauer, Anzahl und Ereignisse.
Starten|Die Anfangszeit des ersten Ereignisses "Ticks".
Dauer|Die Länge des Fragments in "Ticks".
Intervall|Das Zeitintervall jeder Ereigniseintrag im Fragment in "Ticks".
Ereignisse|Jedes Ereignis enthält die Flächen erkannt und innerhalb dieser Laufzeit überwacht. Es ist ein Array von Arrays von Ereignissen. Äußere Array stellt ein Zeitintervall dar. Das interne Array besteht aus 0 oder mehr Ereignisse zu diesem Zeitpunkt. Eine leere Klammer [] bedeutet, dass keine Flächen erkannt wurden.
ID| Die ID der Fläche nachverfolgt wird. Diese Nummer kann versehentlich ändern, wenn ein Gesicht nicht erkannt wird. Eine bestimmte Person sollten dieselbe ID während des gesamten Videos, aber dies kann nicht garantiert werden, aufgrund der Erkennung Algorithmus (Verschluss usw.)
X, Y|Die oberen X und Y-Koordinaten der Fläche umgebenden Felds in einem normalisierten Maßstab zwischen 0,0 und 1,0. <br/>X- und Y Koordinaten sind relativ, Querformat, so haben Sie Hochformat video (oder Kopf, bei iOS), Sie die Koordinaten entsprechend umsetzen müssen.
Breite, Höhe|Die Breite und Höhe der Fläche umgebenden Felds in einem normalisierten Maßstab zwischen 0,0 und 1,0.
facesDetected|Diese befindet sich am Ende des JSON-Ergebnissen und Anzahl der Flächen, die der Algorithmus im Video erkannt zusammengefasst. Da die IDs versehentlich zurückgesetzt werden können, wenn ein Gesicht nicht erkannt wird (z. B. Fläche geht Bildschirm sieht entfernt), diese Zahl möglicherweise nicht immer die tatsächliche Anzahl der Flächen im Video gleich.

Face-Detektor verwendet Techniken Fragmentierung (wo die Metadaten kann in einem zeitbasierten Blöcken aufgeteilt und können nur) und Segmentierung (wo sind die Ereignisse aufgeteilt bei zu groß werden). Einige einfachen Berechnungen helfen Ihnen die Daten transformieren. Beispielsweise, wenn ein Ereignis auf 6300 (Ticks) mit einer Zeitskala von 2997 (Ticks/s) und Framerate von 29,97 (Frames/s), dann:

- Start-Zeitskala 2.1 Sekunden
- Sekunden x (Framerate/Zeitskala) = 63 Frames

Im folgenden ist ein einfaches Beispiel Extrahieren von JSON in eine pro Rahmenformat gesichtserkennung und Überwachung:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Stellen Sie Erkennung Eingabe und Ausgabe Beispiel

###<a name="input-video"></a>Input video

[Input Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Taskkonfiguration (Voreinstellung)

Beim Erstellen eines Tasks mit **Azure Media Gesicht**Geben Sie eine Voreinstellung Konfiguration. Die folgende Konfiguration Voreinstellung ist nur für gesichtserkennung.

    {"version":"1.0"}

###<a name="json-output"></a>JSON-Ausgabe

Im folgende Beispiel JSON-Ausgabe wurde abgeschnitten.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Emotionen Erkennung und-Ausgabe Beispiel


###<a name="input-video"></a>Input video

[Input Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Taskkonfiguration (Voreinstellung)

Beim Erstellen eines Tasks mit **Azure Media Gesicht**Geben Sie eine Voreinstellung Konfiguration. Die folgende Konfiguration Voreinstellung gibt an, dass JSON basierend auf Emotion Erkennung erstellen.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Attribut Beschreibung

Name des Attributs|Beschreibung
---|---
Modus|Flächen: Face nur Erkennung <br/>AggregateEmotion: Return durchschnittliche Emotion Werte für alle Flächen im Rahmen.
AggregateEmotionWindowMs|Verwenden Sie AggregateEmotion-Modus. Gibt die Länge des Videos zu jeder Aggregatergebnis in Millisekunden verwendet.
AggregateEmotionIntervalMs|Verwenden Sie AggregateEmotion-Modus. Gibt an, mit welcher Häufigkeit aggregierte Ergebnisse.

####<a name="aggregate-defaults"></a>Aggregate Standardwerte

Unten werden die Werte für die Eintellungen Fenster und Intervall empfohlen. AggregateEmotionWindowMs sollte länger als AggregateEmotionIntervalMs.

   |Standard (s)|Max(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0,25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>JSON-Ausgabe

JSON Ausgabe für aggregate Emotion (gekürzt):
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Grenzen

- Unterstützte video Eingabeformate enthalten MP4, MOV und WMV.
- Erkennbare Gesicht Bereich beträgt 24 x 24 bis 2048 x 2048 Pixel. Die Flächen außerhalb dieses Bereichs werden nicht erkannt.
- Jedes Video ist die maximale Anzahl der zurückgegebenen Flächen 64.
- Einige Flächen möglicherweise nicht durch Herausforderung erkannt; z. B. sehr große Elementflächenwinkel (Head Pose) und große Verschluss. Vorne und in der Nähe Frontal Flächen haben die besten Ergebnisse.


## <a name="sample-code"></a>Beispielcode

Das folgende Programm veranschaulicht, wie:

1. Eine Anlage erstellen und Laden einer Mediendatei in der Anlage.
1. Erstellt ein Projekt basierend auf eine Konfigurationsdatei mit den folgenden Json-Voreinstellung Gesicht Erkennung Aufgabe. 
                    
        {
            "version": "1.0"
        }

1. Die Ausgabe JSON-Dateien herunter. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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

##<a name="related-links"></a>Verwandte links

[Analytics Übersicht über Services Azure Media](media-services-analytics-overview.md)

[Azure Media Analytics-demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)