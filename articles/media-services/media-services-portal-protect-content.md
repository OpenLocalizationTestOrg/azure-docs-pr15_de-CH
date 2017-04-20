<properties 
    pageTitle="Mithilfe von Azure Portal Content Schutzrichtlinien konfigurieren | Microsoft Azure" 
    description="Dieser Artikel veranschaulicht, wie das Azure-Portal Content Schutzrichtlinien konfigurieren. Der Artikel zeigt außerdem dynamische Verschlüsselung für Ihre Anlagen aktivieren." 
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

# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Mithilfe von Azure Portal Content Schutzrichtlinien konfigurieren

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="overview"></a>Übersicht

Microsoft Azure Media Services (AMS) können Sie Ihre Medien ab secure verlässt Computers durch Speicherung, Verarbeitung und Übermittlung. Media Services können Sie zu Ihrem Inhalt verschlüsselt dynamisch mit Advanced Encryption Standard (AES) (mit 128-Bit-Schlüssel), gemeinsame Verschlüsselung (CENC) mit PlayReady bzw. Widevine DRM und Apple FairPlay. 

AMS bietet einen Dienst für die Bereitstellung von DRM-Lizenzen und AES Schlüssel auf autorisierte Clients löschen. Azure-Portal können Sie einen **Schlüssel/Lizenz Autorisierungsrichtlinie** für alle Arten von Verschlüsselung.

Dieser Artikel veranschaulicht das Azure-Portal Content Schutzrichtlinien konfigurieren. Der Artikel zeigt außerdem dynamische Verschlüsselung Ihrer Ressourcen anwenden.

> [AZURE.NOTE]  Wenn Sie Azure-Verwaltungsportal Richtlinien erstellt, erscheinen die Richtlinien nicht in [Azure-Portal](https://portal.azure.com/). Allerdings bestehen alle alten Richtlinien. Überprüfen Sie mithilfe von Azure Media Services .NET SDK oder [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) Tool (Richtlinien finden mit der rechten Maustaste auf die Anlage-Informationen (F4) -> Anzeigen > Klicken Sie auf der Registerkarte Inhalt Schlüssel -> Klicken Sie auf den Schlüssel). 
> 
> Wenn Sie die Anlage mit neue verschlüsseln möchten, mit der Azure-Portal konfigurieren speichern und auf dynamische Verschlüsselung erneut. 

## <a name="start-configuring-content-protection"></a>Konfigurieren Inhaltsschutz

Gehen Sie wie folgt vor, um das Portal konfigurieren Inhaltsschutz global AMS-Konto:

1. Wählen Sie in [Azure-Portal](https://portal.azure.com/)Azure Media Services-Konto.
2. **Wählen Sie in** > **Schutz**.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection001.png)
 

## <a name="keylicense-authorization-policy"></a>Schlüssel-Lizenz Autorisierungsrichtlinie

AMS unterstützt mehrere Methoden für die Authentifizierung von Benutzern Schlüssel oder Lizenz anfordern. Inhalt wichtige Autorisierungsrichtlinie müssen Sie konfiguriert und vom Client in der Reihenfolge der Schlüssel-Lizenz auf dem Client delived erfüllt. Inhalt wichtige Autorisierungsrichtlinie könnte gelten mindestens Autorisierung: **Öffnen** oder **token** Beschränkung.

Azure-Portal können Sie einen **Schlüssel/Lizenz Autorisierungsrichtlinie** für alle Arten von Verschlüsselung.

###<a name="open"></a>Öffnen 

Open-Einschränkung bedeutet, dass das System den Schlüssel jeder liefern macht eine wichtige Anforderung. Diese Einschränkung kann zu Testzwecken nützlich sein. 

### <a name="token"></a>Token

Die token eingeschränkte Richtlinie ein Token ausgestellt durch eine Secure Token Service (STS) beizufügen. Media Services unterstützt Token im die Simple Web Token (SWT) Format und JSON Web Token (JWT). Media Services bietet keine sichere Token-Services. Erstellen eines benutzerdefinierten STS oder Microsoft Azure ACS Problem Tokens nutzen. Der STS müssen konfiguriert werden, erstellen Sie einen Token signiert mit angegebenen Schlüssel und Thema, die in der Konfiguration token Einschränkung angegeben. Der Schlüssel Zustelldienst Media Services zurück angeforderte Schlüssel (oder Lizenz) Client, wenn das Token gültig ist und die Ansprüche im token dem die konfigurierten Schlüssel (oder Lizenz).

Wenn Richtlinien konfigurieren das Token eingeschränkt werden, müssen Sie primäre Überprüfung Schlüssel, Aussteller und Zielgruppe Parameter angeben. Primäre Überprüfung Schlüssel enthält Schlüssel, dem Token signiert wurde, Aussteller des Sicherheitstokendiensts, die das Token ausstellt. Zielgruppe (Bereich bezeichnet) beschreibt die Absicht des Tokens oder die Ressource autorisiert das Token auf. Wichtige Zustelldienst Media Services überprüft, dass diese Werte im Token die Werte in der Vorlage übereinstimmen.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady Rechtevorlage

Ausführliche Informationen über PlayReady-Rechtevorlage Übersicht [Media Services PlayReady Lizenz Vorlage](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Nicht persistent

Lizenz als nicht persistent konfigurieren, ist es nur im Speicher gehalten, während der Player die Lizenz verwendet.  

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Permanente

Konfigurieren die Lizenz als permanente, wird es im permanenten Speicher auf dem Client gespeichert.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine Rechtevorlage

Ausführliche Informationen über die Rechtevorlage Widevine Übersicht [Widevine Lizenz Vorlage](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Grundlegende

Wenn Sie **grundlegende**auswählen, wird die Vorlage mit allen Standardwerte erstellt.

### <a name="advanced"></a>Erweiterte

Ausführliche Erläuterung erweiterte Option Widevine Konfigurationen finden Sie unter [diesem](media-services-widevine-license-template-overview.md) Thema.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay-Konfiguration

Um FairPlay Verschlüsselung aktivieren, müssen Sie über die Konfigurationsoption FairPlay App Zertifikat und Anwendung geheime Schlüssel (Frage) bereitstellen. Ausführliche Informationen über FairPlay Konfiguration und finden Sie [in diesem](media-services-protect-hls-with-fairplay.md) Artikel.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Dynamische Verschlüsselung für die Anlage gelten

Um dynamische Verschlüsselung nutzen zu können, müssen Sie Folgendes tun:

- Codieren Sie die Quelldatei in adaptive Bitrate MP4-Dateien.
- Erhalten Sie mindestens auf Anforderung Streaming-Einheit für Streaming-Endpunkt zu Ihrem Inhalt soll. Weitere Informationen finden Sie unter [streaming reservierte Einheiten bei Bedarf zu skalieren](media-services-portal-manage-streaming-endpoints.md).

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Wählen Sie eine Anlage, die Sie verschlüsseln möchten

**Um alle Elemente anzuzeigen, wählen Sie in** > **Elemente**.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Verschlüsselung mit AES oder DRM

Drücken Sie **Verschlüsseln** für eine Anlage Sie mit zwei Optionen angezeigt: **AES** oder **DRM**. 

#### <a name="aes"></a>AES

AES-Verschlüsselung auf allen Streamingprotokolle aktiviert deaktivieren: Smooth Streaming, HLS und MPEG-DASH.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM

Bei Auswahl die Registerkarte DRM Sie mit verschiedenen Auswahlen Inhaltsschutz Richtlinien angezeigt (die Sie jetzt konfiguriert haben müssen) + eine Reihe von streaming-Protokolle.

- **PlayReady und Widevine mit MPEG-DASH** - verschlüsselt die MPEG-DASH-Stream PlayReady-DRM Widevine dynamisch.
- **PlayReady und Widevine MPEG-DASH + FairPlay mit HLS** - verschlüsselt dynamisch Sie MPEG-DASH-Stream PlayReady-DRM Widevine. Auch verschlüsselt die HLS Datenströme mit FairPlay.
- **PlayReady mit Smooth Streaming, HLS und MPEG-DASH** - verschlüsselt dynamisch Smooth Streaming, HLS, MPEG-DASH-Streams mit PlayReady-DRM.
- **Widevine mit MPEG-DASH** - verschlüsselt dynamisch Sie MPEG-DASH mit Widevine DRM.
- **FairPlay mit HLS** - verschlüsselt dynamisch die HLS Stream mit FairPlay.

Um FairPlay Verschlüsselung aktivieren, müssen Sie App Zertifikat und Anwendung geheime Schlüssel (Fragen) über die FairPlay-Konfigurationsoption Content Protection Settings Blade bereitzustellen.

![Inhalt schützen](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Sobald die Verschlüsselungsauswahl **anwenden stellen**.

##<a name="next-steps"></a>Nächste Schritte

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





