<properties
    pageTitle="Anträge mit Azure Media Analytics erkennen | Microsoft Azure"
    description="Azure Media Bewegungsmelder Media Prozessor (MP) können Sie Abschnitte der Zinsen in einem ansonsten lange und problemlosen Video effizient identifizieren."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Anträge mit Azure Media Analytics erkennen

##<a name="overview"></a>Übersicht

**Azure Media Bewegungsmelder** Media Prozessor (MP) können Sie Abschnitte der Zinsen in einem ansonsten lange und problemlosen Video effizient identifizieren. Bewegungsmelder kann auf Kamera verwendet werden, um Abschnitte des Videos zu identifizieren, an dem Bewegung stattfindet. Es generiert eine JSON-Datei enthält Metadaten mit Zeitstempel und den umgebenden Bereich, in dem das Ereignis aufgetreten ist.

Sicherheit-video-Feeds ausgerichtet, kann diese Technologie Bewegung in entsprechende Ereignisse und fälschlich wie Schatten und Beleuchtung kategorisieren. Dadurch können Sie für Sicherheitshinweise Kamera Feeds endlose irrelevante Ereignisse und Momente von Interesse extrahieren sehr langen überwachungsvideos aufführen.

**Azure Media Bewegungsmelder** MP ist zurzeit in der Vorschau.

In diesem Thema finden Sie Einzelheiten über **Azure Media Bewegungsmelder** und veranschaulicht, wie sie mit Media Services SDK für .NET


##<a name="motion-detector-input-files"></a>Motion Detektor Dateien

Videodateien. Derzeit werden folgende Formate unterstützt: MP4, MOV und WMV.

##<a name="task-configuration-preset"></a>Taskkonfiguration (Voreinstellung)

Beim Erstellen eines Tasks mit **Azure Media Bewegungsmelder**müssen Sie eine Voreinstellung Konfiguration angeben. 

###<a name="parameters"></a>Parameter

Sie können die folgenden Parameter:

Name|Optionen|Beschreibung|Standard
---|---|---|---
sensitivityLevel|Zeichenfolge: 'Niedrig', 'Mittel', 'high'|Legt die Vertraulichkeitsstufe auf die Anträge gemeldet wird. Passen Sie um falsche Positiva anpassen an|'Mittel'
frameSamplingValue|Positive Ganzzahl|Legt die Frequenz der Algorithmus ausgeführt wird. 1 entspricht jedes Bild 2 bedeutet, dass jede 2. Rahmen usw.;|1
detectLightChange|Boolean: 'true', 'false'|Legt fest, ob die Lichtverhältnisse ändern die Ergebnisse gemeldet werden|'False'
mergeTimeThreshold|Xs-Uhrzeit: Hh: mm:<br/>Beispiel: 00:00:03|Gibt das Zeitfenster zwischen Bewegung, 2 Ereignisse kombiniert und 1 gemeldet werden.|00:00:00
detectionZones|Ein Array von erkennungszonen:<br/>-Erkennung ist ein Array von 3 oder mehr Punkte<br/>-Ist ein x und y Koordinate von 0 auf 1.|Beschreibt die Liste der polygonalen erkennungszonen verwendet werden.<br/>Ergebnisse mit Zonen als ID, wobei der erste 'Id' gemeldet: 0|Einzelne Zone umfasst den gesamten Rahmen.

###<a name="json-example"></a>JSON-Beispiel

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Motion Detektor Ausgabedateien

Ein Motion-Erkennung Auftrag liefert eine JSON Ausgabe Vermögenswertes bewegungsalarme und ihre Kategorien im Video beschreibt. Die Datei enthält Informationen über die Zeit und die Dauer der Bewegung des Videos gefunden.

Motion Detektor API enthält Indikatoren sind Objekte in Bewegung in eine feste Hintergrundvideo (z. B. eine Videoüberwachung). Der Bewegungsmelder ist Fehlalarme wie Licht und Schatten zu trainiert. Aktuelle Grenzen der Algorithmen enthalten Nacht Vision Videos, halb transparente Objekte und Objekte.

###<a id="output_elements"></a>Elemente der JSON-Ausgabedatei

>[AZURE.NOTE]In der neuesten Version Ausgabe JSON-Format wurde geändert und möglicherweise für einige Kunden eine unterbrechende Änderung dar.

Die folgende Tabelle beschreibt die Elemente der JSON-Ausgabedatei.

Element|Beschreibung
---|---
Version|Dies bezieht sich auf die Version des Video-API. Die aktuelle Version ist 2.
Zeitskala|"Ticks" pro Sekunde des Videos.
Offset|Offset der für Zeitstempel in "Ticks". In Version 1.0 von Video-APIs werden dies immer 0. In Zukunft können Szenarios unterstützt diesen Wert ändern.
Bildrate|Bilder pro Sekunde des Videos.
Breite, Höhe|Bezieht sich auf die Breite und Höhe des Videos in Pixel.
Starten|Timestamp für den Start in "Ticks".
Dauer|Die Länge des Ereignisses "Ticks".
Intervall|Das Intervall der einzelnen Einträge im Ereignis "Ticks".
Ereignisse|Jedes Ereignis Fragment enthält die Bewegung innerhalb dieser Dauer.
Typ|Dies ist in der aktuellen Version immer "2" für generische Bewegung. Diese Bezeichnung gibt Video APIs Flexibilität kategorisieren Bewegung in zukünftigen Versionen.
RegionID|Wie bereits dargelegt, werden diese immer 0 in dieser Version. Diese Bezeichnung Flexibilität Video-API Bewegung in verschiedenen Regionen in zukünftigen Versionen gefunden.
Regionen|Bezieht sich auf Bereich im Video, Bewegung kümmern. <br/><br/>-"Id" stellt das Gebiet – in dieser Version ist nur eine ID 0. <br/>-"Typ" Form Region für Bewegung wichtig darstellt. Derzeit sind "Rechteck" und "Polygon" unterstützt.<br/> Angegebene "Rechteck" Region ist Dimensionen in X, Y, Width und Height. Die X- und Y-Koordinaten darstellen links XY-Koordinaten des Bereichs in einem normalisierten Maßstab zwischen 0,0 und 1,0. Die Breite und Höhe dar die Größe des Bereichs in einem normalisierten Maßstab zwischen 0,0 und 1,0. In der aktuellen Version sind X, Y, Width und Height immer fest auf 0, 0 und 1, 1. <br/>Bei Angabe von "Polygon" hat Dimensionen in Punkt. <br/>
Fragmente|Die Metadaten ist in Segmente als Fragmente von chunked. Fragment enthält ein Anfang, Dauer, Anzahl und Ereignisse. Ein Fragment keine Ereignisse bedeutet, dass keine Bewegung, Startzeit und Dauer erkannt wurde.
Klammern]|Jeder Halterung stellt ein Intervall im Ereignis. Leere Klammern Intervall bedeutet, dass keine Bewegung.
Standorte|Dieser neue Eintrag unter Ereignisse listet die Position, an die Bewegung aufgetreten ist. Dies ist als die erkennungszonen.

Nachfolgend ein Beispiel der JSON-Ausgabe

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Grenzen

- Unterstützte video Eingabeformate enthalten MP4, MOV und WMV.
- Bewegungsmelder ist Prag Videos optimiert. Der Algorithmus Schwerpunkt Fehlalarm Beleuchtung ändern, wie Schatten reduzieren.
- Bewegung kann nicht durch technische Probleme festgestellt werden kann; z. B. nachts Vision Videos halb transparente Objekte und Objekte.


## <a name="sample-code"></a>Beispielcode

Das folgende Programm veranschaulicht, wie:

1. Eine Anlage erstellen und Laden einer Mediendatei in der Anlage.
1. Erstellt ein Projekt mit einen video-Bewegungsmelder Erkennung Vorgang basierend auf eine Konfigurationsdatei mit den folgenden Json-Vorgabe. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. Die Ausgabe JSON-Dateien herunter. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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
[Azure Media Services Bewegungsmelder blog](https://azure.microsoft.com/blog/motion-detector-update/)

[Analytics Übersicht über Services Azure Media](media-services-analytics-overview.md)

[Azure Media Analytics-demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
