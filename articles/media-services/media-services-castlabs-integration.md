<properties 
    pageTitle="Mithilfe von CastLabs zu Widevine Lizenzen in Azure Media Services | Microsoft Azure" 
    description="Dieser Artikel beschreibt die Verwendung Azure Media Services (AMS) zu dynamisch AMS mit PlayReady und Widevine DRM verschlüsselt Stream. PlayReady-Lizenz stammt aus Media Services PlayReady-Lizenzserver und Widevine Lizenz vom Lizenzserver CastLabs geliefert." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="Mingfeiy;willzhan;Juliako"/>


#<a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>Mithilfe von CastLabs zu Widevine Lizenzen in Azure Media Services

> [AZURE.SELECTOR]
- [Axinom](media-services-axinom-integration.md)
- [castLabs](media-services-castlabs-integration.md)

##<a name="overview"></a>Übersicht

Dieser Artikel beschreibt die Verwendung Azure Media Services (AMS) zu dynamisch AMS mit PlayReady und Widevine DRM verschlüsselt Stream. PlayReady-Lizenz stammt aus Media Services PlayReady-Lizenzserver und Widevine Lizenz vom Lizenzserver **CastLabs** geliefert.

Wiedergabe Streaminginhalte geschützt durch CENC (PlayReady bzw. Widevine) können Sie [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Details finden Sie in der [AMP Dokument](http://amp.azure.net/libs/amp/latest/docs/) .

Die folgende Abbildung veranschaulicht eine allgemeine Azure Media Services und CastLabs Architektur der Datenintegration.

![Integration](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##<a name="typical-system-set-up"></a>Normale System einrichten

- AMS ist Medieninhalte gespeichert.
- Schlüssel-IDs von Schlüsseln werden in CastLabs und AMS gespeichert.
- CastLabs und AMS? token integrierte Authentifizierung In den folgenden Abschnitten Authentifizierungstokens. 
- Wenn ein Clientanforderungen Stream Video wird der Inhalt dynamisch mit **Gemeinsamen Verschlüsselung** (CENC) verschlüsselt und dynamisch AMS Smooth Streaming und Strich verpackt. Wir bieten außerdem PlayReady M2TS elementaren Stream-Verschlüsselung für HLS streaming-Protokoll.
- PlayReady-Lizenz vom Lizenzserver AMS und Widevine Lizenz vom Lizenzserver CastLabs abgerufen. 
- Media Player entscheidet automatisch, welche Lizenz Sie abrufen auf die Client-Plattform basiert. 

##<a name="authentication-token-generation-for-getting-a-license"></a>Authentication token generieren keine Lizenz

CastLabs und AMS unterstützen token Format JWT (JSON Web Token) verwendet, um eine Lizenz zu autorisieren. 

###<a name="jwt-token-in-ams"></a>JWT-Token AMS 

Die folgende Tabelle beschreibt JWT Token AMS. 

Aussteller|Aussteller-Zeichenfolge aus der ausgewählten Secure Token Service (STS)
---|---
Zielgruppe|Zielgruppe Zeichenfolge verwendeten STS
Ansprüche|Ein
Nicht vor|Gültigkeitsdauer des Tokens starten
Läuft ab|Gültigkeitsdauer des Tokens Ende
SigningCredentials|Der PlayReady-Lizenzserver CastLabs License Server und STS gemeinsame Schlüssel möglicherweise symmetrische oder asymmetrische Schlüssel.

###<a name="jwt-token-in-castlabs"></a>JWT-Token im castLabs

Die folgende Tabelle beschreibt JWT-Token im CastLabs. 

Name|Beschreibung
---|---
optData|Eine JSON-Zeichenfolge mit Informationen. 
CRT|Eine JSON-Zeichenfolge mit Informationen über die Anlage der Lizenzrechte Info und Wiedergabe.
IAT|Der aktuelle Datetime Zeit.
JTI|Ein eindeutiger Bezeichner über dieses Token (jedes Token kann nur einmal im System CastLabs verwendet werden).

##<a name="sample-solution-set-up"></a>Beispielprojektmappe einrichten 

Die [Beispielprojektmappe](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) enthält zwei Projekte:

-   Eine Konsolenanwendung, mit der Anlage bereits aufgenommenen DRM-Beschränkungen für PlayReady und Widevine festlegen.
-   Eine Anwendung, die aus Token übergibt als eine sehr vereinfachte Version eines STS sehen.


Konsole-Anwendung:

1.  Ändern der app.config AMS Anmeldeinformationen, CastLabs Anmeldeinformationen und Schlüsseln STS-Konfiguration einrichten.
2.  AMS hochladen Sie Anlage.
3.  Die UUID von hochgeladenen Ressource, und ändern Sie Zeile 32 der Datei Program.cs:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  Verwenden Sie ein Hilfethema für die Benennung der Anlage im System CastLabs (Zeile 44 in der Datei Program.cs).

    Sie müssen Posten-Nr für **CastLabs**festlegen. Es muss eine eindeutige alphanumerische Zeichenfolge.

5.  Führen Sie das Programm.


Die Web-Anwendung (STS) verwenden:

1.  Ändern der Datei web.config Setup Castlabs Händler-ID, die STS-Konfiguration und den gemeinsamen Schlüssel.
2.  Azure Websites bereitstellen.
3.  Navigieren Sie zu der Website.

##<a name="playing-back-a-video"></a>Ein Video abspielen

Wiedergabe eines Videos mit gemeinsamen Verschlüsselung betreiben (PlayReady bzw. Widevine) können Sie den [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Ausführung die Konsole app sind Content Schlüssel-ID und die Manifest-URL zurückgegeben.

1.  Eine neue Registerkarte öffnen und starten Sie Ihr STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2.  Gehen Sie zu [Azure MediaPlayer](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3.  Fügen Sie in die Streaming-URL.
4.  Aktivieren Sie das Kontrollkästchen **Erweiterte Optionen** .
5.  Wählen Sie in der Dropdownliste **Schutz** PlayReady oder Widevine.
6.  Fügen Sie das Token, das vom STS Token Textfeld haben. 
    
    CastLab-Lizenzserver muss nicht die "Inhaber =" Präfix vor dem Token. So entfernen, die vor dem Senden des Tokens.
7.  Aktualisieren des Players.
8.  Das Video soll wiedergegeben werden.


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
