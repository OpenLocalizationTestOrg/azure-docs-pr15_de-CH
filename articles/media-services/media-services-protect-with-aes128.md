<properties
    pageTitle="Dynamische Verschlüsselung AES 128 mit Schlüssel Zustelldienst | Microsoft Azure"
    description="Microsoft Azure Media Services können Sie Ihre Inhalte mit AES 128-Bit-Schlüssel verschlüsselt. Media Services bietet auch der Schlüssel Zustelldienst, die Verschlüsselungsschlüssel für autorisierte Benutzer übermittelt. Dieses Thema zeigt, wie dynamisch und zum Verschlüsseln mit AES 128 Schlüssel Zustelldienst."
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

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Mithilfe von AES-128 dynamische Verschlüsselung und Schlüssel Lieferservice

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Übersicht

>[AZURE.NOTE] Siehe [Video wie der Medieninhalt mit AES-Verschlüsselung schützen](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) .

Microsoft Azure Media Services können Sie Http-Live-Streaming (HLS) und reibungslose Streams mit Advanced Encryption Standard (AES) (mit 128-Bit-Verschlüsselungsschlüssel) verschlüsselt. Media Services bietet auch der Schlüssel Zustelldienst, die Verschlüsselungsschlüssel für autorisierte Benutzer übermittelt. Für Media Services Vermögenswert verschlüsseln sollten, Sie einen Schlüssel mit der Anlage und Autorisierungsrichtlinien für den Schlüssel auch konfigurieren. Wenn ein Spieler ein Stream angefordert wird, verwendet Media Services angegebenen Schlüssel dynamisch Content mit AES-Verschlüsselung verschlüsselt. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel Schlüssel Paketdienstes. Entscheidung, ob der Benutzer berechtigt ist, den Schlüssel, wertet der Dienst die Autorisierungsrichtlinien für den Schlüssel angegeben.

Media Services unterstützt mehrere Methoden für die Authentifizierung von Benutzern Schlüssel anfordern. Inhalt wichtige Autorisierungsrichtlinie könnte gelten mindestens Autorisierung: Öffnen oder Einschränkung token. Die token eingeschränkte Richtlinie ein Token ausgestellt durch eine Secure Token Service (STS) beizufügen. Media Services unterstützt Token im die [Simple Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) Format und [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Weitere Informationen finden Sie unter [Konfigurieren der Inhaltsschlüssel Autorisierungsrichtlinie](media-services-protect-with-aes128.md#configure_key_auth_policy).

Um dynamische Verschlüsselung nutzen zu können, müssen Sie eine Anlage haben, die eine Reihe von Multi-Bitrate MP4-Dateien oder Multi-Bitrate Smooth Streaming-Quelldateien enthält. Sie müssen auch Delivery-Richtlinie für die Anlage (weiter unten in diesem Thema beschrieben). Dann sorgen basierend auf Streaming-URL angegebenen Format, On-Demand-Streamingserver Stream im Protokoll übermittelt gewählte. Daher müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen und Media-Dienste erstellen und die entsprechende Antwort auf Anfragen von einem Client verwendet.

In diesem Thema nützlich für Entwickler, die auf Applikationen, die geschützte Medien. Das Thema veranschaulicht die wichtigsten Zustelldienst mit Autorisierungsrichtlinien so konfigurieren, dass nur autorisierte Clients die Verschlüsselungsschlüssel empfangen können. Außerdem wird das dynamische Verschlüsselung veranschaulicht.

>[AZURE.NOTE]Um dynamische Verschlüsselung zu starten, müssen Sie zunächst mindestens eine Skalierungseinheit (auch bekannt als Streaming-Einheit) abrufen. Weitere Informationen finden Sie unter [wie Media Service](media-services-portal-manage-streaming-endpoints.md).

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES-128 dynamische Verschlüsselung und Schlüssel Delivery Service Workflow

Es folgen allgemeine Schritte, die Sie durchführen, wenn Ihre Ressourcen mit AES Key Zustelldienst Media Services und auch dynamische Verschlüsselung verschlüsseln.

1. [Eine Anlage erstellen und Hochladen von Dateien in der Anlage](media-services-protect-with-aes128.md#create_asset).
1. [Codierung der Anlage mit der adaptiven Bitrate festgelegten MP4-Datei](media-services-protect-with-aes128.md#encode_asset).
1. [Ein Content erstellen und codierte Anlage zugeordnet](media-services-protect-with-aes128.md#create_contentkey). Media Services enthält den Inhalt der Anlage Verschlüsselungsschlüssel.
1. [Der Inhaltsschlüssel Autorisierungsrichtlinie konfigurieren](media-services-protect-with-aes128.md#configure_key_auth_policy). Inhalt wichtige Autorisierungsrichtlinie müssen Sie konfiguriert und vom Client an den Client übermittelt werden in Reihenfolge für den Content Schlüssel erfüllt.
1. [Konfigurieren Delivery-Richtlinie für eine Anlage](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Konfiguration der Bereitstellung umfasst: wichtige Übernahme URL und Initialisierungsvektor (IV) (AES 128 erfordert dieselbe IV angegeben werden, wenn Verschlüsselung und Entschlüsselung), Übermittlungsprotokoll (z. B. MPEG DASH, HLS, HDS, Smooth Streaming oder alle), die dynamische Verschlüsselung (z. B. Umschlag oder keine dynamische Verschlüsselung).

Sie können jedes Protokoll auf dem gleichen unterschiedliche Richtlinien anwenden. Sie können z. B. PlayReady-Verschlüsselung AES Umschlag HLS zu glätten-Strich anwenden. Alle Protokolle, die nicht in einer Lieferung definiert sind (z. B. hinzufügen eine einzelne Richtlinie, nur HLS als Protokoll) von streaming blockiert werden. Die Ausnahme ist keine Anlage Lieferung Richtlinie überhaupt definiert haben. Dann werden alle Protokolle in Klartext zulässig.

1. [Erstellen eines Locators OnDemand](media-services-protect-with-aes128.md#create_locator) um eine Streaming-URL zu erhalten.

Das Thema wird aufgezeigt, [wie eine Anwendung einen Schlüssel aus der wichtigsten Zustelldienst anfordern kann](media-services-protect-with-aes128.md#client_request).

Sie finden eine vollständige .NET [wird](media-services-protect-with-aes128.md#example) am Ende des Themas.

Die folgende Abbildung zeigt den oben beschriebenen Workflow. Hier wird das Token zur Authentifizierung verwendet.

![Schützen Sie mit AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Im weiteren Verlauf dieses Themas enthält eine ausführliche Erklärung, Codebeispiele und Links zu Themen, die zeigen, wie Sie die oben beschriebenen Aufgaben.

##<a name="current-limitations"></a>Aktuelle Grenzen

Hinzufügen oder Aktualisieren des Vermögenswertes Lieferung Richtlinie müssen Sie vorhandenen Locator (falls vorhanden) löschen und neuen Locator erstellen.

##<a id="create_asset"></a>Eine Anlage erstellen und Hochladen von Dateien in der Anlage

Verwalten, codieren und Streaming-Videos, müssen Sie zuerst den Inhalt in Microsoft Azure Media Services hochladen. Nachdem hochgeladen, wird Ihr Inhalt in der Cloud für Weiterverarbeitung und streaming sicher. 

Ausführliche Informationen finden Sie unter [Dateien in Media Services-Konto hochladen](media-services-dotnet-upload-files.md).

##<a id="encode_asset"></a>Codieren Sie die Anlage mit der adaptiven Bitrate festgelegten MP4-Datei

Dynamische Verschlüsselung benötigen Sie ist eine Anlage erstellen, die eine Reihe von Multi-Bitrate MP4-Dateien oder Multi-Bitrate Smooth Streaming-Quelldateien enthält. Anschließend wird basierend auf dem angegebenen Format in der Anforderung Manifest oder Fragment On-Demand-Streaming Server garantieren Sie den Stream im Protokoll gewählte. Daher müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen und Media-Dienste erstellen und die entsprechende Antwort auf Anfragen von einem Client verwendet. Weitere Informationen finden Sie im Thema [dynamische Verpackung](media-services-dynamic-packaging-overview.md) .

Informationen zum Codieren finden Sie unter [Codieren eine Anlage mit Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).

##<a id="create_contentkey"></a>Erstellen Sie einer Content und codierte Anlage zuordnen

Media Services enthält den Inhalt den Schlüssel, dem Sie eine Anlage verschlüsseln möchten.

Ausführliche Informationen finden Sie unter [Schlüssel erstellen](media-services-dotnet-create-contentkey.md).

##<a id="configure_key_auth_policy"></a>Der Inhaltsschlüssel Autorisierungsrichtlinie konfigurieren

Media Services unterstützt mehrere Methoden für die Authentifizierung von Benutzern Schlüssel anfordern. Inhalt wichtige Autorisierungsrichtlinie müssen Sie konfiguriert und erfüllt vom Client (Player) den Schlüssel an den Client übermittelt werden. Inhalt wichtige Autorisierungsrichtlinie könnte gelten mindestens Autorisierung: öffnen, token Beschränkung oder IP-Einschränkung.

Ausführliche Informationen finden Sie unter [Content Schlüssel Autorisierungsrichtlinie konfigurieren](media-services-dotnet-configure-content-key-auth-policy.md).

##<a id="configure_asset_delivery_policy"></a>Anlage Lieferung Richtlinie konfigurieren 

Konfigurieren der Bereitstellung Richtlinie für die Anlage. Einige Dinge, die Konfiguration der Anlage Lieferung enthält:

- Der Schlüssel Übernahme URL. 
- Der Initialisierungsvektor (IV) für die Umschlag-Verschlüsselung verwenden. AES 128 ist dieselbe IV angegeben werden, wenn Verschlüsselung und Entschlüsselung erforderlich. 
- Das Übermittlungsprotokoll Anlage, (z. B. MPEG DASH, HLS, HDS, Smooth Streaming oder alle).
- Der Typ des dynamische Verschlüsselung (z. B. AES Umschlag) oder keine dynamischen Verschlüsselung. 

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

##<a id="client_request"></a>Wie kann der Client einen Schlüssel aus der wichtigsten Zustelldienst anfordern?

Im vorherigen Schritt erstellt Sie den URL für eine Manifestdatei. Der Client muss erforderlichen Informationen anfordern der wichtigsten Zustelldienst von Streaming-Manifesten-Dateien extrahieren.

###<a name="manifest-files"></a>Manifest-Dateien

Der Client muss in der URL extrahieren (die auch Schlüssel Id (Kind) enthält) Wert aus der Manifestdatei. Der Client versucht dann den Chiffrierschlüssel Schlüssel Paketdienstes erhalten. Der Client muss ebenfalls extrahiert IV Wert und verwendet den Stream entschlüsselt. Der folgende Ausschnitt zeigt die <Protection> Element des Manifests Smooth Streaming.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

Bei HLS wird das Manifest Stamm in Segmentdateien aufgeteilt. 

Root-Manifest ist z. B.: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) und enthält eine Liste der Dateinamen Segment.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Segment-Dateien im Text-Editor öffnen (z. B. http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), es sollte #EXT-X-Schlüssel gibt an, dass die Datei verschlüsselt ist.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>Der Schlüssel Zustelldienst den Schlüssel anfordern

Der folgende Code veranschaulicht die Anfrage an den wichtigsten Übermittlungsdienst Media Services mit einem Schlüssel Uri (die aus dem Manifest extrahiert wurde) und (in diesem Thema spricht nicht wie Simple Web Token von Secure Token Service).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Beispiel

1. Erstellen Sie ein neues Konsolenprojekt.
1. Verwenden Sie NuGet zum Installieren und Azure Media Services .NET SDK Erweiterungen. Installieren dieses Paket, Media Services .NET SDK installiert und andere erforderliche Abhängigkeit hinzugefügt.
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

1. Überschrieben Sie den Code in der Datei Program.cs mit dem Code in diesem Abschnitt dargestellt.
    
    Stellen Sie sicher aktualisieren Variablen Ordner zeigen, wo sich Ihre Dateien befinden.
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
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
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
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
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    // In this case "H264 Multiple Bitrate 720p" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        TaskOptions.None);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
                {
                    // Create envelope encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.EnvelopeEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
                    };
        
                    IAssetDeliveryPolicy assetDeliveryPolicy =
                        _context.AssetDeliveryPolicies.Create(
                                    "AssetDeliveryPolicy",
                                    AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                                    AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                                    assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
                    Console.WriteLine();
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int size)
                {
                    byte[] randomBytes = new byte[size];
                    using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(randomBytes);
                    }
        
                    return randomBytes;
                }
            }
        }


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
