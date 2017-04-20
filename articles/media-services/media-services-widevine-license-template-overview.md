<properties 
    pageTitle="Widevine Vorlage Lizenzübersicht | Microsoft Azure" 
    description="Dieses Thema bietet eine Übersicht über Lizenz Widevine-Vorlage mit Widevine Lizenzen konfiguriert." 
    authors="juliako" 
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

#<a name="widevine-license-template-overview"></a>Übersicht über Vorlagen von Widevine-Lizenz

##<a name="overview"></a>Übersicht

Azure Media Services können jetzt konfigurieren und Widevine Lizenzen anfordern. Wenn Endbenutzer Player versucht, Ihre Widevine geschützte Inhalte wiederzugeben, wird eine Anforderung an den Übermittlungsdienst Lizenz eine Lizenz gesendet. Wenn Lizenz-Dienst die Anforderung genehmigt, stellt er die Lizenz an den Client gesendet und entschlüsselt und Wiedergeben von Inhalt verwendet werden.

Widevine lizenzanforderung wird als JSON-Nachricht formatiert.  

Beachten Sie, dass Sie können eine leere Nachricht ohne Werte erstellen nur "{}" lizenzvorlage mit allen Standardeinstellungen erstellt.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON-Nachricht

Name | Wert | Beschreibung
---|---|---
Nutzlast |Base64-codierte Zeichenfolge |Die lizenzanforderung von einem Client gesendet. 
content_id | Base64-codierte Zeichenfolge|Bezeichner für jede content_key_specs.track_type KeyId(s) und Content-Schlüssel abgeleitet.
Anbieter |Zeichenfolge |Zum Suchen von Schlüsseln und Richtlinien verwendet. Erforderlich.
policy_name | Zeichenfolge |Name der zuvor registrierte Richtlinie. Optionale
allowed_track_types | Enum  | SD_ONLY oder SD_HD. Die content-Schlüssel sollte eine Lizenz sind Kontrollen
content_key_specs | Array von JSON Strukturen finden Sie unter **Content Schlüssel Daten** unten | Eine feinere Abstimmung Steuerelement Inhalte Tasten zurück. Einzelheiten finden Sie unter Content Schlüssel Spec unten.  Nur einer der Allowed_track_types und Content_key_specs kann angegeben werden. 
use_policy_overrides_exclusively | boolescher Wert. WAHR oder falsch | Attribute der Verwendung durch Policy_overrides und alle zuvor gespeicherten Richtlinien weglassen.
policy_overrides | JSON-Struktur, siehe unten **Richtlinie überschreibt** | Richtlinien für diese Lizenz.  Bei dieser Anlage eine vordefinierte Richtlinie verfügt, werden diese angegebenen Werte verwendet. 
session_init | JSON-Struktur, siehe **Sitzung Initialisierung** unten | Optionale Daten übergeben Lizenz.
parse_only | boolescher Wert. WAHR oder falsch | Die Lizenzabfrage wird jedoch keine Lizenz ausgestellt. Allerdings Werte Formular die Lizenzabfrage in der Antwort zurückgegeben.  

##<a name="content-key-specs"></a>Schlüssel-Technische Daten 

Wenn eine vorhandene Richtlinie vorhanden ist Werte Content Key-Spezifikation angegeben.  Dieser Inhalt zugeordnete vorhandene Richtlinie verwendet Ausgabeschutz HDCP wie CGMS bestimmen.  Wenn eine vorhandene Richtlinie nicht mit dem Widevine-Lizenzserver registriert ist, kann Inhaltsanbieter die Werte in die Lizenzabfrage einfügen.   


Jedes Content_key_specs muss für alle Spuren, unabhängig von der Option Use_policy_overrides_exclusively angegeben werden. 


Name | Wert | Beschreibung
---|---|---
Content_key_specs. track_type | Zeichenfolge | Ein Typname verfolgen. Sollten Sie Content_key_specs in die Lizenzabfrage angegeben ist, an, dass alle explizit nachzuverfolgen. Andernfalls führt Fehler Wiedergabe letzten 10 Sekunden. 
content_key_specs  <br/> security_level | UInt32 | Stabilität der Clientanforderungen für die Wiedergabe definiert. <br/> 1 - softwarebasierte Whitebox-Verschlüsselung ist erforderlich. <br/> 2 - Software-Verschlüsselung und eine verschleierte Decoder ist erforderlich. <br/> 3 - Schlüssel und kryptographische Vorgänge müssen in einer gesicherten vertrauenswürdige Ausführung Hardware-Umgebung ausgeführt werden. <br/> 4-Verschlüsselung und Entschlüsselung der Inhalte müssen innerhalb einer gesicherten vertrauenswürdige Ausführung Hardware-Umgebung ausgeführt werden.  <br/> 5-Crypto, Decodierung und alle Versand von Medien (komprimiert und dekomprimiert) müssen innerhalb einer Umgebung mit Hardware unterstützt vertrauenswürdige Ausführung behandelt werden.  
content_key_specs <br/> required_output_protection.hdc | String - eines: HDCP_NONE, HDCP_V1, HDCP_V2 | Gibt an, ob HDCP erforderlich
content_key_specs <br/>Schlüssel | Base64 <br/>codierte Zeichenfolge|Inhalt Schlüssel für diese Spur. Wenn angegeben, ist die Track_type oder Key_id erforderlich.  Mit dieser Option können Inhaltsanbieter einfügen Inhaltsschlüssel zu diesem Titel anstatt Widevine Lizenzserver generieren oder einen Schlüssel zu suchen.
content_key_specs.key_ID| Base64-codierte Zeichenfolge binary 16 Byte | Eindeutiger Bezeichner für den Schlüssel. 


##<a name="policy-overrides"></a>Außer Kraft setzt 

Name | Wert | Beschreibung
---|---|---
Policy_overrides. can_play | boolescher Wert. WAHR oder falsch | Gibt die Wiedergabe des Inhalts ist zulässig. Standardwert ist false.
Policy_overrides. can_persist | boolescher Wert. WAHR oder falsch |Gibt an, dass die Lizenz nicht flüchtigen Speicher für offline-Verwendung beibehalten werden kann. Standardwert ist false.
Policy_overrides. can_renew | Boolean True oder false |Gibt an, dass die Verlängerung der Lizenz zulässig ist. Wenn true, kann die Lizenz Takt verlängert. Standardwert ist false. 
Policy_overrides. license_duration_seconds | Int64 | Gibt das Zeitfenster für diese bestimmte Lizenz an. Der Wert 0 gibt an, dass es keine Begrenzung der Dauer gibt. Standardwert ist 0. 
Policy_overrides. rental_duration_seconds | Int64 | Gibt das Fenster während der Wiedergabe zulässig ist. Der Wert 0 gibt an, dass es keine Begrenzung der Dauer gibt. Standardwert ist 0. 
Policy_overrides. playback_duration_seconds | Int64 | Fenster anzeigen einmal innerhalb der Lizenz die Wiedergabe beginnt. Der Wert 0 gibt an, dass es keine Begrenzung der Dauer gibt. Standardwert ist 0. 
Policy_overrides. renewal_server_url |Zeichenfolge | Alle Heartbeat (Erneuerung) Anfragen für diese Lizenz werden zum angegebenen URL weitergeleitet. Dieses Feld wird nur verwendet, wenn Can_renew true ist.
Policy_overrides. renewal_delay_seconds |Int64 |Wie viele Sekunden nach License_start_time vor der Erneuerung zuerst. Dieses Feld wird nur verwendet, wenn Can_renew true ist. Standardwert ist 0 
Policy_overrides. renewal_retry_interval_seconds | Int64 | Gibt die Verzögerung in Sekunden zwischen den nachfolgenden Lizenz Verlängerungsanträge Ausfall. Dieses Feld wird nur verwendet, wenn Can_renew true ist. 
Policy_overrides. renewal_recovery_duration_seconds | Int64 | Das Zeitfenster, in der Wiedergabe darf Erneuerung weiter ist versuchte noch Backend-Probleme mit dem Lizenzserver fehlgeschlagen. Der Wert 0 gibt an, dass es keine Begrenzung der Dauer gibt. Dieses Feld wird nur verwendet, wenn Can_renew true ist.
Policy_overrides. renew_with_usage | Boolean True oder false |Gibt an, dass die Lizenz für Verlängerung übermittelt Verwendung begann. Dieses Feld wird nur verwendet, wenn Can_renew true ist. 

##<a name="session-initialization"></a>Sitzung-Initialisierung

Name | Wert | Beschreibung
---|---|---
provider_session_token | Base64-codierte Zeichenfolge |Diese Sitzungstoken in der Lizenz übergeben und in nachfolgenden Erneuerung besteht.  Das Sitzungstoken bleiben nicht über Sessions. 
provider_client_token | Base64-codierte Zeichenfolge | Clienttoken in der Lizenzantwort senden.  Wenn die Lizenzabfrage Clienttoken enthält, wird dieser Wert ignoriert. Das Clienttoken bleiben über Lizenz Sessions.
override_provider_client_token | boolescher Wert. WAHR oder falsch |False und die Lizenzabfrage enthält eine Clienttoken, das Token aus der Anforderung auch wenn verwenden Sie Clienttoken in dieser Struktur angegeben wurde.  Wenn true, immer verwenden Sie in dieser Struktur angegebene Token.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Konfigurieren Sie Ihre Widevine Lizenzen mit .NET

Media Services stellt .NET APIs, mit denen Sie Ihre Lizenzen Widevine konfigurieren können. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klassen im Media Services .NET SDK

Es folgen die Definitionen dieser Typen.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Beispiel

Das folgende Beispiel veranschaulicht das .NET APIs verwenden, um eine einfache Widevine Lizenz konfigurieren.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Siehe auch

[PlayReady und Widevine dynamische Verschlüsselung gemeinsame](media-services-protect-with-drm.md)
