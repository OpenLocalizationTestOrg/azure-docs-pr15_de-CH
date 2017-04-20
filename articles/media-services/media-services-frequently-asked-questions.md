<properties 
    pageTitle="Häufig gestellte Fragen | Microsoft Azure" 
    description="Häufig gestellte Fragen (FAQs)" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>


#<a name="frequently-asked-questions"></a>Häufig gestellte Fragen

##<a name="general-ams-faqs"></a>Allgemeine AMS-FAQs

F: wie skalieren Sie Indizierung?

A: die reservierten Einheiten sind identisch für Codierung und Indizierung. Anleitung zum [Codierung reservierte Maßeinheiten](media-services-scale-media-processing-overview.md). **Hinweis** Indexer Leistung reservierte Einheitentyp nicht betroffen ist.

F: hochgeladen, codiert und Video veröffentlicht. Was wäre Grund das Video nicht wiedergegeben, wenn sie streamen versuch?

A: eine der häufigsten Ursachen ist Sie nicht mindestens eine reservierte streaming Einheit auf Streaming-Endpunkt aus dem Wiedergabe möchten reserviert.  Anleitung zum [Streaming reservierte Maßeinheiten](media-services-portal-scale-streaming-endpoints.md).

F: kann ich Zusammensetzung für einen Livestream?

A: Zusammensetzung auf live derzeit nicht in Azure Media Services angeboten wird, Sie bereits auf Ihrem Computer erstellen müssen.

F: verwenden kann ich Azure CDN mit Live-Streaming?

A: Media Services Integration in Azure CDN unterstützt (Weitere Informationen finden Sie unter [Verwalten von Streaming Endpunkte ein Media Services-Konto](media-services-portal-manage-streaming-endpoints.md)).  Sie können Live-streaming mit CDN. Azure Media Services bietet reibungslosen Streaming HLS und MPEG-DASH-Ausgaben. Diese Formate verwenden HTTP zum Übertragen von Daten und Vorteile von HTTP-caching. Im live-streaming tatsächliche Video-oder Audio-Daten Fragmente aufgeteilt und diese einzelnen Fragmente erhalten CDN zwischengespeichert. Nur muss aktualisiert werden manifest. CDN aktualisiert regelmäßig Manifestdaten.

F: ist Azure Media Services unterstützen das Speichern von Bildern?

A: Wenn Sie nur auf JPEG- oder PNG-Bilder speichern, sollten Sie in Azure BLOB-Speicher. Es gibt keine Vorteile, sie in Ihrem Media Services-Konto, wenn zu Video- und Audio-Assets zugeordnet werden soll. Oder Sie müssen die Bilder als Overlays im video Encoder können. Media Encoder Standard unterstützt Überlagerung Bilder, Videos, und wie JPEG und PNG unterstützt aufgeführt Eingabeformate. Weitere Informationen finden Sie unter [Erstellen von Overlays](media-services-custom-mes-presets-with-dotnet.md#overlay).

F: wie kann ich Anlagen ein Media Services-Konto in ein anderes kopieren.

A: auf Anlagen von einem Media Services-Konto in ein anderes kopieren mit .NET verfügbar [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) -Erweiterungsmethode in [Azure Media Services.NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) Repository verwenden. Weitere Informationen finden Sie unter [diesen](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) Forumsthread beenden.

Was sind die unterstützten Zeichen zum Benennen von Dateien beim Arbeiten mit AMS?

A: Media Services verwendet den Wert der Eigenschaft IAssetFile.Name beim Erstellen von URLs für das streaming von Inhalten (z. B. http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Aus diesem Grund ist die Prozent-Codierung nicht zulässig. Der Wert der **Name** -Eigenschaft keinen der folgenden [%-Codierung-reservierten Zeichen](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Es können nur auch einen "." für die Erweiterung.


F: wie mit anderen?

A: Nachdem erfolgreich eine Verbindung mit https://media.windows.net, erhalten Sie eine 301-Umleitung einer anderen Media Services URI angegeben. Nachfolgende Aufrufe der neuen URI Siehe [Herstellen einer Verbindung mit Media Services REST-API verwenden](media-services-rest-connect-programmatically.md)müssen. 


F: wie kann ich eine Video während des Codierungsvorgangs drehen.

A: [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) unterstützt Drehung um Winkel von 90, 180/270. Das Standardverhalten ist "Auto", wo versucht die Drehung Metadaten eingehenden MP4-MOV-Datei und ausgleichen. Gehören Sie folgende **Quellen** Element eines der Json-Vorgaben definiert [hier](http://msdn.microsoft.com/library/azure/mt269960.aspx):
    
    "Version": 1.0,
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




##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
