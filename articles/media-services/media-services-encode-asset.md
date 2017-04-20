<properties 
    pageTitle="Vergleich von Azure auf Anforderung Media Encoder | Microsoft Azure" 
    description="Dieses Thema enthält eine Übersicht und einen Vergleich von Azure auf Anforderung Media Encoder." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Vergleich von Azure auf Anforderung Media Encoder

##<a name="encoding-overview"></a>Codierung (Übersicht)

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

Dieser Artikel bietet eine Übersicht auf Anforderung Media Encoder und enthält Links zu Artikeln, die ausführlichere Informationen. Das Thema enthält auch Vergleich der Encoder.

Beachten Sie, dass standardmäßig jedes Media Services-Konto eine aktive Codierung Aufgabe haben kann. Sie können encoding Einheiten reservieren, mit denen Sie mehrere Codierungsaufgaben ausgeführt, eine für jede Codierung reservierte Einheit erworben haben. Informationen finden Sie unter [Skalierung Codierung Einheiten](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Media Encoder Standard

###<a name="how-to-use"></a>Verwendung

[Das Media Encoder Standard codieren](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Formate

[Formate und codecs](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Voreinstellungen

Media Encoder Standard sieht eine der Encoder-Vorgaben beschrieben [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Eingabe- und Metadaten

Die Eingabe Encoder Metadaten beschrieben [hier](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Encoder Ausgabe Metadaten beschrieben [hier](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Generieren von Miniaturansichten

Informationen finden Sie unter [wie Miniaturen Media Encoder Standard generiert](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Zuschneiden von Videos (Clipping)

Informationen finden Sie unter [Media Encoder Standard mit Videos Zuschneiden](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###<a name="create-overlays"></a>Überlagern

Informationen finden Sie unter [Erstellen von Overlays Media Encoder Standard verwenden](media-services-custom-mes-presets-with-dotnet.md#overlay).

###<a name="see-also"></a>Siehe auch

[Media Services-blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Media Encoder Premium-Workflow

###<a name="overview"></a>Übersicht

[Einführung in Premium-Codierung in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Verwendung

Media Encoder Premium-Workflow wird über komplexe Workflows konfiguriert. Workflow-Dateien erstellt werden können und mit dem [Workflow-Designer](media-services-workflow-designer.md) -Tool aktualisiert.

[Wie Sie Premium Codierung in Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Bekannte Probleme

Enthält Ihre Eingabe Video keine enthält Untertitel, die Ausgabe Anlage weiterhin eine leere TTML-Datei. 


##<a id="compare_encoders"></a>Encoder vergleichen

###<a id="billing"></a>Abrechnung Meter von jeder Encoder verwendet

Medienname Prozessor|Geltenden Preise|Notizen
---|---|---
**Media Encoder Standard** |ENCODER|Codierung Vorgänge berechnet die Größe der Ausgabe Anlage in GB angegebenen [hier][1], Spalte ENCODER.
**Media Encoder Premium-Workflow** |PREMIUM-ENCODER|Codierung Vorgänge berechnet die Größe der Ausgabe Anlage in GB angegebenen [hier][1], Spalte PREMIUM ENCODER.


Dieser Abschnitt vergleicht die codieren Funktionen des **Media Encoder Standard** und **Media Encoder Premium-Workflow**.


###<a name="input-containerfile-formats"></a>Eingabeformate Container-Datei

Eingabeformate Container-Datei|Media Encoder Standard|Media Encoder Premium-Workflow
---|---|---
Adobe® Flash® F4V           |Ja|Ja
MXF/SMPTE 377M              |Ja|Ja
GXF                         |Ja|Ja
MPEG-2-Streams    |Ja|Ja
MPEG-2 Programm-Streams      |Ja|Ja
MPEG-4/MP4                  |Ja|Ja
Windows Media/ASF           |Ja|Ja
AVI (dekomprimiert 8 Bit/10 Bit)|Ja|Ja
3GPP-3GPP2                  |Ja|Nein
Reibungslose Streaming-Dateiformat (PIFF 1.3)|Ja|Nein
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Ja|Nein
Matroska/WebM               |Ja|Nein
QuickTime (MOV) |Ja|Nein

###<a name="input-video-codecs"></a>Input Videocodecs

Input Videocodecs|Media Encoder Standard|Media Encoder Premium-Workflow
---|---|---
AVC 8-Bit/10-Bit, 4:2:2, einschließlich AVCIntra   |8-bit-4:2:0 und 4:2:2|Ja
Avid DNxHD (in MXF)                                 |Ja|Ja
DVCPro/DVCProHD (in MXF)                            |Ja|Ja
JPEG2000                                            |Ja|Ja
MPEG-2 (422 Profil und hohe, einschließlich Varianten wie XDCAM XDCAM HD, XDCAM IMX, CableLabs® und D10)|Bis zu 422 Profil|Ja
MPEG-1                                              |Ja|Ja
Windows Media Video/VC-1                            |Ja|Ja
Canopus HQ/HQX                                      |Nein|Nein
MPEG-4 Teil 2                                       |Ja|Nein
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ja|Nein
Apple ProRes 422    |Ja|Nein
Apple ProRes 422 LT |Ja|Nein
Apple ProRes 422 HQ |Ja|Nein
Apple ProRes Proxy|Ja|Nein
Apple ProRes 4444 |Ja|Nein
Apple ProRes 4444 XQ |Ja|Nein

###<a name="input-audio-codecs"></a>Geben Sie Audio-Codecs

Geben Sie Audio-Codecs|Media Encoder Standard|Media Encoder Premium-Workflow
---|---|---
AES (SMPTE 331 M und M 302, AES3 2003)        |Nein|Ja
Dolby® E                                    |Nein|Ja
Dolby® Digital (AC3)                        |Nein|Ja
Dolby® Digital Plus (E-AC3)                 |Nein|Ja
AAC (AAC-LC und AAC-HE-AAC-HEv2; bis 5.1)|Ja|Ja
MPEG Layer 2|Ja|Ja
MP3 (MPEG-1 Audio Layer 3)|Ja|Ja
Windows Media Audio|Ja|Ja
WAV-PCM|Ja|Ja
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ja|Nein
[Werk](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Ja|Nein
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ja|Nein


###<a name="output-containerfile-formats"></a>Container-Datei Ausgabeformate

Container-Datei Ausgabeformate|Media Encoder Standard|Media Encoder Premium-Workflow
---|---|---
Adobe® Flash® F4V|Nein|Ja
MXF (OP1a und XDCAM AS02)|Nein|Ja
DPP (einschließlich hs11)|Nein|Ja
GXF|Nein|Ja
MPEG-4/MP4|Ja|Ja
MPEG-TS|Ja|Ja
Windows Media/ASF|Nein|Ja
AVI (dekomprimiert 8 Bit/10 Bit)|Nein|Ja
Reibungslose Streaming-Dateiformat (PIFF 1.3)|Nein|Ja

###<a name="output-video-codecs"></a>Ausgabe von Video-Codecs

Ausgabe von Video-Codecs|Media Encoder Standard|Media Encoder Premium-Workflow
---|---|---
AVC (h. 264-, 8-Bit; bis hoher Profil Ebene 5.2; 4 K Ultra HD; AVC-Intra)|Nur 8-bit-4:2:0|Ja
Avid DNxHD (in MXF)|Nein|Ja
MPEG-2 (422 Profil und hohe, einschließlich Varianten wie XDCAM XDCAM HD, XDCAM IMX, CableLabs® und D10)|Nein|Ja
MPEG-1|Nein|Ja
Windows Media Video/VC-1|Nein|Ja
JPEG-Miniaturansicht erstellen|Nein|Ja

###<a name="output-audio-codecs"></a>Ausgang Audio-Codecs

Ausgang Audio-Codecs|Media Encoder Standard|Media Encoder Premium-Workflow
---|---|---
AES (SMPTE 331 M und M 302, AES3 2003)|Nein|Ja
Dolby® Digital (AC3)|Nein|Ja
Dolby® Digital Plus (E-AC3) bis 7.1|Nein|Ja
AAC (AAC-LC und AAC-HE-AAC-HEv2; bis 5.1)|Ja|Ja
MPEG Layer 2|Nein|Ja
MP3 (MPEG-1 Audio Layer 3)|Nein|Ja
Windows Media Audio|Nein|Ja


##<a name="error-codes"></a>Fehlercodes  

Die folgende Tabelle listet die Fehlercodes zurückgegeben werden können, falls ein Fehler während der Codierung Taskausführung aufgetreten.  Verwenden Sie zum Abrufen von Fehlerinformationen in Ihren [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) -Klasse. Um Fehlerinformationen im übrigen Code abzurufen, verwenden Sie die [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST-API.

ErrorDetail.Code|Mögliche Ursachen für Fehler
-----|-----------------------
Unbekannt| Fehler beim Ausführen des Tasks
ErrorDownloadingInputAssetMalformedContent|Kategorie von Fehlern, die Fehler herunterladen input Anlage wie ungültige Dateinamen Null Länge-Dateien falsch behandelt und formatiert.
ErrorDownloadingInputAssetServiceFailure|Kategorie der Fehler, die Probleme auf Service - Beispiel Netzwerk- oder Speicher Downloadfehler.
ErrorParsingConfiguration|Kategorie von Fehlern, Aufgabe <see cref="MediaTask.PrivateData"/> (Konfiguration) ist ungültig, z. B. die Konfiguration nicht gültiges System voreingestellte oder enthält ungültiges XML.
ErrorExecutingTaskMalformedContent|Kategorie von Fehlern während der Ausführung der Aufgabe, Probleme in der Eingabe Mediendateien fehlschlägt.
ErrorExecutingTaskUnsupportedFormat|Kategorie von Fehlern, Media-Prozessor kann die Dateien nicht verarbeiten - Medienformat nicht, unterstützt, oder die Konfiguration stimmt nicht überein. Zum Beispiel versuchen, eine Anlage nur-Audio-Ausgabe erzeugen, die nur Videos
ErrorProcessingTask|Kategorie andere Fehler Media Prozessor findet während der Verarbeitung der Aufgabe, die nicht im Zusammenhang mit Inhalt.
ErrorUploadingOutputAsset|Fehler beim Hochladen der Anlage Ausgabe Kategorie
ErrorCancelingTask|Kategorie von Fehlern auf Fehler beim Abbrechen der Aufgabe
TransientError|Kategorie von Fehlern vorübergehende Probleme (z. b. temporäre Netzwerkprobleme mit Azure-Speicher)


Hilfe von **Media Services** Team Öffnen einer [support-Ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Verwandte Artikel

- [Aufgaben Sie erweiterte Codierung Media Encoder Standard Vorgaben anpassen](media-services-custom-mes-presets-with-dotnet.md)
- [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
