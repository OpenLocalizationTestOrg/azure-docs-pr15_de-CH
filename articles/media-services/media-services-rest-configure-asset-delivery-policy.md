<properties 
    pageTitle="Anlage mit Media Services REST API Richtlinien konfigurieren | Microsoft Azure" 
    description="In diesem Thema veranschaulicht, wie andere Anlage mit Media Services REST API Richtlinien konfigurieren." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Anlage Richtlinien konfigurieren

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Wenn Sie dynamisch verschlüsselte Ressourcen bereitstellen möchten, ist einer der Schritte im Workflow Inhaltsübermittlung Media Services Richtlinien für Anlagen konfigurieren. Anlage Lieferung Richtlinie teilt Mediendienste wie für die Anlage übermittelt werden sollen: in der streaming-Protokoll soll die Anlage dynamisch verpackt (z. B. MPEG DASH, HLS, Smooth Streaming, oder alle), oder ob Sie Ihre Anlage dynamisch verschlüsseln möchten und wie (Umschlag oder allgemeine Verschlüsselung).

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
- Haben Sie eine Anlage mit einer vorhandenen streaming Locator können Sie verknüpfen eine neue Richtlinie mit der Anlage, aufheben eine Richtlinie aus dem Vermögenswert oder aktualisieren eine Lieferung Richtlinie Anlage zugeordnet.  Zuerst müssen Streaming-Locator entfernen, passen Sie die Richtlinien und Streaming-Locator neu erstellen.  Sie können dieselbe LocatorId neu streaming Locator, aber Sie sollten sicherstellen, die Probleme wird nicht für Clients da Inhalte von Ursprung oder einer nachgelagerten CDN zwischengespeichert werden kann.

>[AZURE.NOTE] Beim Arbeiten mit Media Services REST-API berücksichtigen die folgenden Aspekte:
>
>Wenn Entitäten in Media Services zugreifen, müssen Sie bestimmten Feldern und Werten in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Setup für Media Services REST API-Entwicklung](media-services-rest-how-to-use.md).

>Nach dem erfolgreichen Verbindungsaufbau zu https://media.windows.net erhalten Sie eine 301-Umleitung einer anderen Media Services URI angegeben. Nachfolgende Aufrufe der neuen URI Siehe [Herstellen einer Verbindung mit Media Services REST-API verwenden](media-services-rest-connect-programmatically.md)müssen.


##<a name="clear-asset-delivery-policy"></a>Anlage löschen Lieferung Richtlinie

###<a id="create_asset_delivery_policy"></a>Anlage Lieferung Richtlinie erstellen
Die folgende HTTP-Anforderung erstellt eine Anlage Lieferung Richtlinie, dynamische Verschlüsselung nicht anwendbar und Streams in eines der folgenden Protokolle: MPEG DASH HLS und Smooth Streaming-Protokolle. 

Informationen über Werte angeben können, wenn Sie eine AssetDeliveryPolicy erstellen finden Sie im Abschnitt [beim Definieren von AssetDeliveryPolicy verwendet](#types) .   


Anforderung:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Antwort:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Link-Anlage mit Anlage Lieferung

HTTP-Anforderung verknüpft angegebene Ressource mit der Anlage Lieferung Richtlinie.

Anforderung:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Antwort:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption Anlage Lieferung Richtlinie 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Erstellen Sie Inhalt des EnvelopeEncryption-Typs und verknüpfen Sie der Anlage

Wenn DynamicEnvelopeEncryption Übermittlung Richtlinie angeben, müssen Sie sicherstellen, dass die Anlage Inhaltsschlüssel EnvelopeEncryption Typ verknüpfen. Weitere Informationen finden Sie unter: [Erstellen eines Schlüssels mit Inhalten](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Lieferung URL abrufen

Rufen Sie Bereitstellung-URL für die angegebene Methode der im vorherigen Schritt erstellten Content Schlüssel ab Ein Client verwendet die zurückgegebene URL AES-Schlüssel anfordern oder eine PlayReady Lizenz zu Wiedergabe geschützte Inhalte.

Geben Sie die URL im Hauptteil der HTTP-Anforderung abgerufen. Wenn Sie Ihre Inhalte mit PlayReady, schützen anfordern Lizenzerwerbs-URLs ein Media Services PlayReady, verwenden Sie 1 für die KeyDeliveryType: {"KeyDeliveryType": 1}. Wenn Sie Ihre Inhalte mit der Umschlag Verschlüsselung schützen, Erwerb URL durch Angabe 2 KeyDeliveryType anfordern: {"KeyDeliveryType": 2}.

Anforderung:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Antwort:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Anlage Lieferung Richtlinie erstellen

Die folgende HTTP-Anforderung erstellt **AssetDeliveryPolicy** , die konfiguriert **HLS** Protokoll Fahrzeugumgrenzungslinie Verschlüsselung (**DynamicEnvelopeEncryption**) zugewiesen (in diesem Beispiel andere Protokolle blockiert werden von streaming). 


Informationen über Werte angeben können, wenn Sie eine AssetDeliveryPolicy erstellen finden Sie im Abschnitt [beim Definieren von AssetDeliveryPolicy verwendet](#types) .   

Anforderung:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Antwort:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Link-Anlage mit Anlage Lieferung

Siehe [Anlage verknüpfen mit Anlage Lieferung](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption Anlage Lieferung Richtlinie 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Erstellen Sie Inhalt des CommonEncryption-Typs und verknüpfen Sie der Anlage

Wenn DynamicCommonEncryption Übermittlung Richtlinie angeben, müssen Sie sicherstellen, dass die Anlage Inhaltsschlüssel CommonEncryption Typ verknüpfen. Weitere Informationen finden Sie unter: [Erstellen eines Schlüssels mit Inhalten](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Lieferung URL abrufen

Ruft die URL Lieferung für PlayReady-Übermittlungsmethode der im vorherigen Schritt erstellten Inhaltsschlüssel. Ein Client verwendet die zurückgegebene URL um eine PlayReady-Lizenz zu Wiedergabe von geschützten Inhalten. Weitere Informationen finden Sie unter [Delivery URL abrufen](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Anlage Lieferung Richtlinie erstellen

Die folgende HTTP-Anforderung erstellt **AssetDeliveryPolicy** , die konfiguriert das **Smooth Streaming** -Protokoll dynamische gemeinsame Verschlüsselung (**DynamicCommonEncryption**) zugewiesen (in diesem Beispiel andere Protokolle blockiert werden von streaming). 

Informationen über Werte angeben können, wenn Sie eine AssetDeliveryPolicy erstellen finden Sie im Abschnitt [beim Definieren von AssetDeliveryPolicy verwendet](#types) .   


Anforderung:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Wenn Sie Inhalte mit Widevine DRM schützen möchten, AssetDeliveryConfiguration Werte um WidevineLicenseAcquisitionUrl verwenden (mit dem Wert 7) und geben Sie die URL einer Lizenz Übermittlungsdienst. Verwenden der folgenden AMS-Partner beim Widevine Lizenzen liefern: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [CastLabs](http://castlabs.com/company/partners/azure/).

Zum Beispiel: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Beim Verschlüsseln mit Widevine wäre Sie nur liefern mit Bindestrich. Stellen Sie sicher das Übermittlungsprotokoll Anlage Strich (2) geben.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Link-Anlage mit Anlage Lieferung

Siehe [Anlage verknüpfen mit Anlage Lieferung](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Beim Definieren von AssetDeliveryPolicy verwendet

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

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

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

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

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

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
