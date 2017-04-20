<properties
    pageTitle="Erste Schritte mit Inhalt bei Bedarf mit .NET | Azure"
    description="Dieses Lernprogramm führt Sie durch die Schritte zum Implementieren einer auf Anforderung Inhaltsübermittlung Anwendung mit Azure Media Services mit .NET."
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
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Erste Schritte mit Inhalt bei Bedarf mit .NET SDK

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>Übersicht 

Dieses Lernprogramm führt Sie durch die Schritte zum Implementieren einer Demand (VoD) Inhalten Anwendung mit Azure Media Services (AMS) SDK für .NET.


Das Lernprogramm führt grundlegende Media Services Workflow und häufigsten Programmierobjekte und Aufgaben für die Entwicklung von Media Services erforderlich. Nach Abschluss des Lernprogramms werden Sie zu streamen oder progressives Herunterladen eine Media-Beispieldatei, die hochgeladen, codiert und heruntergeladen.

## <a name="what-youll-learn"></a>Sie erfahren

Das Lernprogramm zeigt, wie die folgenden Aufgaben ausführen:

1.  Erstellen Sie ein Media Services-Konto (mit Azure-Portal).
2.  Konfigurieren Sie Streaming-Endpunkt (mit Azure-Portal).
3.  Erstellen Sie und konfigurieren Sie eines Visual Studio.
5.  Media Services-Konto verbinden.
6.  Erstellen einer neuen Anlage und eine Videodatei hochladen.
7.  Codieren Sie die Quelldatei in adaptive Bitrate MP4-Dateien.
8.  Veröffentlichen der Anlage und URLs für streaming und progressiven Download.
9.  Testen von Ihrem Inhalt.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die folgenden müssen für dieses Lernprogramm erforderlich.

- Um dieses Lernprogramm benötigen Sie ein Azure-Konto. 
    
    Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](/pricing/free-trial/?WT.mc_id=A261C142F). Sie erhalten ein Guthaben, die versuchen, bezahlte Azure Services verwendet werden können. Auch nach die Abspann verbraucht werden, können Sie das Konto und kostenlose Azure Dienste und Funktionen wie das Web Apps in Azure App Service verwenden.
- Betriebssysteme: Windows 8 oder höher, Windows 2008 R2 und Windows 7.
- .NET Framework 4.0 oder höher
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate oder Express) oder höher.


##<a name="download-sample"></a>Beispiel herunterladen

Abrufen und ein Beispiel [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/)ausführen.

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Erstellen Sie ein Azure-Portal mit Azure Media Services-Konto

Die Schritte in diesem Abschnitt zeigen, wie ein AMS-Konto erstellen.

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com/).
2. Klicken Sie auf **+ Neuer** > **Media + CDN** > **Media Services**.

    ![Media Services erstellen](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Geben Sie die gewünschten Werte, in **MEDIA SERVICES-Konto erstellen** .

    ![Media Services erstellen](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Geben Sie den Namen des neuen AMS **Kontonamen**. Ein Kontoname Media Services ist alle Kleinbuchstaben Zahlen oder Buchstaben ohne Leerzeichen und 3 bis 24 Zeichen lang ist.
    2. Wählen Sie im Abonnement unter verschiedenen Azure-Abonnements, denen Sie Zugriff haben.
    
    2. **Ressourcengruppe**wählen Sie die neue oder vorhandene Ressource.  Eine Ressourcengruppe ist eine Sammlung von Ressourcen, die Lebenszyklus, Berechtigungen und Richtlinien. Weitere Informationen [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Speicherort**dient Auswählen der Region die Medien- und Metadaten für Ihr Media Services-Konto gespeichert. Dieser Bereich wird zum Verarbeiten und Streamen von Medien. Die verfügbaren Media Services Bereiche werden im Dropdown-Listenfeld angezeigt. 
    
    3. Wählen Sie **Konto**ein Speicherkonto BLOB-Speicher der Medieninhalte von der Media Services-Konto. Auswählen eines vorhandenen Speicherkontos in derselben geographischen Region als Media Services-Konto oder ein Speicherkonto erstellen. Ein neues Speicherkonto wird in derselben Region erstellt. Die Regeln für Namen Speicher entsprechen Media Services-Konten.

        Erfahren Sie mehr über Speicher [hier](storage-introduction.md).

    4. **Pin Dashboard** die Fortschritte der Bereitstellung Konto auswählen
    
7. Klicken Sie am unteren Rand des Formulars **Erstellen** .

    Sobald das Konto erfolgreich erstellt wurde, wird der Status **ausgeführt**. 

    ![Media Services settings](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Das AMS-Konto verwalten (z. B. Hochladen Videos Codieren von Ressourcen, Überwachung des Projektstatus) **Settings** Fenster.

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurieren Sie Streaming-Endpunkte mithilfe von Azure-portal

Arbeit mit Azure Media Services, die eines der häufigsten Szenarios adaptive Bitrate streaming Video an die Clients übermittelt. Media Services unterstützt die folgende adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern).

Media Services bietet dynamische Verpackung der adaptive Bitrate codiert MP4 Inhalt streaming-Formate von Media Services (MPEG DASH HLS Smooth Streaming, HDS) just-in-Time, ohne vorkonfigurierte Versionen dieser streaming-Formaten speichern zu kann.

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- Codieren Sie die Aufsteckkarte (Quelle) Datei in adaptive Bitrate MP4-Dateien (Codierung Schritte werden später in diesem Lernprogramm gezeigt).  
- Erstellen Sie mindestens eine Streaming-Einheit für die *streaming-Endpunkt* aus der Lieferung des Inhalts werden soll. Schritte anzeigen zum Ändern der Anzahl der Streaming-Einheiten

Dynamische Verpackung müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen mit Media Services erstellt und die entsprechende Antwort auf Anfragen von einem Client fungiert.

Um die Anzahl der reservierte Einheiten streaming zu erstellen, führen Sie folgende Schritte aus:


1. Klicken Sie im Fenster **Eigenschaften** auf **Streaming-Endpunkte**. 

2. Klicken Sie auf die Standardeinstellungen für streaming-Endpunkt. 

    **STREAMING ENDPUNKTDATEN STANDARDMÄßIG** angezeigt.

3. Ziehen Sie zum Angeben der Streaming-Einheiten Schieberegler **Streaming-Einheiten** .

    ![Streaming-Einheiten](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klicken Sie auf **Speichern** , um die Änderung zu speichern.

    >[AZURE.NOTE]Die Zuordnung neuer Einheiten kann bis zu 20 Minuten dauern.

##<a name="create-and-configure-a-visual-studio-project"></a>Erstellen und Konfigurieren eines Visual Studio-Projekts

1. Erstellen Sie eine neue C# in Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 SP1. Geben Sie **Name**, **Speicherort**und **Projektmappennamen**, und klicken Sie auf **OK**.

2. Verwenden Sie [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) NuGet-Paket **Azure Media Services.NET SDK Extensions**installieren.  Media Services .NET SDK Extensions ist eine Reihe von Erweiterungsmethoden und Hilfsfunktionen, die Vereinfachung des Codes und Media-Dienste zu erleichtern. Installieren dieses Paket, **Media Services.NET SDK** installiert und andere erforderliche Abhängigkeit hinzugefügt.

3. Fügen Sie einen Verweis auf System.Configuration-Assembly. Diese Assembly enthält die **System.Configuration.ConfigurationManager** -Klasse, die Zugriff auf die Konfigurationsdateien, z. B. App.config.

4. Öffnen Sie die Datei App.config (die Datei dem Projekt hinzugefügt, wenn sie nicht standardmäßig hinzugefügt wurde) und einen *AppSettings* -Abschnitt der Datei hinzufügen. Legen Sie die Werte für Ihren Azure Media Services Account und Schlüssel, wie im folgenden Beispiel gezeigt. Der Kontoname und wichtige Informationen zum [Azure-Portal](https://portal.azure.com/) und AMS-Konto auswählen. **Dann,** > **Schlüssel**. Windows Schlüssel verwalten zeigt den Namen und die primären und sekundären Schlüssel angezeigt.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. Überschreiben der vorhandenen **mithilfe** Anweisung am Anfang der Datei Program.cs mit dem folgenden Code.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Erstellen eines neuen Ordners Projekte im Verzeichnis und eine zu codieren und Streaming progressives Herunterladen. MP4 oder WMV-Datei kopieren. In diesem Beispiel wird der Pfad "C:\VideoFiles" verwendet.

##<a name="connect-to-the-media-services-account"></a>Verbinden Sie mit Media Services-Konto

Bei Media Services mit .NET müssen Sie für die meisten Programmieraufgaben Mediendienste **CloudMediaContext** -Klasse verwenden: mit Media Services-Konto; Erstellen, aktualisieren, Zugriff auf und löschen die folgenden Objekte: Anlagen Objektdateien, Aufträge, Richtlinien, Locators usw..

Überschreiben Sie die Standardklasse Programm mit dem folgenden Code. Der Code veranschaulicht der Werte aus der Datei App.config lesen und **CloudMediaContext** -Objekt erstellen, um den Media Services herstellen. Weitere Informationen zum Verbinden mit Media Services finden Sie unter [Herstellen einer Verbindung mit Media Services mit Media Services SDK für .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

Die **Main** -Funktion ruft Methoden werden in diesem Abschnitt weiter.

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
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Erstellen Sie eine neue Anlage und Hochladen eine Videodatei

Media Services Sie hochladen oder nehmen Ihre digitalen Dateien zu. Die **Aktivposten** -Entität darf Video, Audio, Bilder, Miniaturansichten Sammlungen, Text Titel und Untertitel Dateien (und Metadaten Dateien.)  Sobald die Dateien hochgeladen werden, wird der Inhalt in der Cloud zur weiteren Verarbeitung und streaming sicher. Die Dateien in der Anlage heißen **Objektdateien**.

Die **UploadFile** -Methode ruft **CreateFromFile** (definiert in .NET SDK Extensions) beschrieben. **CreateFromFile** erstellt eine neue Anlage in die angegebene Quelldatei geladen wird.

Die **CreateFromFile** -Methode verwendet **AssetCreationOptions** können Sie die Anlage Erstellungsoptionen angeben:

- **Keine** - keine Verschlüsselung verwendet wird. Dies ist der Standardwert. Beachten Sie, dass bei Verwendung dieser Option der Inhalt nicht während der Übertragung oder im Speicher geschützt ist.
Möchten Sie ein MP4 liefern progressives Herunterladen verwendet diese Option.
- **StorageEncrypted** - mit dieser Option verschlüsseln klare Inhalte lokal mit Advanced Encryption Standard (AES)-256 Bit Verschlüsselung, der dann diese Azure-Speicher hochgeladen Speicherort ruhende verschlüsselt. Mit Storage Verschlüsselung geschützt werden automatisch entschlüsselt ein verschlüsseltes Dateisystem vor Codierung platziert und optional vor dem Hochladen als eine neue Ausgabe Anlage erneut verschlüsselt. Primäre Nutzung bei Speicher-Verschlüsselung ist, wenn Ihre Eingabe Medien in hoher Qualität mit starker Verschlüsselung ruhender auf Datenträger sichern.
- **CommonEncryptionProtected** - verwenden Sie diese Option, wenn Sie Inhalte hochladen, die bereits verschlüsselt und mit gemeinsamen Verschlüsselung oder PlayReady-DRM (z. B. Smooth Streaming geschützt mit PlayReady-DRM) geschützt.
- **EnvelopeEncryptionProtected** – verwenden Sie diese Option, wenn Sie mit AES verschlüsselt HLS hochladen. Beachten Sie, dass Dateien müssen codiert und Transformieren Manager verschlüsselt.

**CreateFromFile** -Methode können Sie einen Rückruf angeben, um den Upload der Datei Fortschritte.

Im folgenden Beispiel geben wir **keine** Anlage Optionen.

Der Program-Klasse die folgende Methode hinzufügen.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Codieren Sie die Quelldatei in adaptive Bitrate MP4-Dateien

Nach Einnahme Anlagen in Media Services, können Medien codiert, Transmuxed mit einem Wasserzeichen versehen, und vor der Lieferung an Clients. Diese Aktivitäten sind geplant und Ausführen mehrerer Instanzen Hintergrund, hohe Performance und Verfügbarkeit zu gewährleisten. Diese Aktivitäten werden Aufträge genannt und jeder Auftrag besteht aus atomare Vorgänge, die die eigentliche auf die Bilddatei Arbeit.

Wie bereits erwähnt wurde beim Arbeiten mit Azure Media Services, liefert eine der häufigsten adaptive Bitrate an Ihre Kunden. Media Services können dynamisch Packen adaptive Bitrate MP4-Dateien in einem der folgenden Formate: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern).

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- Transcodieren der Aufsteckkarte (Quelle) Datei in eine Reihe von adaptiven Bitrate MP4 oder adaptive Bitrate Smooth Streaming-Dateien oder codiert.  
- Rufen Sie mindestens eine Streaming-Einheit für Streaming-Endpunkt aus dem Lieferung des Inhalts werden soll.

Im folgenden Codebeispiel wird eine Codierung einreichen. Das Projekt enthält eine Aufgabe, die transcodiert Mezzanine-Datei in eine Reihe von adaptiven Bitrate MP4s mit **Media Encoder Standard**angegeben. Der Code sendet den Auftrag und wartet, bis er abgeschlossen ist.

Wenn der Auftrag abgeschlossen ist, würde möglicherweise zu streamen Ihrer Ressource progressives Herunterladen durch transcodieren erstellten MP4-Dateien.
Notiz, die Sie nicht mehr als 0 Streaming-Einheiten, um progressives Herunterladen MP4-Dateien benötigen.

Der Program-Klasse die folgende Methode hinzufügen.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Veröffentlichen der Anlage und URLs für streaming und progressiven download

Um stream oder Anlage herunterzuladen, müssen Sie "Veröffentlichen" einen Locator erstellen. Locators ermöglichen den Zugriff auf Dateien in der Anlage. Media Services unterstützt zwei Arten von Locators: OnDemandOrigin Locator zum Streamen von Medien (z. B. MPEG DASH, HLS oder Smooth Streaming) und Access Signatur (SAS)-Locators Mediendateien verwendet.

Nach dem Erstellen der Locators können Sie URLs erstellen, die zum Streamen oder Dateien herunterladen.


Streaming-URL für Smooth Streaming hat das folgende Format:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Streaming-URL für HLS hat das folgende Format:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Streaming-URL für MPEG DASH hat das folgende Format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

SAS-URL zum Herunterladen hat das folgende Format:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK Extensions bieten einfache Hilfsmethoden, die formatierten URLs für die veröffentlichten Anlage zurück.

Der folgende Code verwendet .NET SDK Extensions Locator erstellen und Abrufen streaming und progressiven Download URLs. Der Code veranschaulicht auch Dateien in einen lokalen Ordner herunterladen.

Der Program-Klasse die folgende Methode hinzufügen.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Durch Ihre Inhalte testen  

Nach des Programms im vorherigen Abschnitt definiert ausführen, werden ähnlich der folgenden URLs im Konsolenfenster angezeigt.

Adaptives streaming URLs:

Smooth Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG-STRICH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Progressiver Download URLs (Audio und Video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Verwenden Sie [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)zum Streamen von Videos.

Progressiven Download testen fügen Sie eine URL in einem Browser (z. B. Internet Explorer, Chrome oder Safari ein).


##<a name="next-steps-media-services-learning-paths"></a>Nächste Schritte: Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Sie suchen etwas?

Wenn dieses Thema enthielt, was Sie erwarten etwas fehlt, oder in andere Weise nicht Ihre Bedürfnisse, Bitte geben Sie Ihr Feedback mit den Disqus unten.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
