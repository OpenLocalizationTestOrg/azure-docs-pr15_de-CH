<properties
    pageTitle="Verwenden von Azure Media Analytics Textinhalt in Videodateien in digitalen Text konvertieren | Microsoft Azure"
    description="Azure Media Analytics OCR (texterkennung) können Sie Textinhalt in Videodateien in bearbeitbare, durchsuchbaren digitalen Text konvertieren.  Dadurch werden die Extraktion von aussagekräftigen Metadaten aus das Videosignal Medien zu automatisieren."
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
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Verwenden von Azure Media Analytics Textinhalt in Videodateien in digitalen Text konvertieren 

##<a name="overview"></a>Übersicht

Wenn Sie Textinhalt aus Videodateien und bearbeitbaren durchsuchbaren digitalen Text erstellen möchten, verwenden Sie Azure Media Analytics OCR (texterkennung). Dieser Prozessor Azure Media Textinhalt Sie Videodateien erkennt und Textdateien zur Verwendung generiert. OCR ermöglicht die Extraktion von aussagekräftigen Metadaten aus das Videosignal Medien zu automatisieren.

Bei Verwendung in Verbindung mit einer Suchmaschine können problemlos Medien Text indiziert, und verbessern die Auffindbarkeit Ihres Contents. Dies ist äußerst nützlich, hochgradig Text Video Videoaufnahme oder Bildschirmaufnahme einer Diashow. Azure OCR Media Prozessor ist optimiert für digitale Text.

**Azure Media OCR** Media Prozessor ist zurzeit in der Vorschau.

Dieses Thema enthält Informationen über **Azure Media OCR** und veranschaulicht, wie sie mit Media Services SDK für .NET. Weitere Informationen und Beispiele finden Sie in [diesem Blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##<a name="ocr-input-files"></a>OCR-Dateien

Videodateien. Derzeit werden folgende Formate unterstützt: MP4, MOV und WMV.

##<a name="task-configuration"></a>Task-Konfiguration 

Taskkonfiguration (Voreinstellung). Beim Erstellen eines Tasks mit **Azure Media OCR**müssen Sie eine voreingestellte mit JSON oder XML-Konfiguration angeben. 

###<a name="attribute-descriptions"></a>Attribut Beschreibung

Name des Attributs|Beschreibung
---|---
Sprache|(optional) beschreibt die Sprache des Textes, suchen. Einer der folgenden: automatische Erkennung (Standard), Arabisch, ChineseSimplified, ChineseTraditional, Tschechisch, Dänisch, Niederländisch, Englisch, Finnisch, Französisch, Deutsch, Griechisch, Ungarisch, Italienisch, Japanisch, Koreanisch, Norwegisch, Polnisch, Portugiesisch, Rumänisch, Russisch, SerbianCyrillic, SerbianLatin, Slowakisch, Spanisch, Schwedisch, Türkisch.
TextOrientation|(optional) beschreibt die Ausrichtung von Text, suchen.  "Links" bedeutet, dass alle Buchstaben oben links zeigt.  Standardtext (wie in einem Buch finden) kann "" ausgerichtet aufgerufen werden.  Einer der folgenden: automatische Erkennung (Standard), oben, rechts, unten, links.
TimeInterval|(optional) beschreibt die Sampling-Rate.  Standard ist alle 1/2 Sekunde.<br/>JSON-Format – ss. SSS (standardmäßig 00:00:00.500)<br/>XML Format W3C-XSD Dauer primitiven (Standard PT0.5)
DetectRegions|(optional) Ein Objektarray DetectRegion geben die Bereiche innerhalb des Videoframes, Text erkennen.<br/>Ein DetectRegion-Objekt besteht aus den folgenden vier ganzzahligen Werte:<br/>Links – Pixel vom linken Rand<br/>Top-Pixel vom oberen Seitenrand<br/>Breite des Bereichs in Pixel Breite<br/>Höhe – der Region in Pixel

####<a name="json-preset-example"></a>Voreingestellte JSON-Beispiel
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>Vordefinierte XML-Beispiel

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>OCR-Ausgabedateien

Die Ausgabe des Prozessors Media OCR ist eine JSON-Datei.

###<a name="elements-of-the-output-json-file"></a>Elemente der JSON-Ausgabedatei

Video OCR-Ausgabe liefert Zeit segmentierten Daten auf die Zeichen in Ihrem Video.  Können Sie Attribute wie Sprache oder Ausrichtung, Büro-auf genau die Wörter, die Sie analysieren möchten. 

Die Ausgabe enthält die folgenden Attribute:

Element|Beschreibung
---|---
Zeitskala|"Ticks" pro Sekunde des Videos
Offset|Zeit für Zeitstempel versetzt. In Version 1.0 von Video-APIs werden dies immer 0.
Bildrate|Bilder pro Sekunde des Videos
Breite|Breite des Videos in Pixel
Höhe|Höhe des Videos in Pixel
Fragmente|Array von zeitbasierte Datenblöcke in den Metadaten chunked video
Starten|Startzeit eines Fragments in "Ticks"
Dauer|Länge des Fragments in "Ticks"
Intervall|jedes Ereignis im angegebenen Fragment Zeitintervall
Ereignisse|Array mit Bereichen
Region|Wörter oder Ausdrücke entdeckt Objekt darstellt
Sprache|Sprache des Texts innerhalb eines Bereichs
Ausrichtung|Ausrichtung von Text innerhalb eines Bereichs
Zeilen|Array von Textzeilen innerhalb eines Bereichs
Text|der eigentliche text

###<a name="json-output-example"></a>Beispielausgabe JSON

Das folgende Ausgabebeispiel enthält allgemeine Videoinformationen und mehrere video-Fragmente. In jedem video Fragment enthält jeder Region von MP OCR Sprache erkannt wird und die Ausrichtung von Text. Der Bereich enthält auch alle Word-Zeile in diesem Bereich den Text der Zeile, die Zeilenposition und alle Word-Informationen (Word-Inhalt, Position und vertrauen) in dieser Zeile. Folgendes ist ein Beispiel, und ich habe einige Kommentare Inline.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Beispielcode

Das folgende Programm veranschaulicht, wie:

1. Eine Anlage erstellen und Laden einer Mediendatei in der Anlage.
1. Erstellt ein Projekt mit einer OCR-Konfiguration-Voreinstellung Datei.
1. Die Ausgabe JSON-Dateien herunter. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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
