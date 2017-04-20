<properties 
    pageTitle="ContentKeys mit .NET erstellen" 
    description="Informationen Sie zum Erstellen von Schlüsseln, die sicheren Zugriff auf Ressourcen." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="create-contentkeys-with-net"></a>ContentKeys mit .NET erstellen

> [AZURE.SELECTOR]
- [REST](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Media Services können Sie schaffen und verschlüsselte Elemente. **ContentKey** bietet sicheren Zugriff auf die **Anlage**s. 

Wenn Sie eine neue Anlage (z. B. bevor Sie [Dateien hochladen](media-services-dotnet-upload-files.md)) erstellen, können Sie folgenden Verschlüsselungsoptionen: **StorageEncrypted**, **CommonEncryptionProtected**oder **EnvelopeEncryptionProtected**. 

Wenn Ressourcen Ihren Kunden liefern, können Sie mit einem der folgenden zwei Verschlüsselungsstufen [Konfigurieren für Ressourcen dynamisch verschlüsselt werden](media-services-dotnet-configure-asset-delivery-policy.md) : **DynamicEnvelopeEncryption** oder **DynamicCommonEncryption**.

Verschlüsselte Elemente müssen **ContentKey**s zugeordnet. Dieser Artikel beschreibt, wie Inhalt erstellt.

>[AZURE.NOTE] Beim Erstellen einer neuen **StorageEncrypted** -Anlage mit Media Services .NET SDK **ContentKey** automatisch erstellt und verknüpft mit der Anlage.

##<a name="contentkeytype"></a>ContentKeyType

Die Werte, wenn Sie festlegen müssen, erstellt einen Schlüssel ist der Schlüssel Inhaltstyp. Wählen Sie eine der folgenden Werte. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>Erstellen Sie Umschlag ContentKey

Der folgende Codeausschnitt erstellt Inhaltsschlüssel Verschlüsselungstyp Umschlag. Es ordnet dann den Schlüssel die angegebene Anlage.

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

        asset.ContentKeys.Add(key);

        return key;
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

Aufrufen

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Allgemeine Typ ContentKey erstellen    

Der folgende Codeausschnitt erstellt Inhaltsschlüssel allgemeine Verschlüsselungstyp. Es ordnet dann den Schlüssel die angegebene Anlage.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
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
Aufrufen

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
