<properties 
    pageTitle="Mit Axinom Widevine Lizenzen Azure Media Services zu | Microsoft Azure" 
    description="Dieser Artikel beschreibt die Verwendung Azure Media Services (AMS) zu dynamisch AMS mit PlayReady und Widevine DRM verschlüsselt Stream. PlayReady-Lizenz stammt aus Media Services PlayReady-Lizenzserver und Widevine Lizenz vom Lizenzserver Axinom geliefert." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Axinom verwenden Widevine Lizenzen Azure Media Services zu  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>Übersicht

Azure Media Services (AMS) hat Google Widevine dynamischen Schutz (Details finden Sie im [Blog des Mingfei](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) ). Azure Media Player (AMP) hat darüber hinaus auch Unterstützung für Widevine (Einzelheiten [AMP Dokument](http://amp.azure.net/libs/amp/latest/docs/) ). Ist eine große Leistung im Strich Streaminginhalte geschützt durch CENC mit multi-native-DRM (PlayReady und Widevine) in modernen Browsern ausgerüstet mit Maus und EME

Beginnend mit Media Services .NET SDK, Version 3.5.2 ermöglicht Media Services konfigurieren Widevine lizenzvorlage und Widevine Lizenzen. Sie können auch die folgenden AMS-Partner gerne Widevine Lizenzen liefern: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [CastLabs](http://castlabs.com/company/partners/azure/).

Dieser Artikel beschreibt, wie zu testen Widevine Lizenzserver vom Axinom verwaltet. Insbesondere umfasst:  

- Dynamische gemeinsame Verschlüsselung konfigurieren mit Multi-DRM (PlayReady und Widevine) mit entsprechenden Lizenz Übernahme URLs;
- Generieren eines JWT-Tokens Erfüllung der License Server;
- Entwicklung Azure Media Player app Lizenzerwerb JWT token Authentifizierung behandelt;

Das komplette System und die Übermittlung von Inhalten Key, Key ID Schlüsselwert JTW Token und seine Ansprüche am besten mit folgendem Diagramm beschrieben werden kann.

![Strich und CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Content-Schutz

Dynamischen Schutz und wichtige Lieferung Richtlinie konfigurieren, finden Sie in Mingfeis Blog: [Widevine Verpackung Azure Media Services konfigurieren](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Dynamischen CENC Schutz können mit mehreren DRM Bindestrich streaming müssen Folgendes konfigurieren:

1. PlayReady Schutz für MS Edge und IE11, die ein token Berechtigung einschränken. Die token eingeschränkte Richtlinie ein Token ausgestellt durch eine Secure Token Service (STS) wie Active Directory Azure beizufügen;
1. Widevine Schutz für Chrome können sie mit einem anderen STS ausgestellten Token token Authentifizierung erfordern. 

[JWT Token Generation](media-services-axinom-integration.md#jwt-token-generation) Siehe Abschnitt Warum Azure Active Directory als ein STS für Axinom Widevine-Lizenzserver verwendet werden kann.

###<a name="considerations"></a>Hinweise

1. Verwenden Sie die angegebenen Axinom Schlüsselwert (8888000000000000000000000000000000000000) und die generierten oder ausgewählte Schlüssel-ID zu Content Schlüssel für wichtige Zustelldienst konfigurieren. Axinom-Lizenzserver ausgestellt alle Lizenzen mit Schlüsseln basierend auf den gleichen Schlüsselwert, für Tests und Produktion.
1. Die Widevine Lizenzerwerbs-URLs für das Testen: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP und HTTS sind zulässig.

##<a name="azure-media-player-preparation"></a>Azure Media Player vorbereiten

EVA v1.4.0 unterstützt die Wiedergabe von AMS-Inhalt, der dynamisch mit PlayReady und Widevine DRM verpackt ist.
Wenn Widevine Lizenzserver keine token Authentifizierung erforderlich ist, ist keine zusätzliche Sie einen Strich Inhalt geschützt durch Widevine testen müssen. Beispielsweise stellt das EVA-Team ein einfaches [Beispiel](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), es Rand und IE11 mit PlayReady mit Widevine finden Sie.
Axinom gemäß Widevine-Lizenzserver ist JWT token Authentifizierung erforderlich. Das JWT-Token muss mit lizenzanforderung über einen HTTP-Header "X-AxDRM-Nachricht". Zu diesem Zweck müssen Sie das folgende Javascript in die Webseite AMP vor der Quelle hinzufügen:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

Der Rest der EVA Code ist standard AMP API wie AMP [hier](http://amp.azure.net/libs/amp/latest/docs/).

Beachten Sie, dass über Javascript für benutzerdefinierte Einstellung Autorisierungsheader noch einen kurzfristigen Ansatz vor dem langfristiger Ansatz EVA freigegeben wird.

##<a name="jwt-token-generation"></a>JWT Token generieren

Axinom Widevine Lizenzserver Testen erfordert JWT token Authentifizierung. Außerdem gehört die Ansprüche im Token JWT der ein komplexes Objekt primitiven Datentyp.

Leider kann Azure AD JWT-Token nur mit primitiven Typen ausstellen. .NET Framework-API (System.IdentityModel.Tokens.SecurityTokenHandler und JwtPayload) können hingegen nur Eingabe komplexer Typ als Ansprüche. Die Ansprüche werden jedoch weiterhin als Zeichenfolge serialisiert. Daher können wir keinen der beiden zum Generieren von JWT-Token Widevine Lizenz Anforderung verwenden.


John Sheehans [JWT Nuget-Paket](https://www.nuget.org/packages/JWT) entspricht damit wir diese Nuget-Paket verwenden.

Im folgenden ist der Code für Generieren von JWT-Token mit der erforderlichen gemäß Axinom Widevine Lizenzserver zum Testen:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

Axinom Widevine-Lizenzserver

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Hinweise

1.  Obwohl AMS PlayReady Lizenz Zustelldienst erfordert "Träger =" vor ein Authentifizierungstoken, Axinom Widevine-Lizenzserver wird nicht verwendet.
2.  Axinom Kommunikation Schlüssel dient als Signaturschlüssel. Beachten Sie, dass der Schlüssel eine hexadezimale Zeichenfolge, aber es als eine Folge von Bytes keine Zeichenfolge behandelt werden muss, bei der Codierung. Dies wird durch die Methode ConvertHexStringToByteArray erreicht.

##<a name="retrieving-key-id"></a>Schlüssel-ID abrufen

Möglicherweise haben Sie bemerkt, dass im Code ein JWT generieren Key token-ID erforderlich ist. Seit der JWT token muss vor laden AMP Player Schlüssel muss ID abgerufen werden, um JWT-Token zu generieren.

Es sind mehrere Varianten zu Schlüssel-ID Beispielsweise kann eine speichern Schlüssel-ID mit Content-Metadaten in einer Datenbank. Oder rufen Sie key ID Strich MPD (Media Präsentation Beschreibung) Datei. Der folgende Code ist für letztere.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Zusammenfassung

Neueste Widevine Unterstützung Inhaltsschutz von Azure Media Services und Azure Media Player können wir implementieren streaming Bindestrich + Multi-native-DRM (PlayReady + Widevine) sowohl PlayReady Lizenz Service AMS und Widevine Lizenzserver aus Axinom die folgenden Browser:

- Chrom
- Microsoft Windows 10 Kante
- IE 11 unter Windows 8.1 und Windows 10
- Silverlight und dieselbe URL Azure Media Player werden (Desktop) Firefox und Safari auf einem Mac (nicht iOS) ebenfalls unterstützt.

Die folgenden Parameter sind Mini-Lösung mit Axinom Widevine-Lizenzserver erforderlich. Außer Schlüssel-ID, die restlichen Parameter bereitgestellt Axinom basierend auf ihrer Widevine Server-Setup.


Parameter|Verwendung
---|---
Kommunikation Schlüssel-ID|Als Wert der Forderung "Com_key_id" im JWT-Token enthalten sein (siehe [hierzu](media-services-axinom-integration.md#jwt-token-generation) ).
Schlüssel-Kommunikation|Signaturschlüssel JWT Token verwendet werden (siehe [hierzu](media-services-axinom-integration.md#jwt-token-generation) ).
Schlüsselwert|Muss verwendet werden, um Inhalt Schlüssel mit bestimmten Inhalten Schlüssel-ID (siehe [hierzu](media-services-axinom-integration.md#content-protection) ).
Widevine Lizenzerwerbs-URLs|Muss verwendet werden, Konfigurieren der Anlage Lieferung Richtlinie Bindestrich streaming [(Siehe hierzu)](media-services-axinom-integration.md#content-protection) .
Inhalt Schlüssel-ID|Bestandteil Anspruch Nachricht Forderung JWT Token werden (siehe [hierzu](media-services-axinom-integration.md#jwt-token-generation) ). 


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Danksagungen 

Wir möchten die folgenden Personen, die dieses Dokument beigetragen bestätigen: Kristjan Jõgi von Axinom, Mingfei Yan und Amit Rajput.
