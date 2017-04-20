<properties 
    pageTitle="Media Services-Konto mit anderen Dateien hochladen | Microsoft Azure" 
    description="Informationen Sie zu Medieninhalte Media Services erstellen und Hochladen von Ressourcen." 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Media Services-Konto mit anderen hochladen Sie Dateien

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [REST](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

Media Services wird Ihre digitalen Dateien zu laden. Die [Aktivposten](https://msdn.microsoft.com/library/azure/hh974277.aspx) -Entität darf Video, Audio, Bilder, Miniaturansichten Sammlungen, Text Titel und Untertitel Dateien (und Metadaten Dateien.)  Nach Dateien in der Anlage hochgeladen werden, wird der Inhalt in der Cloud zur weiteren Verarbeitung und streaming sicher. 

>[AZURE.NOTE]Beim Auswählen eines Namens für die Anlage ist Folgendes zu berücksichtigen:
>
>- Media Services verwendet den Wert der Eigenschaft IAssetFile.Name beim Erstellen von URLs für das streaming von Inhalten (z. B. http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Aus diesem Grund ist die Prozent-Codierung nicht zulässig. Der Wert der **Name** -Eigenschaft keinen der folgenden [%-Codierung-reservierten Zeichen](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Es können nur auch einen "." für die Erweiterung.
>
>- Die Länge des Namens sollte nicht länger als 260 Zeichen sein.

Grundlegende Workflow für den Upload von Anlagen ist in folgende Abschnitte unterteilt:

- Eine Anlage
- Verschlüsseln einer Anlage (Optional)
- Hochladen einer Datei in den BLOB-Speicher

AMS können Sie Anlagen gebündelt hochladen. Weitere Informationen finden Sie in [diesem](media-services-rest-upload-files.md#upload_in_bulk) Abschnitt.

##<a name="upload-assets"></a>Uploaden von Anlagen

###<a name="create-an-asset"></a>Eine Anlage

>[AZURE.NOTE] Beim Arbeiten mit Media Services REST-API berücksichtigen die folgenden Aspekte:
>
>Wenn Entitäten in Media Services zugreifen, müssen Sie bestimmten Feldern und Werten in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Setup für Media Services REST API-Entwicklung](media-services-rest-how-to-use.md).

>Nach dem erfolgreichen Verbindungsaufbau zu https://media.windows.net erhalten Sie eine 301-Umleitung einer anderen Media Services URI angegeben. Nachfolgende Aufrufe der neuen URI Siehe [Herstellen einer Verbindung mit Media Services REST-API verwenden](media-services-rest-connect-programmatically.md)müssen. 
 
Ein Vermögenswert ist ein Container für mehrere Arten oder Gruppen von Objekten in Media Services wie Video, Audio, Bilder, Miniaturansichten Sammlungen, Textspuren und Untertitel-Dateien. In REST-API erfordert das Erstellen einer Anlage an Media Services POST-Anforderung senden und Informationen zu Ihrer Ressource im Hauptteil Anforderung platziert.

Eine der Eigenschaften, die beim Erstellen einer Anlage **Optionen**angeben können. **Optionen** wird ein Enumerationswert, der die Verschlüsselungsoptionen beschreibt, denen mit eine Anlage erstellt werden kann. Ein gültiger Wert ist einer der Werte aus der Liste keine Kombination von Werten. 

- **Keine** = **0**: keine Verschlüsselung verwendet. Dies ist der Standardwert. Beachten Sie, dass bei Verwendung dieser Option der Inhalt nicht während der Übertragung oder im Speicher geschützt ist.
    Möchten Sie ein MP4 liefern progressives Herunterladen verwendet diese Option. 

- **StorageEncrypted** = **1**: angeben, ob für die Dateien mit AES-256-Bit-Verschlüsselung für Uploads und Speicher verschlüsselt werden soll.

    Ist die Anlage Speicher verschlüsselt, müssen Anlagen Lieferung Richtlinie konfigurieren. Informationen finden Sie unter [Konfigurieren von Anlage Lieferung Richtlinie](media-services-rest-configure-asset-delivery-policy.md).

- **CommonEncryptionProtected** = **2**: angeben, wenn Sie mit einer gemeinsamen Verschlüsselungsmethode (z. B. PlayReady) geschützte Dateien hochladen. 

- **EnvelopeEncryptionProtected** = **4**: angeben, wenn Sie mit AES verschlüsselt HLS hochladen. Beachten Sie, dass Dateien müssen codiert und Transformieren Manager verschlüsselt.

>[AZURE.NOTE]Wenn Ihre Anlage Verschlüsselung verwenden, müssen Sie **ContentKey** erstellen und die Anlage verknüpfen, wie im folgenden Thema beschrieben:[eine ContentKey erstellen](media-services-rest-create-contentkey.md). Beachten Sie, dass nach dem Hochladen der Dateien in der Anlage um Verschlüsselungseigenschaften für die **AssetFile** -Entität mit den Werten aktualisieren, während die **Anlage** Verschlüsselung habe. Verwenden sie dazu **ZUSAMMENFÜHREN** HTTP-Anforderung. 


Im folgende Beispiel veranschaulicht eine Anlage erstellen.

**HTTP-Anforderung**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

**HTTP-Antwort**

Wenn erfolgreich, wird Folgendes zurückgegeben:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Erstellen einer AssetFile

[AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) -Entität stellt eine Video- oder audio-Datei, die in einem BLOB-Container gespeichert. Eine Bilddatei ist immer Anlagen und Anlage enthalten einen oder mehrere Objektdateien. Media Services Encoder-Task fehlschlägt, wenn ein Dateiobjekt Anlage nicht digitale Datei in einem BLOB-Container zugeordnet ist.

Beachten Sie, dass die **AssetFile** -Instanz und die aktuelle Mediendatei zwei verschiedene Objekte. Die AssetFile-Instanz enthält Metadaten über die Mediendatei Mediendatei tatsächlichen Medien.

Nach dem Hochladen der digitalen Mediendatei in einem BLOB-Container verwenden Sie **ZUSAMMENFÜHREN** HTTP-Anforderung zum Aktualisieren der AssetFile mit Informationen über die Mediendatei (siehe weiter unten). 

**HTTP-Anforderung**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-Antwort**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Erstellen die AccessPolicy mit Schreibberechtigung. 

Vor dem Upload in BLOB-Speicher, festlegen Sie den Zugriff richtlinienrechte zum Schreiben in eine Anlage. Buchen Sie dazu eine HTTP-Anforderung an AccessPolicies-Entitätenmenge. Definieren Sie einen DurationInMinutes-Wert bei der Erstellung oder 500 internen Server-Fehlermeldung wird als Antwort empfangen. Weitere Informationen zu AccessPolicies finden Sie unter [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Im folgenden Beispiel wird veranschaulicht, wie ein AccessPolicy erstellen:
        
**HTTP-Anforderung**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-Anforderung**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

###<a name="get-the-upload-url"></a>Rufen Sie die Upload-URL

Um die tatsächliche Upload-URL erhalten, erstellen Sie SAS-Locator Locator definieren die Startzeit und die Art der Verbindungsendpunkt für Clients, die Dateien in einer Anlage zugreifen möchten. Sie können mehrere Locator Entitäten für ein angegebenen AccessPolicy und Ressourcen mit anderen Clients und erstellen. Diese Locators nutzen StartTime-Wert plus der DurationInMinutes von den AccessPolicy, die Länge des URL verwendet werden kann. Weitere Informationen finden Sie im [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


SAS-URL hat das folgende Format:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Einige zu berücksichtigen:

- Sie können nicht mehr als fünf eindeutige Locators Assets gleichzeitig zugeordnet haben. Weitere Informationen finden Sie im Locator.
- Benötigen Sie die Dateien sofort hochzuladen, sollten Sie fünf Minuten vor der aktuellen Zeit StartTime-Wert fest. Dies ist möglicherweise Clock skew zwischen dem Clientcomputer und Media-Dienste. StartTime-Wert muss auch im folgenden DateTime-Format: JJJJ-MM-: ssZ (z. B. "2014-05-23T17:53:50Z").   
- Möglicherweise 30 bis 40 Sekunden Verzögerung nach ein Locator erstellt wird, wenn es verfügbar ist. Dieses Problem betrifft SAS-URL und Ursprung Locators.

Im folgenden Codebeispiel einen Locator SAS-URL erstellen gemäß der Type-Eigenschaft im Hauptteil Anforderung (für SAS-Locator "1") und "2" für einen auf-Anforderung-Ursprung-Locator. Die **Path** -Eigenschaft zurückgegebene enthält die URL, mit denen Sie Ihre Datei hochladen müssen.
    
**HTTP-Anforderung**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-Antwort**

Wenn erfolgreich, wird die folgende Antwort zurückgegeben:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Hochladen einer Datei in eine Blob-Behälter
    
Haben Sie AccessPolicy und Locator festlegen, wird die eigentliche Datei ein Azure BLOB-Speicher-Container mit Azure Storage REST APIs hochgeladen. Seite hochladen oder Blobs blockieren. 

>[AZURE.NOTE] Sie müssen den Dateinamen für die Datei hinzufügen Locator **Pfad** erhaltene im vorherigen Abschnitt hinzufügen möchten. Beispielsweise https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Weitere Informationen zum Arbeiten mit Azure-Speicher-Blobs finden Sie unter [Blob Service REST-API](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>Aktualisieren der AssetFile 

Nun, dass Sie die Datei hochgeladen haben, aktualisieren Sie die Informationen FileAsset Größe (und andere). Zum Beispiel:
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-Antwort**

Wenn erfolgreich, wird Folgendes zurückgegeben: HTTP/1.1 204 kein Inhalt

### <a name="delete-the-locator-and-accesspolicy"></a>Locator und AccessPolicy löschen 

**HTTP-Anforderung**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**HTTP-Antwort**

Wenn erfolgreich, wird Folgendes zurückgegeben:

    HTTP/1.1 204 No Content 
    ...

**HTTP-Anforderung**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**HTTP-Antwort**

Wenn erfolgreich, wird Folgendes zurückgegeben:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Anlagen gebündelt hochladen

###<a name="create-the-ingestmanifest"></a>Erstellen der IngestManifest

Die IngestManifest ist ein Container für eine Gruppe von Ressourcen, Objektdateien und statistische Informationen, die mit den Fortschritt Bulk Einnahme für den Satz zu bestimmen.


**HTTP-Anforderung**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Erstellen

Vor dem Erstellen der IngestManifestAsset, müssen Sie die Anlage erstellen, die mit Bulk Einnahme abgeschlossen wird. Ein Vermögenswert ist ein Container für mehrere Arten oder Gruppen von Objekten in Media Services wie Video, Audio, Bilder, Miniaturansichten Sammlungen, Textspuren und Untertitel-Dateien. In der REST-API erfordert das Erstellen einer Anlage senden einer HTTP POST-Anforderung an Microsoft Azure Media Services und Eigenschaftsinformationen über die Anlage in den Hauptteil der Anforderung. In diesem Beispiel wird die Anlage mit der Anforderungstext enthaltene StorageEncrption(1)-Option erstellt.

**HTTP-Antwort**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>Erstellen der IngestManifestAssets

IngestManifestAssets stellen Ressourcen innerhalb einer IngestManifest Bulk Einnahme verwendet werden. Die Anlage im Wesentlichen dem Manifest verknüpfen. Azure Media Services überwacht intern für den Dateiupload basiert auf IngestManifestFiles, der IngestManifestAsset zugeordnet ist. Nachdem diese Dateien hochgeladen werden, ist die Anlage abgeschlossen. Sie können eine neue IngestManifestAsset mit einem HTTP POST-Anforderung erstellen. Enthalten Sie im Hauptteil Anforderung die IngestManifest und Anlage-Id, die das IngestManifestAsset für Massen Einnahme verbinden soll.

**HTTP-Antwort**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>Die IngestManifestFiles für jede Anlage erstellen

Eine IngestManifestFile stellt ein tatsächliche Video- oder Blob-Objekt, das als Teil der Bulk Einnahme für eine Anlage hochgeladen wird. Verschlüsselung-bezogenen Eigenschaften nicht erforderlich sind, sofern die Anlage eine Verschlüsselungsoption verwenden. Das Beispiel in diesem Abschnitt veranschaulicht, StorageEncryption ein IngestManifestFile erstellen, für die zuvor erstellte Anlage verwendet werden.


**HTTP-Antwort**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>Laden Sie die Dateien in den BLOB-Speicher

Sie können Clientanwendungen hoher Geschwindigkeit kann auf die BLOB-Speicher Behälter Uri von der BlobStorageUriForUpload-Eigenschaft der IngestManifest Asset Upload. Eine wichtige Hochgeschwindigkeits-Upload-Dienst wird [Aspera auf Azure-Anwendung](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Monitor Masse Aufnahme Fortschritt

Sie können den Fortschritt der Vorgänge für ein IngestManifest durch Abrufen der Statistik-Eigenschaft der IngestManifest Einnahme Bulk überwachen. Diese Eigenschaft ist ein komplexer Typ [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). Zum Abrufen der Statistik-Eigenschaft Senden einer HTTP GET-Anforderung übergeben IngestManifest Id.
 

##<a name="create-contentkeys-used-for-encryption"></a>Erstellen von ContentKeys für die Verschlüsselung

Wenn die Anlage Verschlüsselung verwenden, legen Sie ContentKey für die Verschlüsselung vor dem Erstellen der Ressource-Dateien verwendet werden. Für speicherverschlüsselung sollte die folgenden Eigenschaften im Hauptteil Anforderung enthalten sein.
 
Anforderung Body-Eigenschaft   | Beschreibung
---|---
ID | Die ContentKey-Id die generieren wir uns mit dem folgenden Format "Nb:kid:UUID:<NEW GUID>".
ContentKeyType | Ist der Schlüssel Inhaltstyp als Integer für diesen Inhalt Schlüssel Den Wert 1 für speicherverschlüsselung übergeben.
EncryptedContentKey | Wir erstellen einen neuen Content-Wert (32 Byte) 256-Bit-Wert ist. Der Schlüssel wird verschlüsselt mit Speicher Verschlüsselung x. 509-Zertifikat dem wir eine HTTP GET-Anforderung für die GetProtectionKeyId und GetProtectionKey-Methoden Ausführen von Microsoft Azure Media Services abrufen.
ProtectionKeyId | Dies ist die Key-Kennzeichnung für Speicher Verschlüsselung x. 509-Zertifikat, das verwendet wurde, um unsere Schlüssel verschlüsseln.
ProtectionKeyType | Dies ist der Verschlüsselungstyp für die Schlüssel, mit dem der Inhalten Schlüssel verschlüsselt. Dieser Wert ist StorageEncryption(1) Beispiel.
Prüfsumme |Die berechnete MD5-Prüfsumme für die Content. Er wird berechnet durch Content Id mit symmetrischen Schlüssel verschlüsseln. Im Codebeispiel wird veranschaulicht, wie die Prüfsumme berechnet.


**HTTP-Antwort**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>Verknüpfen der ContentKey mit der Anlage

Der ContentKey wird eine oder mehrere Anlagen durch Senden einer HTTP POST-Anforderung. Bitte sehen Sie Beispiel ContentKey Beispiel Anlage-ID verknüpfen

**HTTP-Antwort**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**HTTP-Antwort**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Nächstes

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
