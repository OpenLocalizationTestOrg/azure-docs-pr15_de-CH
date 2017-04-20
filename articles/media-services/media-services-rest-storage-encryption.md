<properties 
    pageTitle="Verschlüsseln Ihre Inhalte mit Speicher-Verschlüsselung mit AMS-REST-API" 
    description="Erfahren Sie, wie Ihre Inhalte AMS REST APIs mit Speicher-Verschlüsselung verschlüsselt." 
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


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Verschlüsseln Ihre Inhalte mit Speicher-Verschlüsselung mit AMS-REST-API

Es wird dringend empfohlen, Inhalte lokal mit AES-256-Bit-Verschlüsselung verschlüsselt und dann zum Azure-Speicher, Speicherung, ruhende verschlüsselt hochladen.

Dieser Artikel Überblick AMS speicherverschlüsselung und zeigt, wie Sie den verschlüsselten Inhalt hochzuladen:

- Erstellen Sie einen symmetrischen Schlüssel.
- Erstellen einer Anlage. Legen Sie die AssetCreationOption auf StorageEncryption beim Erstellen der Anlage.

     Verschlüsselte Ressourcen müssen mit Schlüsseln.
- Verknüpfen Sie den Inhaltsschlüssel mit Anlage.  
- Die Verschlüsselung Parameter auf die AssetFile beziehen.
 
>[AZURE.NOTE]Ggf. verschlüsselten Speicherressourcen liefern müssen die Anlage Lieferung Richtlinie konfigurieren. Bevor die Anlage übertragen kann, streaming Server Speicher entschlüsselt und Inhalte der Richtlinie angegebenen Übermittlung mit streams. Weitere Informationen finden Sie unter [Anlage Richtlinien konfigurieren](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE] Beim Arbeiten mit Media Services REST-API berücksichtigen die folgenden Aspekte:
>
>Wenn Entitäten in Media Services zugreifen, müssen Sie bestimmten Feldern und Werten in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Setup für Media Services REST API-Entwicklung](media-services-rest-how-to-use.md).

>Nach dem erfolgreichen Verbindungsaufbau zu https://media.windows.net erhalten Sie eine 301-Umleitung einer anderen Media Services URI angegeben. Nachfolgende Aufrufe der neuen URI Siehe [Herstellen einer Verbindung mit Media Services REST-API verwenden](media-services-rest-connect-programmatically.md)müssen. 

##<a name="storage-encryption-overview"></a>Übersicht über Verschlüsselung 

AMS speicherverschlüsselung gilt für die gesamte Datei Verschlüsselung **AES CTR** im.  AES-CTR ist eine Blockchiffre, die Daten beliebiger Länge ohne Abstand verschlüsseln kann. Er arbeitet durch Verschlüsselung einen Leistungsindikator-Block mit dem AES-Algorithmus und XOR-Wert der Ausgabe von AES mit Daten ver-oder entschlüsseln.  Zähler Block verwendet wird durch den Wert der Initialisierungsvektor Byte 0 bis 7 der Indikatorwert kopieren erstellt und 8 bis 15 Bytes der Indikatorwert werden auf NULL gesetzt. Block Leistungsindikator 16 Byte werden Bytes 8 bis 15 (d. h. die niedrigstwertigen Byte) als eine einfache 64-Bit-Ganzzahl ohne Vorzeichen, die für jeden folgenden Datenblock um eins inkrementiert wird verarbeitet und ist in Netzwerk-Bytereihenfolge verwendet. Beachten Sie, dass diese Ganzzahl dem Maximalwert (0xFFFFFFFF) erhöhen sie setzt Block gegen Null (Bytes 8 bis 15) erreicht ohne die 64 Bit des Zählers (d. h. Byte 0 bis 7).   Um die Sicherheit der Verschlüsselung im AES-CTR verwalten Initialisierungsvektor Wert für einen angegebenen Schlüssel-ID für jeden Content Schlüssel für jede Datei und Dateien sind kleiner als 2 ^ 64 Blöcke enthalten.  Dadurch wird sichergestellt, dass ein mit einem bestimmten Schlüssel niemals wiederverwendet wird. Weitere Informationen über die CTR-Modus finden Sie in [dieser Wiki-Seite](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (Wiki-Artikel verwendet den Begriff "Nonce" statt "Initialisierungsvektor").

**Speicher-Verschlüsselung** klare Inhalte lokal mit AES-256-Bit-Verschlüsselung verschlüsselt verwenden und auf Azure-Speicher Speicherort verschlüsselt ruhende hochladen. Mit Storage Verschlüsselung geschützt werden automatisch entschlüsselt ein verschlüsseltes Dateisystem vor Codierung platziert und optional vor dem Hochladen als eine neue Ausgabe Anlage erneut verschlüsselt. Primäre Nutzung bei speicherverschlüsselung wird Sie hochwertige input Mediendateien mit starker Verschlüsselung ruhender auf Festplatte sichern möchten.

Um verschlüsselte Speicherressourcen zu liefern, müssen so Media Services weiß, wie Ihre Inhalte werden soll die Anlage Lieferung Richtlinie konfigurieren. Bevor die Anlage übertragen kann, streaming Server Speicher entschlüsselt und überträgt Inhalte mit angegebenen Lieferung Policy (AES, allgemeine Verschlüsselung oder ohne Verschlüsselung).

##<a name="create-contentkeys-used-for-encryption"></a>Erstellen von ContentKeys für die Verschlüsselung

Verschlüsselte Ressourcen müssen Speicher Schlüssel zugeordnet. Erstellen Sie Content Schlüssel für die Verschlüsselung vor dem Erstellen der Objektdateien. Dieser Abschnitt beschreibt, wie Inhalt erstellt.

Es folgen allgemeine Schritte zum Generieren von Schlüsseln, die Ressourcen zugeordnet werden, die verschlüsselt werden sollen. 

1. Speicherverschlüsselung zufällig einen 32-Byte-AES-Schlüssel zu generieren. 

    Der Inhaltsschlüssel für die Anlage werden die bedeutet, dass alle Dateien mit diesem demselben Inhaltsschlüssel bei der Entschlüsselung verwenden. 
2.  Rufen Sie die Methoden [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) und [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) zu Ihrem Schlüssel verschlüsseln verwendet werden muss der richtige x. 509-Zertifikat.
3.  Verschlüsseln Sie Ihre symmetrischen Schlüssel mit dem öffentlichen Schlüssel des x. 509-Zertifikats. 

    Media Services .NET SDK wird RSA bei der Verschlüsselung mit OAEP verwendet.  Sie sehen ein Beispiel .NET [EncryptSymmetricKeyData Funktion](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Erstellen Sie eine Prüfsumme berechnet die Schlüssel-ID und Schlüssel. Das folgende berechnet die Prüfsumme der GUID Teil der Schlüsselbezeichner und klar Inhaltsschlüssel.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. Erstellen Sie Content Schlüssel mit **EncryptedContentKey** (base64-codierte Zeichenfolge konvertiert), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**und **Prüfsummenwerte in den vorherigen Schritten erhalten** .

    
    Für speicherverschlüsselung sollte die folgenden Eigenschaften im Hauptteil Anforderung enthalten sein.
     
    Anforderung Body-Eigenschaft   | Beschreibung
    ---|---
    ID | Die ContentKey-Id die generieren wir uns mit dem folgenden Format "Nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Ist der Schlüssel Inhaltstyp als Integer für diesen Inhalt Schlüssel Den Wert 1 für speicherverschlüsselung übergeben.
    EncryptedContentKey | Wir erstellen einen neuen Content-Wert (32 Byte) 256-Bit-Wert ist. Der Schlüssel wird verschlüsselt mit Speicher Verschlüsselung x. 509-Zertifikat dem wir eine HTTP GET-Anforderung für die GetProtectionKeyId und GetProtectionKey-Methoden Ausführen von Microsoft Azure Media Services abrufen. So finden Sie im folgenden .NET Code: die Methode **EncryptSymmetricKeyData** [hier](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Dies ist die Key-Kennzeichnung für Speicher Verschlüsselung x. 509-Zertifikat, das verwendet wurde, um unsere Schlüssel verschlüsseln.
    ProtectionKeyType | Dies ist der Verschlüsselungstyp für die Schlüssel, mit dem der Inhalten Schlüssel verschlüsselt. Dieser Wert ist StorageEncryption(1) Beispiel.
    Prüfsumme |Die berechnete MD5-Prüfsumme für die Content. Er wird berechnet durch Content Id mit symmetrischen Schlüssel verschlüsseln. Im Codebeispiel wird veranschaulicht, wie die Prüfsumme berechnet.
    

###<a name="retrieve-the-protectionkeyid"></a>Abrufen der ProtectionKeyId 
 

Im folgenden Beispiel wird veranschaulicht, wie verwenden müssen, der Schlüssel zum Verschlüsseln von ProtectionKeyId einen Zertifikatfingerabdruck des Zertifikats abrufen. Wiederholen Sie diesen Schritt, um sicherzustellen, dass Sie bereits das entsprechende Zertifikat auf Ihrem Computer.


Anforderung:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Antwort:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>Die ProtectionKey für die ProtectionKeyId abrufen

Im folgenden Beispiel wird veranschaulicht, wie das x. 509-Zertifikat über die ProtectionKeyId, die Sie im vorherigen Schritt erhalten abzurufen.

Anforderung:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Antwort:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>Erstellen Sie content 

Nach x. 509-Zertifikat abgerufen und verwendet den öffentlichen Schlüssel den Schlüssel verschlüsseln **ContentKey** Entität erstellen und Eigenschaften entsprechend festlegen.

Der Werte muss nur festgelegt werden, wenn beim Erstellen des Inhalts ist der Schlüssel. Bei der speicherverschlüsselung ist der Wert "1". 

Im folgenden Codebeispiel wird veranschaulicht, wie eine **ContentKey** mit **ContentKeyType** für speicherverschlüsselung ("1") erstellt und **ProtectionKeyType** auf "0" gibt an, dass der Schutzschlüssel Id x. 509-Zertifikatfingerabdruck festgelegt.  


Anforderung

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Antwort:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Eine Anlage

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
    
    {"Name":"BigBuckBunny" "Options":1}


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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>Der ContentKey einer Anlage zuordnen

Nach dem Erstellen der ContentKey zugeordnet der Anlage mit der $links-Operation wie im folgenden Beispiel gezeigt:
    
Anforderung:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Antwort:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Erstellen einer AssetFile

[AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) -Entität stellt eine Video- oder audio-Datei, die in einem BLOB-Container gespeichert. Eine Bilddatei ist immer Anlagen und Anlage enthalten einen oder mehrere Objektdateien. Media Services Encoder-Task fehlschlägt, wenn ein Dateiobjekt Anlage nicht digitale Datei in einem BLOB-Container zugeordnet ist.

Beachten Sie, dass die **AssetFile** -Instanz und die aktuelle Mediendatei zwei verschiedene Objekte. Die AssetFile-Instanz enthält Metadaten über die Mediendatei Mediendatei tatsächlichen Medien.

Nach dem Hochladen der digitalen Mediendatei in einem BLOB-Container verwenden Sie **ZUSAMMENFÜHREN** HTTP-Anforderung der AssetFile mit Informationen über die Mediendatei (nicht in diesem Thema dargestellt) aktualisiert. 

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
