<properties 
    pageTitle="Media Encoder Premium-Workflow Formate und Codecs | Microsoft Azure" 
    description="Dieses Thema bietet eine Übersicht über Media Encoder Premium Workflow Formate Formate und codecs" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium-Workflow Formate und codecs


>[AZURE.NOTE]Premium Encoder Fragen per e-Mail an Mepd unter Microsoft.com.
>
>In diesem Thema erläuterten Media Encoder Premium-Workflow Media Prozessor ist nicht verfügbar in China. 

Dieses Dokument enthält eine Liste von Eingabe und Ausgabe-Dateiformate und Codecs, die durch die public Preview-Version von **Media Encoder Premium-Workflow** -Encoder unterstützt werden.

[Media Encoder Premium Prinect Eingabe Formate und Codecs](#input_formats)

[Media Encoder Premium Befragungs-Ausgabeformate und Codecs](#output_formats)

**Media Encoder Premium-Workflow** unterstützt Untertitel in [diesem](#closed_captioning) Abschnitt beschrieben. 


##<a id="input_formats"></a>Formate und Codecs Media Encoder Premium-Workflow eingeben

Im folgenden Abschnitt listet die Codecs und Formate, die dieser Medienprozessor als Eingabe unterstützt.

###<a name="input-containerfile-formats"></a>Eingabeformate Container-Datei

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- MPEG-2-Streams
- MPEG-2 Programm-Streams
- MPEG-4/MP4
- Windows Media/ASF
- AVI (dekomprimiert 8 Bit/10 Bit)

###<a name="input-video-codecs"></a>Input Videocodecs

- AVC 8-Bit/10-Bit, 4:2:2, einschließlich AVCIntra
- Avid DNxHD (in MXF)
- DVCPro/DVCProHD (in MXF)
- JPEG2000
- MPEG-2 (422 Profil und hohe, einschließlich Varianten wie XDCAM XDCAM HD, XDCAM IMX, CableLabs® und D10)
- MPEG-1
- Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Geben Sie Audio-Codecs

- AES (SMPTE 331 M und M 302, AES3 2003)
- Dolby® E
- Dolby® Digital (AC3)
- AAC (AAC-LC und AAC-HE-AAC-HEv2; bis 5.1)
- MPEG Layer 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio
- WAV-PCM
 
##<a id="output_format"></a>Media Encoder Premium Workflow Ausgabeformate und Codecs

Abschnitt listet die Codecs und Formate, die als Ausgabe dieser Medienprozessor unterstützt werden.

###<a name="output-containerfile-formats"></a>Container-Datei Ausgabeformate

- Adobe® Flash® F4V
- MXF (OP1a und XDCAM AS02)
- DPP (einschließlich hs11)
- GXF
- MPEG-4/MP4
- Windows Media/ASF
- AVI (dekomprimiert 8 Bit/10 Bit)
- Reibungslose Streaming-Dateiformat (PIFF 1.3)
- MPEG-TS 


###<a name="output-video-codecs"></a>Ausgabe von Video-Codecs

- AVC (h. 264-, 8-Bit; bis hoher Profil Ebene 5.2; 4 K Ultra HD; AVC-Intra)
- Avid DNxHD (in MXF)
- DVCPro/DVCProHD (in MXF)
- MPEG-2 (422 Profil und hohe, einschließlich Varianten wie XDCAM XDCAM HD, XDCAM IMX, CableLabs® und D10)
- MPEG-1
- Windows Media Video/VC-1
- JPEG-Miniaturansicht erstellen

###<a name="output-audio-codecs"></a>Ausgang Audio-Codecs

- AES (SMPTE 331 M und M 302, AES3 2003)
- Dolby® Digital (AC3)
- Dolby® Digital Plus (E-AC3) bis 7.1
- AAC (AAC-LC und AAC-HE-AAC-HEv2; bis 5.1)
- MPEG Layer 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Unterstützung für Untertitel

Auf Aufnahme **Media Encoder Premium-Workflow** unterstützt:

1. SCC-Dateien
1. SMPTE-TT-Dateien
1. CEA-608/CEA-708 – als Benutzerdaten (SEI Nachrichten von h. 264 elementare Streams ATSC-53, SCTE20) oder als zusätzliche Daten in MXF-GXF Dateien
1. STL-Untertitel

Bei der Ausgabe sind die folgenden Optionen verfügbar:

1. CEA-608 CEA-708 Übersetzung
1. CEA-608/CEA-708 durchlaufen (SEI Nachrichten elementaren h. 264-Streams eingebettet oder als Zusatzdaten in MXF-Dateien durchgeführt)
1. SCC
1. SMPTE Timed Text (von Quelle CEA-608 pro SMPTE-RP2052, einschließlich DFXP Erstellung)
1. SRT Untertiteldatei
1. DVB-Untertitel-streams

Hinweis: nicht alle der oben genannten Ausgabeformate sind für die Übermittlung per streaming in Azure Media Services unterstützt.

##<a name="known-issues"></a>Bekannte Probleme

Enthält Ihre Eingabe Video keine enthält Untertitel, die Ausgabe Anlage weiterhin eine leere TTML-Datei. 


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
