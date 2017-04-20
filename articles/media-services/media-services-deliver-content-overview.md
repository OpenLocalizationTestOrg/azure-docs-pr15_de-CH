<properties
    pageTitle="Bereitstellung von Content für Kunden | Microsoft Azure"
    description="Dieses Thema bietet eine Übersicht über Was ist Ihre Inhalte mit Azure Media Services an."
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


# <a name="deliver-content-to-customers"></a>Bereitstellung von Content für Kunden
Wenn Sie Ihre streaming oder On-Demand-Inhalte an Kunden liefern, ist Ihr Ziel hohe Bildqualität auf verschiedene Geräte unter andere Netzwerkprobleme.

Um dieses Ziel zu erreichen, können Sie:

- Codieren der Stream einen Videostream Multi-Bitrate (adaptive Bitrate). Diese kümmern Vorschriften und.
- Verwenden Sie Microsoft Azure Media Services [dynamische Verpackung](media-services-dynamic-packaging-overview.md) der Stream in unterschiedlichen Protokollen dynamisch neu gepackt. Diese kümmern streaming auf verschiedenen Geräten. Media Services unterstützt die Bereitstellung der folgenden adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG-DASH und HDS (für Adobe Primetime/Access nur Lizenznehmern).

Dieser Artikel bietet eine Übersicht wichtiger Content Delivery Konzepte.

Überprüfen Sie bekannte Probleme finden Sie unter [bekannte Probleme](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamische Verpackung

Mit der Media Services bietet dynamische Verpackung liefern Sie adaptive Bitrate MP4 oder Smooth Streaming codierte Inhalte in streaming-Formate von Media Services (MPEG-DASH HLS Smooth Streaming, HDS) ohne diese Streaming-Formate erneut verpacken. Es wird empfohlen, Ihre Inhalte mit dynamischen Verpackung.

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- Codieren der Datei Aufsteckkarte (Quelle) in eine Reihe von adaptiven Bitrate MP4 oder adaptive Bitrate Smooth Streaming-Dateien.
- Erhalten Sie mindestens auf Anforderung Streaming-Einheit für Streaming-Endpunkt zu Ihrem Inhalt soll. Weitere Informationen finden Sie unter [streaming reservierte Einheiten bei Bedarf zu skalieren](media-services-portal-manage-streaming-endpoints.md).

Mit dynamischen speichern und die Dateien in einzelne Speicherformat bezahlen. Media Services erstellen und die entsprechende Antwort auf Ihre Anfragen bedienen.

Bietet Zugriff auf dynamische Verpackung, bieten streaming reservierte Einheiten bei Bedarf in Schritten von 200 Mbit/s erworben werden dedizierte Egress-Kapazität. Standardmäßig wird bei Bedarf streaming in einem Modell freigegebene Instanz konfiguriert für die Server Ressourcen (z. B. COMPUTE- oder Ausgang Kapazität) mit anderen Benutzern gemeinsam genutzt werden. Einen Streaming-Durchsatz bei Bedarf können Sie bei Bedarf streaming reservierte Einheiten verbessern.

Weitere Informationen finden Sie unter [dynamische Verpackung](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filter und dynamische Manifeste

Sie können Filter für Ihre Anlagen mit Media Services definieren. Diese Filter sind serverseitige Regeln, mit denen Ihre Kunden beispielsweise einen bestimmten Abschnitt des Videos wiedergeben oder eine Untermenge der Audio- und Formatvarianten, die Ihr Kunde Gerät (statt alle Formatvarianten, die der Anlage zugeordnet sind) behandeln kann. Diese Filterung erfolgt durch *dynamische Manifeste* , wenn Ihr Kunde möchte einen Videos basierend auf angegebenen Filter erstellt werden.

Weitere Informationen finden Sie unter [Filter und dynamische Manifeste](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Locators

Um Ihre Benutzer mit einer URL angeben, das zum Streamen oder Ihre Inhalte herunterladen, müssen Sie an Ihrer Ressource Locator erstellen. Ein Locator bietet einen Zugriff auf die Dateien in einer Anlage. Media Services unterstützt zwei Arten von Locators:

- OnDemandOrigin-Locators. Diese werden Medien (z. B. MPEG-DASH, HLS oder Smooth Streaming) oder progressiv herunterladen.
-  Gemeinsamer Zugriff Signatur (SAS) URL-Locators. Diese dienen Mediendateien auf Ihrem lokalen Computer herunterladen.

Eine *Richtlinie* wird zum Definieren der Berechtigungen (wie lesen, schreiben und Liste) und für die ein Client Zugriff für eine bestimmte Ressource verfügt. Beachten Sie, dass die Berechtigung (AccessPermissions.List) nicht verwendet werden soll, erstellen Sie einen Locator OrDemandOrigin.

Locators haben Ablaufdaten. Azure-Portal wird ein Ablaufdatum 100 Jahre für Locator.

>[AZURE.NOTE] Wenn Sie Azure-Portal Locators vor März 2015 erstellt, wurden diese Locators nach zweijähriger Gültigkeitsdauer festgelegt.

Aktualisieren Sie ein Ablaufdatum ein Locator verwenden Sie [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) oder [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs. Beachten Sie, dass beim Aktualisieren des Ablaufdatum des SAS-Locator URL.

Locators sollen keine benutzerspezifischen Zugriffskontrolle verwalten. Unterschiedliche Zugriffsrechte für einzelne Benutzer erhalten durch Digital Rights Management (DRM) Lösungen. Weitere Informationen finden Sie unter [Sichern von Medien](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Wenn Sie einen Locator erstellen, möglicherweise 30 Sekunden Speicherbedarf und Verteilung Prozesse in Azure-Speicher.


## <a name="adaptive-streaming"></a>Adaptives streaming

Adaptive Bitrate Technologies ermöglichen Videoplayer Netzwerkprobleme ermitteln und wählen Sie aus mehreren Bitraten. Wenn Kommunikation nimmt ab, kann der Client damit niedrigere Bildqualität Wiedergabe fortsetzen kann eine niedrigere Bitrate auswählen. Netzwerkprobleme zu verbessern, kann der Client eine höhere Bitrate video Qualität wechseln. Azure Media Services unterstützt die folgenden adaptive Bitrate Technologies: http-Live-Streaming (HLS), Smooth Streaming MPEG-DASH und HDS.

Um Benutzer streaming URLs, müssen Sie zunächst einen Locator OnDemandOrigin erstellen. Locator können Sie den Pfad der Anlage mit dem Inhalt zu streamen. Um diese Inhalte streamen zu können, müssen Sie jedoch weitere diesen Pfad ändern. Zum Erstellen einer vollständigen URLs streaming Manifestdatei muss Verketten der Locator Pfadwert und das Anwendungsmanifest (filename.ism) Dateinamen. Anschließend angefügt **Manifest** und ein geeignetes Format (sofern erforderlich) den Locator-Pfad.

>[AZURE.NOTE]Sie können auch die Inhalte über eine SSL-Verbindung übertragen. Dazu sicherzustellen Sie, dass Ihre streaming URLs mit HTTPS beginnen.

Sie können nur über SSL streamen, wenn Streaming-Endpunkt aus dem Ihre Inhalte liefern nach dem 10. September 2014 erstellt wurde. Wenn Ihre streaming URLs auf streaming Endpunkte erstellt am 10. September 2014 enthält die URL "streaming.mediaservices.windows.net". Streaming URLs mit "origin.mediaservices.windows.net" (das alte Format) unterstützen SSL. Wenn der URL im alten Format ist über SSL übertragen werden soll, erstellen Sie eine neue streaming. URLs basierend auf neuen Streaming-Endpunkt zum Verwenden des Inhalts über SSL.


## <a name="streaming-url-formats"></a>Streaming-URL-Formate

### <a name="mpeg-dash-format"></a>MPEG-DASH-format

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=MPD-Time-CSF)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP-Live-Streaming (HLS) V4-Formats

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP-Live-Streaming (HLS) V3-format

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL-V3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP-Live-Streaming (HLS) Format mit nur-Audio-filter

Befinden sich nur-Audio-Tracks in der HLS Manifest. Dies ist erforderlich für Apple Store-Zertifizierung für Mobilfunknetze. In diesem Fall wechselt ein Client keine ausreichenden Bandbreite oder über 2G-Verbindung verbunden ist, Sie Wiedergabe nur Audio. Dadurch zu Content streaming ohne Pufferung, aber kein Bild. In einigen Szenarien kann über nur-Audio-Player Pufferung bevorzugte befinden. Wenn nur-Audio-Titel entfernen hinzufügen **nur Audio = False** auf die URL.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL-v3,Audio-only=false)

Weitere Informationen finden Sie unter [Manifest Satz Support und HLS Zusatzfunktionen ausgegeben](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).


### <a name="smooth-streaming-format"></a>Reibungslose Streaming format

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Beispiel:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest

### <a id="fmp4_v20"></a>Glatte Streaming 2.0-Manifests (legacy Manifest)

Standardmäßig enthält Manifestformat Smooth Streaming wiederholen Tag (R-Tag). Allerdings unterstützen einige Spieler keine R-Tag. Mit diesen Clients können eine Format, die R-Tag deaktiviert:

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>HDS (für Adobe PrimeTime/Access Lizenznehmer nur)

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Progressiver download

Beim progressiven herunterladen können Sie starten, Medienwiedergabe, bevor die gesamte Datei heruntergeladen wurde. ISM * (Ismv, Isma, Ismt oder Ismc) Dateien können nicht progressiv heruntergeladen.

Inhalt progressiv herunterladen, verwenden Sie die OnDemandOrigin Locator. Das folgende Beispiel zeigt die URL, die auf dem OnDemandOrigin Locator basiert:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Sie müssen Anlagen Speicher verschlüsselt entschlüsseln, die vom Ursprung Dienst für progressiven Download übertragen werden soll.

## <a name="download"></a>Herunterladen

Um die Client-Gerät herunterladen, müssen Sie einen SAS-Locator erstellen. SAS-Locator erhalten Sie Zugriff auf den Azure-Speicher-Container die Datei befindet. Um die Download-URL zu erstellen, müssen Sie den Dateinamen zwischen Host und Signatur SAS-einbetten.

Das folgende Beispiel zeigt die beruht SAS-Locator URL:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Folgendes gilt:

- Sie müssen Anlagen Speicher verschlüsselt entschlüsseln, die vom Ursprung Dienst für progressiven Download übertragen werden soll.
- Ein Download, der nicht innerhalb von 12 Stunden abgeschlossen ist, schlägt fehl.

## <a name="streaming-endpoints"></a>Streaming-Endpunkte

Streaming-Endpunkt stellt einen Streaming-Dienst, der Inhalte an eine Clientanwendung Player oder ein Content Delivery Network (CDN) für die weitere Verteilung liefern kann. Ausgehenden Datenstrom von einem Streaming-Endpunkt-Dienst kann einen Livestream oder eine Anlage Demand Media Services-Konto. Sie können auch die Kapazität des Streaming-Endpunkt Service Bandbreite Bedürfnisse anpassen streaming reservierte Einheiten behandeln steuern. Sie sollten mindestens eine reservierte Einheit in einer Anwendung zuweisen. Weitere Informationen finden Sie unter [wie Media Service](media-services-portal-manage-streaming-endpoints.md).

## <a name="known-issues"></a>Bekannte Probleme

### <a name="changes-to-smooth-streaming-manifest-version"></a>Ändert Smooth Streaming manifest version

Vor Juli 2016 Service Release bei Anlagen von Media Encoder Standard Media Encoder Premium-Workflow oder früheren Azure Media Encoder wurden übertragen mit dynamischen Packaging - Smooth Streaming Manifest zurückgegeben würde Version 2.0 entsprechen. In Version 2.0 verwenden Fragment Dauer nicht so genannte wiederholen (R)-Tags. Zum Beispiel:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

Juli 2016 Service Release entspricht das generierte Manifest Smooth Streaming Version 2.2 Fragment Dauer mit Tags wiederholen. Zum Beispiel:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Einige Legacyclients Smooth Streaming unterstützen die wiederholen-Tags und nicht das Manifest geladen. Um dieses Problem zu verringern, verwenden Sie den Parameter legacy Manifestformat **(Format = v20 fmp4)** oder aktualisieren Sie Ihren Client auf die neueste Version der Wiederholung Tags unterstützt. Weitere Informationen finden Sie unter [Glätten Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwandte Themen

[Aktualisieren Sie Media Services Locators nach Rollen Speicherschlüssel](media-services-roll-storage-access-keys.md)
