<properties
    pageTitle="Verschlüsselung PlayReady bzw. Widevine dynamische gemeinsame | Microsoft Azure"
    description="Microsoft Azure Media Services können Sie zu MPEG-DASH Smooth Streaming und Http-Live-Streaming (HLS) Streams mit Microsoft PlayReady-DRM geschützt. Sie können auch zur Lieferung Strich mit Widevine DRM verschlüsselt. Dieses Thema zeigt, wie dynamisch Widevine DRM mit PlayReady verschlüsseln."
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
    ms.topic="get-started-article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>PlayReady und Widevine Verschlüsselung dynamische gemeinsame

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Microsoft Azure Media Services können Sie zu MPEG-DASH Smooth Streaming und HTTP-Live-Streaming (HLS) Streams mit [Microsoft PlayReady-DRM](https://www.microsoft.com/playready/overview/)geschützt. Sie können verschlüsselte Datenströme Strich mit Widevine DRM Lizenzen liefern. PlayReady und Widevine werden gemäß der Spezifikation allgemeine Verschlüsselung (ISO/IEC 23001 7 CENC) verschlüsselt. [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (ab Version 3.5.1) oder REST-API können Sie Ihre AssetDeliveryConfiguration Widevine Verwendung konfigurieren.

Media Services bietet für PlayReady und Widevine DRM Lizenzen. Media Services bietet APIs, mit denen Sie das Rechte und beschränkt die PlayReady oder Widevine DRM Runtime zu erzwingen, wenn ein Benutzer wiedergegeben Inhalt geschützt. Wenn ein Benutzer eine DRM-geschützte Inhalte anfordert, wird die Player-Anwendung eine Lizenz AMS-Lizenz-Service anfordern. AMS-Lizenzdienst wird der Player eine Lizenz ausstellen, wenn er berechtigt ist. PlayReady oder Widevine Lizenz enthält den Entschlüsselungsschlüssel vom Client Player entschlüsseln und der Inhalte verwendet werden.


Sie können auch die folgenden AMS-Partner gerne Widevine Lizenzen liefern: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [CastLabs](http://castlabs.com/company/partners/azure/). Weitere Informationen finden Sie unter: Integration mit [Axinom](media-services-axinom-integration.md) und [CastLabs](media-services-castlabs-integration.md).

Media Services unterstützt mehrere Methoden autorisieren Benutzer Schlüssel anfordern. Inhalt wichtige Autorisierungsrichtlinie könnte gelten mindestens Autorisierung: Öffnen oder Einschränkung token. Die token eingeschränkte Richtlinie ein Token ausgestellt durch eine Secure Token Service (STS) beizufügen. Media Services unterstützt Token im die [Simple Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) Format und [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Weitere Informationen finden Sie unter Konfigurieren der Inhaltsschlüssel Autorisierungsrichtlinie.

Um dynamische Verschlüsselung nutzen zu können, müssen Sie eine Anlage haben, die eine Reihe von Multi-Bitrate MP4-Dateien oder Multi-Bitrate Smooth Streaming-Quelldateien enthält. Sie müssen auch die Richtlinien für die Anlage (weiter unten in diesem Thema beschrieben). Dann sorgen basierend auf Streaming-URL angegebenen Format, On-Demand-Streamingserver Stream im Protokoll übermittelt gewählte. Daher müssen nur speichern und Dateien in einem einzigen Speicherformat bezahlen und Media Services erstellt und dienen die entsprechende HTTP-Antwort basierend auf jede Anforderung von einem Client.

In diesem Thema nützlich für Entwickler, die auf Applikationen, die Medien mit mehreren DRM PlayReady wie Widevine geschützt. Das Thema veranschaulicht der PlayReady-Lizenz Zustelldienst mit Autorisierungsrichtlinien so konfigurieren, dass nur autorisierte Clients PlayReady oder Widevine Lizenzen erhalten konnte. Außerdem wird das dynamische Verschlüsselung mit PlayReady oder Widevine DRM über Strich Verschlüsselung veranschaulicht.

>[AZURE.NOTE]Um dynamische Verschlüsselung zu starten, müssen Sie zunächst mindestens eine Skalierungseinheit (auch bekannt als Streaming-Einheit) abrufen. Weitere Informationen finden Sie unter [wie Media Service](media-services-portal-manage-streaming-endpoints.md).


##<a name="download-sample"></a>Beispiel herunterladen

Sie können das [hier](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm)beschriebene Beispiel.

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Dynamische gemeinsame Verschlüsselung und DRM-Lizenz Delivery Services konfigurieren

Es folgen allgemeine Schritte, die Sie beim Schutz Ihrer Ressourcen mit PlayReady mithilfe der Media Services Lizenz Zustelldienst sowie auch dynamische Verschlüsselung durchführen.

1. Eine Anlage erstellen und Hochladen von Dateien in der Anlage.
1. Codieren Sie die Anlage mit der adaptiven Bitrate festgelegten MP4-Datei.
1. Erstellen Sie einer Content und codierte Anlage zuordnen. Media Services enthält den Inhalt der Anlage Verschlüsselungsschlüssel.
1. Konfigurieren der Inhaltsschlüssel Autorisierungsrichtlinie. Inhalt wichtige Autorisierungsrichtlinie müssen Sie konfiguriert und vom Client an den Client übermittelt werden in Reihenfolge für den Content Schlüssel erfüllt.

Beim Content Schlüssel Autorisierungsrichtlinie erstellen, müssen Sie Folgendes angeben: Methode PlayReady oder Widevine, Lieferbeschränkungen (offen oder token) und Informationen zum Schlüssel Lieferart, die definiert, wie der Schlüssel an den Client ([PlayReady](media-services-playready-license-template-overview.md) oder [Widevine](media-services-widevine-license-template-overview.md) lizenzvorlage) gesendet wird.
1. Konfigurieren der Richtlinie Übermittlung einer Anlage. Konfiguration der Bereitstellung umfasst: Übermittlungsprotokoll (z. B. MPEG DASH, HLS, HDS, Smooth Streaming oder alle), die dynamische Verschlüsselung (z. B. gemeinsame Verschlüsselung) PlayReady oder Widevine Lizenzerwerbs-URLs.

Sie können jedes Protokoll auf dem gleichen unterschiedliche Richtlinien anwenden. Sie können z. B. PlayReady-Verschlüsselung AES Umschlag HLS zu glätten-Strich anwenden. Alle Protokolle, die nicht in einer Lieferung definiert sind (z. B. hinzufügen eine einzelne Richtlinie, nur HLS als Protokoll) von streaming blockiert werden. Die Ausnahme ist keine Anlage Lieferung Richtlinie überhaupt definiert haben. Dann werden alle Protokolle in Klartext zulässig.
1. Erstellen Sie einen Locator OnDemand erzielen eine Streaming-URL.

Ein vollständiges Beispiel für .NET am Ende des Themas finden.

Die folgende Abbildung zeigt den oben beschriebenen Workflow. Hier wird das Token zur Authentifizierung verwendet.

![Mit PlayReady schützen](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Im weiteren Verlauf dieses Themas enthält eine ausführliche Erklärung, Codebeispiele und Links zu Themen, die zeigen, wie Sie die oben beschriebenen Aufgaben.

##<a name="current-limitations"></a>Aktuelle Grenzen

Hinzufügen oder aktualisieren eine Anlage Delivery-Richtlinie müssen Sie den hinzugefügten Locator (falls vorhanden) löschen und neuen Locator erstellen.

Beschränkung beim Widevine mit Azure Media Services verschlüsseln: mehreren Schlüsseln werden derzeit nicht unterstützt.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Eine Anlage erstellen und Hochladen von Dateien in der Anlage

Verwalten, codieren und Streaming-Videos, müssen Sie zuerst den Inhalt in Microsoft Azure Media Services hochladen. Nachdem hochgeladen, wird Ihr Inhalt in der Cloud für Weiterverarbeitung und streaming sicher.

Ausführliche Informationen finden Sie unter [Dateien in Media Services-Konto hochladen](media-services-dotnet-upload-files.md).

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Codieren Sie die Anlage mit der adaptiven Bitrate festgelegten MP4-Datei

Dynamische Verschlüsselung benötigen Sie ist eine Anlage erstellen, die eine Reihe von Multi-Bitrate MP4-Dateien oder Multi-Bitrate Smooth Streaming-Quelldateien enthält. Dann wird basierend auf dem angegebenen Format in das Manifest und Fragment-Anforderung, On-Demand-Streaming-Server garantieren Sie den Stream im Protokoll gewählte. Daher müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen und Media-Dienste erstellen und die entsprechende Antwort auf Anfragen von einem Client verwendet. Weitere Informationen finden Sie im Thema [dynamische Verpackung](media-services-dynamic-packaging-overview.md) .

Informationen zum Codieren finden Sie unter [Codieren eine Anlage mit Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).


##<a id="create_contentkey"></a>Erstellen Sie einer Content und codierte Anlage zuordnen

Media Services enthält den Inhalt den Schlüssel, dem Sie eine Anlage verschlüsseln möchten.

Ausführliche Informationen finden Sie unter [Schlüssel erstellen](media-services-dotnet-create-contentkey.md).


##<a id="configure_key_auth_policy"></a>Der Inhaltsschlüssel Autorisierungsrichtlinie konfigurieren

Media Services unterstützt mehrere Methoden für die Authentifizierung von Benutzern Schlüssel anfordern. Inhalt wichtige Autorisierungsrichtlinie müssen Sie konfiguriert und erfüllt vom Client (Player) den Schlüssel an den Client übermittelt werden. Inhalt wichtige Autorisierungsrichtlinie könnte gelten mindestens Autorisierung: Öffnen oder Einschränkung token.

Ausführliche Informationen finden Sie unter [Content Schlüssel Autorisierungsrichtlinie konfigurieren](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

##<a id="configure_asset_delivery_policy"></a>Anlage Lieferung Richtlinie konfigurieren 

Konfigurieren der Bereitstellung Richtlinie für die Anlage. Einige Dinge, die Konfiguration der Anlage Lieferung enthält:

- Die DRM-Lizenz Übernahme URL. 
- Das Übermittlungsprotokoll Anlage, (z. B. MPEG DASH, HLS, HDS, Smooth Streaming oder alle). 
- Der Typ der dynamischen Verschlüsselung (in diesem Fall häufig Verschlüsselung). 

Ausführliche Informationen finden Sie in [Anlage Delivery-Richtlinie konfigurieren ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Erstellen Sie eine streaming-Locator um eine Streaming-URL erhalten OnDemand

Sie müssen Benutzer mit streaming URL glatt, Bindestrich oder HLS vorzusehen.

>[AZURE.NOTE]Hinzufügen oder Aktualisieren des Vermögenswertes Lieferung Richtlinie müssen Sie vorhandenen Locator (falls vorhanden) löschen und neuen Locator erstellen.

Informationen zu veröffentlichen einer Anlage eine Streaming-URL finden Sie unter [erstellen eine Streaming-URL](media-services-deliver-streaming-content.md).

##<a name="get-a-test-token"></a>Einen Test token abrufen

Erhalten Sie einen Test token basierend auf die token Beschränkung für die wichtigsten Autorisierungsrichtlinie verwendet wurde.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

    
[AMS-Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) können Sie Ihre Stream testen.

##<a id="example"></a>Beispiel


Das folgende Beispiel veranschaulicht in Azure Media Services SDK für .net eingeführte Funktionalität-Version 3.5.2 (insbesondere die Möglichkeit, eine Widevine lizenzvorlage definieren und Azure Media Services Widevine Lizenz anfordern). Nuget-Paket Befehl wurde verwendet, um das Paket zu installieren:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Erstellen Sie ein neues Konsolenprojekt.
1. Verwenden Sie NuGet zum Installieren und Hinzufügen von Azure Media Services .NET SDK.
2. Fügen Sie zusätzliche Verweise: System.Configuration.
2. Fügen Sie Config-Datei mit dem Kontonamen und wichtige Informationen hinzu:
    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. Rufen Sie mindestens eine Streaming-Einheit für Streaming-Endpunkt aus dem Lieferung des Inhalts werden soll. Weitere Informationen finden Sie unter: [streaming Endpunkte konfigurieren](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. Überschrieben Sie den Code in der Datei Program.cs mit dem Code in diesem Abschnitt dargestellt.
    
    Stellen Sie sicher aktualisieren Variablen Ordner zeigen, wo sich Ihre Dateien befinden.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey, 
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
                    Console.ReadLine();
                }
        
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
            
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
            
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
            
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
            
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
            
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
            
                    return job.OutputMediaAssets[0];
                }
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
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
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    // Get the PlayReady license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
                
                    // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
                    // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
                    // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
                    // to append /? KID =< keyId > to the end of the url when creating the manifest.
                    // As a result Widevine license acquisition URL will have KID appended twice, 
                    // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.
            
                    Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
                    UriBuilder uriBuilder = new UriBuilder(widevineUrl);
                    uriBuilder.Query = String.Empty;
                    widevineUrl = uriBuilder.Uri;
        
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);
        
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


## <a name="next-step"></a>Nächstes

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Siehe auch

[CENC mit mehreren DRM und Zugriffskontrolle](media-services-cenc-with-multidrm-access-control.md)

[Widevine Verpackung mit AMS konfigurieren](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Ankündigung von Google Widevine Lizenz Lieferdienste in Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
