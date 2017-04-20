<properties
    pageTitle="Übersicht über dynamische Verpackung | Microsoft Azure"
    description="Thema gibt und Übersicht über dynamische Verpackung."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Dynamische Verpackung

##<a name="overview"></a>Übersicht

Microsoft Azure Media Services zu viele Quelle Mediendateiformate Media streaming-Formate verwendet werden und Schutz an Clienttechnologien formatiert (z. B. iOS XBOX, Silverlight, Windows 8). Diese Clients unterschiedliche Protokolle verstehen und iOS erfordert beispielsweise ein HTTP-Live-Streaming (HLS) V4-Formats Silverlight und Xbox erfordern Smooth Streaming. Haben Sie eine Reihe von adaptiven Bitrate (Multi-Bitrate) MP4 (ISO Base 14496-12) Mediendateien oder adaptive Bitrate Smooth Streaming-Dateien, die Sie für Clients bereitgestellt, die MPEG DASH HLS oder Smooth Streaming verstehen möchten, Sie sollten nutzen Media Services dynamische Verpackung.

Mit dynamischen Verpackung alles ist eine Anlage zu erstellen, die eine adaptive Bitrate MP4 oder adaptive Bitrate Smooth Streaming-Dateien enthält. Anschließend wird basierend auf dem angegebenen Format in der Anforderung Manifest oder Fragment On-Demand-Streaming Server garantieren Sie den Stream im Protokoll gewählte. Daher müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen und Media-Dienste erstellen und die entsprechende Antwort auf Anfragen von einem Client verwendet.

Das folgende Diagramm zeigt der traditionellen Codierung und statische Verpackung.

![Statische Codierung](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Das folgende Diagramm zeigt den Workflow dynamische Verpackung.

![Dynamische Codierung](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Um dynamische Verpackung nutzen zu können, müssen Sie zunächst mindestens bei Bedarf Streaming-Einheit für Streaming-Endpunkt abrufen, aus denen Lieferung des Inhalts werden soll. Weitere Informationen finden Sie unter [Skalierung Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="common-scenario"></a>Häufig

1. Hochladen einer Eingabedatei (Mezzanine-Datei bezeichnet). Z. B. h. 264 MP4 oder WMV (für die Liste der unterstützten Formate finden Sie unter [Media Encoder Standard unterstützt](media-services-media-encoder-standard-formats.md).

1. Die Aufsteckkarte Datei auf h. 264 MP4 adaptive Bitrate codiert.

1. Veröffentlichen Sie die Anlage, die die adaptive Bitrate enthält MP4 festgelegt Locator auf Anforderung erstellen.

1. Streaming URLs zugreifen und Ihre Inhalte zu erstellen.


##<a name="preparing-assets-for-dynamic-streaming"></a>Vorbereitung von Ressourcen für dynamische streaming

Um die Anlage für dynamische streaming vorbereiten haben Sie zwei Optionen:

1. [Eine master-Datei hochladen](media-services-dotnet-upload-files.md).
2. [Verwenden Sie Media Encoder Standard Encoder h. 264 MP4 adaptive Bitrate Sätze erstellen](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Stream den Inhalt](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>Dynamische Verpackung nicht unterstützten Formaten

Dateiformate von Quelle werden dynamische Verpackung nicht unterstützt.

- Dolby digital mp4-Dateien.
- Dolby digital glatte Dateien.

##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
