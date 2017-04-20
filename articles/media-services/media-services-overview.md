<properties 
    pageTitle="Übersicht über Azure Media Services und Szenarien | Microsoft Azure" 
    description="Dieses Thema bietet eine Übersicht über Azure Media Services" 
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
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>

#<a name="azure-media-services-overview-and-common-scenarios"></a>Übersicht über Azure Media Services und Szenarien

Microsoft Azure Media Services ist eine erweiterbare cloudbasierte Plattform, mit der Entwickler skalierbare Media Management und Bereitstellung Applikationen erstellen kann. Media Services basieren auf REST-APIs, mit denen Sie sicher hochladen, speichern, codieren und Verpacken Video- oder Audio-Inhalten für on-Demand und live streaming für verschiedene Clients (TV, PC und mobile Geräte).

End-to-End-Workflows mit vollständig Media Services erstellen. Sie können auch Teile des Workflows mit Drittanbieterkomponenten. Codieren Sie z. B. mit einem Drittanbieter-Encoder. Anschließend uploaden, schützen, Paket, übermitteln mit Media Services.

Sie können der Inhalt live oder Bereitstellung von Content bei Bedarf Stream. In diesem Thema werden Szenarien für die Bereitstellung der Inhalte [live](media-services-overview.md#live_scenarios) oder [bei Bedarf](media-services-overview.md#vod_scenarios) Das Thema bietet außerdem links zu anderen Themen.

## <a name="sdks-and-tools"></a>SDKs und tools

Zum Erstellen von Media Services Solutions können Sie:

- [Media Services REST-API](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- Eines der verfügbaren Client-SDKs:
- [Azure Media Services SDK für .NET](https://github.com/Azure/azure-sdk-for-media-services)
- [Azure SDK für Java](https://github.com/Azure/azure-sdk-for-java)
- [Azure PHP-SDK](https://github.com/Azure/azure-sdk-for-php)
- [Azure Media Services für Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Dies ist eine nicht-Microsoft-Version von Node.js-SDK. Verwaltet eine Community und haben derzeit nicht 100 %-igen AMS-APIs).
- Vorhandene Tools:
- [Azure-portal](https://portal.azure.com/)
- [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) ist eine Winforms / C#-Anwendung für Windows)

##<a name="media-services-learning-paths"></a>Media Services Lernpfade

AMS Lernpfade hier sehen:

- [AMS Live-Streaming-Workflow](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [AMS bedarfsgesteuerte Workflows Streaming](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##<a name="prerequisites"></a>Erforderliche Komponenten

Um mit Azure Media Services, benötigen Sie Folgendes:

3. Ein Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com).
2. Ein Azure Media Services-Konto. Mit der Azure-Portal, .NET oder REST API Azure Media Services Account erstellt. Weitere Informationen finden Sie unter [Konto erstellen](media-services-portal-create-account.md).
3. (Optional) Umgebung einrichten. Wählen Sie .NET oder REST API für die Umgebung ein. Weitere Informationen finden Sie in der [Umgebung eingerichtet](media-services-dotnet-how-to-use.md).

Auch Informationen Sie zum programmgesteuerten [Verbinden](media-services-dotnet-connect-programmatically.md)verbinden.
4. (Empfohlen) Reservieren Sie eine oder mehrere Maßeinheiten. Es wird empfohlen, eine oder mehrere Maßeinheiten für Applikationen in Produktion zugewiesen werden.   Weitere Informationen finden Sie unter [Verwalten von streaming-Endpunkte](media-services-portal-manage-streaming-endpoints.md).

##<a name="concepts-and-overview"></a>Konzepte und Übersicht

Azure Media Services Konzepte finden Sie unter [Konzepte](media-services-concepts.md).

Gewusst-wie-Serie, die zu den wichtigsten Komponenten Azure Media Services führt, finden Sie unter [Azure Media Services schrittweise Lernprogramme](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Diese Serie hat einen groben Überblick über die Konzepte und AMSE Tool um AMS Aufgaben veranschaulichen. Beachten Sie, dass AMSE Tool Windows-Tool. Dieses Tool unterstützt die meisten der Aufgaben, die Sie programmgesteuert mit [AMS-SDK für .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK für Java](https://github.com/Azure/azure-sdk-for-java)oder [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)erreichen.

##<a id="vod_scenarios"></a>Bereitstellung von Medien bei Bedarf Azure Media Services: Allgemeine Szenarios und Aufgaben

Dieser Abschnitt beschreibt Szenarien und Links zu relevanten Themen. Das folgende Diagramm zeigt die Hauptbestandteile der Media Services-Plattform, die beteiligt sind bei der Bereitstellung von Inhalten bei Bedarf. 

![VoD-workflow](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)


###<a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Schutz von Content im Speicher und bieten streaming Media in Klartext (unverschlüsselt)

1. Anlage hochladen Sie eine qualitativ hochwertige Mezzanine-Datei.
    
    Es wird empfohlen, der Anlage zum Schutz des Inhalts beim Hochladen und in Ruhe im Speicher Verschlüsselungsoption Speicher zuweisen.
 
1. Codieren Sie adaptive Bitrate MP4-Dateien. 

    Es wird empfohlen, Ausgabe Asset Schutz des Inhalts ruhende Verschlüsselungsoption Speicher zuweisen.
    
1. Konfigurieren Sie Asset Lieferung Richtlinie (dynamische Verpackung verwendet). 
    
    Ist die Anlage Speicher verschlüsselt, konfigurieren **muss** Anlage Lieferung Richtlinie. 

1. Veröffentlichen der Anlage einen auf-Anforderung-Locator erstellen.

    Müssen Sie mindestens eine reservierte Einheit auf Streaming-Endpunkt zum Streaming von Inhalten soll streaming.

1. Stream veröffentlichten Inhalte.

###<a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Schutz von Content im Speicher, liefern dynamisch verschlüsselte Stream-Medien  

Um dynamische Verschlüsselung verwenden zu können, muss zunächst mindestens eine reservierte Einheit streaming auf Streaming-Endpunkt abgerufen, verschlüsselte Inhalte übertragen werden soll.

1. Anlage hochladen Sie eine qualitativ hochwertige Mezzanine-Datei. Gelten Sie Verschlüsselung Speicheroption für die Anlage.
1. Codieren Sie adaptive Bitrate MP4-Dateien. Gelten Sie Verschlüsselung Speicheroption für die Ausgabe Anlage.
1. Erstellen Sie Content Verschlüsselungsschlüssel für die Anlage, dynamisch während der Wiedergabe verschlüsselt werden soll.
2. Konfigurieren Sie Content Schlüssel Autorisierungsrichtlinie.
1. Konfigurieren Sie Asset Lieferung Richtlinie (dynamische Verpackung und dynamische Verschlüsselung verwendet).
1. Veröffentlichen der Anlage einen auf-Anforderung-Locator erstellen.
1. Stream veröffentlichten Inhalte. 

###<a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Verwenden Sie Media Analytics ableiten umsetzbare Einsichten von videos 

Media Analytics ist eine Sammlung von Sprache und Vision, die Organisationen und Unternehmen leiten Sie umsetzbare Einsichten von ihren Videodateien erleichtern. Weitere Informationen finden Sie unter [Übersicht über Azure Media Services Analytics](media-services-analytics-overview.md).

1. Anlage hochladen Sie eine qualitativ hochwertige Mezzanine-Datei.
2. Verwenden Sie eines der folgenden Medien Analytics Videos verarbeiten:
    
    - **Indexer** - [Prozess Videos mit Azure Media Indexer 2](media-services-process-content-with-indexer2.md)
    - **Hyperlapse** – [Hyperlapse Mediendateien mit Azure Media Hyperlapse](media-services-hyperlapse-content.md)
    - **Bewegungsmelder** – [Bewegungserkennung Azure Media Analytics](media-services-motion-detection.md).
    - **Face-Erkennung und Gesicht Emotionen** [Fläche und Emotionen Azure Media Analytics](media-services-face-and-emotion-detection.md).
    - **Video-Zusammenfassung** – [Mit Azure Video Miniaturansichten Erstellen einer Video-Zusammenfassung](media-services-video-summarization.md)
3. Media Analytics Medienprozessoren erzeugen MP4 oder JSON-Dateien. Wenn Media Prozessor eine MP4-Datei erzeugt, können Sie schrittweise die Datei herunterladen. Media-Prozessor eine JSON produziert, laden Sie die Datei aus dem Azure blobspeicher. 


###<a name="deliver-progressive-download"></a>Progressiven Download bereitstellen 

1. Anlage hochladen Sie eine qualitativ hochwertige Mezzanine-Datei.
1. Codiert eine MP4-Datei.
1. Veröffentlichen der Anlage OnDemand oder SAS-Locator erstellen.

    Wenn OnDemand Locator verwenden, müssen Sie mindestens einen Stream reservierten Einheit für den Streaming-Endpunkt Inhalt progressiv herunterladen möchten.

    SAS-Locator verwenden, wird der Inhalt von Azure BLOB-Speicher heruntergeladen. In diesem Fall müssen Sie keine reservierte Einheiten streaming haben.
  
1. Inhalt progressiv herunterladen.

##<a id="live_scenarios"></a>Bereitstellung von Live-Streaming-Ereignisse mit Azure Media Services

Beim Arbeiten mit Live-Streaming sind häufig die folgenden Komponenten beteiligt:

- Eine Kamera, die ein Ereignis verwendet wird.
- Ein live video-Encoder, der Signale von der Kamera zu konvertiert, die ein live-streaming-Dienst gesendet werden.

Optional Encoder mehrere Echtzeit synchronisiert. Für bestimmte kritische live Ereignisse bei Bedarf sehr hohe Verfügbarkeit und Qualität sollten aktive redundante Encoder mit SynchronisierungUm erreichen nahtloses Failover ohne Datenverlust beschäftigen.
- Ein live streaming-Dienst, der Ihnen Folgendes ermöglicht:

- Aufnehmen von live-Inhalten über verschiedene live streaming-Protokolle (z. B. RTMP oder Smooth Streaming)
- Codieren Sie der Stream (optional) adaptive Bitrate Stream
- Vorschau von live-Streams
- Speichern des aufgenommenen Inhalts um übertragen (Demand) höher
- Bereitstellung von Content über gängige Streaming-Protokolle (z. B. MPEG DASH optimierten HLS, HDS) direkt an Ihre Kunden oder zu einem Content Delivery Network (CDN) für die weitere Verteilung.


**Microsoft Azure Media Services** (AMS) ermöglicht die Aufnahme codieren, Vorschau, speichern und Ihre live-Streaming-Inhalte.

Wenn Ihre Kunden liefern ist Ihr Ziel eine hohe Bildqualität auf verschiedene Geräte unter andere Netzwerkprobleme. Verwenden Sie um zu kümmern und Konditionen, live Encoder der Stream Multi-Bitrate (adaptive Bitrate)-Videostream codiert.  Um streaming auf verschiedenen Geräten, verwenden Sie Media Services [dynamische Verpackung](media-services-dynamic-packaging-overview.md) dynamisch der Stream verschiedene Protokolle erneut verpacken. Media Services unterstützt die Bereitstellung der folgenden adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern).

In Azure Media Services, **Kanäle**, **Programme**und **StreamingEndpoints** behandeln Sie alle live-Streaming-Funktionen einschließlich Erfassung, Formatierung, DVR, Sicherheit, Skalierbarkeit und Redundanz.

Ein **Kanal** stellt eine Pipeline für die Verarbeitung von live-streaming-Inhalten. Ein Kanal erhalten eine Eingabe Streams auf folgende Weise:

- Für lokale live Encoder sendet Multi-Bitrate **RTMP** oder **Smooth Streaming** (fragmentierte MP4) an den Kanal für **Pass-Through-** Bereitstellung konfiguriert ist. **Pass-Through** wird beim **Kanal**aufgenommenen Streams passieren, ohne weitere Verarbeitung. Können die folgenden live Encoder, die Multi-Bitrate Smooth Streaming ausgeben: elementar, Envivio, Cisco.  Die folgenden live Encoder Ausgabe RTMP: Adobe Flash Live, Telestream Wirecast und Tricaster Kodierungsprogramme.  Live-Encoder kann auch einzelne Bitrate Stream an einen Kanal senden, für live-Codierung nicht aktiviert ist, aber nicht empfohlen. Bei Bedarf bietet Media Services Stream.

>[AZURE.NOTE] Pass-Through-Methode ist die wirtschaftlichste Methode zum streaming Leben Wenn Sie Ereignisse über einen längeren Zeitraum, und bereits in lokalen Encoder investiert haben. [Preisdetails](/pricing/details/media-services/) anzeigen

- Encoder lokale live sendet Single-Bitrate an den Kanal aktiviert ist, führen Sie live-Codierung mit Media Services in einem der folgenden Formate: RTP (MPEG-TS) RTMP oder Smooth Streaming (fragmentiert MP4). Der Kanal führt dann live Codierung des eingehenden Datenstrom auf einen Videostream Multi-Bitrate (adaptiv) einzelne Bitrate. Bei Bedarf bietet Media Services Stream.


###<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Arbeiten mit Channels, die Multi-Bitrate Livestream aus lokalen Encoder (Pass) empfangen

Das folgende Diagramm zeigt die Hauptbestandteile der AMS-Plattform, die beteiligt sind, **Pass-Through-** Workflow.

![Live-workflow][live-overview2]

Weitere Informationen finden Sie unter [Arbeiten mit Channels, empfangen Multi-Bitrate Livestream aus lokalen Encoder](media-services-live-streaming-with-onprem-encoders.md).

###<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Arbeiten mit Channels die aktivierten live Codierung mit Azure Media Services ausführen

Das folgende Diagramm zeigt die Hauptbestandteile der AMS-Plattform, die beteiligt sind, Live-Streaming-Workflows, ein Kanal live Codierung mit Media Services ausführen aktiviert ist.

![Live-workflow][live-overview1]

Weitere Informationen finden Sie unter [Arbeiten mit Channels, die zum Ausführen mit Azure Media Services Livecodierungsmodus aktiviert sind](media-services-manage-live-encoder-enabled-channels.md).


##<a name="consuming-content"></a>Inhalte

Azure Media Services bietet Werkzeuge funktionsreiche, dynamische Player Clientanwendungen für die meisten Plattformen einschließlich erstellen: iOS Geräte, Android, Windows, Windows Phone, Xbox und Set-Top-Boxen. Das folgende Thema enthält Links zu SDKs und Player-Frameworks, mit denen Sie eigene Clientanwendungen entwickeln, die Stream-Medien von Media Services nutzen können.

[Video-Player Anwendungsentwicklung](media-services-develop-video-players.md)

##<a name="enabling-azure-cdn"></a>Aktivieren von Azure CDN

Media Services unterstützt die Integration mit Azure CDN. Informationen zum Aktivieren von Azure CDN finden Sie unter [Verwalten von Streaming Endpunkte ein Media Services-Konto](media-services-portal-manage-streaming-endpoints.md).

##<a name="scaling-a-media-services-account"></a>Skalieren von Media Services-Konto

**Media Services** zu skalieren, die Anzahl der **Streaming reserviert** und **Codierung reservierte Einheiten** , die Ihr Konto bereitgestellt werden soll.

Sie können auch Ihr Media Services-Konto Speicherkonten hinzu skalieren. Jedes Storage-Konto beträgt 500 TB. Erweitern Sie Ihren Speicher hinaus Standard können Sie ein einzelnes Media Services-Konto mehrere Storage-Konten zuordnen.

[Dieses](media-services-portal-scale-streaming-endpoints.md) Thema enthält Links zu relevanten Themen.

##<a name="support"></a>Unterstützung

[Azure-Support](https://azure.microsoft.com/support/options/) bietet Support-Optionen für Azure Media Services einschließlich.

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="service-level-agreement-sla"></a>Vereinbarung zum Servicelevel (SLA)

- Media Services Codierung garantieren wir Verfügbarkeit von 99,9 % der REST API-Transaktionen.
- Für Streaming, werden wir erfolgreich Clientanfragen mit einer Verfügbarkeit von 99,9 % von vorhandenen Medieninhalten beim Erwerb mindestens eine Streaming-Einheit.
- Für Live-Kanäle garantieren wir mit Channels externe Konnektivität mindestens 99,9 % der Zeit.
- Content Protection garantieren wir, dass wir erfolgreich wichtige Anfragen mindestens 99,9 % der Zeit erfüllen.
- Für Indexer werden wir erfolgreich Indexer Aufgabenanfragen verarbeitet ein Kodierung reserviert service Unit 99,9 % der Zeit.

Weitere Informationen finden Sie unter [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
