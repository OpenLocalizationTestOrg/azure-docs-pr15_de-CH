<properties
    pageTitle="Wie BLOB-Speicher von Node.js | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud Azure BLOB-Speicher (Storage Objekt)."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>



# <a name="how-to-use-blob-storage-from-nodejs"></a>Verwendung von Node.js-BLOB-Speicher

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

Dieser Artikel veranschaulicht die allgemeine Szenarien mit BLOB-Speicher. Die Beispiele sind API Node.js geschrieben. Die Szenarios umfassen hochladen, Liste, herunterladen und Löschen von Blobs.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Erstellen Sie eine Node.js-Anwendung

Anleitung zum Erstellen einer Anwendung Node.js finden Sie unter [Erstellen einer Node.js Web app in Azure App Service], [Erstellen und Bereitstellen eine Anwendung Node.js Azure Cloud Service] mithilfe von Windows PowerShell oder [Erstellen und Bereitstellen eine Node.js Web app in Azure mit Web Matrix].

## <a name="configure-your-application-to-access-storage"></a>Konfigurieren Sie Ihre Anwendung auf Speicher zugreifen

Um Azure-Speicher zu verwenden, benötigen Sie Azure Storage SDK für Node.js, die benutzerfreundliche Bibliotheken enthält, die mit Storage REST-Dienste kommunizieren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden Sie das Paket zu Knoten Paket-Manager (NPM)

1.  Verwenden Sie eine Befehlszeilenschnittstelle **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix) zu dem Ordner navigieren, in dem die beispielanwendung erstellt.

2.  Geben Sie im Befehlsfenster **Npm install Azure-Speicher** . Ausgabe des Befehls ähnelt dem folgenden Codebeispiel.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Sie können manuell ausführen des Befehls **ls** überprüfen, ob ein **Knoten\_Module** wurde erstellt. Suchen Sie in diesem Ordner **Azure Storage -** Paket enthält die Bibliotheken, die Sie Zugriff auf Speicher.

### <a name="import-the-package"></a>Paket importieren

Mit dem Editor oder einem anderen Text-Editor, fügen Sie Folgendes am Anfang der **server.js** -Datei der Anwendung Speicher verwendet werden soll:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindungs Azure-Speicher

Azure-Modul liest die Umgebungsvariablen `AZURE_STORAGE_ACCOUNT` und `AZURE_STORAGE_ACCESS_KEY`, oder `AZURE_STORAGE_CONNECTION_STRING`, für die Verbindung zu Ihrem Konto Azure-Speicher erforderlich. Wenn diese Umgebungsvariablen nicht festgelegt werden, geben Sie die Kontoinformationen, wenn **CreateBlobService**aufgerufen.

Beispielsweise festlegen von Umgebungsvariablen in [Azure-Portal](https://portal.azure.com) für eine Azure Web app anzeigen Sie [Node.js WebApp mithilfe des Diensts für Azure Tabelle]

## <a name="create-a-container"></a>Erstellen eines Containers

Das **BlobService** -Objekt können Sie Behälter mit Blobs arbeiten. Der folgende Code erstellt ein **BlobService** -Objekt. Fügen Sie im oberen Bereich des **server.js**Folgendes:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] Sie können einen Blob mit **CreateBlobServiceAnonymous** und die Hostadresse anonym zugreifen. Z. B. `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Um einen neuen Container erstellen möchten, verwenden Sie **CreateContainerIfNotExists**. Das folgende Codebeispiel erstellt einen neuen Container mit dem Namen "Mycontainer":

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Wenn der Container neu erstellt, `result.created` gilt. Wenn der Container bereits vorhanden ist, `result.created` ist falsch. `response`enthält Informationen zum Vorgang angezeigt, einschließlich der ETag-Informationen für den Container.

### <a name="container-security"></a>Containersicherheit

Neuen Container standardmäßig private und nicht anonym zugegriffen werden. Um den Container zu veröffentlichen, damit Sie anonym zugreifen können, können Sie den Container Zugriffsebene **Blob** oder **Container**festlegen.

* **Blob** - ermöglicht anonymen Lesezugriff auf BLOB-Inhalt und Metadaten in diesem Container, nicht jedoch für Container Metadaten wie alle Blobs in einem Container aufgelistet

* **Container** - ermöglicht anonymen Lesezugriff auf BLOB-Inhalt und Metadaten sowie Container Metadaten

Im folgenden Codebeispiel veranschaulicht das Festlegen der Zugriffsebene auf **Blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

Alternativ können Sie die Zugriffsebene eines Containers mit **SetContainerAcl** auf der Zugriffsebene ändern. Im folgenden Codebeispiel wird die Zugriffsebene Container:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Das Ergebnis enthält Informationen zum Vorgang angezeigt, einschließlich der aktuellen **ETag** für den Container.

### <a name="filters"></a>Filter

Sie können Operationen **BlobService**mit optionale Filteroperationen anwenden. Filteroperationen kann Protokollierung automatisch wiederholen usw. enthalten. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

    function handle (requestOptions, next)

Danach seine Vorverarbeitungsschritten Optionen Anforderung muss die Methode rufen Sie "Weiter" übergibt einen Rückruf mit der folgenden Signatur:

    function (returnObject, finalCallback, next)

In diesem Rückruf und nach der Verarbeitung der ReturnObject (die Antwort auf die Anforderung an den Server) muss der Rückruf aufgerufen, anschließend zur weiteren Bearbeitung anderer Filter vorhanden oder rufen Sie einfach die FinalCallback um den Dienstaufruf beenden.

Zwei Filter, die Wiederholungslogik implementieren enthaltenen Azure SDK Node.js, **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**. Die folgenden erstellt ein **BlobService** -Objekt, das **ExponentialRetryPolicyFilter**verwendet:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Ein Container einen Blob hochladen

Es gibt drei Arten von Blobs: blockieren Blobs, Seite Blobs und Blobs anfügen. Block-Blobs können Sie große Datenmengen effizient hochladen. Append Blobs sind optimiert für Vorgänge anfügen. Seitenblobs sind für Lese-/Schreibvorgänge optimiert. Weitere Informationen finden Sie unter [Understanding Block Blobs, Anhängen Blobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Block-blobs

Verwenden Sie zum Hochladen von Daten auf ein Folgendes ein:

* **CreateBlockBlobFromLocalFile** - erstellt ein neuen Block-Blob und lädt den Inhalt einer Datei

* **CreateBlockBlobFromStream** - erstellt ein neuen Block-Blob und lädt den Inhalt eines Streams

* **CreateBlockBlobFromText** - erstellt ein neuen Block-Blob und lädt den Inhalt einer Zeichenfolge

* **CreateWriteStreamToBlockBlob** - stellt einen Stream schreiben, um ein

Im folgenden Codebeispiel wird der Inhalt der Datei **test.txt** in **Myblob**hochgeladen.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Die `result` von diesen Methoden zurückgegebenen Informationen, wie das **ETag** des BLOBs enthält.

### <a name="append-blobs"></a>Anfügen von blobs

Verwenden Sie zum Hochladen von Daten in ein neues Anhängen Blob Folgendes ein:

* **CreateAppendBlobFromLocalFile** - erstellt ein neue Anhängen Blob und lädt den Inhalt einer Datei

* **CreateAppendBlobFromStream** - erstellt ein neue Anhängen Blob und lädt den Inhalt eines Streams

* **CreateAppendBlobFromText** - erstellt ein neue Anhängen Blob und lädt den Inhalt einer Zeichenfolge

* **CreateWriteStreamToNewAppendBlob** - ein neues Anhängen Blob und anschließend einen Stream zu schreiben

Im folgenden Codebeispiel wird der Inhalt der Datei **test.txt** in **Myappendblob**hochgeladen.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Um einen Block in eine bestehende Anhängen Blob anzufügen, verwenden Sie Folgendes:

* **AppendFromLocalFile** - Inhalt einer Datei an eine bestehende Anhängen Blob Anfügen

* **AppendFromStream** - ein vorhandenes Anhängen Blob den Inhalt eines Streams hinzufügen

* **AppendFromText** - Inhalt einer Zeichenfolge an eine bestehende Anhängen Blob Anfügen

* **AppendBlockFromStream** - ein vorhandenes Anhängen Blob den Inhalt eines Streams hinzufügen

* **AppendBlockFromText** - Inhalt einer Zeichenfolge an eine bestehende Anhängen Blob Anfügen

> [AZURE.NOTE] AppendFromXXX APIs machen einige clientseitige Validierung schnell zu Unncessary Server-Aufruf fehl. AppendBlockFromXXX nicht.

Im folgenden Codebeispiel wird der Inhalt der Datei **test.txt** in **Myappendblob**hochgeladen.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Seitenblobs

Verwenden Sie zum Hochladen von Daten in ein Seitenblob Folgendes:

* **CreatePageBlob** - erstellt ein neue Seitenblob mit einer bestimmten Länge

* **CreatePageBlobFromLocalFile** - erstellt ein neue Seitenblob und lädt den Inhalt einer Datei

* **CreatePageBlobFromStream** - erstellt ein neue Seitenblob und lädt den Inhalt eines Streams

* **CreateWriteStreamToExistingPageBlob** - stellt einen Stream Schreiben in eine vorhandene Seitenblob

* **CreateWriteStreamToNewPageBlob** - erstellt ein neue Seitenblob und dann einen Stream zu schreiben

Im folgenden Codebeispiel wird der Inhalt der Datei **test.txt** in **Mypageblob**hochgeladen.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Seitenblobs bestehen aus 512 Byte 'Seiten'. Sie erhalten eine Fehlermeldung beim Hochladen von Daten mit einer Größe, die kein Vielfaches von 512 ist.

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Um die Blobs in einem Container aufzulisten, verwenden Sie die **ListBlobsSegmented** -Methode. Wenn Sie Blobs mit einem bestimmten Präfix zurückgeben möchten, verwenden Sie **ListBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

Die `result` enthält eine `entries` Auflistung, die ein Array von Objekten, die jedes Blob beschreiben. Wenn alle Blobs zurückgegeben werden, die `result` bietet außerdem eine `continuationToken`, als zweiten Parameter mit möglicherweise zusätzliche Einträge abrufen.

## <a name="download-blobs"></a>Herunterladen von blobs

Download von Daten von einem Blob, verwenden Sie Folgendes:

* **GetBlobToLocalFile** - schreibt den BLOB-Inhalt in die Datei

* **GetBlobToStream** - schreibt den BLOB-Inhalt in einem stream

* **GetBlobToText** - schreibt den BLOB-Inhalt in eine Zeichenfolge

* **CreateReadStream** - stellt einen Stream aus dem Blob gelesen

Im folgenden Codebeispiel wird veranschaulicht, wie mit **GetBlobToStream** Inhalt der Blob **Myblob** heruntergeladen und in der Datei **output.txt** mit einem Stream speichern:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

Die `result` enthält Informationen über das Blob **ETag** -Informationen.

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Rufen Sie anschließend zum Löschen eines BLOBs **DeleteBlob**. Im folgenden Codebeispiel wird das Blob mit dem Namen **Myblob**gelöscht.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Gleichzeitiger Zugriff

Um gleichzeitigen Zugriff von mehreren Clients oder mehrere Prozessinstanzen in ein Blob unterstützen, können Sie **ETags** oder **Leases**.

* **Etag** - lässt erkennen, dass das Blob oder Container wurde von einem anderen Prozess geändert

* **Leasing** - Weise erhalten exklusive, erneuerbare, schreiben oder löschen auf ein Blob für einen Zeitraum

### <a name="etag"></a>ETag

ETags verwenden, benötigen Sie mehrere Clients oder Instanzen Block Blob oder Seite schreiben können Blob gleichzeitig. Das ETag können Sie bestimmen, ob der Container oder Blob geändert wurde zunächst gelesen oder erstellt, nicht von einem anderen Client oder Prozess Änderungen überschrieben werden können.

ETag Startbedingungen legen Sie mithilfe der optionalen `options.accessConditions` Parameter. Im folgenden Codebeispiel wird nur die Datei **test.txt** hochgeladen, wenn das Blob bereits vorhanden und hat der ETag-Wert enthalten `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Bei Verwendung von ETags ist das allgemeine Muster:

1. Das ETag als Ergebnis einer erstellen, Liste oder Get-Operation zu erhalten.

2. Eine Aktion, ob der ETag-Wert nicht geändert wurde.

Wenn der Wert geändert wurde, bedeutet dies, dass eine andere Client oder Instanz geändert Blob oder Container seit der ETag-Wert abgerufen.

### <a name="lease"></a>Leasing

Erhalten Sie ein neues **AcquireLease** Methode, Angabe des Blob oder Container, dem Sie wünschen, eine Lease zu erhalten. Der folgende Code erhält beispielsweise eine Lease **Myblob**.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Nachfolgende Operationen auf **Myblob** geben die `options.leaseId` Parameter. Die Lease als zurückgegeben `result.id` von **AcquireLease**.

> [AZURE.NOTE] Standardmäßig ist die Gültigkeitsdauer der Lease unendlich. Geben eine Dauer nicht unendlich (zwischen 15 und 60 Sekunden) mit den `options.leaseDuration` Parameter.

Verwenden Sie **ReleaseLease**, um eine Lease zu entfernen. Unterbrechen einer Leases, aber verhindern, dass andere ein neues Leasing erhalten, bis die ursprüngliche Dauer abgelaufen ist, verwenden Sie **BreakLease**.

## <a name="work-with-shared-access-signatures"></a>Arbeiten Sie mit SAS

SAS (SAS) sind sichere Weise präzise Blobs und Container zugreifen ohne Ihr speicherkontoname oder Schlüssel. Eingeschränkter Zugriff auf Ihre Daten wie eine app auf Blobs werden häufig SAS verwendet.

> [AZURE.NOTE] Während Sie anonymen Zugriff auf Blobs auch können, ermöglichen SAS mehr kontrollierten Zugriff SAS generieren zu müssen.

Eine vertrauenswürdige Anwendung wie eine cloudbasierte SAS mit **GenerateSharedAccessSignature** der **BlobService**generiert und an eine nicht vertrauenswürdige oder teilweise vertrauenswürdige Anwendung wie eine app bietet. Gemeinsamer Zugriff Signaturen werden generiert mithilfe einer Richtlinie beschreibt am Anfang und Ende der gemeinsamen Zugriff Signaturen gültig sind, sowie die Zugriffsebene für den gemeinsamen Zugriff Signaturen Inhaber gewährt.

Im folgenden Codebeispiel wird eine neue freigegebene Richtlinie, die gemeinsamen Zugriff Signaturen Inhaber **Myblob** Blob Lesevorgänge ausführen kann generiert und 100 Minuten nach der Erstellung läuft.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Beachten Sie, dass der Host auch Informationen müssen wie beim gemeinsamen Zugriff Signaturen Inhaber versucht, Zugriff auf den Container erforderlich ist.

Dann die Clientanwendung verwendet SAS mit **BlobServiceWithSAS** gegen das Blob Operationen. Die folgenden wird Informationen **Myblob**.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Da bei Blob ändern die SAS mit schreibgeschützten Zugriff generiert wurden, wird ein Fehler zurückgegeben.

### <a name="access-control-lists"></a>Zugriffssteuerungslisten

Eine Zugriffssteuerungsliste (ACL) können Sie die Zugriffsrichtlinie für SAS festgelegt. Dies ist nützlich, wenn mehrere Clients Zugriff auf einen Container aber verschiedene Richtlinien für jeden Client ermöglichen möchten.

Eine ACL erfolgt mit einem Array von Richtlinien, mit der ID jeder Richtlinie zugeordnet. Im folgenden Codebeispiel definiert zwei Richtlinien für "user1" und für "Benutzer2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Im folgenden Codebeispiel wird die aktuelle ACL für **Mycontainer**und fügt die neuen Richtlinien mit **SetBlobAcl**. Dieser Ansatz ermöglicht:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Sobald die ACL festgelegt ist, können Sie anhand einer Richtlinie SAS erstellen. Das folgende Codebeispiel erstellt neue SAS "Benutzer2":

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen.

-   [Azure-Speicher SDK für Knoten-API-Referenz][]
-   [Azure-Speicher-Teamblog][]
-   [Azure Storage SDK für Knoten][] Repository auf GitHub
-   [Node.js-Entwicklercenter](/develop/nodejs/)
-   [Datenübertragung mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)

[Azure-Speicher SDK für Knoten]: https://github.com/Azure/azure-storage-node

[Erstellen Sie Node.js Web app in Azure App Service]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Node.js WebApp mit Azure Tabelle Service]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Erstellen und Bereitstellen einer Node.js Web app in Azure mit Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Erstellen und Bereitstellen einer Anwendung Node.js Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure-Speicher SDK für Knoten-API-Referenz]: http://dl.windowsazure.com/nodestoragedocs/index.html
