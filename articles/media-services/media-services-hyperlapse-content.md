<properties
    pageTitle="Mediendateien mit Azure Media Hyperlapse Hyperlapse | Microsoft Azure"
    description="Azure Media Hyperlapse erstellt reibungslosen Ablauf Zeit Videos vom ersten Person oder Aktion Kamera Inhalt. In diesem Thema veranschaulicht, wie mit Media Indexer."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Hyperlapse Mediendateien mit Azure Media Hyperlapse

Azure Media Hyperlapse ist eine Media Prozessor (MP), der reibungslose Ablauf Zeit Videos aus Ego- oder Aktion Kamera Inhalt erstellt.  [Telefon Hyperlapse Mobile](http://aka.ms/hyperlapse)zu Microsoft Research desktop Hyperlapse Pro nebengeordneten cloudbasierten Microsoft Hyperlapse für Azure Media Services nutzt massiv Azure Media Services Media-Processing-Plattform zum horizontalen Skalieren und parallelisieren Massenkopieren Hyperlapse verarbeiten.

>[AZURE.IMPORTANT]Microsoft Hyperlapse soll Ego-Inhalt eine bewegliche Kamera am besten.  Obwohl noch Kamera Aufnahmen noch möglich, werden die Leistung und Qualität der Azure Media Hyperlapse Media Prozessor für andere Arten von Inhalten gewährleistet.  Weitere Informationen zu Microsoft Hyperlapse für Azure Media Services finden Sie unter Beispielvideos und Anzeigen der [einführenden Blogbeitrag](http://aka.ms/azurehyperlapseblog) von public preview

Ein Projekt als wird Azure Media Hyperlapse Eingabedatei ein MP4, MOV und WMV Anlage eine Konfigurationsdatei, die angibt, welche Videoframes Zeit verstrichen sein soll und welche Geschwindigkeit (z. B. die ersten 10.000 frames mit 2 X).  Die Ausgabe ist eine Formatvariante stabilisiert und Zeit abgelaufen Eingabezeile video.

Die neuesten Updates Azure Media Hyperlapse anzeigen Sie [Media Services Blogs](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse Anlage

Zuerst müssen Sie die gewünschte Eingabedatei Azure Media Services hochladen.  Weitere Informationen über Konzepte hochladen und Verwalten von Inhalten finden Sie im [Content-Management-Artikel](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Voreinstellung für Hyperlapse Konfiguration

Sobald Ihre Inhalte in Ihrem Media Services-Konto ist, müssen Sie die Voreinstellung Konfiguration erstellen.  Die folgende Tabelle erläutert die benutzerdefinierten Felder:

 Feld | Beschreibung
-------|-------------
StartFrame|Der Frame, auf dem die Microsoft Hyperlapse Verarbeitung beginnen soll.
Mit der NumFrames|Die Anzahl der Frames zu
Geschwindigkeit|Der Faktor, input Video zu beschleunigen.

Das folgende ist ein Beispiel einer Konfigurationsdatei konform in XML und JSON:

**XML-Voreinstellung:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**JSON-Voreinstellung:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>Microsoft Hyperlapse AMS .NET SDK

Die folgende Methode lädt eine Mediendatei als Vermögenswert und erstellt ein Projekt mit dem Azure Media Hyperlapse Media-Prozessor.

> [AZURE.NOTE] Sie haben bereits eine CloudMediaContext im Bereich mit dem Namen "Kontext" für diesen Code zu.  Erfahren Sie mehr zu diesem [Content Management Artikel](media-services-dotnet-get-started.md)lesen.

> [AZURE.NOTE] Das String-Argument "HyperConfig" soll eine konforme Konfiguration in JSON oder XML-Vorgaben, wie oben beschrieben.

static Bool RunHyperlapseJob (Zeichenfolge, Zeichenfolgenausgabe Zeichenfolge HyperConfig) {/ / create Anlage mit Eingabedatei IAsset = Context. Anlagen. CreateAssetAndUploadSingleFile (Eingabe "Meine Hyperlapse Input", AssetCreationOptions.None).

Greifen Sie Instanzen von Azure Media Hyperlapse MP IMediaProcessor VA = Context. MediaProcessors. GetLatestMediaProcessorByName ("Azure Media Hyperlapse");

Erstellen Sie Auftrag mit Hyperlapse Aufgabe IJob = Context. Aufträge. Erstellen Sie (String.Format ("Hyperlapse {0}", Input)).

Wenn (String.IsNullOrEmpty(hyperConfig)) {/ / Config nicht leere return False;}

HyperConfig = File.ReadAllText(hyperConfig);

ITask HyperlapseTask = Auftrag. Tasks.AddNew ("Hyperlapse Task" mp, HyperConfig, TaskOptions.None); hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse Output", AssetCreationOptions.None);


Auftrag. Ein;

Status drucken und Aufgaben Aufgabe ProgressPrintTask Abfragen erstellen neue Task(() = = > {}

IJob JobQuery = Null; Führen Sie {Var ProgressContext = Context; JobQuery = progressContext.Jobs. Wo (j = > j.Id == Auftrag. ID). First(); Console.WriteLine (Zeichenfolge. Format ("{0} {1} \t \t {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Progress)); Thread.Sleep(10000); } während (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Unterstützte Dateitypen

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Verwandte links

[Analytics Übersicht über Services Azure Media](media-services-analytics-overview.md)

[Azure Media Analytics-demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
