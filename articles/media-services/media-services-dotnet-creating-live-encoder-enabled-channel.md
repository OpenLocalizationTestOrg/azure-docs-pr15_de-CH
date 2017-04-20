<properties 
    pageTitle="Wie führen Sie live-streaming mit Azure Media Services Multi-Bitrate Streams mit .NET erstellen | Microsoft Azure" 
    description="Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen eines Kanals, der einen einzigen Bitrate Livestream empfängt und Multi-Bitrate Stream mit .NET SDK codiert." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Wie führen Sie live-streaming mit Azure Media Services Multi-Bitrate Streams mit .NET erstellen

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST-API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>Übersicht

Dieses Lernprogramm führt Sie schrittweise ein **Kanal** , der Stream Multi-Bitrate codiert und erhält einen einzelnen Bitrate Livestream erstellen.

Weitere grundlegende Informationen zu Kanäle live Codierung aktiviert sind, finden Sie in der [Live-streaming mit Azure Media Services Multi-Bitrate Streams erstellen](media-services-manage-live-encoder-enabled-channels.md).


##<a name="common-live-streaming-scenario"></a>Häufig für Live-Streaming

Die folgenden Schritte beschreiben die Aufgaben beim Erstellen der Anwendung für live-streaming.

>[AZURE.NOTE] Derzeit ist die maximale empfohlene Dauer ein live-Ereignis 8 Stunden. Wenden Sie sich an Amslived unter Microsoft.com, benötigen Sie einen Kanal für längere Zeit ausgeführt.

1. Eine Videokamera an einen Computer anschließen Starten und Konfigurieren von lokalen live Encoder, der eine einzelne Bitrate in eines der folgenden Protokolle Ausgabestream kann: RTMP, Smooth Streaming und RTP (MPEG-TS). Weitere Informationen finden Sie unter [Azure Media Services RTMP-Unterstützung und Live-Encoder](http://go.microsoft.com/fwlink/?LinkId=532824).

Dieser Schritt kann auch nach den Kanal erstellen durchgeführt werden.

1. Erstellen und Starten eines Kanals.

1. Abrufen der Kanal Aufnahme URL.

Die Erfassung URL live Encoder dient den Stream an den Kanal senden.

1. Channel-Vorschau-URL abzurufen.

Verwenden Sie diese URL, um sicherzustellen, dass der Kanal richtig live-Streams empfangen.

2. Erstellen einer Anlage.
3. Folgendermaßen Sie der Anlage dynamisch während der Wiedergabe verschlüsselt werden soll, vor:
1. Erstellen Sie einen symmetrischen Schlüssel.
1. Konfigurieren der Inhaltsschlüssel Autorisierungsrichtlinie.
1. Konfigurieren Sie Asset Lieferung Richtlinie (dynamische Verpackung und dynamische Verschlüsselung verwendet).
3. Erstellen Sie und geben Sie an, dass die Anlage verwenden, die Sie erstellt.
1. Veröffentlichen der Anlage zugeordneten Programms durch Erstellen eines Locators OnDemand.

Müssen Sie mindestens eine reservierte Einheit auf Streaming-Endpunkt zum Streaming von Inhalten soll streaming.

1. Starten Sie das Programm, wenn Sie streaming und Archivierung bereit sind.
2. Optional kann live Encoder eine Ankündigung signalisiert. Die Ankündigung wird in den Ausgabestream.
1. Beenden Sie das Programm jederzeit beenden streaming und Archivierung das Ereignis.
1. Löschen (und optional die Anlage löschen).

## <a name="what-youll-learn"></a>Sie erfahren

In diesem Thema veranschaulicht die verschiedene Arbeitsgängen Kanäle und Programme mit Media Services .NET SDK ausführen. Da viele .NET APIs langer sind, die mit langer verwalten werden Vorgänge verwendet.

Das Thema veranschaulicht Folgendes:

1. Erstellen und Starten eines Kanals. Langer APIs verwendet werden.
1. Abrufen der Kanäle Aufnahme (input) Endpunkt. Dieser Endpunkt an den Encoder vorzusehen, die einen einzelne Bitrate Livestream senden können.
1. Vorschau-Endpunkt zu erhalten. Dieser Endpunkt ist der Stream Vorschau verwendet.
1. Erstellen Sie eine Anlage, die zum Speichern des Inhalts. Richtlinien der Anlage sollte auch, wie im folgenden Beispiel gezeigt konfiguriert werden.
1. Erstellen Sie und geben Sie an, dass die Anlage verwenden, die zuvor erstellt wurde. Starten Sie das Programm. Langer APIs verwendet werden.
1. Erstellen Sie einen Locator für die Anlage der Inhalt veröffentlicht wird und an die Clients übertragen.
1. Ein- und Ausblenden von Schiefer. Starten Sie und beenden Sie anzeigen. Langer APIs verwendet werden.
1. Bereinigen Sie den Kanal und die zugeordneten Ressourcen.


##<a name="prerequisites"></a>Erforderliche Komponenten

Die folgenden müssen für dieses Lernprogramm erforderlich.

- Um dieses Lernprogramm benötigen Sie ein Azure-Konto.

Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](/pricing/free-trial/?WT.mc_id=A261C142F). Sie erhalten ein Guthaben, die versuchen, bezahlte Azure Services verwendet werden können. Auch nach die Abspann verbraucht werden, können Sie das Konto und kostenlose Azure Dienste und Funktionen wie das Web Apps in Azure App Service verwenden.
- Media Services-Konto. Zum Erstellen eines Kontos Media Services finden Sie unter [Konto erstellen](media-services-portal-create-account.md).
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate oder Express) oder höher.
- Verwenden Sie Media Services .NET SDK, Version 3.2.0.0 oder höher.
- Eine Webcam und Encoder, der einen einzelne Bitrate Livestream senden kann.

##<a name="considerations"></a>Hinweise

- Derzeit ist die maximale empfohlene Dauer ein live-Ereignis 8 Stunden. Wenden Sie sich an Amslived unter Microsoft.com, benötigen Sie einen Kanal für längere Zeit ausgeführt.
- Müssen Sie mindestens eine reservierte Einheit auf Streaming-Endpunkt zum Streaming von Inhalten soll streaming.

##<a name="download-sample"></a>Beispiel herunterladen

Abrufen und ein Beispiel [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/)ausführen.


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Richten Sie für die Entwicklung mit Media Services SDK für .NET

1. Erstellen Sie eine mit Visual Studio.
1. Fügen Sie Media Services SDK für .NET zur Konsolenanwendung mit Media Services NuGet-Paket hinzu.

##<a name="connect-to-media-services"></a>Verbinden mit Media Services
Als bewährte Methode sollten Sie eine App von Media Services und größter verwenden.

>[AZURE.NOTE]Um den Namen und Schlüssel Werte finden, Azure-Portal wechseln und Ihr Konto wählen. Das Fenster Einstellungen auf der rechten Seite. Wählen Sie im Einstellungsfenster Schlüssel. Klicken auf das Symbol neben jedem Textfeld kopiert den Wert in die Zwischenablage.

Die App.config.Datei AppSettings-Abschnitt hinzu, und legen Sie die Werte für den Media Services-Konto und Key.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Codebeispiel

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
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
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Nächstes

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Sie suchen etwas?

Wenn dieses Thema enthielt, was Sie erwarten etwas fehlt, oder in andere Weise nicht Ihre Bedürfnisse, Bitte geben Sie Feedback mit den Disqus unten.
