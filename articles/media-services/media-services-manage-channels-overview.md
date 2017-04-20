<properties 
    pageTitle="Übersicht über Live gekocht mit Azure Media Services | Microsoft Azure" 
    description="Dieses Thema gibt einen Überblick über die live gekocht Azure Media Services verwenden." 
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
    ms.date="10/17/2016"
    ms.author="juliako"/>

#<a name="overview-of-live-steaming-using-azure-media-services"></a>Übersicht über Live gekocht mit Azure Media Services

##<a name="overview"></a>Übersicht

Bei live-Streaming-Ereignisse mit Azure Media Services werden häufig die folgenden Komponenten beteiligt:

- Eine Kamera, die ein Ereignis verwendet wird.
- Ein live video-Encoder, der Signale von der Kamera zu konvertiert, die ein live-streaming-Dienst gesendet werden.

    Optional Encoder mehrere Echtzeit synchronisiert. Für bestimmte kritische live Ereignisse bei Bedarf sehr hohe Verfügbarkeit und Qualität sollten aktive redundante Encoder mit Synchronisierung nahtloses Failover ohne Datenverlust zu beschäftigen.
- Ein live streaming-Dienst, der Ihnen Folgendes ermöglicht:
    
    - Aufnehmen von live-Inhalten über verschiedene live streaming-Protokolle (z. B. RTMP oder Smooth Streaming)
    - Codieren Sie der Stream (optional) adaptive Bitrate Stream
    - Vorschau von live-Streams
    - Speichern des aufgenommenen Inhalts um übertragen (Demand) höher
    - Bereitstellung von Content über gängige Streaming-Protokolle (z. B. MPEG DASH optimierten HLS, HDS) direkt an Ihre Kunden oder zu einem Content Delivery Network (CDN) für die weitere Verteilung.


**Microsoft Azure Media Services** (AMS) ermöglicht die Aufnahme codieren, Vorschau, speichern und Ihre live-Streaming-Inhalte.

Wenn Ihre Kunden liefern ist Ihr Ziel eine hohe Bildqualität auf verschiedene Geräte unter andere Netzwerkprobleme. Dazu verwenden Sie live Encoder Codieren der Stream einen Videostream Multi-Bitrate (adaptive Bitrate).  Um streaming auf verschiedenen Geräten, verwenden Sie Media Services [dynamische Verpackung](media-services-dynamic-packaging-overview.md) dynamisch der Stream verschiedene Protokolle erneut verpacken. Media Services unterstützt die Bereitstellung der folgenden adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern).

In Azure Media Services, **Kanäle**, **Programme**und **StreamingEndpoints** behandeln Sie alle live-Streaming-Funktionen einschließlich Erfassung, Formatierung, DVR, Sicherheit, Skalierbarkeit und Redundanz.

Ein **Kanal** stellt eine Pipeline für die Verarbeitung von live-streaming-Inhalten. Ein Kanal erhalten eine Eingabe Streams auf folgende Weise:

- Für lokale live Encoder sendet Multi-Bitrate **RTMP** oder **Smooth Streaming** (fragmentierte MP4) an den Kanal für **Pass-Through-** Bereitstellung konfiguriert ist. **Pass-Through** wird beim **Kanal**aufgenommenen Streams passieren, ohne weitere Verarbeitung. Können die folgenden live Encoder, die Multi-Bitrate Smooth Streaming ausgeben: elementar, Envivio, Cisco.  Die folgenden live Encoder Ausgabe RTMP: Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast und Tricaster Kodierungsprogramme.  Live-Encoder kann auch einzelne Bitrate Stream an einen Kanal senden, für live-Codierung nicht aktiviert ist, aber nicht empfohlen. Bei Bedarf bietet Media Services Stream.

    >[AZURE.NOTE] Pass-Through-Methode ist die wirtschaftlichste Methode zum streaming Leben Wenn Sie Ereignisse über einen längeren Zeitraum, und bereits in lokalen Encoder investiert haben. [Preisdetails](/pricing/details/media-services/) anzeigen
    
    
- Encoder lokale live sendet Single-Bitrate an den Kanal aktiviert ist, führen Sie live-Codierung mit Media Services in einem der folgenden Formate: RTMP oder Smooth Streaming (fragmentierte MP4). RTP (MPEG-TS) wird ebenfalls unterstützt, vorausgesetzt, Sie besitzen eine dedizierte Verbindung Azure-Rechenzentrum. Die folgenden live Encoder mit RTMP bekanntermaßen funktioniert mit diesem Typ: Telestream Wirecast, FMLE. Der Kanal führt dann live Codierung des eingehenden Datenstrom auf einen Videostream Multi-Bitrate (adaptiv) einzelne Bitrate. Bei Bedarf bietet Media Services Stream.


Ab Release Media Services 2.10 Wenn Sie einen Kanal erstellen, können Sie angeben, wie der Kanal Eingabestream erhalten soll und ob der Kanal live Codierung der Stream ausgeführt werden soll. Sie haben zwei Optionen:

- **Keine** (Pass-Through) – Geben Sie diesen Wert, wenn Sie Multi-Bitrate (Pass-Through-Stream) Ausgabestream wird einen lokale live-Encoder verwenden möchten. In diesem Fall Durchlaufen der eingehende Stream zur Ausgabe ohne Kodierung. Dies ist das Verhalten eines Kanals vor 2.10 Version.  

- **Standard** – wählen Sie Wert, wenn Sie Media Services verwenden, um die einzelnen Bitrate Livestream Multi-Bitrate Stream codieren möchten. Diese Methode ist für schnelle Skalierung für seltene Ereignisse. Beachten Sie, dass eine Rechnungsadresse für live-Codierung wirkt und Sie beachten, dass im Zustand "Running" live Codierung Kanal verlassen Gebühren anfallen.  Es wird empfohlen, sofort die laufenden Kanäle beendet Abschluss das live-Streaming-Ereignis zusätzliche stündliche Gebühren zu. 

##<a name="comparison-of-channel-types"></a>Channel im Vergleich

In der folgenden Tabelle Leitfaden ein zum Vergleich der zwei Channel unterstützt Media Services

Funktion|Pass-Through-Kanal|Standard-Kanal
---|---|---
Eingabe der einzelnen Bitrate codiert in mehreren Bitraten in der cloud|Nein|Ja
Maximale Auflösung, Anzahl der Schichten|1080p, 8 Ebenen 60 fps|720p, 6 Ebenen 30 fps
Input-Protokolle|RTMP Smooth Streaming|RTMP Smooth Streaming und RTP
Preis|Die [Preise der Seite](/pricing/details/media-services/) und klicken Sie auf Registerkarte "Live Video"|Siehe [Seite Preise](/pricing/details/media-services/) 
Maximale Zeit|24 x 7|8 Stunden
Unterstützung für Slate einfügen|Nein|Ja
Unterstützung für Ad signalisieren|Nein|Ja
Pass-Through-CEA 608/708 Untertitel|Ja|Ja
Kurze Ständen Beitrag feed Recovery|Ja|Nein (Channel beginnt verriss nach 6 Sekunden Eingabe von Daten)
Ungleichmäßige unterstützt Eingabe GOPs|Ja|Nein – muss Eingabe 2 Sek. GOPs behoben werden
Unterstützung für Variable Frame Rate Eingabe|Ja|Eingabe muss nicht-Framerate behoben werden.<br/>Geringfügige Variationen werden z. B. bei high-Motion Szenen toleriert. Aber Encoder kann nicht zu 10 Bilder pro Sekunde.
Abschalten des Channels Wenn feed geht verloren|Nein|Nach 12 Stunden, wenn kein Programm ausführen 

##<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Arbeiten mit Channels, die Multi-Bitrate Livestream aus lokalen Encoder (Pass) empfangen

Das folgende Diagramm zeigt die Hauptbestandteile der AMS-Plattform, die beteiligt sind, **Pass-Through-** Workflow.

![Live-workflow](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Weitere Informationen finden Sie unter [Arbeiten mit Channels, empfangen Multi-Bitrate Livestream aus lokalen Encoder](media-services-live-streaming-with-onprem-encoders.md).

##<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Arbeiten mit Channels die aktivierten live Codierung mit Azure Media Services ausführen

Das folgende Diagramm zeigt die Hauptbestandteile der AMS-Plattform, die beteiligt sind, Live-Streaming-Workflows, ein Kanal live Codierung mit Media Services ausführen aktiviert ist.

![Live-workflow](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Weitere Informationen finden Sie unter [Arbeiten mit Channels, die zum Ausführen mit Azure Media Services Livecodierungsmodus aktiviert sind](media-services-manage-live-encoder-enabled-channels.md).

##<a name="description-of-a-channel-and-its-related-components"></a>Beschreibung der Kanal und die zugehörigen Komponenten

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


##<a name="billing-implications"></a>Abrechnung folgen

Ein Kanal beginnt Abrechnung wie Zustandsübergänge in "Running" API ist.  

Die folgende Tabelle zeigt wie Channel Mitgliedstaaten Rechnung Zuständen im API und Azure-Portal zugeordnet. Beachten Sie, dass die Mitgliedstaaten geringfügig zwischen API und Portal-UX Ein Kanal ist im Status "Laufend" API oder in den Status "Bereit" oder "Streaming" im Azure-Portal Abrechnung aktiv sein.

Stoppen den Kanal Abrechnung Sie weitere Ihnen Channel API oder in Azure-Portal zu.
Sie sind verantwortlich für die Kanäle beendet, wenn dies mit dem Kanal. Fehler beim Beenden des Kanals führt weiterhin Rechnung.

>[AZURE.NOTE]Beim Arbeiten mit Standardkanäle wird AMS Silhouette keinen Kanal automatisch, die noch im Status "Ausführen" 12 Stunden nach Eingang Feed verloren und es sind keine Programme ausgeführt. Allerdings werden Ihnen noch Zeit berechnet, der Kanal im Zustand "Aktiv" befindet.

###<a id="states"></a>Channel-Status und Zuordnung der Abrechnung Modus 

Der aktuelle Zustand eines Kanals. Mögliche Werte sind:

- **Beendet**. Dies sei der anfängliche Zustand des Kanals nach der Erstellung (Autostart im Portal aktiviert wurde). Keine Abrechung in diesem Zustand. In diesem Zustand können die Kanaleigenschaften aktualisiert streaming ist jedoch nicht zulässig.
- **Starten**. Der Kanal wird gestartet. Keine Abrechung in diesem Zustand. Keine Updates oder streaming während dieses Zustands. Wenn ein Fehler auftritt, gibt der Kanal Zustand beendet.
- **Ausgeführt**. Der Kanal ist live-Streams verarbeiten. Es ist jetzt Verwendung Abrechnung. Beenden Sie den Kanal, um zu verhindern, dass weitere Abrechnung. 
- **Beenden**. Der Kanal wird beendet. Keine Abrechung in diesem vorübergehenden Zustand. Keine Updates oder streaming während dieses Zustands.
- **Löschen**. Der Kanal wird gelöscht. Keine Abrechung in diesem vorübergehenden Zustand. Keine Updates oder streaming während dieses Zustands.

Die folgende Tabelle zeigt wie Channel Mitgliedstaaten Rechnung Modus zugeordnet. 
 
Channel-Zustand|Portal UI anzeigen|Ist Abrechnung?
---|---|---
Starten|Starten|Keine (Übergangszustand)
Ausführen|Bereit (keine Programme)<br/>oder<br/>Streaming (mindestens einem aktiven Programm)|JA
Beenden|Beenden|Keine (Übergangszustand)
Beendet|Beendet|Nein


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Verwandte Themen

[Azure Media Services fragmentierte MP4 Live Aufnahme Spezifikation](media-services-fmp4-live-ingest-overview.md)

[Arbeiten mit Channels, die zum Ausführen mit Azure Media Services Livecodierungsmodus aktiviert sind](media-services-manage-live-encoder-enabled-channels.md)

[Arbeiten mit Channels, die Multi-Bitrate Livestream aus lokalen Encoder empfangen](media-services-live-streaming-with-onprem-encoders.md)

[Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).  

[Media-Dienste (Konzepte)](media-services-concepts.md)
