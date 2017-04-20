<properties 
    pageTitle="Filter und dynamische Manifeste | Microsoft Azure" 
    description="Dieses Thema beschreibt, wie Filter erstellt, damit der Client sie bestimmte Abschnitte eines Streams Stream verwenden kann. Media Services erstellt dynamische Manifeste selektive streaming archivieren." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Filter und dynamische Manifeste

Ab Version 2.11 können Media Services Sie Filter für Ihre Anlagen. Diese Filter sind serverseitige Regeln, mit denen Ihre Kunden wie können: Wiedergabe nur ein Abschnitt des Videos (anstatt das gesamte Video), oder geben Sie nur eine Teilmenge der Audio- und Formatvarianten, die Ihr Kunde Gerät (statt alle Formatvarianten, die der Anlage zugeordnet sind) behandeln kann. Diese Filterung Ihrer Anlagen archiviert durch **Dynamische Manifest**s, die Anfrage des Kunden eine Videos basierend auf angegebenen Filter erstellt werden.

Diese Themen behandelt allgemeine Szenarien, in denen Filter wäre sehr für Ihre Kunden und Links zu Themen, die veranschaulichen, wie ein Filter programmgesteuert erstellen (derzeit können Filter mit anderen APIs nur).

##<a name="overview"></a>Übersicht

Bei den Inhalt (streaming live Ereignisse oder Demand) Kunden ist Ihr Ziel eine hohe Bildqualität auf verschiedene Geräte unter andere Netzwerkprobleme. Zu diesem Ziel gehen Sie folgendermaßen vor:

- Codieren der Stream Multi-Bitrate ([adaptive Bitrate](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) Videodaten (dies kümmern und Zustände) und 
- Mithilfe von Media Services [Dynamische Verpackung](media-services-dynamic-packaging-overview.md) dynamisch der Stream verschiedene Protokolle neu gepackt (dies kümmern streaming auf verschiedenen Geräten). Media Services unterstützt die Bereitstellung der folgenden adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern). 

###<a name="manifest-files"></a>Manifest-Dateien 

Beim Codieren einer Anlage für adaptive Bitrate streaming **manifest** (Wiedergabeliste) Datei erstellt (die Datei ist Text oder XML-basierten). **Die Manifestdatei** enthält Metadaten wie streaming: Typ (Audio, Video oder Text) nachverfolgen, verfolgen Namen Start-und Endzeit Bitrate (Eigenschaften), verfolgen Sprachen Präsentationsfenster (Schiebefenster feste Dauer), video-Codec (FourCC). Es weist auch den Player das nächste Fragment Informationen weiter spielbaren video Fragmente verfügbar und ihren Speicherort abrufen. Fragmente sind Segmente tatsächlichen "Blöcken" des video-Content.


Hier ist ein Beispiel einer Manifestdatei: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Dynamische Manifeste

Es gibt [Szenarien](media-services-dynamic-manifest-overview.md#scenarios) , wenn Ihr Kunde mehr Flexibilität braucht als in der Standard-Anlage Manifestdatei beschrieben. Zum Beispiel:

- Gerät: liefern nur angegebene Formatvarianten Sperren Sprachauswahl angegeben, die vom Gerät unterstützt werden, die zur Wiedergabe der Inhaltsfilter ("Wiedergabe"). 
- Reduzieren Sie das Manifest zum Anzeigen eines Unterclips ein live-Ereignis ("Unterclip filtern").
- Zuschneiden der Start eines Videos ("Video Zuschneiden").
- Stellen Sie Präsentation Fenster (DVR) begrenzten Länge des Fensters DVR Player ("Anpassen des Präsentationsfensters") bereitzustellen.
 
Diese Flexibilität bietet Media Services basierend auf vordefinierten [Filter](media-services-dynamic-manifest-overview.md#filters) **Dynamische Manifeste** .  Nachdem Sie Filter definieren, können Ihre Kunden sie eine bestimmte Formatvariante oder Unterclips des Videos übertragen. Filter geben sie Streaming-URL. Filter kann auf adaptive streaming-Protokolle unterstützt [Dynamische](media-services-dynamic-packaging-overview.md)Verpackung Bitrate: HLS, MPEG-DASH Smooth Streaming und HDS. Zum Beispiel:

MPEG DASH URL Filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Reibungslose Streaming URL Filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Weitere Informationen zu Ihrem Inhalte streaming URLs Übersicht [Übermitteln von Inhalten](media-services-deliver-content-overview.md).


>[AZURE.NOTE]Beachten Sie, dass dynamische zeigt nicht die Anlage und das Standardmanifest für diese Anlage ändern. Die Clients können anfordern eines Streams mit oder ohne Filter. 


###<a id="filters"></a>Filter 

Es gibt zwei Arten von Ressourcen filtern: 

- Globale Filter (jeder Ressource in Azure Media Services Account angewendet werden, Lebensdauer des Kontos) und 
- Lokale Filter (kann nur angewendet werden, um eine Anlage mit dem Filter bei der Erstellung zugeordnet wurde, Lebensdauer der Anlage). 

Globale und lokale Typen haben dieselben Eigenschaften. Der Hauptunterschied zwischen den beiden ist für welche Szenarios welche ein Filer besser geeignet ist. Globale Filter eignen im Allgemeinen Geräteprofile (Formatvariante Filtern), konnte lokale Filter verwendet werden, um eine bestimmte Anlage zuschneiden.


##<a id="scenarios"></a>Häufige Szenarien 

Wie bereits erwähnt wurde, bei den Inhalt (streaming live Ereignisse oder Demand) Kunden Ihr Ziel ist eine hohe Bildqualität auf verschiedene Geräte unter anderen Umständen übermitteln. Darüber hinaus, die möglicherweise andere Anforderungen, die Ihre Ressourcen filtern und **Dynamische Manifest**s verwenden. Die folgenden Abschnitte geben einen kurzen Überblick Szenarien filtern.

- Geben Sie nur eine Teilmenge der Audio- und Formatvarianten, die bestimmte Geräte übernehmen (statt alle Formatvarianten, die der Anlage zugeordnet sind). 
- Einen Abschnitt des Videos (anstatt das gesamte Video) wiedergeben.
- DVR Präsentationsfenster anzupassen.

##<a name="rendition-filtering"></a>Filtern von Formatvariante 

Sie können die Anlage mehrere Codierungsprofile (h. 264 Baseline h. 264 hohe AACL, AACH, Dolby Digital Plus) und mehrere Qualität Bitraten codiert. Nicht alle Client-Geräte unterstützen jedoch alle Ihre Anlagen Profile und Bitraten. Ältere Geräte unterstützt z. B. nur h. 264 Baseline + AACL. Eine höhere Bitrate für ein Gerät nutzen kann nicht senden, verschwendet Bandbreite und Gerät Berechnung. Gerät muss alle angegebene Informationen, nur für die Anzeige verkleinern decodieren.

Mit dynamischen Manifesten können Geräteprofile wie Mobile HD-SD usw. Konsole und die Spuren und Qualitäten zu einem Teil der einzelnen Profile.

 
![Formatvariante Filtern wird][renditions2]

Im folgenden Beispiel wurde ein Encoder eine Aufsteckkarte Anlage in sieben (von 180 p 1080p) ISO MP4s Formatvarianten video codieren verwendet. Codierte Anlage kann dynamisch in eine der folgenden Streamingprotokolle verpackt: HLS, optimierten MPEG DASH und HDS.  Klicken Sie oben im Diagramm HLS-Manifest für die Anlage ohne Filter angezeigt (enthält alle sieben Formatvarianten).  Links unten zeigt HLS Manifest, mit dem Namen "Ott" Filter angewendet wurde. Filter "Ott" gibt an, dass alle Bitraten unter 1 Mbit/s, entfernen Sie unten zwei Werte in der Antwort entfernt wurde.  Unten rechts zeigt HLS Manifest, mit dem Namen "mobile" Filter angewendet wurde. "Mobile" Filter gibt an, dass Formatvarianten entfernen die Auflösung ist größer als 720p die beiden führte 1080p Formatvarianten entfernt wird.

![Filtern von Formatvariante][renditions1]

##<a name="removing-language-tracks"></a>Entfernen von Sprachauswahl

Ihre Anlagen gehören mehrere audio Sprachen wie Englisch, Spanisch, Französisch. In der Regel das Player-SDK-Manager standardmäßig Audiospur Auswahl und verfügbaren Audiospuren pro Benutzerauswahl. Diese Player-SDKs zu schwierig ist je über gerätespezifische Player-Frameworks erfordert. Auf einigen Plattformen Player APIs beschränken auch Audioauswahl Funktion, Benutzer auswählen oder ändern die Standard-Audiospur können nicht, nicht enthält. Mit Anlage Filter steuern Sie das Verhalten von Filter, die nur gewünschte audio Sprachen erstellen.

![Sprachauswahl filtern][language_filter]


##<a name="trimming-start-of-an-asset"></a>Start eines Vermögenswertes Zuschneiden 

In den meisten live streaming Ereignissen führen Operatoren Tests vor der Veranstaltung. Sie können beispielsweise ein Blatt folgendermaßen vor Beginn des Ereignisses: "Programm startet umgehend". Wenn Archivierung die Anwendung den Test Schiefer Daten werden archiviert und werden in der Präsentation enthalten. Allerdings sollten diese Informationen nicht an Clients angezeigt. Mit dynamischen Manifesten können Sie Start Time Filter erstellen und unerwünschten Daten aus dem Manifest entfernen.

![Trimmen starten][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Erstellen von unter-Clips (Ansichten) aus einem aktiven Archiv

Viele live Ereignisse sind langlebig und live-Archiv kann mehrere Ereignisse enthalten. Nach der live-Ereignis sollten enden Sender live-Archiv in logischen Anwendung starten und Beenden von Sequenzen. Veröffentlichen Sie diese virtuellen Programme dann separat ohne Post live Archive verarbeiten und erstellen keine eigenständige Vermögenswerte (die nicht nutzen der vorhandenen zwischengespeicherten Fragmente in der CDNs). Beispiele für solche virtuellen Programme (Unterclips) sind Quartale ein oder Basketballspiels, Innings Baseball oder einzelne Ereignisse nachmittags Olympiade Programm.

Mit dynamischen Manifesten können mit Start-/Endzeiten erstellen und virtuelle Ansichten oberhalb von live-Archiv erstellen. 

![Clipkopie filter][subclip_filter]

Gefilterte Ressourcen:

![Skifahren][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Anpassen des Präsentationsfensters (DVR)

Derzeit bietet Azure Media Services kreisförmigen Archive, die Dauer zwischen 5 Minuten - 25 Stunden konfiguriert werden kann. Manifest Filterung kann ein paralleles DVR oberhalb der Archive erstellen, ohne Löschen von Medien verwendet werden. Es gibt viele Szenarios, in denen Sender sollen ein DVR beschränkt die wechselt mit der live und gleichzeitig eine größere Archivierung Fenster bleibt. Sender die Daten verwenden, die aus dem Fenster DVR Clips markieren möchten oder bestimmter DVR-Fenstern für verschiedene Geräte bereitstellen möchten. Die meisten mobilen Geräte behandeln nicht z. B. DVR Fenster (Sie können ein DVR 2 Minuten für mobile Geräte und 1 Stunde Desktopclients verfügen).

![DVR-Fenster][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>Anpassen des LiveBackoff (live Position)

Manifest Filterung kann verwendet werden, einige Sekunden zwischen live live-Programm entfernen. Dadurch können Sender zu Präsentation zeigen Publikation Vorschau Anzeige Einfügemarken bevor die Zuschauer Stream (in der Regel unterstützt-aus 30 Sekunden) erhalten. Sender können push Werbung auf ihren Clientframeworks rechtzeitig zu erhalten und Informationen vor der Anzeige Möglichkeit.

Neben der Unterstützung Werbung kann LiveBackoff für Client live Download Position anpassen, so dass wenn Clients wandert live Rand sie noch Fragmente von Server anstatt HTTP-Fehler 404 oder 412 können verwendet werden.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Kombinieren mehrerer Regeln in einem filter

Sie können mehrere Filterregeln in einem einzelnen Filter kombinieren. So definieren Sie eine Regel Bereich ein live-Archiv Schiefer entfernen und auch Filtern verfügbare Bitraten. Mehrere Filterregeln setzt das Endergebnis (nur Intersection) dieser Regeln.

![mehrere Regeln][multiple-rules]

##<a name="create-filters-programmatically"></a>Programmgesteuertes Erstellen von Filtern

Im folgende Thema erläutert Media Services Entitäten zu filtern. Das Thema wird gezeigt, wie Filter zu erstellen.  

[Erstellen von Filtern mit REST-APIs](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Kombinieren mehrerer Filter (Filter Zusammensetzung)

Sie können auch mehrere Filter in einer URL kombinieren. 

Das folgende Szenario zeigt, warum Sie Filter kombinieren möchten:

1. Sie müssen Ihr video Eigenschaften für mobile Geräte wie Android oder iPAD Filtern (um video Eigenschaften einschränken). Entfernen Sie unerwünschten Eigenschaften erstellen Sie globalen Filter für Geräteprofilen. Wie bereits erwähnt, können globale Filter für alle Anlagen unter demselben Media Services-Konto ohne weitere Zuordnung verwendet werden. 
2. Sie möchten Start- und Zeitpunkt der Anlage zuschneiden. Zu diesem Zweck Sie einen lokalen Filter erstellen und festlegen die Start-bzw. Endzeit. 
3. Dieser Filter (ohne Kombination hinzufügen Qualität Filtern Filter verkürzen die Filter Verwendung schwierig machen möchten) kombinieren möchten.

Um Filter zu kombinieren, müssen Sie die Filternamen Manifest/Wiedergabeliste URL durch Semikolon getrennt. Angenommen, Sie haben einen Filter mit dem Namen *MyMobileDevice* haben Eigenschaften Filter und anderen benannten *MyStartTime* eine bestimmte Uhrzeit festlegen. Sie können sie wie folgt kombinieren:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Sie können bis zu 3 Filter kombinieren. 

Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) Blog.


##<a name="know-issues-and-limitations"></a>Probleme und Grenzen kennen

- Dynamische Manifest arbeitet im GOP Grenzen (Key-Frames) daher Zuschneiden hat GOP Genauigkeit. 
- Gleichnamige lokale und globale Filter filtern können. Beachten Sie, dass lokale Filter haben Vorrang und globale Filter überschreibt.
- Wenn Sie einen Filter aktualisieren, können bis zu 2 Minuten für Streaming-Endpunkt Regeln aktualisieren dauert. Wenn der Inhalt war auf einige Filter und Proxys und CDN Caches zwischengespeichert kann das Aktualisieren dieser Filter Player Fehlern führen. Es wird empfohlen, den Cache nach der Aktualisierung des Filters. Wenn diese Option nicht möglich ist, sollten Sie einen anderen Filter.


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Siehe auch

[Bereitstellung von Content für Kunden (Übersicht)](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 