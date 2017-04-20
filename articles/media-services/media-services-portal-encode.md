<properties
    pageTitle="Codieren eine Anlage Azure-Portal mit Media Encoder Standard | Microsoft Azure"
    description="Dieses Lernprogramm führt Sie durch die Schritte der Vermögenswert Azure-Portal mit Media Encoder Standard-Codierung."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Codieren einer Anlage Azure-Portal mit Media Encoder Standard

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 

Arbeit mit Azure Media Services, die eines der häufigsten Szenarios adaptives streaming Bitrate an die Clients übermittelt. Media Services unterstützt die folgende adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern). Um Videos für adaptive Bitrate streaming vorbereiten, müssen Sie das Quellvideo in Dateien Multi-Bitrate codiert. Verwenden Sie **Media Encoder Standard** Encoder Codieren von Videos.  

Media Services bietet auch zu Ihrem Multi-Bitrate MP4s in folgenden streaming-Formate ermöglicht dynamische Verpackung: MPEG DASH HLS, Smooth Streaming oder HDS ohne diese Streaming-Formate erneut verpacken. Dynamische Verpackung müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen mit Media Services erstellen und die entsprechende Antwort auf Anfragen von einem Client verwendet.

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- Codieren Sie die Quelldatei in Multi-Bitrate MP4-Dateien (Codierung Schritte werden weiter unten in diesem Abschnitt gezeigt).
- Rufen Sie mindestens eine Streaming-Einheit für Streaming-Endpunkt aus dem Lieferung des Inhalts werden soll. Weitere Informationen finden Sie unter [streaming Endpunkte konfigurieren](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

Skalieren Media Verarbeitung finden Sie [in diesem](media-services-portal-scale-media-processing.md) Thema.

## <a name="encode-with-the-azure-portal"></a>Codieren Sie mit Azure-portal

Dieser Abschnitt beschreibt die Schritte, mit denen Sie Ihre Inhalte mit Media Encoder Standard codieren.

1.  Wählen Sie in [Azure-Portal](https://portal.azure.com/)Azure Media Services-Konto.
2.  Wählen Sie im **Einstellungsfenster** **Anlagen**.  
2.  Wählen Sie im Fenster **Anlagen** Anlage codieren möchten.
3.  Drücken Sie die **Codierung** .
4.  **Codieren einer Anlage** im Fenster "Media Encoder Standard" Prozessor und Auswählen einer Voreinstellung Beispielsweise wenn Sie input Video hat eine Auflösung von 1920 x 1080 Pixel kennen, dann können die "H264 mehrere Bitrate 1080p" voreingestellte. Weitere Informationen über Voreinstellungen finden Sie [diese](https://msdn.microsoft.com/library/azure/mt269960.aspx) Artikel – unbedingt Vorgaben auswählen, die für die Eingabe video am besten geeignet ist. Wenn Sie ein Video mit niedriger Auflösung (640 x 360), Sie sollten nicht die Standardeinstellung verwenden "H264 mehrere Bitrate 1080p" voreingestellte.
    
    Zur einfacheren Verwaltung können Sie den Namen der Anlage Ausgabe, und der Name des Auftrags.
        
    ![Codieren von Anlagen](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Drücken Sie **zu erstellen**.


##<a name="next-step"></a>Nächstes

Sie können encoding Auftrag mit Azure-Portal überwachen wie in [diesem](media-services-portal-check-job-progress.md) Artikel beschrieben.  

##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


