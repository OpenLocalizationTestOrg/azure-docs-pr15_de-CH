<properties 
    pageTitle="Media Encoder Standardformate und codecs" 
    description="Dieses Thema bietet eine Übersicht über Media Encoder Standardformate und Codecs." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder Standardformate und Codecs


Dieses Dokument enthält eine Liste der am häufigsten verwendeten Import / Export-Dateiformate, die mit Media Encoder Standard verwendet.


##<a name="input-containerfile-formats"></a>Eingabeformate Container-Datei

Dateiformate (Datei Extensions)|Unterstützt
---|---|---|---
FLV-Datei (mit h. 264 und AAC-Codecs) (.flv)          |Ja 
MXF (.mxf)                  |Ja 
GXF (.gxf)                  |Ja 
MPEG2-PS, MPEG2-TS 3GP (.ts, PS, 3GP, .3gpp, MPG)   |Ja 
Windows Media Video (WMV) / ASF (WMV, ASF) |Ja 
AVI (dekomprimiert 8 Bit/10 Bit) (AVI)|Ja 
MP4 (MP4, M4A, M4V) / ISMV (.isma, ismv)|Ja 
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Ja 
Matroska/WebM (.mkv)        |Ja 
WAVE/WAV (.wav) |Ja 
QuickTime (MOV) |Ja

>[AZURE.NOTE] Oben ist eine Liste von häufiger auftretenden Datei Extensions. Media Encoder Standard unterstützt zahlreiche andere (Beispiel: .m2ts, .mpeg2video, qt). Wenn Sie versuchen, eine Datei codieren eine Fehlermeldung über das Format nicht unterstützt, geben Sie eine Feedback [hier](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Audio input Container-Formate 

Media Encoder Standard unterstützt die folgenden Audioformate input Container befördern:

- MXF, GXF und QuickTime-Dateien die Audiospuren mit überlappenden Stereo oder 5.1 Beispiele haben

oder

- MXF, GXF und QuickTime Dateien, die Audiowiedergabe erfolgt als PCM-Spuren kanalzuordnung (in Stereo oder 5.1) aus den Metadaten der Datei ableiten

Beachten Sie, Unterstützung für Kanal explizite/benutzerdefinierte Zuordnung in naher Zukunft bereitgestellt werden.


##<a name="input-video-codecs"></a>Input Videocodecs

Input Videocodecs|Unterstützt
---|---|---|---
AVC 8-Bit/10-Bit, 4:2:2, einschließlich AVCIntra   |8-bit-4:2:0 und 4:2:2 
Avid DNxHD (in MXF)                                 |Ja 
DVCPro/DVCProHD (in MXF)                            |Ja 
Digitales Video (DV) (in AVI-Dateien)                   |Ja
JPEG 2000                                           |Ja 
MPEG-2 (422 Profil und hohe, einschließlich Varianten wie XDCAM XDCAM HD, XDCAM IMX, CableLabs® und D10)|Bis zu 422 Profil 
MPEG-1                                              |Ja 
VC-1/WMV9                                           |Ja 
Canopus HQ/HQX                                      |Nein 
MPEG-4 Teil 2                                       |Ja 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ja 
YUV420 nicht komprimiert oder mezzanine                   |Ja
Apple ProRes 422                                    |Ja
Apple ProRes 422 LT |Ja
Apple ProRes 422 HQ |Ja
Apple ProRes Proxy|Ja
Apple ProRes 4444 |Ja
Apple ProRes 4444 XQ |Ja



##<a name="input-audio-codecs"></a>Geben Sie Audio-Codecs

Geben Sie Audio-Codecs|Unterstützt
---|---|---|---
AAC (AAC-LC und AAC-HE-AAC-HEv2; bis 5.1)|Ja 
MPEG Layer 2|Ja 
MP3 (MPEG-1 Audio Layer 3)|Ja 
Windows Media Audio|Ja 
WAV-PCM|Ja 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ja 
[Werk](http://go.microsoft.com/fwlink/?LinkId=822667) |Ja 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ja 
AMR (adaptive Multirate)|Ja
AES (SMPTE 331 M und M 302, AES3 2003)        |Nein 
Dolby® E                                    |Nein 
Dolby® Digital (AC3)                        |Nein 
Dolby® Digital Plus (E-AC3)                 |Nein 


##<a name="output-formats-and-codecs"></a>Formate und codecs

Die folgende Tabelle enthält die Codecs und Formate, die für den Export unterstützt werden.


Dateiformat|Video-Codec|Audio-Codec
---|---|---
MP4 <br/><br/>(einschließlich Multi-Bitrate MP4-Container) |H. 264 (hoher Main und geplante Profile)|AAC-LC, HE-AAC v1, HE-AAC v2 
MPEG2-TS |H. 264 (hoher Main und geplante Profile)|AAC-LC, HE-AAC v1, HE-AAC v2 



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch

[Codierung abrufbare Inhalte mit Azure Media Services](media-services-encode-asset.md)

[Das Media Encoder Standard codieren](media-services-dotnet-encode-with-media-encoder-standard.md)
