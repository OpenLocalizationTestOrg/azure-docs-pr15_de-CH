<properties 
    pageTitle="Veröffentlichen Sie Azure Media Services mit .NET" 
    description="Erfahren Sie, wie einen Locator für die Streaming-URL erstellen erstellen. Code-Beispiele sind in C# geschrieben und Media Services SDK für .NET." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-net"></a>Veröffentlichen Sie Azure Media Services mit .NET
 
> [AZURE.SELECTOR]
- [REST](media-services-rest-deliver-streaming-content.md)
- [.NET](media-services-deliver-streaming-content.md)
- [Portal](media-services-portal-publish.md)

##<a name="overview"></a>Übersicht

Sie können eine adaptive Bitrate streamen MP4 einen streaming OnDemand-Locator erstellen und Erstellen eines Streaming-URL festlegen. Thema [Codierung Anlage](media-services-encode-asset.md) wird wie in adaptive Bitrate codiert MP4 festgelegt. 

>[AZURE.NOTE]Wenn der Inhalt verschlüsselt wird, konfigurieren Sie Anlage Lieferung Richtlinie (wie in [diesem](media-services-dotnet-configure-asset-delivery-policy.md) Thema beschrieben) vor dem Erstellen eines Locators. 

Sie können auch eine streaming-Locator OnDemand URLs erstellen, die auf MP4-Dateien, die progressiv heruntergeladen werden können.  

In diesem Thema zeigt eine streaming-Locator Veröffentlichen Ihrer Ressource und einen optimierten MPEG DASH und streaming URLs HLS OnDemand erstellen. Es zeigt außerdem hot progressiven Download URLs erstellen. 
     
##<a name="create-an-ondemand-streaming-locator"></a>Erstellen Sie eine streaming-Locator OnDemand

Um streaming-Locator OnDemand und URLs müssen Sie Folgendes tun:

   1. Wenn der Inhalt verschlüsselt wird, definieren Sie eine Zugriffsrichtlinie.
   2. Erstellen Sie eine streaming-Locator OnDemand.
   3. Wenn Sie streamen möchten, erhalten Sie Streaming-Manifestdatei (ISM) in der Anlage. 
        
    Möchten Sie progressives Herunterladen, erhalten Sie die Namen der MP4-Dateien in der Anlage.  
   4. URLs Manifestdatei oder MP4-Dateien erstellen. 
   

###<a name="use-media-services-net-sdk"></a>Media Services .NET SDK verwenden 

Streaming URLs erstellen 

    private static void BuildStreamingURLs(IAsset asset)
    {
    
        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();
        
        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

Der Code gibt:
    
    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)
    

>[AZURE.NOTE]Sie können auch die Inhalte über eine SSL-Verbindung übertragen. Dazu sicherzustellen Sie, dass Ihre streaming URLs mit HTTPS beginnen. 

Progressiver Download URLs erstellen 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));
                
        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

Der Code gibt:
    
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4
    
    . . . 

###<a name="use-media-services-net-sdk-extensions"></a>Media Services .NET SDK Erweiterungen

Der folgende Code ruft .NET SDK Erweiterungsmethoden, die einen Locator erstellen und das Smooth Streaming HLS und MPEG-DASH-URLs für adaptives streaming.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));
    
    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();
    
    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch

[Download von Ressourcen](media-services-deliver-asset-download.md)
[Anlage Delivery-Richtlinie konfigurieren](media-services-dotnet-configure-asset-delivery-policy.md)
