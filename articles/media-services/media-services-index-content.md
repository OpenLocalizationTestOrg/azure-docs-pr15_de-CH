<properties
    pageTitle="Mediendateien mit Azure Media Indexer die Indizierung"
    description="Azure Media Indexer können Sie Mediendateien durchsuchbar machen und eine Volltext-Protokoll für geschlossene Untertitelung und Schlüsselwörter erstellen. In diesem Thema veranschaulicht, wie mit Media Indexer."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Mediendateien mit Azure Media Indexer die Indizierung


Azure Media Indexer können Sie Mediendateien durchsuchbar machen und eine Volltext-Protokoll für geschlossene Untertitelung und Schlüsselwörter erstellen. Sie können eine Mediendatei oder mehrere Mediendateien in einem Stapel verarbeiten.  

>[AZURE.IMPORTANT] Beim Indizieren von Inhalt müssen Sie Mediendateien mit sehr klare Sprache (ohne Musik, Rauschen, Effekte oder Mikrofon Rauschen) verwendet werden. Beispiele für geeignete Inhalte: Meetings, Vorträge oder Präsentationen aufgezeichnet. Der folgende Inhalt ist möglicherweise nicht für die Indizierung: Filme, Fernsehsendungen, nichts mit gemischtem Audio und Soundeffekte, schlecht mit Hintergrundgeräusche (Rauschen) aufgezeichnet.


Eine Indizierung Arbeit können die folgenden Ausgaben:

- Beschriftungsdateien in den folgenden Formaten geschlossen: **SAMI**, **TTML**und **WebVTT**.

    Untertitel-Dateien enthalten ein Tag namens Erkennbarkeit, welche Faktoren Indizierung Auftrag basiert wie erkennen die Sprache in das Quellvideo.  Den Wert Erkennbarkeit können Sie zum Bildschirm Ausgabedateien Verwendbarkeit. Niedrigste Punktzahl bedeutet schlechte Indizierung Ergebnisse durch Audioqualität.
- Schlüsselwort-Datei (XML).
- Audio Indizierung BLOB-Datei (AIB) für die Verwendung mit SQL Server.

    Weitere Informationen finden Sie unter [Using AIB Files Azure Media Indexer mit SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).


In diesem Thema veranschaulicht die Indizierung Arbeitsplätze **Index Anlage** und **mehrere Dateien**.

Die neuesten Updates Azure Media Indexer finden Sie unter [Media Services Blogs](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Konfiguration und Manifest-Dateien verwenden für Index-tasks

Taskkonfiguration mit einem können Sie Details für die Indizierung Vorgänge angeben. Beispielsweise können Sie die Metadaten für die Mediendatei verwenden. Diese Metadaten dient das Sprachmodul Vokabular erweitern und verbessert die Genauigkeit der Spracherkennung.  Sie können auch die gewünschten Ausgabedateien an.

Sie können mehrere Dateien mit einer Manifestdatei auch gleichzeitig verarbeiten.

Weitere Informationen finden Sie unter [Task Vorgaben für Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Index einer Anlage

Die folgende Methode lädt eine Mediendatei als Vermögenswert und erstellt ein Projekt zum Indizieren der Anlage.

Beachten Sie, dass keine Konfigurationsdatei angegeben, die Mediendatei mit allen Standardeinstellungen indiziert wird.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
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
<!-- __ -->
### <a id="output_files"></a>Ausgabedateien

Ein Indizierung Auftrag generiert standardmäßig die folgenden Ausgabedateien. Die Dateien werden in Ausgabe Anlage gespeichert.

Wenn mehrere input Mediendatei ist generiert Indexer eine Manifestdatei für Auftrag Ausgaben mit dem Namen "JobResult.txt". Für jede Datei eingeben, die resultierenden AIB SAMI, TTML, WebVTT und Schlüsselwort-Dateien sequenziell nummeriert und mit "Alias" benannt.

Dateiname | Beschreibung
----------|------------
__InputFileName.aib__ | Indizierung BLOB-Audiodatei. <br /><br /> Audiodatei Indizierung Blob (AIB) ist eine binäre Datei, die in Microsoft SQL Server mithilfe der Volltextsuche durchsucht werden kann.  Die AIB-Datei ist leistungsfähiger als die einfache Beschriftungsdateien, da Alternativen für jedes Wort ermöglicht eine viel umfangreichere Umgebung enthält. <br/> <br/>Es erfordert die Installation des Add-Ons Indexer SQL auf einem Computer ausgeführten Microsoft SQL Server 2008 oder höher. Suche AIB mit Microsoft SQL bietet Server-Volltextsuche genauere Suchergebnisse als von WAMI erzeugten Untertitel-Dateien suchen. Ist die AIB Synonyme enthält ähnliche Sound Untertiteldateien höchste Confidence Wort für jedes Segment der Audiodaten enthalten. Wenn Worte Suchen von größter Bedeutung, empfohlen wird, die AIB In Verbindung mit Microsoft SQL Server verwenden.<br/><br/> Klicken Sie das Add-on <a href="http://aka.ms/indexersql">Azure Media Indexer SQL Add-on</a>. <br/><br/>Ist auch möglich, andere Suchmaschinen wie Apache Lucene/Solr einfach das Video Untertitel und Schlüsselwort XML-Dateien anhand indizieren dadurch weniger präzise Suchergebnisse.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Geschlossene Beschriftung (CC) Dateien in Formaten SAMI, TTML und WebVTT.<br/><br/>Sie können Audio-und Videodateien Anhörung Behinderte zugänglich machen verwendet werden.<br/><br/>Geschlossene Includedateien Beschriftung ein Tag namens <b>Erkennbarkeit</b> ist die Faktoren wie erkennbare Sprache das Quellvideo Indizierung Auftrag basiert.  Den Wert <b>Erkennbarkeit</b> können Sie zum Bildschirm Ausgabedateien Verwendbarkeit. Niedrigste Punktzahl bedeutet schlechte Indizierung Ergebnisse durch Audioqualität.
__InputFileName.kw.xml<br />InputFileName.info__ |Schlüsselwort und Info-Dateien. <br/><br/>Schlüsselwort-Datei ist eine XML-Datei mit Schlüsselwörter Frequenz mit Offsetinformationen aus Sprachinhalt extrahiert. <br/><br/>Datei ist eine Textdatei enthält detaillierten Informationen über jeden Begriff erkannt. Die erste Zeile ist ein Sonderfall und Erkennbarkeit Wert enthält. Jede nachfolgende Zeile wird eine tabulatorgetrennte Liste folgende Daten: Startzeit, Endzeit, Wort oder Satzteil, vertrauen. Die Zeiten werden in Sekunden angegeben und das Vertrauen als Zahl zwischen 0 und 1 angegeben. <br/><br/>Beispiel: "1,20 1,45 Word 0,67" <br/><br/>Diese Dateien können zur für verschiedene Zwecke wie Rede Analytics ausführen oder Suchmaschinen wie Bing, Google oder Microsoft SharePoint, um Dateien leichter oder sogar mehr relevante anzeigen zu verfügbar gemacht.
__JobResult.txt__ |Ausgabe Manifest nur vorhanden, wenn mehrere Dateien mit folgenden Angaben Indizierung:<br/><br/><table border="1"><tr><th>Eingabedatei</th><th>Alias</th><th>MediaLength</th><th>Fehler</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Nicht alle input Mediendateien erfolgreich indiziert, Indizierung Auftrag schlägt mit Fehlercode 4000. Weitere Informationen finden Sie unter [Fehlercodes](#error_codes).

## <a name="index-multiple-files"></a>Mehrere Dateien indizieren

Die folgende Methode mehrere Dateien als Anlage hochgeladen und erstellt einen Auftrag, um diese Dateien in einem Batch indizieren.

Eine manifest-Datei mit der Erweiterung .lst wird erstellt und in der Anlage hochgeladen. Die Manifestdatei enthält die Liste alle Objektdateien. Weitere Informationen finden Sie unter [Task Vorgaben für Azure Media Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Auftrag teilweise erfolgreich

Nicht alle input Mediendateien erfolgreich indiziert, Indizierung Auftrag schlägt mit Fehlercode 4000. Weitere Informationen finden Sie unter [Fehlercodes](#error_codes).


Dieselbe Ausgabe (wie erfolgreich Aufträge) entstehen. Finden Sie die Ausgabemanifestdatei um herauszufinden, welche Dateien, nach Spalte Fehlerwerte fehlgeschlagen sind. Für Dateien, die nicht, resultierende AIB, SAMI, TTML, WebVTT und Schlüsselwort Dateien nicht entstehen.

### <a id="preset"></a>Voreinstellung für Azure Media Indexer Aufgabe

Die Verarbeitung von Azure Media Indexer kann eine optionale Aufgabe Voreinstellung neben der Aufgabe bereitstellen angepasst werden.  Die folgenden beschreibt das Format dieser Konfigurations-Xml.

Name | Erforderlich | Beschreibung
----|----|---
__Eingabe__ | falsch | Ressource-Dateien, die Sie indizieren möchten.</p><p>Azure Media Indexer unterstützt Media-Dateiformate: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Sie können den Dateinamen (s) im **Namen** oder **Listen** -Attribut des **input** -Elements angeben (siehe unten). Wenn Sie die Bilddatei Index nicht angeben, wird die primäre Datei entnommen. Wenn keine primäre Bilddatei festgelegt ist, wird die erste Datei in der Eingabe Anlage indiziert.</p><p>Führen Sie zum Namen der Anlage explizit angeben:<br />`<input name="TestFile.wmv">`<br /><br />Sie können auch mehrere Anlagen Dateien gleichzeitig (bis zu 10) index. Dazu:<br /><br /><ol class="ordered"><li><p>Erstellen Sie eine Datei (manifest-Datei), und geben sie eine Erweiterung .lst. </p></li><li><p>Diese manifest-Datei eine Liste aller Dateinamen der Anlage in Ihrer Eingabe Ressource hinzufügen. </p></li><li><p>Anlage (Upload) Thanifest Datei hinzufügen.  </p></li><li><p>Geben Sie den Namen der Manifestdatei in das Input-Attribut.<br />`<input list="input.lst">`</li></ol><br /><br />Hinweis: Wenn die manifest-Datei mehr als 10 Dateien hinzufügen, Indizierung Auftrag 2006 Fehlercode fehl.
__Metadaten__ | falsch | Metadaten für die angegebene Anlage Datei(en) Vokabular Anpassung verwendet.  Für Indexer nicht standardmäßige Wörter wie Eigennamen erkennen vorbereiten.<br />`<metadata key="..." value="..."/>` <br /><br />Sie können __Werte__ für vordefinierte __Schlüssel__angeben. Derzeit werden die folgenden Schlüssel:<br /><br />"Titel" und "Description" - Vokabular Anpassung optimieren die Sprache zum Modell für Ihren Auftrag und Genauigkeit der Spracherkennung verbessern.  Die Werte Samen Internetsuche zu kontextrelevante Textdokumente mit dem Inhalt das interne Wörterbuch für die Dauer der Aufgabe Indizierung erweitern.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__Funktionen__ <br /><br /> In Version 1.2 hinzugefügt. Derzeit ist die einzige unterstützte Funktion Spracherkennung ("ASR").| falsch | Die Spracherkennung hat die folgenden Standardeinstellungen Schlüssel:<table><tr><th><p>Schlüssel</p></th>     <th><p>Beschreibung</p></th><th><p>Beispiel für einen Wert</p></th></tr><tr><td><p>Sprache</p></td><td><p>Die natürliche Sprache in der Multimedia-Datei erkannt werden.</p></td><td><p>Englisch, Spanisch</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>eine durch Semikolons getrennte Liste der gewünschten Beschriftung Ausgabeformate (falls vorhanden)</p></td><td><p>Ttml Sami; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Ein boolesches Flag, das angibt, ob eine Datei AIB (für SQL Server und der Kunde Indexer IFilter) erforderlich ist.  Weitere Informationen finden Sie unter <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Using AIB Files Azure Media Indexer mit SQL Server</a>.</p></td><td><p>True False</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Ein boolesches Flag, das angibt, ob eine Schlüsselwort XML-Datei erforderlich ist.</p></td><td><p>True False. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Ein boolesches Flag, das angibt, ob eine vollständige Beschriftung (unabhängig von der Vertrauensebene) erzwingen.  </p><p>Standard ist false, dabei Wörter und Ausdrücke, die weniger als 50 Prozent aus der letzten Beschriftung Ausgaben weggelassen und durch Auslassungspunkte ("...") ersetzt.  Ellipsen eignen sich für die Beschriftung Kontrolle und Überwachung.</p></td><td><p>True False. </p></td></tr></table>

### <a id="error_codes"></a>Fehlercodes

Bei einem Fehler melden Azure Media Indexer eines der folgenden Fehlercodes:

Code | Name | Mögliche Ursachen
-----|------|------------------
2000 | Ungültige Konfiguration | Ungültige Konfiguration
2001 | Ungültige Eingabe Anlagen | Fehlende Eingabe Vermögenswerte oder leere Anlage.
2002 | Ungültige Manifestdatei | Manifest Manifest enthält oder ungültige Elemente.
2003 | Fehler beim Herunterladen der Mediendatei | Ungültige URL Manifestdatei.
2004 | Nicht unterstütztes Protokoll | Protokoll der Media-URL wird nicht unterstützt.
2005 | Nicht unterstützter Dateityp | Input Media-Dateityp wird nicht unterstützt.
2006 | Zu viele Dateien | Es sind mehr als 10 Dateien in das Eingabemanifest.
3000 | Fehler beim Decodieren der Mediendatei | Nicht unterstützte Mediencodec <br/>oder<br/> Beschädigte Datei <br/>oder<br/> Kein audio Stream input Medien.
4000 | Batch-Indexierung war teilweise erfolgreich. | Einige der eingegebenen Mediendateien konnten nicht indiziert werden. Weitere Informationen finden Sie in <a href="#output_files">Ausgabedateien</a>.
andere | Interne Fehler | Support-Team wenden. indexer@microsoft.com


## <a id="supported_languages"></a>Unterstützte Sprachen

Derzeit werden die Sprachen Englisch und Spanisch. Weitere Informationen finden Sie [im Blogbeitrag v1. 2-Version](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Verwandte links

[Analytics Übersicht über Services Azure Media](media-services-analytics-overview.md)

[AIB Dateien verwenden Azure Media Indexer mit SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indizierung von Mediendateien mit Azure Media Indexer 2 Vorschau](media-services-process-content-with-indexer2.md)
