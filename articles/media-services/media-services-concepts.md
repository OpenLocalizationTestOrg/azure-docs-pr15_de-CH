<properties 
    pageTitle="Azure Media Services Konzepte | Microsoft Azure" 
    description="Dieses Thema bietet eine Übersicht über Azure Media Services (Konzepte)" 
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

#<a name="azure-media-services-concepts"></a>Azure Media Services (Konzepte) 

Dieses Thema bietet eine Übersicht über die wichtigsten Konzepte von Media Services.

##<a id="assets"></a>Ressourcen und Speicher

###<a name="assets"></a>Anlagen

Eine [Anlage](https://msdn.microsoft.com/library/azure/hh974277.aspx) enthält digitale Dateien (Video, Audio, Bilder, Miniaturansichten Sammlungen, Textspuren und Untertitel-Dateien) und Metadaten über diese Dateien. Nachdem zu digitalen Dateien hochgeladen wurden, konnte sie in Media Services codieren und streaming Workflows verwendet werden.

Anlage einen BLOB-Container in Azure Storage-Konto zugeordnet ist und die Dateien in der Anlage als Blobs im Container.

Wenn Media Inhalt speichern in einer Anlage, berücksichtigen die folgenden Aspekte:

- Anlage sollte nur eine einzelne, eindeutige Instanz Medieninhalte. Zum Beispiel einzelne bearbeiten TV-Folge, Film oder Werbung.
- Ein Vermögenswert darf keine mehrere Formatvarianten oder Bearbeitung der audiovisuellen Datei. Ein Beispiel für eine falsche Verwendung des Vermögenswertes würden versuchen, mehr als eine Folge, Werbung oder mehrere Kamerawinkel aus einer einzelnen Produktion in eine Anlage zu speichern. Speichern mehrerer Formatvarianten oder Bearbeitung der audiovisuellen Datei in Anlage führen Probleme Codierung Aufträge streaming und sichern die Lieferung der Anlage später im Workflow weiterleiten.  

###<a name="asset-file"></a>Bilddatei 
Eine [AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) stellt eine tatsächliche Video- oder audio-Datei in einem BLOB-Container. Eine Bilddatei ist immer Anlagen und eine Anlage kann eine oder mehrere Dateien enthalten. Media Services Encoder-Task fehlschlägt, wenn ein Dateiobjekt Anlage nicht digitale Datei in einem BLOB-Container zugeordnet ist.

Die **AssetFile** -Instanz und die aktuelle Mediendatei sind zwei verschiedene Objekte. Die AssetFile-Instanz enthält Metadaten über die Mediendatei Mediendatei tatsächlichen Medien.

Sie sollten nicht versuchen, den Inhalt der Blob-Container ändern, die mit Media Services ohne Media Service-APIs.

###<a name="asset-encryption-options"></a>Anlage-Verschlüsselungsoptionen

Je nach Inhalt zu laden, speichern und liefern bietet Media Services Verschlüsselungsoptionen, denen Sie auswählen können.

**Keine** Keine Verschlüsselung verwendet. Dies ist der Standardwert. Beachten Sie, dass bei Verwendung dieser Option der Inhalt nicht während der Übertragung oder im Speicher geschützt ist.

Möchten Sie ein MP4 liefern progressiv heruntergeladen Verwenden dieser option Ihre Inhalte hochladen.

**StorageEncrypted** – mit dieser Option löschen Inhalte lokal mit AES 256-Bit-Verschlüsselung verschlüsselt und auf Azure-Speicher Speicherort verschlüsselt ruhende hochladen. Mit Storage Verschlüsselung geschützt werden automatisch entschlüsselt ein verschlüsseltes Dateisystem vor Codierung platziert und optional vor dem Hochladen als eine neue Ausgabe Anlage erneut verschlüsselt. Primäre Nutzung bei speicherverschlüsselung wird Sie hochwertige input Mediendateien mit starker Verschlüsselung ruhender auf Festplatte sichern möchten. 

Um verschlüsselte Speicherressourcen zu liefern, müssen so Media Services weiß, wie Ihre Inhalte werden soll die Anlage Lieferung Richtlinie konfigurieren. Bevor die Anlage übertragen kann, streaming Server Speicher entschlüsselt und überträgt Inhalte mit angegebenen Lieferung Policy (AES, PlayReady oder ohne Verschlüsselung). 

**CommonEncryptionProtected** - verwenden Sie diese option, wenn Sie verschlüsseln (oder Hochladen bereits verschlüsselt) mit gemeinsamen Verschlüsselung oder PlayReady-DRM (z. B. Smooth Streaming mit PlayReady-DRM geschützt).

**EnvelopeEncryptionProtected** – verwenden Sie diese option, wenn schützen (oder Hochladen bereits geschützt) HTTP-Live-Streaming (HLS) mit Advanced Encryption Standard (AES) verschlüsselt. Beachten Sie, dass hochladen HLS bereits mit AES verschlüsselt es Transform Manager verschlüsselt worden sein muss.

###<a name="access-policy"></a>Richtlinien 

Ein [AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) definiert Berechtigungen (wie lesen, schreiben und Liste) und Dauer des Zugriffs auf eine Ressource. Sie würden normalerweise AccessPolicy übergeben eines Objekts an einem Locator, die auf die Dateien in einer Anlage verwendet werden würde.


###<a name="blob-container"></a>BLOB-container

Ein BLOB-Container bietet eine Gruppierung einer Gruppe von Blobs. BLOB-Container werden als Grenzpunkt für Zugriffskontrolle und Shared Access Signatur (SAS)-Locators auf Media Services verwendet. Ein Azure Storage-Konto kann eine unbegrenzte Anzahl von Blob-Container enthalten. Ein Container kann eine unbegrenzte Anzahl von Blobs speichern.

>[AZURE.NOTE]Sie sollten nicht versuchen, den Inhalt der Blob-Container ändern, die mit Media Services ohne Media Service-APIs.

###<a id="locators"></a>Locators

[Locator](https://msdn.microsoft.com/library/azure/hh974308.aspx)s bieten einen Einstiegspunkt in einer Anlage enthaltenen Dateien zugreifen. Eine Zugriffsrichtlinie dient zum Definieren der Berechtigungen und Dauer ein Clients auf eine bestimmte Ressource zugreifen. Locator können n-Beziehung eine Zugriffsrichtlinie, verschiedene Locators unterschiedliche Start- und Verbindungstypen für verschiedene Clients bereitstellen können alle mit denselben Berechtigungen und Dauer; jedoch aufgrund einer gemeinsamen Zugriff richtlinieneinschränkung Azure-Speicherdienste legen Sie mehr als fünf eindeutige Locators Assets gleichzeitig zugeordnet nicht möglich. 

Media Services unterstützt zwei Arten von Locators: OnDemandOrigin Locator zum Streamen von Medien (z. B. MPEG DASH, HLS oder Smooth Streaming) oder progressives Herunterladen von Medien und SAS-URL Locators zum Uploaden oder Downloaden von Media-Dateien Nähe Azure-Speicher verwendet. 

Beachten Sie, dass beim Erstellen eines Locators OrDemandOrigin nicht die Berechtigung (AccessPermissions.List) verwendet werden soll. 

###<a name="storage-account"></a>Konto

Zugriff auf Azure Storage erfolgt über ein Speicherkonto. Media Service-Konto kann ein oder mehrere Storage-Konten zuordnen. Ein Konto kann eine unbegrenzte Anzahl von Containern enthalten, solange die Gesamtgröße unter 500 TB pro Konto.  Media Services bietet Ebene SDK-Tools für mehrere Storage-Konten verwalten und Metriken oder zufällige Verteilung die Verteilung Ihrer Anlagen während des Uploads auf diese Konten auf Lastenausgleich. Weitere Informationen finden Sie unter Arbeiten mit [Azure-Speicher](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

##<a name="jobs-and-tasks"></a>Projekte und Aufgaben

Ein [Auftrag](https://msdn.microsoft.com/library/azure/hh974289.aspx) wird normalerweise verwendet, den Prozess (z. B. index oder codieren) ein Audio-Video-Präsentation. Mehrere Videos bearbeiten, erstellen Sie einen Auftrag für jedes Video codiert werden.

Ein Projekt enthält Metadaten über die Verarbeitung ausgeführt werden. Jedes Projekt enthält mindestens eine [Aufgabe](https://msdn.microsoft.com/library/azure/hh974286.aspx)s, die eine atomare Aufbereitungstask Sondervermögens input Anlagen, einen Medienprozessor und die dazugehörigen Einstellungen Ausgabe angeben. Aufgaben innerhalb eines Auftrags können miteinander verkettet, Ausgabe Anlage eine Aufgabe erhält als Eingabe Anlage zur nächsten Aufgabe. Auf diese Weise kann ein Auftrag für eine Multimediapräsentation Verarbeitung enthalten.

##<a id="encoding"></a>Codierung

Azure Media Services bietet mehrere Optionen für die Codierung von Medien in der Cloud.

Mit Media Services ausgehend, wird es wichtig, den Unterschied zwischen Codecs und Formate.
Codecs sind die Software, die Kompression/Dekompression Algorithmen implementiert Dateiformate sind Container, die das komprimierte Video enthalten.

Media Services bietet dynamische Verpackung der adaptive Bitrate MP4 oder Smooth Streaming codierte Inhalte im streaming-Formate von Media Services (MPEG DASH HLS Smooth Streaming, HDS) kann ohne diese Streaming-Formate erneut verpacken.

Um [dynamische Verpackung](media-services-dynamic-packaging-overview.md)nutzen zu können, müssen Sie Folgendes tun:

- Codieren der Datei Aufsteckkarte (Quelle) in eine Reihe von adaptiven Bitrate MP4 oder adaptive Bitrate Smooth Streaming-Dateien (Codierung Schritte werden später in diesem Lernprogramm gezeigt).
- Erhalten Sie mindestens bei Bedarf Streaming-Einheit für Streaming-Endpunkt aus dem Lieferung des Inhalts werden soll. Weitere Informationen finden Sie unter [On Demand Streaming reservierte Maßeinheiten](media-services-portal-manage-streaming-endpoints.md).

Media Services unterstützt die folgenden auf Anforderung-Encoder, die in diesem Artikel beschrieben werden:

- [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder Premium-Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

Informationen zu unterstützten Encoder anzeigen Sie [Encodern](media-services-encode-asset.md)


##<a name="live-streaming"></a>Live-Streaming

In Azure Media Services stellt ein Kanal eine Pipeline für die Verarbeitung von live-streaming-Inhalten. Ein Kanal empfängt live Eingabestreams auf zwei Arten:

- Ein lokalen live-Encoder sendet Multi-Bitrate RTMP oder Smooth Streaming (fragmentiert MP4) an den Kanal. Können die folgenden live Encoder, die Multi-Bitrate Smooth Streaming ausgeben: elementar, Envivio, Cisco. Die folgenden live Encoder Ausgabe RTMP: Adobe Flash Live, Telestream Wirecast und Tricaster Kodierungsprogramme. Die aufgenommenen Datenströme passieren Kanäle ohne weitere Verarbeitung. Bei Bedarf bietet Media Services Stream.

- Ein einzelnes Bitrate Stream (in einem der folgenden Formate: RTP (MPEG-TS)), RTMP oder Smooth Streaming (fragmentiert MP4)) geht an den Kanal live Codierung mit Media Services ausführen aktiviert ist. Der Kanal führt dann live Codierung des eingehenden Datenstrom auf einen Videostream Multi-Bitrate (adaptiv) einzelne Bitrate. Bei Bedarf bietet Media Services Stream.

###<a name="channel"></a>Kanal

In Media Services [Kanal](https://msdn.microsoft.com/library/azure/dn783458.aspx)für die Verarbeitung von live-streaming-Inhalten verantwortlich sind. Ein Kanal stellt anliegenden input (Aufnahme URL), dann live Transcoder anzugeben. Der Kanal live Transcoder live Eingabestreams empfängt und es kann über ein oder mehrere StreamingEndpoints streaming. Kanäle bieten auch einen Endpunkt Preview (Vorschau-URL), mit der Vorschau und der Stream vor der weiteren Verarbeitung und Übermittlung überprüfen.

Erstellen des Kanals kann die Erfassung URL und Vorschau-URL zu erhalten. Zu diesen URLs der Kanal keinen in den gestarteten Zustand. Wenn Sie den Kanal Daten von live Transcoder Einschieben starten möchten, muss der Kanal gestartet. Zeichenvorgang live Transcoder Einnahme Daten können Sie Ihre Stream anzeigen.

Jedes Media Services-Konto kann mehrere Kanäle, mehrere Programme und mehrere StreamingEndpoints enthalten. Je nach Bedarf Bandbreiten- und können einen oder mehrere Kanäle StreamingEndpoint Services gewidmet. Alle StreamingEndpoint kann von jedem Kanal ziehen.


###<a name="program"></a>Programm

Ein [Programm](https://msdn.microsoft.com/library/azure/dn783463.aspx) können Sie das Veröffentlichen und Speichern der Segmente in einen Livestream steuern. Kanäle verwalten Programme. Kanal und Programm Beziehung ist ähnlich dem Kanal einen ständigen Inhalt und eine Programm einige terminiertes Ereignis auf diesem Kanal Gültigkeitsbereich Medium.
Sie können die Anzahl der Stunden, den aufgezeichneten Inhalt für das Programm durch Festlegen der Eigenschaft **ArchiveWindowLength** beibehalten möchten. Dieser Wert kann auf maximal 25 Stunden ab 5 Minuten festgelegt.

ArchiveWindowLength bestimmt auch die maximale Größe des Kunden von der aktuellen Position live zurück suchen kann. Programme können über den angegebenen Zeitraum ausgeführt, aber Inhalt, hinter dem Fensterlänge liegt, kontinuierlich verworfen. Der Wert dieser Eigenschaft bestimmt auch, wie lange der Client Manifeste werden.

Jedes Programm ist eine Anlage zugeordnet. Um die Anwendung zu veröffentlichen müssen Sie ein Verzeichnis für die zugeordnete Anlage erstellen. Mit diesem Locator können Sie Streaming-URL erstellen, die Sie für die Clients bereitstellen können.

Ein Kanal unterstützt bis zu drei Programme gleichzeitig Erstellen mehrerer Archive dem eingehenden Stream. Dadurch können Sie veröffentlichen und Archivierung von Bestandteilen eines Ereignisses. Beispielsweise ist die Anforderung eines Programms von 6 Stunden archiviert aber nur 10 Minuten. Dazu müssen Sie zwei Programme gleichzeitig erstellen. Ein Programm zum Archivieren von 6 Stunden des Ereignisses festgelegt, aber die Anwendung veröffentlicht. Das Programm für 10 Minuten archivieren und dieses Programm veröffentlicht.


Weitere Informationen finden Sie unter:

- [Arbeiten mit Channels, die zum Ausführen mit Azure Media Services Livecodierungsmodus aktiviert sind](media-services-manage-live-encoder-enabled-channels.md)
- [Arbeiten mit Channels, die Multi-Bitrate Livestream aus lokalen Encoder empfangen](media-services-live-streaming-with-onprem-encoders.md)
- [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Schützen von Inhalten

###<a name="dynamic-encryption"></a>Dynamische Verschlüsselung

Azure Media Services können Sie Medien vom Zeitpunkt sichern verlässt Computers durch Speicherung, Verarbeitung und Übermittlung. Media Services können Sie Ihre Inhalte dynamisch mit Advanced Encryption Standard (AES) (mit 128-Bit-Schlüssel) und gemeinsame Verschlüsselung (CENC) mit PlayReady bzw. Widevine DRM verschlüsselt. Media Services stellt einen Dienst für AES-Schlüssel und PlayReady-Lizenzen auf autorisierte Clients.

Sie können derzeit die folgenden streaming-Formate verschlüsseln: HLS MPEG DASH und Smooth Streaming. HDS-streaming-Format können nicht verschlüsselt oder "Progressiv".

Für Media Services Vermögenswert verschlüsseln sollten, Sie einen Schlüssel (CommonEncryption oder EnvelopeEncryption) mit der Anlage und Autorisierungsrichtlinien für den Schlüssel auch konfigurieren.

Ggf. eine Speicherressource verschlüsselten stream müssen Sie die Anlage Lieferung Richtlinie konfigurieren, um angeben wie der Anlage zu.

Wenn ein Spieler ein Stream angefordert wird, verwendet Media Services angegebenen Schlüssel dynamisch Content mit einem Umschlag Verschlüsselung (AES) oder allgemeine Verschlüsselung (mit PlayReady Widevine) verschlüsselt. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel Schlüssel Paketdienstes. Entscheidung, ob der Benutzer berechtigt ist, den Schlüssel, wertet der Dienst die Autorisierungsrichtlinien für den Schlüssel angegeben.


###<a name="token-restriction"></a>Token Einschränkung

Inhalt wichtige Autorisierungsrichtlinie könnte gelten mindestens Autorisierung: öffnen, token Beschränkung oder IP-Einschränkung. Die token eingeschränkte Richtlinie ein Token ausgestellt durch eine Secure Token Service (STS) beizufügen. Media Services unterstützt Token im die Simple Web Token (SWT) Format und JSON Web Token (JWT). Media Services bietet keine sichere Token-Services. Erstellen eines benutzerdefinierten STS oder Microsoft Azure ACS Problem Tokens nutzen. Der STS müssen konfiguriert werden, erstellen Sie einen Token signiert mit angegebenen Schlüssel und Thema, die in der Konfiguration token Einschränkung angegeben. Der Schlüssel Zustelldienst Media Services zurück angeforderte Schlüssel (oder Lizenz) Client, wenn das Token gültig ist und die Ansprüche im token dem die konfigurierten Schlüssel (oder Lizenz).

Wenn Richtlinien konfigurieren das Token eingeschränkt werden, müssen Sie primäre Überprüfung Schlüssel, Aussteller und Zielgruppe Parameter angeben. Primäre Überprüfung Schlüssel enthält Schlüssel, dem Token signiert wurde, Aussteller des Sicherheitstokendiensts, die das Token ausstellt. Zielgruppe (Bereich bezeichnet) beschreibt die Absicht des Tokens oder die Ressource autorisiert das Token auf. Wichtige Zustelldienst Media Services überprüft, dass diese Werte im Token die Werte in der Vorlage übereinstimmen.

Weitere Informationen finden Sie in den folgenden Artikeln:

[Inhaltsübersicht schützen](media-services-content-protection-overview.md)
[Schützen mit AES 128](media-services-protect-with-aes128.md)
[mit DRM schützen](media-services-protect-with-drm.md)

##<a name="delivering"></a>Bereitstellung

###<a id="dynamic_packaging"></a>Dynamische Verpackung

Beim Arbeiten mit Media Services empfohlen wird Aufsteckkarte Dateien in eine adaptive Bitrate MP4 codiert festgelegt und den Satz in das gewünschte Format der [Dynamischen Verpackung](media-services-dynamic-packaging-overview.md)konvertieren.


###<a name="streaming-endpoint"></a>Streaming-Endpunkt

Eine StreamingEndpoint stellt einen Streaming-Dienst, der Inhalte bereitstellen, direkt an eine Clientanwendung Player oder auf ein Content Delivery Network (CDN) für die weitere Verteilung (Azure Media Services bietet jetzt Azure CDN-Integration). Ausgehenden Datenstrom aus StreamingEndpoint Dienst kann live-Streams oder ein Video bei Bedarf Anlage in Ihrem Media Services-Konto. Darüber hinaus können Sie die Kapazität des StreamingEndpoint Dienst behandeln Bandbreite Bedürfnisse anpassen Maßeinheiten (auch bekannt als streaming-Einheiten) steuern. Es wird empfohlen, eine oder mehrere Maßeinheiten für Applikationen in Produktion zugewiesen werden. Maßeinheiten bieten dedizierte Egress-Kapazität, die in Schritten von 200 Mbit/s erhältlich und zusätzliche Funktionen, die derzeit mit dynamischen Verpackung.

Es wird empfohlen, dynamische Verpackung und/oder dynamische Verschlüsselung. Zum Verwenden dieser Funktionen müssen Sie mindestens eine Streaming-Einheit für den Endpunkt verfügen, streamen möchten. Weitere Informationen finden Sie unter [Skalierung streaming-Einheiten](media-services-portal-manage-streaming-endpoints.md).

Standardmäßig können Sie bis zu 2 streaming Endpunkte in Ihrem Media Services-Konto. Eine höhere Grenze finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).

Sie werden nur bei der StreamingEndpoint im Ausführungszustand belastet.

###<a name="asset-delivery-policy"></a>Anlage Lieferung Richtlinie

Einer der Schritte im Workflow Inhaltsübermittlung Media Services ist [Richtlinien für Anlagen ](https://msdn.microsoft.com/library/azure/dn799055.aspx)konfigurieren, die Sie übertragen möchten. Anlage Lieferung Richtlinie teilt Mediendienste wie für die Anlage übermittelt werden sollen: in der streaming-Protokoll soll die Anlage dynamisch verpackt (z. B. MPEG DASH, HLS, Smooth Streaming, oder alle), oder ob Sie Ihre Anlage dynamisch verschlüsseln möchten und wie (Umschlag oder allgemeine Verschlüsselung).

Wenn Sie verschlüsselte Speicherressourcen die Anlage übertragen kann haben, streaming Server Speicher entschlüsselt und Inhalte der Richtlinie angegebenen Übermittlung mit streams. Beispielsweise liefern die Anlage mit Advanced Encryption Standard (AES) Schlüssel verschlüsselt Geben Sie die Richtlinie zu DynamicEnvelopeEncryption. Storage Verschlüsselung entfernen die Anlage als Klartext übertragen und geben Sie die Richtlinie zu NoDynamicEncryption.

###<a name="progressive-download"></a>Progressiver download

Progressiver Download können Sie Medien wiedergegeben werden, bevor die gesamte Datei heruntergeladen wurde. Sie können immer nur eine MP4-Datei.

Beachten Sie, dass Sie verschlüsselte Anlagen entschlüsseln müssen gegebenenfalls zu progressiv heruntergeladen.

Um Benutzer mit progressiven Download URLs angeben, müssen Sie zunächst einen Locator OnDemandOrigin erstellen. Locator können, Sie den Pfad für die Ressource. Sie müssen den Namen des MP4-Datei anfügen. Zum Beispiel:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

###<a name="streaming-urls"></a>Streaming-URLs

Der Streaminginhalt Clients. Um Benutzer streaming URLs, müssen Sie zunächst einen Locator OnDemandOrigin erstellen. Locator können, Sie den Pfad der Anlage mit dem Inhalt zu streamen. Um diese Inhalte übertragen müssen Sie jedoch weitere diesen Pfad ändern. Zum Erstellen einer vollständigen URLs streaming Manifestdatei muss Verketten der Locator Pfadwert und das Anwendungsmanifest (filename.ism) Dateinamen. Dann fügen Sie hinzu/Manifest und ein geeignetes Format (sofern erforderlich) den Locator-Pfad.

Sie können auch die Inhalte über eine SSL-Verbindung übertragen. Dazu sicherzustellen Sie, dass Ihre streaming URLs mit HTTPS beginnen.

Beachten Sie, dass Sie nur über SSL streamen können, wenn Streaming-Endpunkt aus dem Ihre Inhalte liefern nach dem 10. September 2014 erstellt wurde. Wenn streaming URLs auf streaming Endpunkte 10. September erstellt, enthält die URL "streaming.mediaservices.windows.net" (neues Format). Streaming URLs mit "origin.mediaservices.windows.net" (das alte Format) unterstützen SSL. Wenn der URL im alten Format ist über SSL übertragen werden soll, erstellen Sie eine neue streaming. Erstellt basierend auf den neuen Streaming-Endpunkt URLs zum Verwenden des Inhalts über SSL.

Die folgende Liste beschreibt verschiedene streaming-Formate und Beispiele:

- Smooth Streaming

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest


- MPEG-STRICH

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=MPD-Time-CSF)



- Apple HTTP Live Streaming (HLS) V4

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL)



- Apple HTTP Live Streaming (HLS) V3

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL-V3)

- HDS (für Adobe PrimeTime/Access Lizenznehmer nur)

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=f4m-f4f)


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
