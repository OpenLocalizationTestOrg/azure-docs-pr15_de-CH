<properties 
    pageTitle="Nach Rollen Zugriffstasten Storage Media Services aktualisieren | Microsoft Azure" 
    description="In diesem Artikel geben Hinweise wie Media Services nach Rollen Zugriffstasten Speicher aktualisiert." 
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
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Aktualisieren Sie Media Services nach Rollen Zugriffstasten Speicher

Wenn Sie ein neues Azure Media Services-Konto erstellen, müssen Sie ein Azure Storage-Konto auswählen, mit der Medieninhalte speichern. Beachten Sie, dass Sie [mehrere Speicherkonten hinzufügen](meda-services-managing-multiple-storage-accounts.md) zu Ihrem Media Services-Konto.

Beim Erstellen ein neuen Speicherkontos generiert Azure zwei 512-Bit-Speicher Zugriffstasten zum Authentifizieren des Zugriffs auf das Speicherkonto verwendet werden. Um Ihrer Speicher-Clientverbindungen zu erhöhen, wird empfohlen, regelmäßig Regenerieren und drehen den speicherzugriffsschlüssel. Zwei Zugriffstasten (Primär und sekundär) werden bereitgestellt, damit Sie Verbindungen eine Zugriffstaste verwenden, während die andere Taste Regenerieren Storage-Konto verwalten können. Dieses Verfahren wird auch "paralleles Access Keys" bezeichnet.

Media Services hängt ein Speicher bereitgestellt. Insbesondere hängt zum Streamen oder Herunterladen Datenbestände Locator Zugriffstaste angegebenen Speicher. Erstellt ein AMS-Konto übernimmt eine Abhängigkeit Zugriffstaste Primärspeicher standardmäßig als als Benutzer können Sie Speicherschlüssel AMS hat. Sie müssen wissen, welche Schlüssel in diesem Thema beschriebenen Schritte Media Services können sicherstellen. Wenn Speicher Zugriffstasten, müssen Sie auch sicherstellen, dass Ihre Locators aktualisieren wird ohne Unterbrechung der streaming Service (dieser Schritt wird auch im Thema beschrieben).

>[AZURE.NOTE]Haben Sie mehrere Speicherkonten würde dieses Verfahren jedes Storage-Konto ausführen.
>
>Vor der Ausführung auf einem Produktionskonto in diesem Thema beschriebenen Schritte, unbedingt eine Pre-Production-Konto testen.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Schritt 1: Regenerieren Sie Sekundärspeicher Zugriffstaste

Beginnen Sie mit sekundären Schlüssel neu erstellt. Standardmäßig wird der sekundäre Schlüssel nicht von Media Services verwendet.  Informationen wie Speicherschlüssel finden Sie unter [wie: Ansicht kopieren und Regenerieren Speicher Schlüssel](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a id="step2"></a>Schritt 2: Update Media Services an den neuen sekundären Speicher

Update Media Services zum Sekundärspeicher Zugriffstaste verwenden. Eine der beiden folgenden Methoden können Sie regenerierten Speicherschlüssel mit Media Services synchronisieren.

- Azure-Portal verwenden: um den Namen und Schlüssel Werte finden, zur Azure-Portal, und wählen Sie Ihr Konto. Das Fenster Einstellungen auf der rechten Seite. Wählen Sie im Einstellungsfenster Schlüssel. Wählen Sie je nach der Speicherschlüssel Media Services mit synchronisieren möchten, synchronisieren Primärschlüssel oder sekundäre Taste synchronisieren. In diesem Fall verwenden Sie die sekundäre Taste.

- Verwenden Sie Media Services Management REST-API.

Im folgenden Codebeispiel wird veranschaulicht, wie die https://endpoint/**SubscriptionId/Services/Mediaservices/Konten*Kontoname*/StorageAccounts/*StorageAccountName*Installeroption Antrag angegebenen Speicherschlüssel mit Media Services synchronisieren erstellen. In diesem Fall wird der sekundären Speicher Schlüsselwert verwendet. Weitere Informationen finden Sie unter [wie: mit Media Services Management REST API](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Aktualisieren Sie nach diesem Schritt vorhandene Locators (die Abhängigkeit auf den alten Speicher) wie im folgenden dargestellt.

>[AZURE.NOTE]Warten Sie 30 Minuten vor Operationen mit Media Services (z. B. Erstellen neuer Locator-Punkten) um Einfluss auf anstehende Aufträge zu verhindern.

##<a name="step-3-update-locators"></a>Schritt 3: Update locators

>[AZURE.NOTE]Wenn Speicher Zugriffstasten, müssen Sie Ihre vorhandenen Locators aktualisieren deshalb ohne Unterbrechung der streaming-Dienst.

Warten Sie mindestens 30 Minuten nach dem neuen Speicherschlüssel mit AMS synchronisieren. Dann können Sie Ihre OnDemand-Locators wiederherstellen, damit sie Abhängigkeit auf den angegebenen Speicher und bestehende URL.

Beachten Sie, dass bei SAS-Locator aktualisieren (oder erstellen), wird die URL jederzeit ändern.

>[AZURE.NOTE] Um sicherzustellen, dass die vorhandenen URLs OnDemand-Locators beizubehalten, müssen Sie vorhandenen Locator löschen und erstellen eine neue mit der gleichen ID.

.NET veranschaulicht unten, wie einen Locator mit der gleichen ID neu erstellen

Private statische ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ / Eigenschaften der vorhandenen Locator.
Var Anlage = Locator. Anlagen; Var AccessPolicy = Locator. AccessPolicy; Var LocatorId = Locator. ID; Var StartDate = Locator. StartTime; Var LocatorType = Locator. Geben. Var LocatorName = Locator. Name;

Löschen Sie alte Locator.
Locator. Delete();

Wenn (Locator. ExpirationDateTime < = DateTime.UtcNow) {throw new Exception (String.Format ("Locator-Id kann nicht neu = {0} ist der Locator Ablaufzeitpunkt in der Vergangenheit", Locator. ID)); }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Schritt 5: Regenerieren Sie Zugriffstaste Primärspeicher

Regenerieren Sie die Zugriffstaste Primärspeicher. Informationen wie Speicherschlüssel finden Sie unter [wie: Ansicht kopieren und Regenerieren Speicher Schlüssel](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Schritt 6: Update Media Services an den neuen primären Speicher
    
Mit demselben Verfahren wie in [Schritt 2](media-services-roll-storage-access-keys.md#step2) dieses Mal neue Primärspeicher Zugriffstaste mit Media Services-Konto synchronisieren.

>[AZURE.NOTE]Warten Sie 30 Minuten vor Operationen mit Media Services (z. B. Erstellen neuer Locator-Punkten) um Einfluss auf anstehende Aufträge zu verhindern.

##<a name="step-7-update-locators"></a>Schritt 7: Update locators  

Nach 30 Minuten können Sie die OnDemand-Locators wiederherstellen, damit diese Abhängigkeit auf den neuen primären Speicher und bestehende URL.

Verwenden Sie dasselbe wie in [Schritt 3](media-services-roll-storage-access-keys.md#step-3-update-locators)beschrieben.


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Danksagungen 

Wir möchten die folgenden Personen, die dieses Dokument beigetragen bestätigen: Cenk Dingiloglu, Mailand Gada Seva Titov.
