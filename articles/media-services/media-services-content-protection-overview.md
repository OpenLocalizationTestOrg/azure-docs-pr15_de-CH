<properties 
    pageTitle="Inhaltsübersicht Schutz | Microsoft Azure" 
    description="In diesem Artikel Überblick Schutz mit Media Services." 
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
    ms.date="09/27/2016" 
    ms.author="juliako"/>

#<a name="protecting-content-overview"></a>Inhaltsübersicht schützen


Microsoft Azure Media Services können Sie Medien vom Zeitpunkt sichern verlässt Computers durch Speicherung, Verarbeitung und Übermittlung. Media Services können die Live- und on-Demand-Inhalte dynamisch mit Advanced Encryption Standard (AES) (mit 128-Bit-Verschlüsselungsschlüssel) oder eines großen DRM verschlüsselt: Microsoft PlayReady Google Widevine und Apple FairPlay. Media Services bietet auch einen Dienst für AES-Schlüssel und DRM (PlayReady, Widevine und FairPlay) Lizenzen auf autorisierte Clients. 

Das folgende Bild zeigt den Inhaltsschutz Workflows, die AMS unterstützt. 

![Mit PlayReady schützen](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[AZURE.NOTE]Um dynamische Verschlüsselung verwenden zu können, muss zunächst mindestens eine reservierte Einheit streaming auf Streaming-Endpunkt abgerufen, verschlüsselte Inhalte übertragen werden soll.

Dieses Thema erläutert [Konzepte und Begriffe](media-services-content-protection-overview.md) für das Verständnis des Content-Schutz mit AMS. Das Thema enthält außerdem [Links](media-services-content-protection-overview.md#common-scenarios) zu Themen wie Inhaltsschutz Aufgaben. 

##<a name="dynamic-encryption"></a>Dynamische Verschlüsselung

Microsoft Azure Media Services können Sie Ihre Inhalte dynamisch mit AES-Schlüssel zum Löschen oder DRM-Verschlüsselung verschlüsselt: Microsoft PlayReady Google Widevine und Apple FairPlay.

Sie können derzeit die folgenden streaming-Formate verschlüsseln: HLS MPEG DASH und Smooth Streaming. HDS-streaming-Format können nicht verschlüsselt oder "Progressiv".

Für Media Services Vermögenswert verschlüsseln sollten, Sie einen Schlüssel (CommonEncryption oder EnvelopeEncryption) mit der Anlage und Autorisierungsrichtlinien für den Schlüssel auch konfigurieren.

Sie müssen die Anlage Lieferung Richtlinie konfigurieren. Wenn Sie verschlüsselte Speicherressourcen streamen möchten, sollten Sie festlegen wie möchten sie Anlagen Lieferung Richtlinie konfigurieren.

Wenn ein Spieler ein Stream angefordert wird, verwendet Media Services angegebenen Schlüssel dynamisch Ihre Inhalte mithilfe von AES Löschtaste oder DRM-Verschlüsselung verschlüsselt. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel Schlüssel Paketdienstes. Entscheidung, ob der Benutzer berechtigt ist, den Schlüssel, wertet der Dienst die Autorisierungsrichtlinien für den Schlüssel angegeben.

>[AZURE.NOTE]Um dynamische Verschlüsselung nutzen zu können, müssen Sie zunächst mindestens bei Bedarf Streaming-Einheit für Streaming-Endpunkt abrufen, aus denen Bereitstellung verschlüsselten Inhalt soll. Weitere Informationen finden Sie unter [Skalierung Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="storage-encryption"></a>Speicherverschlüsselung

Verwenden Sie speicherverschlüsselung zu verschlüsseln, klare Inhalte lokal mit AES 256-Bit-Verschlüsselung auf Azure-Speicher Speicherort verschlüsselt ruhende hochladen. Mit Storage Verschlüsselung geschützt werden automatisch entschlüsselt ein verschlüsseltes Dateisystem vor Codierung platziert und optional vor dem Hochladen als eine neue Ausgabe Anlage erneut verschlüsselt. Primäre Nutzung bei speicherverschlüsselung wird Sie hochwertige input Mediendateien mit starker Verschlüsselung ruhender auf Festplatte sichern möchten.

Um verschlüsselte Speicherressourcen zu liefern, müssen so Media Services weiß, wie Ihre Inhalte werden soll die Anlage Lieferung Richtlinie konfigurieren. Bevor die Anlage übertragen kann, streaming Server Speicher entschlüsselt und überträgt Inhalte mit angegebenen Lieferung Policy (AES, allgemeine Verschlüsselung oder ohne Verschlüsselung).

## <a name="common-encryption-cenc"></a>Allgemeine Verschlüsselung (CENC)

Allgemeine Verschlüsselung wird verwendet, wenn die Inhalte mit PlayReady verschlüsseln oder / und Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Cbcs Aapl Verschlüsselung

CBCs Aapl wird verwendet, wenn Ihre Inhalte FairPlay verschlüsseln.

## <a name="envelope-encryption"></a>Umschlag-Verschlüsselung 

Verwenden Sie diese Option, wenn Sie Ihre Inhalte mit AES-128 Löschen schützen möchten. Wählen Sie eine sicherere, eine der DRM in diesem Thema aufgeführt. 

##<a name="licenses-and-keys-delivery-service"></a>Lizenzen und Schlüssel Lieferservice

Media Services bietet einen Dienst für die Bereitstellung von Lizenzen für DRM (PlayReady, Widevine, FairPlay) und AES Schlüssel auf autorisierte Clients löschen. [Azure-Portal](media-services-portal-protect-content.md), REST-API oder Media Services SDK für .NET können Sie die Autorisierung und Authentifizierung zu Lizenzen und Schlüssel konfigurieren.

##<a name="token-restriction"></a>Token Einschränkung

Inhalt wichtige Autorisierungsrichtlinie könnte gelten mindestens Autorisierung: Öffnen oder Einschränkung token. Die token eingeschränkte Richtlinie ein Token ausgestellt durch eine Secure Token Service (STS) beizufügen. Media Services unterstützt Token im die Simple Web Token (SWT) Format und JSON Web Token (JWT). Media Services bietet keine sichere Token-Services. Erstellen eines benutzerdefinierten STS oder Microsoft Azure ACS Problem Tokens nutzen. Der STS müssen konfiguriert werden, erstellen Sie einen Token signiert mit angegebenen Schlüssel und Thema, die in der Konfiguration token Einschränkung angegeben. Der Schlüssel Zustelldienst Media Services zurück angeforderte Schlüssel (oder Lizenz) Client, wenn das Token gültig ist und die Ansprüche im token dem die konfigurierten Schlüssel (oder Lizenz).

Wenn Richtlinien konfigurieren das Token eingeschränkt werden, müssen Sie primäre Überprüfung Schlüssel, Aussteller und Zielgruppe Parameter angeben. Primäre Überprüfung Schlüssel enthält Schlüssel, dem Token signiert wurde, Aussteller des Sicherheitstokendiensts, die das Token ausstellt. Zielgruppe (Bereich bezeichnet) beschreibt die Absicht des Tokens oder die Ressource autorisiert das Token auf. Wichtige Zustelldienst Media Services überprüft, dass diese Werte im Token die Werte in der Vorlage übereinstimmen.

##<a name="streaming-urls"></a>Streaming-URLs

Wenn die Anlage mit mehreren DRM verschlüsselt wurde, verwenden Sie eine Verschlüsselung Tag im Streaming-URL: (Format = "m3u8 Aapl" Verschlüsselung = 'Xxx').

Folgendes gilt:

- Nur 0 oder 1 Verschlüsselungstyp kann angegeben werden.
- Verschlüsselungstyp muss in der Url angegeben werden, wenn nur eine Verschlüsselung auf die Anlage angewendet wurde.
- Verschlüsselung wird zwischen Groß-und Kleinschreibung.
- Die folgenden Verschlüsselungstypen können angegeben werden:  
    - **Cenc**: Allgemeine Verschlüsselung (Playready oder Widevine)
    - **CBCs Aapl**: Fairplay
    - **CBC**: Umschlag-AES-Verschlüsselung.

##<a name="common-scenarios"></a>Häufige Szenarien

Die folgenden Themen veranschaulichen Schutz von Content im Speicher dynamisch verschlüsselte Stream-Medien bieten, AMS Schlüssel-Lizenz Zustelldienst verwenden

- [Schützen Sie mit AES](media-services-protect-with-aes128.md) 
- [Schützen Sie mit PlayReady oder Widevine](media-services-protect-with-drm.md)
- [Der Inhalt geschützt HLS mit Apple FairPlay bzw. PlayReady Stream](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Weitere Szenarien

- [Azure PlayReady-Lizenzdienst mit eigenem Encryptor/streaming Server integrieren](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
- [Mithilfe von CastLabs zu DRM-Lizenzen in Azure Media Services](media-services-castlabs-integration.md)
 
##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Verwandte Links

[Ankündigung von PlayReady als Dienst und dynamische AES-Verschlüsselung mit Azure Media Services](http://mingfeiy.com/playready)

[Azure Media Services PlayReady Lizenz Lieferung Preise erläutert](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Das Debuggen für AES verschlüsselten Stream in Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT token authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integration von Azure Media Services owin-MVC Anwendung in Azure Active Directory basieren und Bereitstellung von Inhalten basierend auf JWT Ansprüche beschränken](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Azure ACS Problem Token verwenden](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
