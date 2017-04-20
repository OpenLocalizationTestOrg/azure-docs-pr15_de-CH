<properties
    pageTitle="Azure Media Services Analytics Übersicht | Microsoft Azure"
    description="Azure Media Services bieten die public Preview Azure Media Analytics eine Auflistung von Sprache und Computer Vision auf Unternehmen, Compliance und Sicherheit weltweit. Azure Media Analytics-Services basieren auf Azure Media Services Plattform-Hauptkomponenten und somit können Medien Ebene am ersten Tag verarbeitet. "
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
    ms.date="10/24/2016"   
    ms.author="milanga;juliako;johndeu"/>

# <a name="azure-media-services-analytics-overview"></a>Analytics Übersicht über Services Azure Media

##<a name="overview"></a>Übersicht

Weitere Organisationen und Unternehmen sind Video als bevorzugte Medium ihre Mitarbeiter, Kunden und Unternehmen Dokumentfunktionen betreiben umfassend. Cloud computing macht es effektiv speichern, übertragen und diese große Mediendateien zugreifen, aber Unternehmen ihren Content Videobibliothek wachsen, sie müssen ebenso effektiv extrahieren Erkenntnisse aus Video Erstellung sinnvoller, personalisierte Interaktionen mit ihrem Publikum und nehmen ihre Tätigkeit auf die nächste Ebene.

Auf wachsenden Bedarf am Markt bietet Azure Media Services Media Analytics Sprach- und Vision Komponenten (bei Unternehmen, Kompatibilität, Sicherheit und globale), die Organisationen und Unternehmen leiten Sie umsetzbare Einsichten von ihren Videodateien erleichtern. Azure Media Analytics-Services basieren auf Azure Media Services Plattform-Hauptkomponenten und somit können Medien Ebene am ersten Tag verarbeitet.

Azure Media Analytics können Entwickler schnell mit Video in begrenztem Umfang Vision und erweiterte Funktionalität in Programme bringen. Azure Media Analytics baut mit vollen Skalenendwertes, Compliance, Sicherheit und große Organisationen erforderlichen weltweit verwendet.

Das folgende Diagramm zeigt **Media Analytics** und andere Hauptbestandteile der Media Services-Plattform. 

![VoD-workflow](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

Media Analytics Medienprozessoren erzeugen MP4 oder JSON-Dateien. Wenn Media Prozessor eine MP4-Datei erzeugt, können Sie schrittweise die Datei herunterladen. Media-Prozessor eine JSON produziert, laden Sie die Datei aus dem Azure blobspeicher. 

## <a name="azure-media-analytics-services"></a>Azure Media Analysedienste

- **Indexer** – Azure Media Indexer kann durchsucht werden, sowie das Generieren von geschlossener Untertiteln Spuren Inhalts. Azure Media Services veröffentlicht **Azure Media Indexer 2 Vorschau** mit schneller indizieren und umfassendere Unterstützung von Sprachen. Den unterstützten Sprachen gehören Englisch Spanisch Französisch Deutsch Italienisch Chinesisch Portugiesisch und Arabisch. Ausführliche Informationen und Beispiele finden Sie unter [Prozess Videos mit Azure Media Indexer 2](media-services-process-content-with-indexer2.md)
 
- **Hyperlapse** – Microsoft Hyperlapse ist aus 20 Jahren Vision Forschung bei Microsoft Research (MSR), video-Stabilisierung und Zeit verfallen für schnelle, Verbrauchsmaterial, schön Videos aus der Langform Content kombinieren. Neben Zeitabläufe, auch können Hyperlapse flackernde Videos über Mobiltelefone und Camcordern erfasst stabile Videos erstellen. Ausführliche Informationen und Beispiele finden Sie unter [Hyperlapse Mediendateien mit Azure Media Hyperlapse](media-services-hyperlapse-content.md)
 
- **Bewegungsmelder** – mit diesem Dienst können um Bewegung in einem Video mit Briefpapier zu erkennen. Dies ist ideal für Kunden, die von false Positives bewegungsereignisse von Kameras auf Überwachung video-Feeds überprüfen möchten. Ausführliche Informationen und Beispiele finden Sie unter [Erkennung von Azure Media Analytics Bewegung](media-services-motion-detection.md).
 
- **Face-Erkennung und Gesicht Emotionen** – mit diesem Dienst erkennt Gesichter und Emotionen, einschließlich Glück Trauer Überraschung Wut Missachtung Angst Abschicken und Gleichgültigkeit-Neutral. Dies hat mehrere nützliche Branche beobachten beschrieben, zusammenfassen und Analysieren von Reaktionen der Teilnehmer ein Ereignis. Ausführliche Informationen und Beispiele finden Sie unter [Fläche und Emotionen Azure Media Analytics](media-services-face-and-emotion-detection.md).
 
- **Zusammenfassung der Video** -Video Zusammenfassung können Sie die Zusammenfassung der langen Videos erstellen, indem Sie automatisch das Quellvideo interessante Ausschnitte auswählen. Dies ist nützlich, wenn Sie einen schnellen Überblick auf ein langes Video bereitstellen möchten. Ausführliche Informationen und Beispiele finden Sie unter [Verwenden Azure Video Miniaturansichten Video Zusammenfassung erstellen](media-services-video-summarization.md)

- **Texterkennung** - Azure Media Analytics OCR (texterkennung) können Sie Textinhalt in Videodateien in bearbeitbare, durchsuchbaren digitalen Text konvertieren. Dadurch werden die Extraktion von aussagekräftigen Metadaten aus das Videosignal Medien zu automatisieren.
 
- **Skalierbare Gesicht Schwärzen** - **Azure Media Korrespondent** ist ein Azure Media Analytics MP, die skalierbare Gesicht Schwärzen in die Cloud bietet. Gesicht Schwärzen können Sie das Video um Flächen der ausgewählten Personen Weichzeichnen ändern. Gesicht Schwärzen Service in öffentliche Sicherheit und Medien Szenarien verwenden möchten. Einige Minuten lang, die mehrere Flächen enthält können Stunden manuell Schwärzen, aber mit diesem Dienst Gesicht Schwärzen Prozess nur ein paar einfache Schritte erforderlich. Weitere Informationen finden Sie in [diesem](media-services-face-redaction.md) Artikel.

 
## <a name="common-scenarios"></a>Häufige Szenarien

Unten sind einige Szenarien, Azure Media Analytics können Organisationen und Unternehmen Branchen sammeln Erkenntnisse aus Video mehr personalisierte Zielgruppe und Mitarbeiter Engagements, sowie effektiver verwalten großer Videos:

- **Call Center** – auch mit social Media Kundenanruf Ressourcen weiterhin einen Großteil der Kundentransaktionen erleichtern. Diese Audiodaten codiert ist eine Fülle von Informationen, die zur Verbesserung der Produkt-Roadmaps und Call-Center-Mitarbeiter höhere Kundenzufriedenheit sowie die Schulung analysiert werden können. Kunden können mithilfe von Azure Media Indexer Text und Erstellen eines Suchindexes und Dashboards Intelligence um am häufigsten extrahieren beschwert sich, Quelle der Beschwerden und andere relevanten Daten zu extrahieren.

- **Benutzergenerierter muss** – haben News Medien Polizei, viele Organisationen öffentliche für Portale, wo sie UGC Media wie Videos und Bilder übernehmen. Das Volumen der Inhalt kann aufgrund unerwarteter Ereignisse Spitze. In diesen Szenarien ist es fast unmöglich eine effektive manuelle Überprüfung des Inhalts für angemessen. Kunden können für den Dienst muss sich auf den Inhalt, der geeignet ist.

- **Überwachung** - mit der IP-Kameras besteht Explosionsgefahr überwachungsvideos. Manuell überprüfen Videoüberwachung immer intensive und fehleranfällig. Azure Media Analytics stellt mehrere Komponenten Bewegungsmelder gesichtserkennung und Hyperlapse überprüfen, verwalten und erstellen Derivate einfacher gestalten.

## <a name="media-services-analytics-media-processors"></a>Mediendienste Medienprozessoren Analytics 

Dieser Abschnitt listet alle der Media Services Analytics Media Prozessoren (MP) und zeigt wie .NET oder anderen MP-Objekt abzurufen.

### <a name="mp-names"></a>MP-Namen


- Azure Media Indexer 2 Vorschau
- Azure Media Indexer
- Azure Media Hyperlapse
- Azure Media Gesichtserkennung
- Azure Media Bewegungsmelder
- Azure Media Video-Miniaturen
- Azure Media OCR

### <a name="net"></a>.NET

Die folgende Funktion akzeptiert einen angegebenen Namen MP und MP-Objekt zurückgegeben.

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


## <a name="rest"></a>REST

Anforderung:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net
    
Antwort:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

##<a name="demos"></a>Demos

[Azure Media Analytics-demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

##<a name="next-steps"></a>Nächste Schritte

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-articles"></a>Verwandte Artikel

[Media Services Analytics Ankündigung](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)
  

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
