<properties
    pageTitle="  Veröffentlichen mit Azure-Portal | Microsoft Azure"
    description="Dieses Lernprogramm führt Sie durch die Schritte der Veröffentlichung des Inhalts mit der Azure-Portal."
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

# <a name="publish-content-with-the-azure-portal"></a>Veröffentlichen Sie mit Azure-portal

> [AZURE.SELECTOR]
- [Portal](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [REST](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>Übersicht

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 

Um Ihre Benutzer mit einer URL angeben, das zum Streamen oder Ihre Inhalte herunterladen, müssen Sie "Ihrer Ressource veröffentlichen" einen Locator erstellen. Locators ermöglichen den Zugriff auf Dateien in der Anlage. Media Services unterstützt zwei Arten von Locators: 

- Streaming (OnDemandOrigin)-Locators für adaptives streaming (z. B. auf Streams MPEG DASH HLS oder Smooth Streaming) verwendet. Erstellen ein Streaming-Locators die Anlage muss eine ISM-Datei enthalten. 
- Progressiv (SAS)-Locators, zur Bereitstellung des Videos progressives Herunterladen.


Streaming-URL hat das folgende Format und Sie können Anlagen Smooth Streaming wiedergeben.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Erstellen Sie eine streaming-URL HLS anfügen (Format = m3u8 Aapl) an den URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Erstellen Sie ein MPEG-DASH streaming URL anhängen (Format = Mpd-Zeit-csf) an den URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

SAS-URL hat das folgende Format.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Weitere Informationen finden Sie unter [Delivering Content Overview](media-services-deliver-content-overview.md).

>[AZURE.NOTE] Wenn Sie das Portal Locators vor März 2015 erstellt, wurden Locator mit einem zweijährigen Ablaufdatum erstellt.  

Aktualisieren Sie ein Ablaufdatum ein Locator verwenden Sie [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) oder [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs. Beachten Sie, dass beim Aktualisieren des Ablaufdatum des SAS-Locator URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Mit dem Portal Vermögenswert veröffentlichen

Um das Portal zu Vermögenswert veröffentlichen, führen Sie folgende Schritte aus:

1. Wählen Sie in [Azure-Portal](https://portal.azure.com/)Azure Media Services-Konto.
1. **Wählen Sie in** > **Elemente**.
1. Wählen Sie die Anlage, die Sie veröffentlichen möchten.
1. Klicken Sie auf die Schaltfläche **Veröffentlichen** .
1. Wählen Sie den Locator.
2. Drücken Sie **Hinzufügen**.

    ![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Die URL wird die Liste der **URLs veröffentlicht**hinzugefügt werden.

## <a name="play-content-from-the-portal"></a>Wiedergeben von Inhalten aus dem portal

Azure-Portal bietet einen Content-Player, mit denen Sie das Video zu testen.

Klicken Sie auf das gewünschte Video, und klicken Sie dann auf die Schaltfläche **Wiedergabe** .

![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-play.png)

Einige zu berücksichtigen:

- Stellen Sie sicher, dass das Video veröffentlicht wurde.
- **MediaPlayer** spielt den Standardwert streaming-Endpunkt. Wenn Sie von einem nicht standardmäßigen streaming-Endpunkt wiedergeben möchten, klicken Sie kopieren Sie die URL und einen anderen Player. Z. B. [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Streaming-Endpunkt Sie streaming muss ausgeführt werden.  
- Fügen Sie zum Streamen von Streaming-Endpunkt mindestens eine Streaming-Einheit. Weitere Informationen finden Sie in [diesem](media-services-portal-scale-streaming-endpoints.md) Thema.   

##<a name="next-steps"></a>Nächste Schritte

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


