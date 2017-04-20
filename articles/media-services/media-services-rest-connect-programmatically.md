<properties 
    pageTitle="Media Services-Konto mithilfe von REST-API verbinden | Microsoft Azure" 
    description="In diesem Thema veranschaulicht, wie Media Services Einzelhandelsabonnements REST API verbinden." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Herstellen einer Verbindung mit Media Services-Konto mit Media Services REST-API

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect-programmatically.md)
- [REST](media-services-rest-connect-programmatically.md)

So erhalten Sie eine programmgesteuerte Verbindung zu Microsoft Azure Media Services beim Programmieren mit Media Services REST-API beschrieben.

Zwei Dinge sind erforderlich, wenn Microsoft Azure Media Services zugreifen: Zugriffstoken von Azure Access Control Service (ACS) und URI Media Services selbst bereitgestellt. Mitteln können, wann diese Anfragen erstellen, geben Sie den richtigen Headerwerte und übergeben im Zugriffstoken ordnungsgemäß beim Aufrufen in Media Services.

Die folgenden Schritte beschreiben den häufigsten Workflow, wenn Media Services REST-API mit Media Services herstellen:

1. Ein Zugriffstoken abrufen 
2. Herstellen einer Verbindung mit Media Services URI 

    >[AZURE.NOTE] Nach dem erfolgreichen Verbindungsaufbau zu https://media.windows.net erhalten Sie eine 301-Umleitung einer anderen Media Services URI angegeben. Nachfolgende Aufrufe von neuen URI müssen.
Sie erhalten auch eine HTTP/1.1 200-Antwort mit der ODATA-API Metadatenbeschreibung.

3. Buchen Sie die weiteren API-Aufrufe auf die neue URL. 

    Nach Aufbau der Verbindung erhalten Sie z. B. die folgenden:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Sie sollten die folgenden API-Aufrufe https://wamsbayclus001rest-hs.cloudapp.net/api/ buchen.

##<a name="access-control-address"></a>Adresse

Media Services-Adresse ist https://wamsprodglobal001acs.accesscontrol.windows.net außer Nordchina Bereich ist der https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

##<a name="getting-an-access-token"></a>Ein Zugriffstoken abrufen

Media Services direkt über die REST-API zugreifen von ACS Zugriffstoken abrufen und bei jeder HTTP-Anforderung, den Dienst zu, verwenden. Dieses Token ist wie andere Token von ACS basierend auf Access Ansprüche gemäß den Header einer HTTP-Anforderung und das OAuth-v2-Protokoll bereitgestellt. Andere erforderliche Komponenten brauchen nicht vor an Media Services.

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

Sie müssen die Client_id und Client_secret Werte im Text der Anforderung. Client_id und Client_secret entsprechen den Werten Kontoname und AccountKey. Diese Werte erhalten Sie von Media Services beim Einrichten Ihres Kontos. 

Beachten Sie, dass AccountKey für Ihr Konto Media Services URL codiert (Siehe Verwendung als Client_secret Wert in Ihre Anfrage token [%-Codierung](http://tools.ietf.org/html/rfc3986#section-2.1) .

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
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Es wird empfohlen, die Werte "Zugriffstoken" und "Expires_in" ein externes Speichergerät Zwischenspeichern. Token Daten können später aus dem Speicher abgerufen und wieder in die Media Services REST API-Aufrufe verwendet. Dies ist besonders für Szenarien, in denen das Token sicher zwischen mehreren Prozessen oder Computern gemeinsam genutzt werden kann.

Sollten Sie den Wert "Expires_in" des Zugriffstokens überwachen und aktualisieren die REST API-Aufrufe mit neuen Token nach Bedarf.

###<a name="connecting-to-the-media-services-uri"></a>Herstellen einer Verbindung mit Media Services URI

Der Stamm-URI für Media Services ist https://media.windows.net/. Sie sollten zunächst eine Verbindung mit dieser URI und man eine 301-Umleitung als Antwort sollten Sie nachfolgende Aufrufe der neue URI. Darüber hinaus verwenden Sie automatische Umleitung/folgen Logik in Ihre Anfragen. HTTP-Verben und Anforderung stellen werden nicht an den neuen URI weitergeleitet.

Beachten Sie, dass der Stamm-URI für das Hochladen und Herunterladen von Anlagen https://yourstorageaccount.blob.core.windows.net/ Storage-Kontonamen ist, während Ihr Media Services Account Setup verwendet.

Das folgende Beispiel veranschaulicht die HTTP-Anforderung an den Media Services Stamm-URI (https://media.windows.net/). Die Anforderung wird als Antwort eine 301-Umleitung. Die nachfolgende Anforderung verwendet den neuen URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-Anforderung**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
     


>[AZURE.NOTE] Sobald erhalten den neuen URI ist der URI, der Kommunikation mit Media Services verwendet werden soll. 


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
