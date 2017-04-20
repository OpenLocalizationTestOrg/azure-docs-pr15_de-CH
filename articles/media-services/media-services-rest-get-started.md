<properties 
    pageTitle="Erste Schritte mit Inhalt bei Bedarf mit anderen | Microsoft Azure" 
    description="Dieses Lernprogramm führt Sie schrittweise eine auf Anforderung Inhalten Anwendung mit Azure Media Services über REST-API implementieren." 
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
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Erste Schritte mit Inhalt bei Bedarf mit anderen 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](/pricing/free-trial/?WT.mc_id=A261C142F). 


Dieser Schnellstart führt Sie durch die Schritte zum Implementieren einer Demand (VoD) Inhalten Anwendung Azure Media Services (AMS) REST-APIs. 

Das Lernprogramm führt grundlegende Media Services Workflow und häufigsten Programmierobjekte und Aufgaben für die Entwicklung von Media Services erforderlich. Nach Abschluss des Lernprogramms werden Sie zu streamen oder progressives Herunterladen eine Media-Beispieldatei, die hochgeladen, codiert und heruntergeladen.  

## <a name="prerequisites"></a>Erforderliche Komponenten
Die folgenden erforderlichen Komponenten müssen Entwickeln mit Media Services REST-APIs.

- Kenntnisse über die Entwicklung mit Media Services REST-API. Weitere Informationen finden Sie unter [Media-Services-Rest-Übersicht](http://msdn.microsoft.com/library/azure/hh973616.aspx).
- Eine Anwendung Ihrer Wahl, die HTTP-Anfragen und Antworten senden können. Diese praktische Einführung verwendet [Fiddler](http://www.telerik.com/download/fiddler). 

Die folgenden Aufgaben werden in diesem Schnellstart angezeigt.

1.  Erstellen Sie ein Media Services-Konto (mit Azure-Portal).
2.  Konfigurieren Sie Streaming-Endpunkt (mit Azure-Portal).
1.  Verbinden Sie mit Media Services Account mit REST-API.
1.  Erstellen einer neuen Anlage und eine Videodatei mit REST-API.
1.  Konfigurieren Sie Streaming-Einheiten mit REST-API.
2.  Codieren Sie die Quelldatei in adaptive Bitrate MP4-Dateien mit REST-API.
1.  Veröffentlichen Sie Anlage und Get streaming und progressiven Download URLs mit REST-API. 
1.  Wiedergeben des Inhalts. 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Erstellen Sie ein Azure-Portal mit Azure Media Services-Konto

Die Schritte in diesem Abschnitt zeigen, wie ein AMS-Konto erstellen.

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com/).
2. Klicken Sie auf **+ Neuer** > **Media + CDN** > **Media Services**.

    ![Media Services erstellen](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Geben Sie die gewünschten Werte, in **MEDIA SERVICES-Konto erstellen** .

    ![Media Services erstellen](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Geben Sie den Namen des neuen AMS **Kontonamen**. Ein Kontoname Media Services ist alle Kleinbuchstaben Zahlen oder Buchstaben ohne Leerzeichen und 3 bis 24 Zeichen lang ist.
    2. Wählen Sie im Abonnement unter verschiedenen Azure-Abonnements, denen Sie Zugriff haben.
    
    2. **Ressourcengruppe**wählen Sie die neue oder vorhandene Ressource.  Eine Ressourcengruppe ist eine Sammlung von Ressourcen, die Lebenszyklus, Berechtigungen und Richtlinien. Weitere Informationen [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Speicherort**dient Auswählen der Region die Medien- und Metadaten für Ihr Media Services-Konto gespeichert. Dieser Bereich wird zum Verarbeiten und Streamen von Medien. Die verfügbaren Media Services Bereiche werden im Dropdown-Listenfeld angezeigt. 
    
    3. Wählen Sie **Konto**ein Speicherkonto BLOB-Speicher der Medieninhalte von der Media Services-Konto. Auswählen eines vorhandenen Speicherkontos in derselben geographischen Region als Media Services-Konto oder ein Speicherkonto erstellen. Ein neues Speicherkonto wird in derselben Region erstellt. Die Regeln für Namen Speicher entsprechen Media Services-Konten.

        Erfahren Sie mehr über Speicher [hier](storage-introduction.md).

    4. **Pin Dashboard** die Fortschritte der Bereitstellung Konto auswählen
    
7. Klicken Sie am unteren Rand des Formulars **Erstellen** .

    Sobald das Konto erfolgreich erstellt wurde, wird der Status **ausgeführt**. 

    ![Media Services settings](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Das AMS-Konto verwalten (z. B. Hochladen Videos Codieren von Ressourcen, Überwachung des Projektstatus) **Settings** Fenster.

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurieren Sie Streaming-Endpunkte mithilfe von Azure-portal

Arbeit mit Azure Media Services, die eines der häufigsten Szenarios adaptive Bitrate streaming Video an die Clients übermittelt. Media Services unterstützt die folgende adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern).

Media Services bietet dynamische Verpackung der adaptive Bitrate codiert MP4 Inhalt streaming-Formate von Media Services (MPEG DASH HLS Smooth Streaming, HDS) just-in-Time, ohne vorkonfigurierte Versionen dieser streaming-Formaten speichern zu kann.

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- Codieren Sie die Aufsteckkarte (Quelle) Datei in adaptive Bitrate MP4-Dateien (Codierung Schritte werden später in diesem Lernprogramm gezeigt).  
- Erstellen Sie mindestens eine Streaming-Einheit für die *streaming-Endpunkt* aus der Lieferung des Inhalts werden soll. Schritte anzeigen zum Ändern der Anzahl der Streaming-Einheiten

Dynamische Verpackung müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen mit Media Services erstellt und die entsprechende Antwort auf Anfragen von einem Client fungiert.

Um die Anzahl der reservierte Einheiten streaming zu erstellen, führen Sie folgende Schritte aus:


1. Klicken Sie im Fenster **Eigenschaften** auf **Streaming-Endpunkte**. 

2. Klicken Sie auf die Standardeinstellungen für streaming-Endpunkt. 

    **STREAMING ENDPUNKTDATEN STANDARDMÄßIG** angezeigt.

3. Ziehen Sie zum Angeben der Streaming-Einheiten Schieberegler **Streaming-Einheiten** .

    ![Streaming-Einheiten](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klicken Sie auf **Speichern** , um die Änderung zu speichern.

    >[AZURE.NOTE]Die Zuordnung neuer Einheiten kann bis zu 20 Minuten dauern.

## <a id="connect"></a>Der Media Services Konto mit REST-API

Zwei Dinge sind beim Zugriff auf Azure Media Services erforderlich: ein Zugriffstoken von Azure Access Control Service (ACS) und URI Media Services selbst bereitgestellt. Mitteln können, wann diese Anfragen erstellen, geben Sie den richtigen Headerwerte und übergeben im Zugriffstoken ordnungsgemäß beim Aufrufen in Media Services.

Die folgenden Schritte beschreiben den häufigsten Workflow, wenn Media Services REST-API mit Media Services herstellen:

1. Ein Zugriffstoken abrufen. 
2. Verbindung mit Media Services URI.  

    Beachten Sie, dass nach dem erfolgreichen Verbindungsaufbau zu https://media.windows.net 301-Umleitung einer anderen Media Services-URI angegeben wird. Nachfolgende Aufrufe von neuen URI müssen. Sie erhalten auch eine HTTP/1.1 200-Antwort mit der ODATA-API Metadatenbeschreibung.
3. Buchen die weiteren API-Aufrufe auf die neue URL. 
    
    Nach Aufbau der Verbindung erhalten Sie z. B. die folgenden:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Sie sollten die folgenden API-Aufrufe https://wamsbayclus001rest-hs.cloudapp.net/api/ buchen.

###<a name="getting-an-access-token"></a>Ein Zugriffstoken abrufen

Media Services direkt über die REST-API zugreifen von ACS Zugriffstoken abrufen und bei jeder HTTP-Anforderung, den Dienst zu, verwenden. Andere erforderliche Komponenten brauchen nicht vor an Media Services.

Das folgende Beispiel zeigt die HTTP-Anfrage-Header und Body einen Token abgerufen.

**Header**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Text**:

Sie müssen die Werte Client_id und Client_secret im Text der Anforderung war. Client_id und Client_secret entsprechen den Werten Kontoname und AccountKey. Diese Werte erhalten Sie von Media Services beim Einrichten Ihres Kontos. 

AccountKey für Ihr Konto Media Services muss URL-codierte Verwendung als Client_secret Wert in Ihre Anfrage token.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Zum Beispiel: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Das folgende Beispiel zeigt die HTTP-Antwort, die den Zugriff im Antworttext.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Es wird empfohlen, die Werte "Zugriffstoken" und "Expires_in" ein externes Speichergerät Zwischenspeichern. Token Daten können später aus dem Speicher abgerufen und wieder in die Media Services REST API-Aufrufe verwendet. Dies ist besonders für Szenarien, in denen das Token sicher zwischen mehreren Prozessen oder Computern gemeinsam genutzt werden kann.

Sollten Sie den Wert "Expires_in" des Zugriffstokens überwachen und aktualisieren die REST API-Aufrufe mit neuen Token nach Bedarf.

###<a name="connecting-to-the-media-services-uri"></a>Herstellen einer Verbindung mit Media Services URI

Der Stamm-URI für Media Services ist https://media.windows.net/. Sie sollten zunächst eine Verbindung mit dieser URI und man eine 301-Umleitung als Antwort sollten Sie nachfolgende Aufrufe der neue URI. Darüber hinaus verwenden Sie automatische Umleitung/folgen Logik in Ihre Anfragen. HTTP-Verben und Anforderung stellen werden nicht an den neuen URI weitergeleitet.

Stamm-URI für das Hochladen und Herunterladen von Anlagen ist https://yourstorageaccount.blob.core.windows.net/ Storage-Kontonamen, während Ihr Media Services Account Setup verwendet.

Das folgende Beispiel veranschaulicht die HTTP-Anforderung an den Media Services Stamm-URI (https://media.windows.net/). Die Anforderung wird als Antwort eine 301-Umleitung. Die nachfolgende Anforderung verwendet den neuen URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-Anforderung**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP-Antwort**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP-Anforderung** (mithilfe den neuen URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-Antwort**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Von nun an wird der neue URI in diesem Lernprogramm verwendet.

## <a id="upload"></a>Erstellen Sie eine neue Anlage und einer Videodatei mit REST-API

Media Services wird Ihre digitalen Dateien zu laden. Die **Aktivposten** -Entität darf Video, Audio, Bilder, Miniaturansichten Sammlungen, Text Titel und Untertitel Dateien (und Metadaten Dateien.)  Nach Dateien in der Anlage hochgeladen werden, wird der Inhalt in der Cloud zur weiteren Verarbeitung und streaming sicher. 

Einer der Werte, die Sie beim Erstellen einer Anlage Anlage Optionen bereitstellen. Die **Options** -Eigenschaft ist ein Enumerationswert, der die Verschlüsselungsoptionen beschreibt, denen mit eine Anlage erstellt werden kann. Ein gültiger Wert ist einer der Werte aus der Liste keine Kombination von Werten aus dieser Liste:

 
- **Keine** = **0** - keine Verschlüsselung verwendet. Bei Verwendung dieser Option wird der Inhalt nicht während der Übertragung oder im Speicher geschützt.
    Möchten Sie ein MP4 liefern progressives Herunterladen verwendet diese Option. 
- **StorageEncrypted** = **1** - klare Inhalte lokal mit AES-256-Bit-Verschlüsselung verschlüsselt und lädt diese in Azure-Speicher gespeichert ist statisch verschlüsselt. Mit Storage Verschlüsselung geschützt werden automatisch entschlüsselt ein verschlüsseltes Dateisystem vor Codierung platziert und optional vor dem Hochladen als eine neue Ausgabe Anlage erneut verschlüsselt. Primäre Nutzung bei Speicher-Verschlüsselung ist, wenn Ihre Eingabe Medien in hoher Qualität mit starker Verschlüsselung ruhender auf Datenträger sichern.
- **CommonEncryptionProtected** = **2** - verwenden Sie diese option, wenn Sie Inhalte hochladen, die bereits verschlüsselt und mit gemeinsamen Verschlüsselung oder PlayReady-DRM (z. B. Smooth Streaming geschützt mit PlayReady-DRM) geschützt.
- **EnvelopeEncryptionProtected** = **4** – verwenden Sie diese Option, wenn Sie mit AES verschlüsselt HLS hochladen. Die Dateien müssen codiert und Transformieren Manager verschlüsselt.

### <a name="create-an-asset"></a>Eine Anlage

Ein Vermögenswert ist ein Container für mehrere Arten oder Gruppen von Objekten in Media Services wie Video, Audio, Bilder, Miniaturansichten Sammlungen, Textspuren und Untertitel-Dateien. In REST-API erfordert das Erstellen einer Anlage an Media Services POST-Anforderung senden und Informationen zu Ihrer Ressource im Hauptteil Anforderung platziert.

Im folgende Beispiel veranschaulicht eine Anlage erstellen.

**HTTP-Anforderung**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

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
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Erstellen einer AssetFile

[AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) -Entität stellt eine Video- oder audio-Datei, die in einem BLOB-Container gespeichert. Eine Bilddatei ist immer Anlagen und Anlage enthalten einen oder mehrere AssetFiles. Media Services Encoder-Task fehlschlägt, wenn ein Dateiobjekt Anlage nicht digitale Datei in einem BLOB-Container zugeordnet ist.

Nach dem Hochladen der digitalen Mediendatei in einem BLOB-Container verwenden Sie **ZUSAMMENFÜHREN** HTTP-Anforderung zum Aktualisieren der AssetFile mit Informationen über die Mediendatei (siehe weiter unten). 

**HTTP-Anforderung**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
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

Vor dem Upload in BLOB-Speicher, festlegen Sie den Zugriff richtlinienrechte zum Schreiben in eine Anlage. Buchen Sie dazu eine HTTP-Anforderung an AccessPolicies-Entitätenmenge. Definieren Sie einen DurationInMinutes-Wert beim Erstellen oder Sie Fehlermeldung 500 internen Server wieder auf. Weitere Informationen zu AccessPolicies finden Sie unter [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Im folgenden Beispiel wird veranschaulicht, wie ein AccessPolicy erstellen:
        
**HTTP-Anforderung**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-Antwort**

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
    X-Powered-By: ASP.NET
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

### <a name="get-the-upload-url"></a>Rufen Sie die Upload-URL

Um die tatsächliche Upload-URL erhalten, erstellen Sie SAS-Locator Locator definieren die Startzeit und die Art der Verbindungsendpunkt für Clients, die Dateien in einer Anlage zugreifen möchten. Sie können mehrere Locator Entitäten für ein angegebenen AccessPolicy und Ressourcen mit anderen Clients und erstellen. Jede dieser Locators verwendet StartTime-Wert plus der DurationInMinutes von der AccessPolicy bestimmt die Zeitdauer eine URL verwendet werden kann. Weitere Informationen finden Sie im [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


SAS-URL hat das folgende Format:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Einige zu berücksichtigen:

- Sie können nicht mehr als fünf eindeutige Locators Assets gleichzeitig zugeordnet haben. Weitere Informationen finden Sie im Locator.
- Benötigen Sie die Dateien sofort hochzuladen, sollten Sie fünf Minuten vor der aktuellen Zeit StartTime-Wert fest. Dies ist möglicherweise Clock skew zwischen dem Clientcomputer und Media-Dienste. StartTime-Wert muss auch im folgenden DateTime-Format: JJJJ-MM-: ssZ (z. B. "2014-05-23T17:53:50Z").   
- Möglicherweise 30 bis 40 Sekunden Verzögerung nach ein Locator erstellt wird, wenn es verfügbar ist. Dieses Problem betrifft SAS-URL und Ursprung Locators.

Im folgenden Codebeispiel einen Locator SAS-URL erstellen gemäß der Type-Eigenschaft im Hauptteil Anforderung (für SAS-Locator "1") und "2" für einen auf-Anforderung-Ursprung-Locator. Die **Path** -Eigenschaft zurückgegebene enthält die URL, mit denen Sie Ihre Datei hochladen müssen.
    
**HTTP-Anforderung**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
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
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-Antwort**

Wenn erfolgreich, wird Folgendes zurückgegeben: HTTP/1.1 204 kein Inhalt

## <a name="delete-the-locator-and-accesspolicy"></a>Locator und AccessPolicy löschen 

**HTTP-Anforderung**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**HTTP-Antwort**

Wenn erfolgreich, wird Folgendes zurückgegeben:

    HTTP/1.1 204 No Content 
    ...

**HTTP-Anforderung**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-Antwort**

Wenn erfolgreich, wird Folgendes zurückgegeben:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>REST-API Streaming-Einheiten konfigurieren

Arbeit mit Azure Media Services, die eines der häufigsten Szenarios adaptives streaming Bitrate an die Clients übermittelt. Mit adaptiver Bitrate streaming können Client auf einen höheren oder niedrigeren Bitrate Stream Videos angezeigt wird die aktuelle Netzwerkbandbreite, CPU-Auslastung und anderen Faktoren abhängig. Media Services unterstützt die folgende adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern). 

Media Services bietet dynamische Verpackung des adaptive Bitrate MP4 oder Smooth Streaming codierten Inhalts im streaming-Formate von Media Services (MPEG DASH HLS Smooth Streaming, HDS) ohne Verpacken in diese Formate streamen zu kann. 

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- erhalten Sie mindestens eine Streaming-Einheit für die **streaming-Endpunkt **zu den Inhalt (in diesem Abschnitt beschrieben) möchten.
- Codieren oder Transcode der Aufsteckkarte (Quelle) in eine Reihe von adaptiven Bitrate MP4 oder adaptive Bitrate Smooth Streaming-Dateien (Codierung Schritte werden später in diesem Lernprogramm gezeigt) Datei  

Dynamische Verpackung müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen mit Media Services erstellt und die entsprechende Antwort auf Anfragen von einem Client fungiert. 


>[AZURE.NOTE] Informationen über Preise finden Sie in der [Media-Preisdetails](http://go.microsoft.com/fwlink/?LinkId=275107).

Ändern Sie die Anzahl der reservierte Einheiten streaming folgendermaßen Sie vor:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Abrufen des Streaming-Endpunkts zu aktualisieren

Lass z. B. ersten Streaming-Endpunkt in Ihrem Konto (Sie können bis zu zwei streaming Endpunkte im Ausführungszustand gleichzeitig haben.)

**HTTP-Anforderung**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-Antwort**
    
Wenn erfolgreich, wird Folgendes zurückgegeben:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Streaming-Endpunkt skalieren
 
**HTTP-Anforderung**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**HTTP-Antwort**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>Überprüfen Sie den Status einer langwierigen Vorgang

Die Zuordnung neuer Einheiten dauert etwa 20 Minuten. Überprüfen Sie den Status der Operation **Operations** -Methode, und geben Sie die Id des Vorgangs. Der Vorgang wurde auf Anforderung **Skalierung** Id zurückgegeben.

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**HTTP-Anforderung**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**HTTP-Antwort**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>Codieren Sie die Quelldatei in adaptive Bitrate MP4-Dateien

Nach Einnahme, Anlagen in Media Services Medien codiert werden, Transmuxed mit einem Wasserzeichen versehen, und, bevor es Clients erfolgt. Diese Aktivitäten sind geplant und Ausführen mehrerer Instanzen Hintergrund, hohe Performance und Verfügbarkeit zu gewährleisten. Diese Aktivitäten werden Aufträge genannt, und jeder [Auftrag](http://msdn.microsoft.com/library/azure/hh974289.aspx) besteht aus atomare Vorgänge, die die eigentliche auf die Bilddatei Arbeit. 

Wie bereits erwähnt, beim Arbeiten mit Azure Media Services eine der häufigsten adaptive Bitrate auf Ihre Kunden liefert. Media Services können dynamisch Packen adaptive Bitrate MP4-Dateien in einem der folgenden Formate: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern). 

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- Codieren oder Transcode der Aufsteckkarte (Quelle) Datei in eine Reihe von adaptiven Bitrate MP4 oder adaptive Bitrate Smooth Streaming-Dateien  
- Rufen Sie mindestens eine Streaming-Einheit für Streaming-Endpunkt zu Ihrem Inhalt soll. 

Der folgende Abschnitt zeigt ein Projekt erstellen, die ein Kodierung Aufgabe enthält. Die Aufgabe gibt transcodiert Mezzanine-Datei in eine Reihe von adaptiven Bitrate MP4s **Media Encoder Standard**verwenden. Der Abschnitt wird gezeigt, wie Verarbeitungsvorgang Projekt überwachen. Wenn der Auftrag abgeschlossen ist, würden Sie möglicherweise Locator erstellen, die Zugriff auf Ihre Ressourcen zu erhalten. 

### <a name="get-a-media-processor"></a>Ein Media Prozessor

Media Services ist ein Medienprozessor eine Komponente, die eine bestimmte Verarbeitungsaufgabe Formatumwandlung verschlüsseln oder entschlüsseln Medieninhalte Codierung behandelt. In diesem Lernprogramm gezeigt Codierung Aufgabe werden wir Media Encoder Standard verwenden.

Der folgende Code fordert die Encoder-Id an. 

**HTTP-Anforderung**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-Antwort**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Erstellen Sie einen Auftrag

Jeder Auftrag können eine oder mehrere Aufgaben je nach der Art der Verarbeitung, die Sie erreichen möchten. Durch die REST-API können Aufträge und die Aufgaben auf zwei Arten: Aufgaben werden können über Aufgaben Navigationseigenschaft Auftrag Entitäten oder OData-Stapelverarbeitung. Media Services SDK verwendet die Stapelverarbeitung. Für die Lesbarkeit der Codebeispiele in diesem Thema sind Aufgaben jedoch Inline definiert. Informationen über Stapelverarbeitung finden Sie in der [Stapelverarbeitung Open Data Protocol (OData)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Im folgende Beispiel wird das Erstellen und Buchen eines Auftrags mit einem Vorgang legen Sie ein Video mit einer bestimmten Auflösung und Qualität codiert veranschaulicht. Die folgende Dokumentation enthält die Liste alle die [Aufgabe Vorgaben](http://msdn.microsoft.com/library/mt269960) unterstützt Media Encoder Standard-Prozessor.  

**HTTP-Anforderung**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-Antwort**

Wenn erfolgreich, wird die folgende Antwort zurückgegeben:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Es gibt einige wichtige Dinge in jeder auftragsanforderung beachten:

- TaskBody Eigenschaften müssen XML-literal verwenden, legen Sie die Anzahl der Eingabe oder Ausgabe von Anlagen, die vom Task verwendet werden. Aufgabenthema enthält die XML-Schemadefinition für XML.
- Definition der TaskBody jeder inneren Wert für <inputAsset> und <outputAsset> JobInputAsset(value) oder JobOutputAsset(value) festgelegt werden.
- Eine Aufgabe kann mehrere Ausgabe-Anlagen haben. Eine Joboutputasset(x)-Objekt kann nur einmal als Ausgabe einer Aufgabe in einem Projekt verwendet werden.
- Sie können JobInputAsset oder JobOutputAsset als Vermögenswert Eingabe eines Vorgangs angeben.
- Aufgaben müssen keine Schleife bilden.
- Der Wertparameter an JobInputAsset oder JobOutputAsset darstellt den Indexwert für eine Anlage. Die tatsächlichen Vermögenswerte werden in der Eigenschaften InputMediaAssets und OutputMediaAssets auf Auftragsdefinition Entität definiert. 

>[AZURE.NOTE] Da Media Services OData v3 erstellt wird, wird die einzelnen Ressourcen in InputMediaAssets und OutputMediaAssets Navigation Eigenschaftsauswahlen mit einem "__metadata: Uri" Name-Wert-Paar. 

- InputMediaAssets ordnet eine oder mehrere Anlagen in Media Services erstellt haben. OutputMediaAssets werden vom System erstellt. Sie verweisen nicht auf eine vorhandene Anlage.
- OutputMediaAssets können mit dem Zweck der Ausgabe Attribut benannt werden. Dieses Attribut ist nicht vorhanden, wenn der Name der OutputMediaAsset wird der innere Text der Wert der <outputAsset> Element ist mit einem Suffix Auftragsname Wert oder die Auftrags-Id-Wert (in dem Fall, in dem die Name-Eigenschaft ist nicht definiert). Setzt einen Wert für "Beispiel" Zweck der Ausgabe, würden die OutputMediaAsset Name-Eigenschaft ebenfalls nach "Sample" festgelegt werden. Wenn Sie keinen Wert für Zweck der Ausgabe festlegen, aber hat den Auftragsnamen an "NewJob" festgelegt, würde der OutputMediaAsset Name, "_NewJob JobOutputAsset (Wert)" sein. 

    Das folgende Beispiel zeigt, wie Zweck der Ausgabe-Attribut festgelegt:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Verketten von Task aktivieren:

    - Ein Auftrag muss mindestens zwei Aufgaben
    - Es muss mindestens eine Aufgabe, deren Eingabe Ausgabe eines anderen Vorgangs im Auftrag.

Weitere Informationen finden Sie unter [Erstellen eines Auftrags Codierung mit Media Services REST-API](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Verarbeitungsvorgang Monitor

Sie können den Status abrufen, mithilfe der State-Eigenschaft, wie im folgenden Beispiel gezeigt. 

**HTTP-Anforderung**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-Antwort**

Wenn erfolgreich, wird die folgende Antwort zurückgegeben:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Abbrechen eines Auftrags

Media Services können Sie über die CancelJob Funktion ausgeführten Aufträge abbrechen. Dieser Aufruf gibt einen 400 Fehlercode beim Abbrechen eines Auftrags, wenn Zustand abgebrochen wird abgebrochen, Fehler oder beendet.

Im folgenden Beispiel wird veranschaulicht, wie CancelJob aufrufen.


**HTTP-Anforderung**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Bei Erfolg wird kein Nachrichtentext 204 Antwortcode zurückgegeben.

>[AZURE.NOTE] Müssen Sie URL-Kodierung die Auftrags-Id (normalerweise Nb:jid:UUID: Somevalue), wenn es als Parameter an CancelJob übergeben.


### <a name="get-the-output-asset"></a>Abrufen der Ausgabe-Anlage 

Im folgenden Codebeispiel wird veranschaulicht, wie die Anlage Ausgabe ID 


**HTTP-Anforderung**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-Antwort**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Veröffentlichen Sie Anlage und Get streaming und progressiven Download URLs mit REST-API

Um stream oder Anlage herunterzuladen, müssen Sie "Veröffentlichen" einen Locator erstellen. Locators ermöglichen den Zugriff auf Dateien in der Anlage. Media Services unterstützt zwei Arten von Locators: OnDemandOrigin Locator zum Streamen von Medien (z. B. MPEG DASH, HLS oder Smooth Streaming) und Access Signatur (SAS)-Locators Mediendateien verwendet.

Nach dem Erstellen der Locators können Sie URLs erstellen, die zum Streamen oder Dateien herunterladen. 


Streaming-URL für Smooth Streaming hat das folgende Format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Streaming-URL für HLS hat das folgende Format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Streaming-URL für MPEG DASH hat das folgende Format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


SAS-URL zum Herunterladen hat das folgende Format:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Dieser Abschnitt veranschaulicht die folgenden Aufgaben müssen Ihre "Veröffentlichen".  

- Erstellen die AccessPolicy mit Leseberechtigung 
- Erstellen einer URL SAS für herunterladen 
- Ursprungs-URL erstellen für streaming-Inhalte 

###<a name="creating-the-accesspolicy-with-read-permission"></a>Erstellen die AccessPolicy mit Leseberechtigung

Vor dem Herunterladen oder streaming Media-Inhalte definieren Sie zunächst AccessPolicy mit Leseberechtigungen und erstellen Sie Locator Einheit, die Sie für Ihre Kunden aktivieren möchten Mechanismus angibt. Weitere Informationen zu den verfügbaren Eigenschaften finden Sie unter [Elementeigenschaften AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

Im folgenden Beispiel wird veranschaulicht, wie AccessPolicy für Leseberechtigungen für Assets an.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Bei Erfolg wird 201 Erfolgscode zurückgegeben beschreiben die AccessPolicy-Entität, die Sie erstellt haben. Verwenden Sie die AccessPolicy-Id mit der Aktiva-Id der Anlage mit der Datei (z. B. Anlage Ausgabe) liefern Locator Entität erstellen möchten.

>[AZURE.NOTE]
Dieser grundlegende Workflow entspricht dem Hochladen einer Datei als Anlage (wie weiter oben in diesem Thema behandelten) Einnahme. Außerdem wie Hochladen von Dateien, wenn Sie (oder Clients) sofort auf Ihre Dateien zugreifen müssen, festgelegt StartTime-Wert auf fünf Minuten vor der aktuellen Zeit. Dieser Vorgang ist notwendig, weil möglicherweise Clock skew zwischen Client und Media-Dienste. StartTime-Wert muss im folgenden DateTime-Format: JJJJ-MM-: ssZ (z. B. "2014-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>Erstellen einer URL SAS für herunterladen 

Der folgende Code veranschaulicht, wie zu einer URL, die mit eine Mediendatei erstellt und geuploadet wurde zuvor herunterladen. Die AccessPolicy hat Berechtigungssatz und der Locator Pfad bezieht sich auf einen SAS-Download-URL.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Wenn erfolgreich, wird die folgende Antwort zurückgegeben:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


Die zurückgegebene **Path** -Eigenschaft enthält die SAS-URL.

>[AZURE.NOTE]
Wenn Sie verschlüsselten Inhalt herunterladen, müssen Sie manuell vor dem Rendern entschlüsseln oder MediaProcessor Entschlüsselung Speicher in einer Verarbeitungsaufgabe verarbeitete Dateien in Klartext, ein OutputAsset und download von dieser Anlage verwenden. Weitere Informationen zur Verarbeitung finden Sie unter Erstellen eines Auftrags Codierung mit Media Services REST-API. SAS-URL Locators kann auch aktualisiert werden, nachdem sie erstellt wurden. Beispielsweise kann nicht den gleichen Locator mit einem aktualisierten StartTime-Wert wiederverwenden. Dies ist aufgrund der SAS-URLs erstellt werden. Wenn Sie eine Anlage zum Download nach Ablauf ein Locators zugreifen möchten, müssen Sie eine neue Startzeit erstellen.

###<a name="download-files"></a>Dateien herunterladen

Sobald Sie AccessPolicy und Locator festgelegt haben, laden Sie Dateien mit Azure Storage REST-APIs.  

>[AZURE.NOTE] Sie müssen den Dateinamen für die Datei hinzufügen Locator **Pfad** erhaltene im vorherigen Abschnitt herunterladen möchten. Beispielsweise https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Weitere Informationen zum Arbeiten mit Azure-Speicher-Blobs finden Sie unter [Blob Service REST-API](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Durch die Codierungsauftrags früheren (Codierung zu Adaptive MP4) ausgeführt, müssen Sie mehrere MP4-Dateien, die Sie progressiv herunterladen können. Zum Beispiel:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Erstellen eine Streaming-URL für streaming-Inhalte


Der folgende Code zeigt, wie streaming URL Locator erstellen:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Wenn erfolgreich, wird die folgende Antwort zurückgegeben:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Zum Übertragen einer Smooth Streaming Ursprungs-URL in einem streaming MediaPlayer müssen Sie den Pfad mit den Namen der Smooth Streaming-Manifestdatei, gefolgt von "/ manifest" anfügen.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Zum Streamen HLS anfügen (Format = m3u8 Aapl) nach der "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Zum Streamen MPEG DASH anfügen (Format = Mpd-Zeit-csf) nach der "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Wiedergeben des Inhalts  

Verwenden Sie [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)zum Streamen von Videos.

Testen Sie progressiven Download fügen Sie eine URL in einem Browser (z. B. Internet Explorer, Chrome, Safari ein).


##<a name="next-steps-media-services-learning-paths"></a>Nächste Schritte: Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Sie suchen etwas?

Wenn dieses Thema enthielt, was Sie erwarten etwas fehlt, oder in andere Weise nicht Ihre Bedürfnisse, Bitte geben Sie Ihr Feedback mit den Disqus unten.



