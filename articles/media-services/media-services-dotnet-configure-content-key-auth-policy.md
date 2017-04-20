<properties 
    pageTitle="Inhalt wichtige Autorisierungsrichtlinie mit Media Services .NET SDK konfigurieren | Microsoft Azure" 
    description="Informationen Sie zum Konfigurieren von Autorisierungsrichtlinien für eine Inhaltsschlüssel mit Media Services .NET SDK." 
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
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dynamische Verschlüsselung: Content Schlüssel Autorisierungsrichtlinie konfigurieren

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>Übersicht

Microsoft Azure Media Services können Sie zu MPEG-DASH Smooth Streaming und HTTP-Live-Streaming (HLS) Streams mit Advanced Encryption Standard (AES) (mit 128-Bit-Verschlüsselungsschlüssel) oder [Microsoft PlayReady-DRM](https://www.microsoft.com/playready/overview/)geschützt. AMS können Sie Bindestrich Streams mit Widevine DRM verschlüsselt übermittelt. PlayReady und Widevine werden gemäß der Spezifikation allgemeine Verschlüsselung (ISO/IEC 23001 7 CENC) verschlüsselt.

Media Services stellt einen **Schlüssel-Lizenz Übermittlungsdienst** aus dem Kunden AES-Schlüssel oder PlayReady-Widevine Lizenzen zur Wiedergabe verschlüsselten Inhalts abrufen können.

Für Media Services Vermögenswert verschlüsseln sollten, Sie einen Verschlüsselungsschlüssel (**CommonEncryption** oder **EnvelopeEncryption**) mit Anlage (Beschreibung [hier](media-services-dotnet-create-contentkey.md)) und auch Autorisierungsrichtlinien für den Schlüssel (wie in diesem Artikel beschrieben) konfigurieren.

Wenn ein Spieler ein Stream angefordert wird, verwendet Media Services angegebenen Schlüssel dynamisch Content mit AES oder DRM-Verschlüsselung verschlüsselt. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel Schlüssel Paketdienstes. Entscheidung, ob der Benutzer berechtigt ist, den Schlüssel, wertet der Dienst die Autorisierungsrichtlinien für den Schlüssel angegeben.

Media Services unterstützt mehrere Methoden für die Authentifizierung von Benutzern Schlüssel anfordern. Inhalt wichtige Autorisierungsrichtlinie könnte gelten mindestens Autorisierung: **Öffnen** oder **token** Beschränkung. Die token eingeschränkte Richtlinie ein Token ausgestellt durch eine Secure Token Service (STS) beizufügen. Media Services unterstützt Token im die **Simple Web Token** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) Format und **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)).

Media Services bietet keine sichere Token-Services. Erstellen eines benutzerdefinierten STS oder Microsoft Azure ACS Problem Tokens nutzen. Der STS müssen konfiguriert werden, erstellen Sie einen Token mit den angegebenen Schlüssel und Thema, die in der Konfiguration token Einschränkung angegeben (wie in diesem Artikel beschrieben) signiert. Schlüssel Zustelldienst Media Services wird den Schlüssel an den Client zurückgegeben, wenn das Token gültig ist und die Ansprüche im Token für Content Schlüssel konfiguriert übereinstimmen.

Weitere Informationen finden Sie unter

[JWT token authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integration von Azure Media Services owin-MVC Anwendung in Azure Active Directory basieren und Bereitstellung von Inhalten basierend auf JWT Ansprüche beschränken](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Azure ACS Problem Token verwenden](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Einige zu berücksichtigen:

- Um dynamische Verpackung und dynamische Verschlüsselung verwenden zu können, müssen Sie mindestens eine reservierte Einheit streaming sicherstellen. Weitere Informationen finden Sie unter [wie Media Service](media-services-portal-manage-streaming-endpoints.md).
- Die Anlage muss eine Reihe von adaptiven Bitrate MP4s oder adaptive Bitrate Smooth Streaming-Dateien enthalten. Weitere Informationen finden Sie unter [Codieren einer Anlage](media-services-encode-asset.md).
- Hochladen Sie und kodieren Sie Datenbestände mit **AssetCreationOptions.StorageEncrypted** Option.
- Möchten Sie mehreren Schlüsseln haben, die die gleichen Richtlinienkonfiguration benötigen, wird empfohlen zu einzelnen Autorisierungsrichtlinie mit mehreren Schlüsseln wiederverwenden.
- Der Zustelldienst Schlüssel speichert ContentKeyAuthorizationPolicy und seine zugehörigen Objekte (Optionen und Einschränkungen) für 15 Minuten.  Wenn ein ContentKeyAuthorizationPolicy erstellen und angeben, dass eine "Token" Einschränkung verwendet testen, aktualisieren Sie die Richtlinie auf "Öffnen" Einschränkung und dauert ca. 15 Minuten, bevor die Richtlinie auf "Öffnen" Version der Richtlinie wechselt.
- Hinzufügen oder Aktualisieren des Vermögenswertes Lieferung Richtlinie müssen Sie vorhandenen Locator (falls vorhanden) löschen und neuen Locator erstellen.
- Derzeit HDS streaming-Format können nicht verschlüsselt oder "Progressiv".


##<a name="aes-128-dynamic-encryption"></a>Dynamische Verschlüsselung AES-128

###<a name="open-restriction"></a>Open-Einschränkung

Open-Einschränkung bedeutet, dass das System den Schlüssel jeder liefern macht eine Anforderung. Diese Einschränkung kann zu Testzwecken nützlich sein.

Im folgende Beispiel erstellt eine Autorisierungsrichtlinie öffnen und Inhalt Schlüssel hinzugefügt.

static public void AddOpenAuthorizationPolicy (IContentKey ContentKey) {/ / ContentKeyAuthorizationPolicy mit offenen erstellen / / und Autorisierungsrichtlinie IContentKeyAuthorizationPolicy = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("offene Autorisierung Policy"). Ergebnis.

Liste<ContentKeyAuthorizationPolicyRestriction> Einschränkungen neue Liste =<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };
    
        restrictions.Add(restriction);
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


###<a name="token-restriction"></a>Token Einschränkung

Dieser Abschnitt beschreibt die Content Schlüssel Autorisierungsrichtlinie erstellen und Inhalte Schlüssel zuordnen. Die Autorisierungsrichtlinie beschreibt, welche Autorisierung erfüllt sein müssen, um festzustellen, ob der Benutzer berechtigt ist, den Schlüssel (z. B. wird die Liste "Überprüfung" enthalten Schlüssel, dem Token signiert wurde).

Konfigurieren Sie die Option token Einschränkung müssen Sie XML verwenden, um das Token Autorisierung beschreiben. Token Beschränkung Konfigurations-XML muss das folgende XML-Schema entsprechen.

####<a id="schema"></a>Token Beschränkung schema
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Beim Konfigurieren von **token** Richtlinie beschränkt, müssen Sie die primäre **** Überprüfung Schlüssel**, **Aussteller** und** Parameter angeben. **Primäre Überprüfung Schlüssel **enthält Schlüssel, dem Token signiert wurde, **Aussteller** des Sicherheitstokendiensts, die das Token ausstellt. Die **Zielgruppe** ( **Bereich**bezeichnet) beschreibt die Absicht des Tokens oder Ressource autorisiert das Token auf. Wichtige Zustelldienst Media Services überprüft, dass diese Werte im Token die Werte in der Vorlage übereinstimmen. 

Bei **Media Services SDK für .NET**können Sie die **TokenRestrictionTemplate** -Klasse, Einschränkung Token generiert.
Das folgende Beispiel erstellt eine Autorisierungsrichtlinie mit token. In diesem Beispiel müsste der Client an enthält ein Token: Signaturschlüssel (VerificationKey), ein Tokenherausgebers, und erforderliche Ansprüche.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();
    
        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };
    
        restrictions.Add(restriction);
    
        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );
    
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

####<a id="test"></a>Testtoken

Zu einem testtoken basierend auf die token Beschränkung für die wichtigsten Autorisierungsrichtlinie verwendet wurde folgendermaßen Sie vor.
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>Dynamische PlayReady-Verschlüsselung 

Media Services können Sie konfigurieren, Rechte und beschränkt die PlayReady-DRM Runtime zu erzwingen, wenn ein Benutzer versucht, geschützten Inhalte wiedergeben. 

Schutz der Inhalte mit PlayReady ist eines der Dinge, die Sie in Ihre Autorisierungsrichtlinie angeben einer XML-Zeichenfolge, die die [PlayReady lizenzvorlage](media-services-playready-license-template-overview.md)definiert. Media Services SDK für .NET hilft Klassen **PlayReadyLicenseResponseTemplate** und **PlayReadyLicenseTemplate** der PlayReady-Lizenzvorlage definieren.

[In diesem Thema](media-services-protect-with-drm.md) veranschaulicht, wie Ihre Inhalte **Widevine**mit **PlayReady** verschlüsseln.

###<a name="open-restriction"></a>Open-Einschränkung
    
Open-Einschränkung bedeutet, dass das System den Schlüssel jeder liefern macht eine Anforderung. Diese Einschränkung kann zu Testzwecken nützlich sein.

Im folgende Beispiel erstellt eine Autorisierungsrichtlinie öffnen und Inhalt Schlüssel hinzugefügt.

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
    
        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Token Einschränkung

Konfigurieren Sie die Option token Einschränkung müssen Sie XML verwenden, um das Token Autorisierung beschreiben. Token Beschränkung Konfigurations-XML muss in [diesem](#schema) Abschnitt angezeigten XML-Schemas entsprechen.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    
        return tokenTemplateString;
    }
    
    static private string GenerateTokenRequirements()
    {
    
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
    
        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
    
    
        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
    
        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 
    
    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
       
        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


[Zu ein testtoken basierend auf token Beschränkung die wichtigsten Autorisierung verwendete Richtlinie Abschnitt.](#test) 

##<a id="types"></a>Beim Definieren von ContentKeyAuthorizationPolicy verwendet

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Nächstes
Jetzt gehen Sie Autorisierungsrichtlinie Content Schlüssel konfiguriert haben, zum Thema [Anlage Lieferung Richtlinie konfigurieren](media-services-dotnet-configure-asset-delivery-policy.md) .
 
