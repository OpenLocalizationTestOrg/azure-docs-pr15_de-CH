<properties 
    pageTitle="Wiedergabe des Inhalts | Microsoft Azure" 
    description="In diesem Thema werden vorhandene Spieler, zur Wiedergabe des Inhalts verwenden können." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>Der Inhalt mit vorhandenen

Azure Media Services unterstützt viele gängige streaming-Formate, Smooth Streaming HTTP-Live-Streaming und MPEG-Dash. Dieses Thema verweist auf vorhandene Spieler, die Datenströme zu testen.

>[AZURE.NOTE]Wiedergabe dynamisch verpackt oder dynamisch verschlüsselter Inhalt, müssen Sie mindestens eine Streaming-Einheit für den Streaming-Endpunkt erhalten Ihre Inhalte liefern soll. Weitere Informationen zum Skalieren von Streaming-Einheiten: [wie Streaming-Einheiten](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Azure Portal Media Services Content player

**Azure** -Portal bietet einen Content-Player, mit denen Sie das Video zu testen.

Klicken Sie auf das gewünschte Video (achten [sie war](media-services-portal-publish.md)) und auf die Schaltfläche **Wiedergabe** am unteren Rand des Portals.

Einige zu berücksichtigen:

- **MEDIA SERVICES CONTENT PLAYER** spielt die Standardeinstellungen für streaming-Endpunkt. Aus einer nicht standardmäßigen Streaming-Endpunkt, verwenden Sie einen anderen Spieler. Z. B. [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Azure MediaPlayer

Verwenden von [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) zur Wiedergabe der Inhalte (löschen oder geschützte) in einem der folgenden Formate:

- Smooth Streaming
- MPEG-STRICH
- HLS
- Progressive MP4


###<a name="flash-player"></a>Flash Player

####<a name="aes-encrypted-with-token"></a>AES verschlüsselt mit

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Silverlight-Playern

####<a name="monitoring"></a>Überwachung

[http://SMF.cloudapp.NET/HealthMonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>PlayReady mit Token

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>Bindestrich-Spieler

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Andere

HLS URLs testen Sie ebenfalls verwenden können:

- **Safari** auf einem iOS-Gerät oder
- **3ivx HLS Player** unter Windows.

##<a name="developing-video-players"></a>Entwicklung Videoplayer

Informationen zu der Spieler finden Sie unter [Developing Videoplayer](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
