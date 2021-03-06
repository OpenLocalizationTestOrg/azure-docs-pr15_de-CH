<properties 
    pageTitle="Erweiterte Media Encoder Standard Codierung" 
    description="In diesem Thema veranschaulicht, wie führen Sie erweiterte Codierung Media Encoder Standard Aufgabe Vorgaben anpassen. Das Thema beschreibt, wie Sie Media Services .NET SDK ein Kodierung Aufgabe und Projekt. Es wird gezeigt, wie benutzerdefinierte Vorgaben Codierungsauftrags angeben." 
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
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Erweiterte Media Encoder Standard Codierung

##<a name="overview"></a>Übersicht

In diesem Thema veranschaulicht, wie erweiterte Codierung Media Encoder Standard Aufgaben. Das Thema beschreibt, [wie Sie .NET ein Kodierung Aufgabe und ein Projekt, das diese Aufgabe ausführt](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Es wird gezeigt, wie benutzerdefinierte Vorgaben Codierung Vorgang angeben. Beschreibung der Elemente, die die Vorgaben verwendet, finden Sie [Dieses Dokument](https://msdn.microsoft.com/library/mt269962.aspx). 

Die benutzerdefinierten Voreinstellungen, die Codierung Aufgaben werden veranschaulicht:

- [Generieren von Miniaturansichten](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [Kürzen Sie eine Video (Clipping)](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Erstellen Sie eine Überlagerung](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Beim hat keine fügen Sie automatische Audiospur ein](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Automatische Deinterlacing deaktivieren](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Nur-Audio-Voreinstellungen](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [Verketten von zwei oder mehr Videodateien](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Zuschneiden von Videos mit Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Wenn eine Videospur hat kein video](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Drehen eines Videos](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>Kodierung mit Media Services .NET SDK

Im folgenden Codebeispiel Media Services .NET SDK folgende Aufgaben:

- Erstellen einer Codierung.
- Abrufen eines Verweises auf die Media Encoder Standard Encoder.
- Laden Sie die XML- oder JSON-Vorgaben. Sie können XML oder JSON (z. B. [XML](media-services-custom-mes-presets-with-dotnet.md#xml) oder [JSON](media-services-custom-mes-presets-with-dotnet.md#json) in einer Datei und verwenden Sie den folgenden Code zum Laden der Datei.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Ein Kodierung Vorgang dem Projekt hinzufügen. 
- Geben Sie input Anlage codiert werden.
- Erstellen Sie Vermögenswert Ausgabe, die die codierte Anlage enthält.
- Fügen Sie einen Ereignishandler, um den Job-Status überprüfen.
- Senden Sie den Auftrag.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
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
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
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
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
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
        
            }
        }


##<a name="support-for-relative-sizes"></a>Unterstützung für relative Größen

Beim Generieren von Miniaturansichten der müssen Sie nicht immer Ausgabe Breite und Höhe in Pixel angeben. Sie können sie in Prozentsätze [1 % und 100 %].

###<a name="json-preset"></a>JSON-Voreinstellung 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>XML-Voreinstellung
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>Generieren von Miniaturansichten

Dieser Abschnitt zeigt, wie eine Voreinstellung anpassen, die Miniaturansichten generiert. Die nachstehend definierte Voreinstellung enthält Informationen wie Sie Ihre Datei sowie Informationen zum Generieren von Miniaturansichten codieren möchten. Sie können eines der MWS Vorgaben dokumentiert [hier](https://msdn.microsoft.com/library/mt269960.aspx) und Miniaturansichten generierten Code hinzufügen.  

>[AZURE.NOTE]Die **SceneChangeDetection** der folgenden vordefinierten kann nur werden eingestellt auf true fest, wenn Sie ein einzelnes video Bitrate codieren. Wenn Sie Multi-Bitrate Video codieren und **SceneChangeDetection** auf True festgelegt, zurückgegeben der Encoder ein Fehler.  


Informationen zum Schema finden Sie [in diesem](https://msdn.microsoft.com/library/mt269962.aspx) Thema.

Überprüfen Sie im Abschnitt [Hinweise](media-services-custom-mes-presets-with-dotnet.md#considerations) .

###<a id="json"></a>JSON-Voreinstellung


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>XML-Voreinstellung


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Hinweise

Folgendes gilt:

- Die Verwendung von expliziten Zeitstempel Anfangsbereich/Schritt setzt voraus, dass die Eingabequelle mindestens 1 Minute.
- JPG, Png/BmpImage starten, Schritt und Attribute den Bereich – können diese interpretiert als:

    - Frame-Nummer sind nicht Negative ganze Zahlen, z. B. "Start": "120"
    - Relativ zur Quelle Dauer, wenn als % Suffix, z. B. "Start": "15 %" oder
    - Zeitstempel, wenn ss ausgedrückt. Format, z. B. "Start": "00: 01:00"

    Sie können mischen und Notationen Sie bitte.
    
    Start unterstützt darüber hinaus auch ein Spezialmakro: {am besten}, der versucht zu ermitteln, "Interesse" Bildposition Content Hinweis: (Schritt und Bereich werden ignoriert beim Start {besten} festgelegt ist)
    
    - Standard: Start: {beste}
- Ausgabeformat muss explizit für jedes Bildformat: Jpg, Png/BmpFormat. Vorhanden, entspricht MES JpgVideo, JpgFormat und so weiter. OutputFormat stellt neue Image-Codec bestimmte Makro: {Index} werden muss vorhanden (und nur einmal) für Image-Ausgabe.

##<a id="trim_video"></a>Kürzen Sie eine Video (Clipping)

Zum Ändern der Encoder-Vorgaben zu clip input Video die Eingabe einer sogenannten Mezzanine oder bei Bedarf Datei ist kürzen erörtert. Der Encoder kann auch zum clip oder eine Anlage erfasst oder archiviert live-Streams Zuschneiden – Details hierzu stehen in [diesem Blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Um Ihre Videos zuzuschneiden, können Sie alle der MWS Vorgaben dokumentiert [hier](https://msdn.microsoft.com/library/mt269960.aspx) und **Quellen** Element (siehe unten) ändern. Der Wert von StartTime muss absolute input Video-Zeitstempel überein. Beispielsweise hat der erste Frame des Videos input Zeitstempel 12:00:10.000, dann StartTime sollte mindestens 12:00:10.000 und höher. Im folgenden Beispiel wird angenommen, dass input Video starten Zeitstempel von NULL hat. **Quellen** am Anfang der Voreinstellung eingefügt. 
 
###<a id="json"></a>JSON-Voreinstellung
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    } 

###<a name="xml-preset"></a>XML-Voreinstellung
    
Um Ihre Videos zuzuschneiden, können Sie alle der MWS Vorgaben dokumentiert [hier](https://msdn.microsoft.com/library/mt269960.aspx) und **Quellen** Element (siehe unten) ändern.

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Erstellen Sie eine Überlagerung

Media Encoder Standard können Sie ein Bild auf einer vorhandenen Bild überlagert. Derzeit unterstützt die folgenden Formate: Png, Jpg, Gif und Bmp. Die nachstehend definierte Voreinstellung ist ein einfaches Beispiel einer Videooverlay.

Neben einer Voreinstellungsdatei, können Sie Media Services stellen die Datei in der Anlage das Overlay-Bild und die Datei das Quellvideo auf dem Bild überlagern lassen. Die Datei muss die **primäre** Datei. 

.NET Beispiel oben definiert zwei Funktionen: **UploadMediaFilesFromFolder** und **EncodeWithOverlay**. Die Funktion UploadMediaFilesFromFolder uploads von Dateien aus einem Ordner (z. B. BigBuckBunny.mp4 und Image001.png) und legt die mp4-Datei die primäre Datei in der Anlage. Die **EncodeWithOverlay** -Funktion verwendet benutzerdefinierten Vorgabendatei wurde übergeben (z. B. die folgende Einstellung) die Codierung Aufgabe erstellen. 

>[AZURE.NOTE]Aktuelle Grenzen:
>
>Die Deckkraft der Überlagerung wird nicht unterstützt.
>
>Die Quellvideodatei und die Überlagerungsdatei müssen in derselben Anlage und die Videodatei als Primärdatei in dieser Anlage festgelegt werden soll.

###<a name="json-preset"></a>JSON-Voreinstellung
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>XML-Voreinstellung
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Beim hat keine fügen Sie automatische Audiospur ein

Standardmäßig eine Eingabe an den Encoder zu senden, der nur Videos und kein Audio enthält enthält Ausgabe Anlage, die nur Daten enthalten. Einige Spieler möglicherweise nicht so Ausgabeströme verarbeiten. Diese Einstellung können Sie erzwingen den Encoder die Ausgabe in diesem Szenario eine automatische Audiospur hinzufügen.

Erzwingen den Encoder, und die Anlage hat keine automatische Audiospur enthält als Wert "InsertSilenceIfNoAudio".

Sie können einem der MWS Vorgaben dokumentiert [hier](https://msdn.microsoft.com/library/mt269960.aspx)und die folgende Änderung:

###<a name="json-preset"></a>JSON-Voreinstellung

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>XML-Voreinstellung

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Automatische Deinterlacing deaktivieren

Kunden müssen nicht tun möchten Interlace-Inhalt automatisch de-interlaced sein. (Standard) wird die automatische Deinterlacing MES ist die automatische Erkennung von interlaced-Frames als nur Rahmen gekennzeichnet mit Zeilensprung de verschränkt.

Sie können Deaktivieren der automatischen Deinterlacing. Diese Option wird nicht empfohlen.

###<a name="json-preset"></a>JSON-Voreinstellung
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>XML-Voreinstellung
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Nur-Audio-Voreinstellungen

Dieser Abschnitt zeigt zwei nur Audio MWS-Voreinstellungen: AAC-Audio und AAC gute Qualität Audio.

###<a name="aac-audio"></a>AAC-Audio 

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>AAC gute Tonqualität

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>Verketten von zwei oder mehr Videodateien

Das folgende Beispiel veranschaulicht, wie eine Voreinstellung zum Verketten von zwei oder mehr Videodateien erstellen. Das häufigste Szenario ist, wenn eine Kopfzeile oder ein Anhänger Hauptvideo hinzufügen. Der Verwendungszweck ist wenn die Videodateien gemeinsam bearbeitet Eigenschaften (Auflösung, Framerate, Anzahl der Audiospur usw.) freigeben. Vorsicht nicht um Videos von unterschiedlichen Bildraten und verschiedene audio-Tracks mischen.

###<a name="requirements-and-considerations"></a>Vorschriften und Hinweise

- Videoeingänge sollte nur eine Audiospur enthalten.
- Videoeingänge müssen alle dieselbe Framerate.
- Sie müssen Sondervermögen Videos hochladen und Videos als Primärdatei in jede Anlage.
- Sie müssen die Dauer des Videos kennen.
- Voreingestellte Beispiele wird vorausgesetzt, dass die Eingabe Videos mit einem Zeitstempel von null beginnen. Sie müssen die Startzeit ändern Videos haben andere starten Timestamp wie normalerweise mit aktiven Archiven.
- JSON-Voreinstellung kann explizite Verweise auf Werte Hilfethema input Anlagen.
- Der Code setzt voraus, dass JSON-Voreinstellung in eine lokale Datei wie "C:\supportFiles\preset.json" gespeichert wurde. Es wird davon ausgegangen, dass zwei Anlagen hochladen zwei Videodateien erstellt wurden und die resultierende AssetID Werte kennen.
- Der Codeausschnitt und JSON Voreinstellung zeigt ein Beispiel für zwei Videodateien verketten. Sie können es auf mehr als zwei Videos nach:

    1. Aufrufen von Task. InputAssets.Add() wiederholt, um weitere Videos hinzufügen.
    2. Ausführenden entspricht bearbeitet "Quellen"-Element in JSON, indem Sie weitere Einträge in der Reihenfolge. 


###<a name="net-code"></a>.NET code

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>JSON-Voreinstellung

Aktualisieren Sie die benutzerdefinierte Voreinstellung mit den Ids der Anlagen, die Sie verketten möchten und mit der entsprechenden Zeit für jedes Video.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="crop"></a>Zuschneiden von Videos mit Media Encoder Standard

Finden Sie [Videos Zuschneiden Media Encoder Standard](media-services-crop-video.md) .

##<a id="no_video"></a>Wenn eine Videospur hat kein video

Standardmäßig eine Eingabe an den Encoder zu senden, die nur Audio und kein Video enthält enthält Anlage Ausgabe Dateien, die nur Audiodaten enthalten. Einige Spieler einschließlich Azure Media Player (siehe [dieses](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) mit diesen Streams möglicherweise nicht. Diese Einstellung können Sie erzwingen den Encoder die Ausgabe in diesem Szenario eine monochrome Videospur hinzu. 

>[AZURE.NOTE]Erzwingen des Encoders eine Videospur Ausgabe einfügen vergrößert die Ausgabe Anlage und damit die Kosten für die Codierung Aufgabe. Führen Sie hat diese resultierende Erhöhung nur eine geringfügige Auswirkung auf die monatlichen Gebühren überprüfen.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Einfügen von Video mit der niedrigsten bitrate

Angenommen, Verwendung einer mehrere Bitrate Codierung wie voreingestellte ["H264 mehrere Bitrate 720p"](https://msdn.microsoft.com/library/mt269960.aspx) den gesamten Eingaben Katalog für streaming codieren aus Video- und Audio-Dateien enthält. In diesem Szenario bei Eingabe kein Bild wurde möchten Sie erzwingen den Encoder eine monochrome Videospur mit nur der niedrigsten Bitrate mit jeder Ausgabe-Bitrate Video einfügen einfügen. Dazu müssen Sie das Flag "InsertBlackIfNoVideoBottomLayerOnly" angeben.

Sie können einem der MWS Vorgaben dokumentiert [hier](https://msdn.microsoft.com/library/mt269960.aspx)und die folgende Änderung:

#### <a name="json-preset"></a>JSON-Voreinstellung

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-Voreinstellung

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Bitraten Ausgabe auf Video einfügen

Angenommen, Verwendung einer mehrere Bitrate Codierung wie voreingestellte ["H264 mehrere Bitrate 720p](https://msdn.microsoft.com/library/mt269960.aspx) den gesamten Eingaben Katalog für streaming codieren aus Video- und Audio-Dateien enthält. In diesem Szenario bei Eingabe kein Bild wurde möchten Sie erzwingen den Encoder einer monochromen Videospur überhaupt Bitraten Ausgabe einfügen. Dadurch wird sichergestellt, dass die Ausgabe sind alle homogenen Anzahl Video-und Audiospuren. Dazu müssen Sie das Flag "InsertBlackIfNoVideo" angeben.

Sie können einem der MWS Vorgaben dokumentiert [hier](https://msdn.microsoft.com/library/mt269960.aspx)und die folgende Änderung:

#### <a name="json-preset"></a>JSON-Voreinstellung

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-Voreinstellung
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Drehen eines Videos

[Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) unterstützt Drehung um Winkel 0/90/180/270. Das Standardverhalten ist "Auto", bei denen versucht die Drehung Metadaten eingehende Videodatei und ausgleichen. Gehören Sie das folgende **Quellen** Element eine der Vorgaben definiert [hier](http://msdn.microsoft.com/library/azure/mt269960.aspx):

### <a name="json-preset"></a>JSON-Voreinstellung

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>XML-Voreinstellung

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

[Weitere Informationen](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) Siehe auch auf Encoder die Breite und Höhe Einstellungen in der Voreinstellung Interpretation Drehung Vergütung ausgelöst.

Den Wert "0" können an den Encoder Drehung Metadaten ggf. video Eingabe ignoriert. 


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch 

[Mediendienste Codierung (Übersicht)](media-services-encode-asset.md)
