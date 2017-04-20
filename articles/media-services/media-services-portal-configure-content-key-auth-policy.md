<properties 
    pageTitle="Konfigurieren mithilfe von Azure Portal Content Schlüssel Autorisierungsrichtlinie | Microsoft Azure" 
    description="Informationen Sie zum Konfigurieren von Autorisierungsrichtlinien für einen symmetrischen Schlüssel." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>



#<a name="configure-content-key-authorization-policy"></a>Inhalt wichtige Autorisierungsrichtlinie konfigurieren
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>Übersicht

Microsoft Azure Media Services können Sie zu MPEG-DASH Smooth Streaming und HTTP-Live-Streaming (HLS) Streams mit Advanced Encryption Standard (AES) (mit 128-Bit-Verschlüsselungsschlüssel) oder [Microsoft PlayReady-DRM](https://www.microsoft.com/playready/overview/)geschützt. AMS können Sie Bindestrich Streams mit Widevine DRM verschlüsselt übermittelt. PlayReady und Widevine werden gemäß der Spezifikation allgemeine Verschlüsselung (ISO/IEC 23001 7 CENC) verschlüsselt.

Media Services stellt einen **Schlüssel-Lizenz Übermittlungsdienst** aus dem Kunden AES-Schlüssel oder PlayReady-Widevine Lizenzen zur Wiedergabe verschlüsselten Inhalts abrufen können.

In diesem Thema veranschaulicht, wie mit der Azure-Portal Content Schlüssel Autorisierungsrichtlinie konfigurieren. Der Schlüssel kann später verwendet werden, Ihre Inhalte dynamisch zu verschlüsseln. Beachten Sie, dass derzeit die folgenden streaming verschlüsseln können formatiert: HLS MPEG DASH und Smooth Streaming. HDS-streaming-Format können nicht verschlüsselt oder "Progressiv".

Wenn ein Spieler einen Stream, der dynamisch verschlüsselt werden soll anfordert, verwendet Media Services konfigurierten Schlüssel dynamisch Content mit AES oder DRM-Verschlüsselung verschlüsselt. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel Schlüssel Paketdienstes. Entscheidung, ob der Benutzer berechtigt ist, den Schlüssel, wertet der Dienst die Autorisierungsrichtlinien für den Schlüssel angegeben.


Möchten Sie mehrere Content Schlüssel oder eine **Schlüssel-Lizenz Zustelldienst** URL als Schlüssel Zustelldienst Media Services angeben möchten, verwenden Sie Media Services .NET SDK oder REST-APIs.

[Konfigurieren Sie Content-Taste Autorisierungsrichtlinie mit Media Services .NET SDK](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfigurieren von Content Schlüssel Autorisierungsrichtlinie mit Media Services REST-API](media-services-rest-configure-content-key-auth-policy.md)

###<a name="some-considerations-apply"></a>Einige zu berücksichtigen:

- Um dynamische Verpackung und dynamische Verschlüsselung verwenden zu können, müssen Sie mindestens eine reservierte Einheit streaming sicherstellen. Weitere Informationen finden Sie unter [wie Media Service](media-services-portal-manage-streaming-endpoints.md).
- Die Anlage muss eine Reihe von adaptiven Bitrate MP4s oder adaptive Bitrate Smooth Streaming-Dateien enthalten. Weitere Informationen finden Sie unter [Codieren einer Anlage](media-services-encode-asset.md).
- Der Zustelldienst Schlüssel speichert ContentKeyAuthorizationPolicy und seine zugehörigen Objekte (Optionen und Einschränkungen) für 15 Minuten.  Wenn ein ContentKeyAuthorizationPolicy erstellen und angeben, dass eine "Token" Einschränkung verwendet testen, aktualisieren Sie die Richtlinie auf "Öffnen" Einschränkung und dauert ca. 15 Minuten, bevor die Richtlinie auf "Öffnen" Version der Richtlinie wechselt.


##<a name="how-to-configure-the-key-authorization-policy"></a>Gewusst wie: konfigurieren die wichtigsten Autorisierungsrichtlinie

Konfigurieren Sie die wichtigsten Autorisierungsrichtlinie wählen Sie **INHALTSSCHUTZ** Seite aus

Media Services unterstützt mehrere Methoden für die Authentifizierung von Benutzern Schlüssel anfordern. Inhalt wichtige Autorisierungsrichtlinie haben **, **token**oder Einschränkung des **IP-** Autorisierung (**IP** kann mit anderen oder .NET SDK konfiguriert werden)**.

###<a name="open-restriction"></a>Open-Einschränkung

Einschränkung **Öffnen** bedeutet, dass das System den Schlüssel jeder liefern macht eine wichtige Anforderung. Diese Einschränkung kann zu Testzwecken nützlich sein.

![OpenPolicy][open_policy]

###<a name="token-restriction"></a>Token Einschränkung

Wählen Sie die token eingeschränkte Richtlinie drücken Sie **TOKEN** .

Die **token** eingeschränkte Richtlinie ein **Secure Token Service** (STS) ausgestellte Token beizufügen. Media Services unterstützt Token im die **Simple Web Token** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) Format und **JSON Web Token** (JWT). Informationen finden Sie unter [JWT-token-Authentifizierung](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services bietet keine **Sichere Token-Services**. Erstellen eines benutzerdefinierten STS oder Microsoft Azure ACS Problem Tokens nutzen. Der STS müssen konfiguriert werden, erstellen Sie einen Token signiert mit angegebenen Schlüssel und Thema, die in der Konfiguration token Einschränkung angegeben. Schlüssel Zustelldienst Media Services wird den Schlüssel an den Client zurückgegeben, wenn das Token gültig ist und die Ansprüche im Token für Content Schlüssel konfiguriert übereinstimmen. Weitere Informationen finden Sie unter [Azure ACS Problem Token verwenden](http://mingfeiy.com/acs-with-key-services).

Wenn Richtlinien konfigurieren **TOKEN** eingeschränkt werden, müssen Sie Werte für **Überprüfung**, **Aussteller** und **Zielgruppe**festlegen. Primäre Überprüfung Schlüssel enthält Schlüssel, dem Token signiert wurde, Aussteller des Sicherheitstokendiensts, die das Token ausstellt. Zielgruppe (Bereich bezeichnet) beschreibt die Absicht des Tokens oder die Ressource autorisiert das Token auf. Wichtige Zustelldienst Media Services überprüft, dass diese Werte im Token die Werte in der Vorlage übereinstimmen.

###<a name="playready"></a>PlayReady

Schutz der Inhalte mit **PlayReady**ist eines der Dinge, die Sie in Ihre Autorisierungsrichtlinie angeben einer XML-Zeichenfolge, die die PlayReady-Lizenz-Vorlage definiert. Standardmäßig wird die folgende Richtlinien festgelegt:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <LicenseType>Nonpersistent</LicenseType>
          <PlayRight>
            <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
          </PlayRight>
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Klicken Sie auf **Richtlinie Xml importieren** und bieten verschiedene XML entspricht dem XML-Schema definiert [hier](https://msdn.microsoft.com/library/azure/dn783459.aspx).


##<a name="next-step"></a>Nächstes

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

