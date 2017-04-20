<properties 
    pageTitle="Video-Player Anwendungsentwicklung" 
    description="Das Thema enthält Links zu Player Frameworks und Plug-Ins, mit denen Sie eigene Clientanwendungen entwickeln, die Stream-Medien von Media Services nutzen können." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="develop-video-player-applications"></a>Video-Player Anwendungsentwicklung

##<a name="overview"></a>Übersicht

Azure Media Services bietet Werkzeuge funktionsreiche, dynamische Player Clientanwendungen für die meisten Plattformen einschließlich erstellen: iOS Geräte, Android, Windows, Windows Phone, Xbox und Set-Top-Boxen. Dieses Thema enthält auch Links zu SDKs und Player-Frameworks, mit denen Sie eigene Clientanwendungen entwickeln, die Stream-Medien von Azure Media Services nutzen können.

##<a name="azure-media-player"></a>Azure MediaPlayer

[Azure Media Player](http://aka.ms/ampinfo) ist ein Web-Videoplayer erstellt, um Medieninhalte von Microsoft Azure Media Services auf einer Vielzahl von Browsern und Geräten wiedergeben. Azure Media Player nutzt Branchenstandards HTML5 Media Quelle Extensions (MSE) und verschlüsselt Media Extensions (EME), erweiterte adaptive streaming Erlebnis bereitzustellen. Wenn diese Standards nicht auf einem Gerät oder in einem Browser verfügbar sind, verwendet Azure Media Player Flash und Silverlight als fallback-Technologie. Unabhängig von der Wiedergabe-Technologie verwendet müssen Entwickler eine einheitliche JavaScript-Schnittstelle auf APIs zugreifen. Dadurch können Inhalte von Azure Media Services über eine Vielzahl von Geräten und Browsern ohne zusätzlichen Aufwand gespielt werden.

Microsoft Azure Media Services können Inhalte mit Bindestrich Smooth Streaming und Streaming-Formate HLS übertragen werden Inhalte wiedergeben. Azure Media Player berücksichtigt diese unterschiedlichen Formaten und gibt automatisch die beste Verbindung basierend auf den Platform-Browser-Funktionen. Microsoft Azure Media Services ermöglicht dynamische Verschlüsselung von Anlagen mit PlayReady-Verschlüsselung oder AES-128-bit-Verschlüsselung Umschlag. Azure Media Player ermöglicht Entschlüsselung PlayReady und AES 128 bit verschlüsselten Inhalt entsprechend konfiguriert. 

Weitere Informationen:

- [Azure MediaPlayer](http://aka.ms/ampinfo)
- [Azure Media Player Dokumentation](http://aka.ms/ampdocs) 
- [Erste Azure MediaPlayer gestartet Blog](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Bleiben Sie mit Azure Media Player Stand](http://aka.ms/ampsignup)
- [Fügen Sie neue Wünsche, Ideen und feedback](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Andere Tools zum Erstellen von Anwendungen

Sie können auch den folgenden SDKs:

- [Streaming-Client SDK Glätten](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Reibungslose Streaming Windows Store-App](media-services-build-smooth-streaming-apps.md)
- [Microsoft Media-Plattform: Player-Framework](http://playerframework.codeplex.com/) 
- [HTML5 Player Framework-Dokumentation](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [Microsoft Smooth Streaming-Plugin für OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Lizenzierung Microsoft® Glätten Streaming-Client Portieren Kit](http://aka.ms/sspk) 
- [XBOX Video-Anwendungsentwicklung](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Werbung

Azure Media Services bietet Unterstützung für Werbung über die Windows Media-Plattform: Player-Frameworks. Frameworks mit Active Directory-Player sind für Windows 8, Silverlight, Windows Phone 8 und iOS-Geräte verfügbar. Jede Player-Framework enthält Beispielcode, der wie eine Player-Anwendung implementiert wird. Es gibt drei verschiedene Arten anzeigen, die Sie in Ihre Medien einfügen:

Linear – Vollbild anzeigen, die wichtigste Video anhalten

Nichtlineare-Overlay-Anzeigen der wichtigsten Videowiedergabe angezeigt, in der Regel ein Logo oder andere statische Bild im player

Companion-anzeigen, die außerhalb des Players angezeigt werden

Anzeigen können jederzeit das Hauptvideo Zeitleiste platziert werden. Sie müssen dem Player beim Ad spielen und welche anzeigen spielen mitteilen. Dies erfolgt mithilfe einer Reihe von standard-XML-basierte Dateien: Video Ad Service Vorlage (VAST), Digital Video mehrere Ad Wiedergabeliste (VMAP), Media abstrakte Sequenzierung Vorlage (MAST) und Digital Video Player Ad Interface Definition (VPAID). GROßE Dateien angeben, welche anzeigen. VMAP-Dateien festlegen, wann verschiedenen anzeigen und große XML enthalten. MAST Dateien können Sequenz anzeigen, die auch große XML enthalten. VPAID-Dateien definieren Sie eine Schnittstelle zwischen den video-Player, Anzeige und Ad Server. Weitere Informationen finden Sie unter [Anzeigen einfügen](https://msdn.microsoft.com/library/dn387398.aspx).

Informationen zur Unterstützung von geschlossenen Untertiteln und anzeigen in Live-Streaming-Videos finden Sie unter [unterstützte Untertitel und Ad einfügen](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch

[Einbetten von MPEG-DASH Adaptives Streaming-Video in HTML5 Anwendung mit DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js repository](https://github.com/Dash-Industry-Forum/dash.js)
 
