<properties 
    pageTitle="Schützen Sie Ihre HLS mit Apple FairPlay bzw. Microsoft PlayReady | Microsoft Azure" 
    description="Dieses Thema bietet eine Übersicht über und zeigt, wie Azure Media Services dynamisch Ihre HTTP-Live-Streaming (HLS) Inhalte mit Apple FairPlay verschlüsseln. Es wird gezeigt, wie der Media Services Lizenz nutzen FairPlay Lizenzen an Clients übermittelt." 
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

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>Schützen Sie Ihre mit Apple FairPlay bzw. Microsoft PlayReady HLS

Azure Media Services können Sie dynamisch verschlüsseln Content HTTP-Live-Streaming (HLS) mit den folgenden Formaten:  

- **AES-128 Umschlag clear-Taste** 

    Der gesamte Textbaustein mit **AES-128 CBC** -Modus verschlüsselt. Die Entschlüsselung des Streams iOS und OSX Player nativ unterstützt. Weitere Informationen finden Sie in [diesem Artikel](media-services-protect-with-aes128.md).

- **Apple FairPlay** 

    Die einzelnen Video- und Beispiele werden mit **AES-128 CBC** -Modus verschlüsselt. **Streaming-FairPlay** Betriebssysteme Geräte mit Unterstützung für iOS und Apple TV (FPS) integriert. Safari unter Mac OS kann verschlüsselte Media Extensions (EME) Schnittstelle Unterstützung FPS.
- **Microsoft PlayReady**

Die folgende Abbildung veranschaulicht den Workflow **dynamic HLS + FairPlay oder PlayReady-Verschlüsselung** .

![Schützen Sie mit FairPlay](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

In diesem Thema veranschaulicht, wie Azure Media Services dynamisch Ihre Inhalte HLS Apple FairPlay verschlüsseln. Es wird gezeigt, wie der Media Services Lizenz nutzen FairPlay Lizenzen an Clients übermittelt.

>[AZURE.NOTE] Wenn Sie Ihre HLS Inhalte mit PlayReady verschlüsseln möchten, müssen Sie eine gemeinsame Content erstellen und die Anlage zugeordnet. Sie müssen auch die Inhaltsschlüssel Autorisierungsrichtlinie wie [dynamische gemeinsame Verschlüsselung mithilfe von PlayReady](media-services-protect-with-drm.md) beschrieben.

    
## <a name="requirements-and-considerations"></a>Vorschriften und Hinweise

- Folgendes ist erforderlich wenn AMS HLS verschlüsselt mit FairPlay und FairPlay Lizenzen zu verwenden.

    - Ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](/pricing/free-trial/?WT.mc_id=A261C142F).
    - Media Services-Konto. Zum Erstellen eines Kontos Media Services finden Sie unter [Konto erstellen](media-services-portal-create-account.md).
    - Melden Sie sich mit [Apple Entwicklungsprogramm](https://developer.apple.com/).
    - Apple ist Besitzer des Inhalts zu [Bereitstellungspaket](https://developer.apple.com/contact/fps/)erforderlich. Geben Sie die Anforderung, dass Sie bereits KSM (Key-Sicherheitsmodul) mit Azure Media Services und Anfordern von FPS-Paket. Anleitung in das Paket FPS Zertifizierung und Fragen, die Sie verwenden sollen, FairPlay erhalten werden. 

    - Azure Media Services .NET SDK Version **3.6.0** oder höher.

- Auf wichtige Lieferung AMS müssen Folgendes festgelegt werden:
    - **App-Cert (Netzbetrieb)** - PFX-Datei, die privaten Schlüssel enthält. Diese Datei wird vom Kunden und Kunden mit einem Kennwort verschlüsselt. 
        
        Wenn der Kunde wichtige Lieferung Richtlinie konfiguriert, müssen sie das Kennwort und PFX im base64-Format angeben.

        Die folgenden Schritte beschreiben, wie ein Pfx-Zertifikat für FairPlay.
        
        1. Installieren von OpenSSL aus https://slproweb.com/products/Win32OpenSSL.html
        
            Der Ordner FairPlay Zertifikat und andere Dateien von Apple übermittelt werden.
        
        2. Befehlszeile Pem Cer konvertieren:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" X509-Kenntnis der-in fairplay.cer-, Fairplay-out.pem
        
        3. Befehlszeile konvertieren Pem in Pfx mit dem privaten Schlüssel (das Kennwort für die Pfx-Datei wird dann vom OpenSSL gestellte).
        
            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-export-, Fairplay-out.pfx-inkey privatekey.pem-in Fairplay-out.pem - Passin file:privatekey-pem-pass.txt 
        
    - **Zertifikatkennwort app** - Kundenkennwort für die PFX-Datei erstellen.
    - **Kennwort-ID des app-Cert** - Kunden muss das Kennwort, wie sie andere AMS-Schlüssel und **ContentKeyType.FairPlayPfxPassword** -Enumerationswert mit ähnlichen hochladen. Als Ergebnis erhalten sie AMS-Id ist was sie in die wichtigsten Richtlinienoption verwenden müssen.
    - **iv** - Zufallswert 16 Bytes muss in der Lieferung Anlage iv entsprechen. Kunden IV generiert und an beiden Stellen eingefügt: Anlage Lieferung Richtlinie und Schlüssel Lieferung Richtlinienoption. 
    - **ASK** - Fragen (Anwendung Geheimschlüssel) beim Erstellen der Zertifizierung mit Apple Developer Portal empfangen wird. Jedes Entwicklungsteam erhalten einen eindeutigen Fragen. Speichern Sie die Fragen und an einem sicheren Ort speichern. Sie müssen hier als FairPlayAsk für Azure später konfigurieren. 
    -  **BITTEN Sie ID** - entsteht der Kunde Fragen in AMS hochlädt. Der Kunde muss mit **ContentKeyType.FairPlayASk** -Enumerationswert Fragen hochladen. Folglich AMS zurückgegeben, und das ist was bei die Option wichtige Richtlinien verwendet werden.

- Vom Client FPS müssen Folgendes festgelegt werden:
    - **App-Cert (Netzbetrieb)** -.cer/.der-Datei mit öffentlichen Schlüssel OS verwendet einige Nutzlast verschlüsselt. AMS muss wissen, da sie vom Player erforderlich ist. Der Schlüssel Zustelldienst entschlüsselt es mit dem entsprechenden privaten Schlüssel.

- Zur Wiedergabe einer FairPlay verschlüsselten Stream müssen Sie echte Fragen erste und generieren ein echtes Zertifikat. Dieser Prozess erstellt alle 3 Teile:

    -  .der, 
    -  PFX und 
    -  das Kennwort für die PFX-Datei.
 
- Clients, die mit **AES-128 CBC** -Verschlüsselung HLS unterstützen: OS X TV Apple iOS Safari.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>Schritte zum Konfigurieren von FairPlay dynamische Verschlüsselung und Lizenz Lieferdienste

Es folgen allgemeine Schritte, die Sie beim Schutz Ihrer Ressourcen mit FairPlay mithilfe der Media Services Lizenz Zustelldienst sowie auch dynamische Verschlüsselung durchführen.

1. Eine Anlage erstellen und Hochladen von Dateien in der Anlage. 
1. Codieren Sie die Anlage mit der adaptiven Bitrate festgelegten MP4-Datei.
1. Erstellen Sie einer Content und codierte Anlage zuordnen.  
1. Konfigurieren der Inhaltsschlüssel Autorisierungsrichtlinie. Beim Erstellen von Inhalten Schlüssel Autorisierungsrichtlinie müssen Sie Folgendes angeben: 
    
    - Übermittlungsmethode (in diesem Fall FairPlay), 
    - FairPlay Optionskonfiguration. Informationen zum Konfigurieren von FairPlay finden Sie unter ConfigureFairPlayPolicyOptions()-Methode im folgenden Beispiel.
    
        >[AZURE.NOTE] Normalerweise sollten Sie FairPlay Optionen nur einmal konfigurieren, da Sie nur einen Satz und Fragen haben.
-Einschränkungen (geöffnet oder token)- und Informationen zu wichtigen Lieferart, die definiert, wie der Schlüssel an den Client gesendet wird. 
    
2. Konfigurieren Sie Anlage Lieferung Richtlinie. Konfiguration der Bereitstellung umfasst: 

    - Übermittlungsprotokoll (HLS) 
    - die Art der dynamische Verschlüsselung (Common CBC-Verschlüsselung), 
    - Lizenzerwerbs-URLs. 
    
    >[AZURE.NOTE]Wenn einen Stream bereitstellen, der mit FairPlay und anderen DRM verschlüsselt werden soll, müssen Sie separate Richtlinien konfigurieren 
    >
    >- Ein IAssetDeliveryPolicy Strich mit PlayReady mit CENC (PlayReady + WideVine) konfigurieren. 
    >- Ein anderes IAssetDeliveryPolicy FairPlay für HLS konfigurieren

1. Einen Locator OnDemand Streaming-URL zu erstellen.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>Player-Client apps mithilfe FairPlay Schlüssel Lieferung

Kunden können Spieler apps mit iOS SDK entwickeln. Um FairPlay Inhalte wiedergeben müssen Kunden Lizenz Exchange-Protokoll implementiert. Lizenz Exchange-Protokoll wurde von Apple nicht angegeben. Es obliegt jeder Anwendung wichtige Anfragen zu senden. Der Schlüssel Lieferdienste AMS FairPlay erwartet SPC wie Www-form-url codierte Beitrags in der folgenden Form: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player unterstützt keine Wiedergabe im Paket FairPlay. Kunden müssen Beispiel Player Entwicklerkonto Apple MAC OSX FairPlay Wiedergabe zu erhalten. 
 
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


##<a name="net-example"></a>Beispiel für .NET


Das folgende Beispiel veranschaulicht in Azure Media Services SDK für .net eingeführte Funktionalität-Version 3.6.0 (die Möglichkeit, Azure Media Services verwenden, um Ihre Inhalte mit FairPlay verschlüsselt). Nuget-Paket Befehl wurde verwendet, um das Paket zu installieren:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Erstellen Sie ein Konsolenprojekt.
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
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
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
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
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
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
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
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
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
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
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


##<a name="next-steps-media-services-learning-paths"></a>Nächste Schritte: Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
