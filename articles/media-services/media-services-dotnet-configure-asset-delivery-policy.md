<properties 
    pageTitle=".NET SDK Anlage Richtlinien konfigurieren | Microsoft Azure" 
    description="Dieses Thema zeigt Azure Media Services .NET SDK gleichnamiges Richtlinien konfigurieren." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>.NET SDK Anlage Richtlinien konfigurieren
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>Übersicht

Soll auf Übermittlung verschlüsselt ist einer der Schritte im Workflow Inhaltsübermittlung Media Services Richtlinien für Anlagen konfigurieren. Anlage Lieferung Richtlinie teilt Mediendienste wie für die Anlage übermittelt werden sollen: in der streaming-Protokoll soll die Anlage dynamisch verpackt (z. B. MPEG DASH, HLS, Smooth Streaming, oder alle), oder ob Sie Ihre Anlage dynamisch verschlüsseln möchten und wie (Umschlag oder allgemeine Verschlüsselung).

In diesem Thema wird erläutert, warum und Anlage Lieferung Richtlinien erstellen und konfigurieren.

>[AZURE.NOTE]Um dynamische Verpackung und dynamische Verschlüsselung verwenden zu können, müssen Sie mindestens eine Skalierungseinheit (auch bekannt als Streaming-Einheit) zu sicherstellen. Weitere Informationen finden Sie unter [wie Media Service](media-services-portal-manage-streaming-endpoints.md).
>
>Die Anlage muss auch eine Reihe von adaptiven Bitrate MP4s oder adaptive Bitrate Smooth Streaming-Dateien enthalten.

Sie können unterschiedliche Richtlinien für die gleiche Ressource anwenden. Sie können z. B. PlayReady-Verschlüsselung Smooth Streaming und AES Umschlag Verschlüsselung MPEG DASH und HLS anwenden. Alle Protokolle, die nicht in einer Lieferung definiert sind (z. B. hinzufügen eine einzelne Richtlinie, nur HLS als Protokoll) von streaming blockiert werden. Die Ausnahme ist keine Anlage Lieferung Richtlinie überhaupt definiert haben. Dann werden alle Protokolle in Klartext zulässig.

Ggf. verschlüsselten Speicherressourcen liefern müssen die Anlage Lieferung Richtlinie konfigurieren. Bevor die Anlage übertragen kann, streaming Server Speicher entschlüsselt und Inhalte der Richtlinie angegebenen Übermittlung mit streams. Beispielsweise liefern die Anlage mit Advanced Encryption Standard (AES) umschlagschlüssel verschlüsselt Geben Sie die Richtlinie zu **DynamicEnvelopeEncryption**. Storage Verschlüsselung entfernen die Anlage als Klartext übertragen und geben Sie die Richtlinie zu **NoDynamicEncryption**. Führen Sie die Beispiele, die zeigen, wie diese Richtlinie konfigurieren.

Je nach Konfiguration Anlage Lieferung Richtlinie würde möglicherweise dynamisch Paket dynamisch verschlüsseln und folgenden Streamingprotokolle stream: Smooth Streaming, HLS MPEG DASH und HDS Streams.

Die folgende Liste zeigt die Formate, glatt, HLS Strich und HDS stream verwenden.

Smooth Streaming:

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-STRICH

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HDS

{streaming-Endpunkt Namen Media Services Account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Informationen zu veröffentlichen einer Anlage eine Streaming-URL finden Sie unter [erstellen eine Streaming-URL](media-services-deliver-streaming-content.md).

##<a name="considerations"></a>Hinweise

- Ein AssetDeliveryPolicy Zusammenhang mit einer Anlage gibt ein Locator OnDemand (streaming) für diese Anlage kann nicht gelöscht werden. Es wird empfohlen, die Richtlinie aus dem Vermögenswert entfernen, bevor Sie die Richtlinie löschen.
- Streaming-Locator kann auf einem verschlüsselten Speicher erstellt werden, wenn keine Anlage Übermittlung festgelegt ist.  Wenn die Anlage Speicher verschlüsselt ist, können System Sie einen Locator erstellen und Übertragen der Anlage löschen ohne Übermittlung einer Anlage.
- Mehrere Anlagen Richtlinien ein Vermögenswert zugeordnet haben, aber Sie können nur eine Methode, eine bestimmte AssetDeliveryProtocol.  D. h., wenn Sie versuchen, zwei Richtlinien zu verknüpfen, die das AssetDeliveryProtocol.SmoothStreaming-Protokoll, das zu einem Fehler führt angeben, da das System nicht weiß, Sie wollen angewendet, wenn ein Client eine Anforderung Smooth Streaming.
- Haben Sie eine Anlage mit einer vorhandenen Streaming-Locator keine neue Richtlinie mit Anlage verknüpfen (Sie aufheben eine Richtlinie aus dem Vermögenswert oder aktualisieren eine Lieferung Richtlinie zugeordnete Anlage).  Zuerst müssen Streaming-Locator entfernen, passen Sie die Richtlinien und Streaming-Locator neu erstellen.  Sie können dieselbe LocatorId neu streaming Locator, aber Sie sollten sicherstellen, die Probleme wird nicht für Clients da Inhalte von Ursprung oder einer nachgelagerten CDN zwischengespeichert werden kann.


##<a name="clear-asset-delivery-policy"></a>Anlage löschen Lieferung Richtlinie

Die folgende **ConfigureClearAssetDeliveryPolicy** -Methode gibt nicht dynamische Verschlüsselung angewendet und Streams in eines der folgenden Protokolle: MPEG DASH HLS und Smooth Streaming-Protokolle. Sie möchten Ihre Speicherressourcen verschlüsselt diese Richtlinie zuweisen.

Informationen über Werte angeben können, wenn Sie eine AssetDeliveryPolicy erstellen finden Sie im Abschnitt [beim Definieren von AssetDeliveryPolicy verwendet](#types) .

static public void ConfigureClearAssetDeliveryPolicy (IAsset Asset) {IAssetDeliveryPolicy Richtlinie = _context. AssetDeliveryPolicies.Create ("Klare", AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

Anlage. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption Anlage Lieferung Richtlinie


**CreateAssetDeliveryPolicy** folgt erstellt **AssetDeliveryPolicy** , die darauf konfiguriert ist, dynamische gemeinsame Verschlüsselungsmethode (**DynamicCommonEncryption**) für eine reibungslose streaming-Protokoll (andere Protokolle werden streaming gesperrt). Die Methode verwendet zwei Parameter: **Anlage** (die Anlage Sie Delivery-Richtlinie anwenden möchten) und **IContentKey** (Content Schlüssel vom Typ **CommonEncryption** Weitere Informationen finden Sie unter: [Erstellen eines Schlüssels mit Inhalten](media-services-dotnet-create-contentkey.md#common_contentkey)).

Informationen über Werte angeben können, wenn Sie eine AssetDeliveryPolicy erstellen finden Sie im Abschnitt [beim Definieren von AssetDeliveryPolicy verwendet](#types) .


static public void CreateAssetDeliveryPolicy (IAsset Anlage, IContentKey Schlüssel) {Uri AcquisitionUrl = Schlüssel. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

Dictionary < AssetDeliveryPolicyConfigurationKey, String > AssetDeliveryPolicyConfiguration = new Dictionary < AssetDeliveryPolicyConfigurationKey, String > {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()}};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media Services können Sie Widevine Verschlüsselung hinzufügen. Veranschaulicht PlayReady und Widevine der Anlage Lieferung Richtlinie hinzugefügt.


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
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Beim Verschlüsseln mit Widevine wäre Sie nur liefern mit Bindestrich. Sollten Sie Bindestrich in dem Übermittlungsprotokoll Anlage angeben.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption Anlage Lieferung Richtlinie 

Die folgende **CreateAssetDeliveryPolicy** -Methode erstellt **AssetDeliveryPolicy** , dessen Konfiguration die Protokolle Smooth Streaming, HLS und Strich Fahrzeugumgrenzungslinie Verschlüsselung (**DynamicEnvelopeEncryption**) zuweisen (Wenn Sie nicht einige Protokolle angeben, sie blockiert werden von streaming). Die Methode verwendet zwei Parameter: **Anlage** (die Anlage Sie Delivery-Richtlinie anwenden möchten) und **IContentKey** (Content Schlüssel vom Typ **EnvelopeEncryption** Weitere Informationen finden Sie unter: [Erstellen eines Schlüssels mit Inhalten](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Informationen über Werte angeben können, wenn Sie eine AssetDeliveryPolicy erstellen finden Sie im Abschnitt [beim Definieren von AssetDeliveryPolicy verwendet](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
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
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Beim Definieren von AssetDeliveryPolicy verwendet

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


