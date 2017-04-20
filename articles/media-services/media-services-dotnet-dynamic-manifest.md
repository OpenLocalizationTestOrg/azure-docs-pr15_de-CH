<properties 
    pageTitle="Erstellen von Filtern mit Azure Media Services .NET SDK" 
    description="Dieses Thema beschreibt, wie Filter erstellt, damit der Client sie bestimmte Abschnitte eines Streams Stream verwenden kann. Media Services erstellt dynamische Manifeste selektive streaming zu." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Erstellen von Filtern mit Azure Media Services .NET SDK

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [REST](media-services-rest-dynamic-manifest.md)

Ab Version 2.11 können Media Services Sie Filter für Ihre Anlagen. Diese Filter sind serverseitige Regeln, mit denen Ihre Kunden wie können: Wiedergabe nur ein Abschnitt des Videos (anstatt das gesamte Video), oder geben Sie nur eine Teilmenge der Audio- und Formatvarianten, die Ihr Kunde Gerät (statt alle Formatvarianten, die der Anlage zugeordnet sind) behandeln kann. Diese Filterung Ihrer Ressourcen erfolgt durch **Dynamische Manifest**s, die Anfrage des Kunden eine Videos basierend auf angegebenen Filter erstellt werden.

Ausführlichere Informationen zu filtern und dynamische Manifest finden Sie unter [Übersicht über dynamische Manifeste](media-services-dynamic-manifest-overview.md).

In diesem Thema veranschaulicht, wie mit Media Services .NET SDK erstellen, aktualisieren und Löschen von Filtern. 


Beachten Sie bei der Aktualisierung eines Filters streaming-Endpunkt um Regeln aktualisieren bis zu 2 Minuten dauern kann. Wenn der Inhalt war mit diesem Filter und Proxys und CDN Caches zwischengespeichert kann das Aktualisieren dieser Filter Player Fehlern führen. Es wird empfohlen, den Cache nach der Aktualisierung des Filters. Wenn diese Option nicht möglich ist, sollten Sie einen anderen Filter. 

##<a name="types-used-to-create-filters"></a>Typen zum Erstellen von Filtern

Die folgenden Typen werden verwendet, wenn Filter erstellen: 

- **IStreamingFilter**.  Dieser Typ basiert auf den folgenden REST API- [Filter](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Dieser Typ basiert auf folgenden REST-API [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Dieser Typ basiert auf folgenden REST-API [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** und **IFilterTrackPropertyCondition**. Diese Typen basieren auf den folgenden REST-APIs [FilterTrackSelect und FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Globale Filter erstellen/aktualisieren/lesen/löschen

Der folgende Code zeigt, wie .NET erstellen, aktualisieren, lesen und Löschen von Ressourcen filtern.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Erstellen/Aktualisieren/lesen/löschen Anlage Filter

Der folgende Code zeigt, wie .NET erstellen, aktualisieren, lesen und Löschen von Ressourcen filtern.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>Erstellen Sie streaming-URLs, die Filter

Informationen zu veröffentlichen Ihre Ressourcen finden Sie unter [Content auf Kunden-Überblick liefern](media-services-deliver-content-overview.md).


Folgende Beispiele zeigen, wie streaming URLs Filter hinzu.

**MPEG-STRICH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Smooth Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**HDS**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Siehe auch 

[Übersicht über dynamische Manifeste](media-services-dynamic-manifest-overview.md)
 

