<properties
    pageTitle="Wie Sie Azure-Tabellenspeicher von Node.js | Microsoft Azure"
    description="Strukturierte Daten in der Cloud Azure-Tabellenspeicher, einem NoSQL-Datenspeicher gespeichert."
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


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Wie Sie Azure-Tabellenspeicher von Node.js

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht

In diesem Thema veranschaulicht, wie häufige Szenarien mit der Azure-Diensts in einer Anwendung Node.js.

Die Codebeispiele in diesem Thema wird davon ausgegangen, Node.js-Anwendung bereits. Informationen zum Erstellen einer Anwendung Node.js in Azure finden Sie unter folgenden Themen:

- [Erstellen Sie Node.js Web app in Azure App Service](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Erstellen und Bereitstellen einer Node.js Web app in Azure mithilfe von WebMatrix](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Erstellen und Bereitstellen eine Anwendung Node.js Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (mithilfe von Windows PowerShell)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurieren Sie Ihre Anwendung auf Azure-Speicher

Um Azure-Speicher zu verwenden, benötigen Sie Azure Storage SDK für Node.js, die benutzerfreundliche Bibliotheken enthält, die mit Storage REST-Dienste kommunizieren.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Verwenden Sie Knoten Paket-Manager (NPM), um das Paket zu installieren

1.  Verwenden Sie eine Befehlszeilenschnittstelle **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix), und navigieren Sie zu dem Ordner, in dem die Anwendung erstellt.

2.  Geben Sie im Befehlsfenster **Npm install Azure-Speicher** . Ausgabe des Befehls ähnelt dem folgenden Beispiel.

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

3.  Sie können manuell ausführen des Befehls **ls** überprüfen, ob ein **Knoten\_Module** wurde erstellt. In diesem Ordner finden Sie das **Azure Storage** -Paket enthält die Bibliotheken, die Sie Zugriff auf Speicher müssen.

### <a name="import-the-package"></a>Paket importieren

Fügen Sie den folgenden Code am Anfang der Datei **server.js** in Ihrer Anwendung:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindungs Azure-Speicher

Azure-Modul liest die Umgebungsvariablen AZURE\_Speicher\_Konto und AZURE\_Speicher\_Zugang\_Schlüssel oder AZURE\_Speicher\_Verbindung\_Zeichenfolge für die Verbindung zu Ihrem Konto Azure-Speicher erforderlich. Wenn diese Umgebungsvariablen nicht festgelegt werden, geben Sie die Kontoinformationen, wenn **TableService**aufgerufen.

Ein Beispiel zum Festlegen von Umgebungsvariablen in [Azure-Portal](https://portal.azure.com) für Azure-Website finden Sie unter [Node.js WebApp mit Azure Tabelle Service].

## <a name="create-a-table"></a>Erstellen einer Tabelle

Der folgende Code erstellt ein **TableService** -Objekt und zum Erstellen einer neuen Tabelle verwendet. Fügen Sie folgenden oberen **server.js**.

    var tableSvc = azure.createTableService();

Der Aufruf von **CreateTableIfNotExists** erstellt eine neue Tabelle mit dem angegebenen Namen, wenn es nicht bereits vorhanden ist. Das folgende Beispiel erstellt eine neue Tabelle namens "Mytable", wenn es nicht bereits vorhanden ist:

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

Die `result.created` werden `true` wird eine neue Tabelle erstellt, und `false` die Tabelle existiert bereits. Die `response` enthält Informationen über die Anforderung.

### <a name="filters"></a>Filter

Optionale Filteroperationen können Operationen mit **TableService**angewendet werden. Filteroperationen kann Protokollierung automatisch wiederholen usw. enthalten. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

    function handle (requestOptions, next)

Danach seine Vorverarbeitungsschritten Optionen Anforderung muss die Methode rufen Sie "Weiter" übergibt einen Rückruf mit der folgenden Signatur:

    function (returnObject, finalCallback, next)

In diesem Rückruf und nach der Verarbeitung der ReturnObject (die Antwort auf die Anforderung an den Server) muss der Rückruf aufgerufen, um anschließend zur weiteren Bearbeitung anderer Filter vorhanden oder einfach FinalCallback andernfalls aufrufen, um den Dienstaufruf beenden.

Zwei Filter, die Wiederholungslogik implementieren enthaltenen Azure SDK Node.js, **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**. Die folgenden erstellt ein **TableService** -Objekt, das **ExponentialRetryPolicyFilter**verwendet:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Um ein Element hinzuzufügen, müssen Sie zunächst erstellen Sie ein Objekt, das die Eigenschaften der Entität definiert. Alle Entitäten enthalten eine **PartitionKey** und **RowKey**, die eindeutige Bezeichner für die Entität.

* **PartitionKey** - bestimmt die Partition, der die Entität gespeichert ist

* **RowKey** - identifiziert Entität innerhalb der Partition eindeutig

**PartitionKey** und **RowKey** müssen Werte vom Typ String sein. Weitere Informationen finden Sie unter [Understanding Tabelle Service-Datenmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Folgendes ist ein Beispiel für eine Entität definieren. Hinweis: Diese **DueDate** als ein **Edm.DateTime**definiert ist. Der Typ ist optional und Typen werden abgeleitet, wenn nicht angegeben.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Es ist auch ein **Timestamp** -Feld für jeden Datensatz von Azure festgelegt wird, wenn eine Entität eingefügt oder aktualisiert wird.

**EntityGenerator** können Sie Entitäten erstellen. Im folgende Beispiel wird dieselbe Aufgabenentität mit **EntityGenerator**erstellt.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Um der Tabelle eine Entität hinzuzufügen, übergeben Sie das Entitätsobjekt Methode **InsertEntity** .

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Wenn der Vorgang erfolgreich ist `result` enthält das [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) des eingefügten Datensatzes und `response` enthält Informationen über den Vorgang.

Beispielantwort:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] In der Standardeinstellung **InsertEntity** kehrt das eingefügte Element als Teil der `response` Informationen. Wenn Sie andere Vorgänge auf diese Entität planen, oder die Informationen im cache, es kann nützlich sein, es als Teil der `result`. Dazu aktivieren **EchoContent** wie folgt:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Aktualisieren einer Entität

Es gibt mehrere Methoden zum Aktualisieren einer vorhandenen Entität:

* **ReplaceEntity** - aktualisiert eine vorhandene Entität durch Ersetzen

* **MergeEntity** - aktualisiert eine vorhandene Entität neue Eigenschaftswerte in die vorhandene Entität zusammenführen

* **InsertOrReplaceEntity** - aktualisiert eine vorhandene Entität ersetzt. Wenn keine Entität vorhanden ist, wird eine neue eingefügt werden

* **InsertOrMergeEntity** - aktualisiert eine vorhandene Entität neue Eigenschaftswerte in die bestehende zusammenführen. Wenn keine Entität vorhanden ist, wird eine neue eingefügt werden

Das folgende Beispiel veranschaulicht das Aktualisieren einer Entität mit **ReplaceEntity**:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Standardmäßig überprüft Aktualisieren einer Entität nicht, wenn die aktualisierten Daten zuvor von einem anderen Prozess geändert wurde. Parallele Updates unterstützen:
>
> 1. Erhalten Sie den ETag des Objekts aktualisiert. Dies wird zurückgegeben, als Teil der `response` für eine Entität Vorgang und über abgerufen werden kann `response['.metadata'].etag`.
>
> 2. Bei einem Aktualisierungsvorgang für eine Entität fügen Sie ETag Informationen bereits auf das neue Element hinzu Zum Beispiel:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. Führen Sie den Aktualisierungsvorgang. Wenn das Element geändert wurde, seit ETag-Wert wie eine andere Instanz der Anwendung abgerufenen ein `error` zurückgegeben, dass die in der Anforderung angegebene Update-Bedingung nicht erfüllt wurde.

Mit **ReplaceEntity** und **MergeEntity**ist die Entität, die aktualisiert wird nicht vorhanden, fehl dann der Aktualisierungsvorgang. Daher unabhängig von eine Entität gespeichert werden soll, ob er bereits vorhanden ist, verwenden Sie **InsertOrReplaceEntity** oder **InsertOrMergeEntity**.

Die `result` für Aktualisierung Vorgänge **Etag** der aktualisierten Entität enthalten.

## <a name="work-with-groups-of-entities"></a>Arbeiten Sie mit Entitäten

Manchmal ist es sinnvoll, mehrere Operationen zusammen in einem Batch darauf atomaren Verarbeitung durch den Server zu senden. Dazu verwenden Sie die **TableBatch** -Klasse um zu erstellen, und verwenden Sie die **ExecuteBatch** -Methode des **TableService** die Batchoperationen ausführen.

 Das folgende Beispiel veranschaulicht zwei Entitäten in einem Stapel senden:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Für erfolgreiche Batchoperationen `result` enthält Informationen für jeden Vorgang im Batch.

### <a name="work-with-batched-operations"></a>Arbeiten mit Batchoperationen

Operationen zu einem Stapel hinzugefügt anzeigen überprüft werden die `operations` Eigenschaft. Die folgenden Methoden können Sie mit Vorgängen arbeiten:

* **Löschen** - löscht alle Vorgänge aus einem Stapel

* **GetOperations** - Ruft einen Vorgang aus dem Stapel ab.

* **HasOperations** - Liefert WAHR, wenn der Stapel Vorgänge enthält

* **RemoveOperations** - entfernt einen Vorgang

* **Größe** - zurückgegeben die Anzahl der Vorgänge im batch

## <a name="retrieve-an-entity-by-key"></a>Abrufen einer Entität Schlüssel

Um eine bestimmte Entität basierend auf **PartitionKey** und **RowKey**zurückzugeben, verwenden Sie die **RetrieveEntity** -Methode.

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Nach Abschluss dieses Vorgangs `result` die Entität enthält.

## <a name="query-a-set-of-entities"></a>Eine Reihe von Entitäten Abfragen

Zum Abfragen einer Tabelle mithilfe des **TableQuery** -Objekts bilden einen Abfrageausdruck mit den folgenden Klauseln:

* **Wählen Sie** -Felder von der Abfrage zurückgegeben werden

* **wo** die Where-Klausel

    * **und** – eine `and` Bedingung

    * **oder** – eine `or` Bedingung

* **Top** - Anzahl der Elemente abrufen


Das folgende Beispiel erstellt eine Abfrage, die die oberen fünf Elemente mit einer PartitionKey von "Hometasks" zurück.

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

Da **Wählen** nicht verwendet wird, werden alle Felder zurückgegeben. Verwenden Sie zum Ausführen der Abfrage eine Tabelle **QueryEntities**. Im folgende Beispiel verwendet diese Abfrage zurückzugebenden Entitäten from Mytable".

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Wenn erfolgreich, `result.entries` enthält ein Array von Elementen, die die Abfrage entsprechen. Wurde die Abfrage nicht alle Entitäten `result.continuationToken` ungleich*null* und dienen als dritten Parameter der **QueryEntities** mehr Ergebnisse abrufen. Verwenden Sie für die ursprüngliche Abfrage *null* für den dritten Parameter ein.

### <a name="query-a-subset-of-entity-properties"></a>Abfrage einer Teilmenge von Entitätseigenschaften

Eine Abfrage in eine Tabelle kann nur wenige Felder einer Entität abrufen.
Dies verringert Bandbreite und abfrageleistung, insbesondere bei großen Unternehmen verbessern kann. Verwenden der **select** -Klausel, und übergeben Sie die Namen der Felder zurückgegeben werden. Beispielsweise gibt die folgende Abfrage nur die Felder **Beschreibung** und **DueDate** zurück.

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Löschen einer Entität

Sie können eine Entität mit der Partition und Zeile löschen. In diesem Beispiel enthält das Objekt **task1** die Werte **RowKey** und **PartitionKey** der Entität, die gelöscht werden. Das Objekt wird dann an die **DeleteEntity** -Methode übergeben.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Erwägen Sie ETags beim Löschen von Elementen, um sicherzustellen, dass das Element nicht von einem anderen Prozess geändert wurden. Informationen zur Verwendung von ETags finden Sie unter [Aktualisieren eine Entität](#update-an-entity) .

## <a name="delete-a-table"></a>Löschen einer Tabelle

Der folgende Code Löscht eine Tabelle aus ein Speicherkonto.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Wenn Sie unsicher sind, ob die Tabelle vorhanden ist, verwenden Sie **DeleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Fortsetzungstoken verwenden

Abfragen von Tabellen für umfangreiche Ergebnisse suchen Sie Fortsetzungstoken. Möglicherweise große Mengen von Daten für die Abfrage, die Sie nicht erkennen können, wenn Sie nicht erkennen, wenn ein Fortsetzungstoken vorhanden erstellen.

Die Ergebnisse Objekt zurückgegebenen Entitäten legt Abfragen einer `continuationToken` -Eigenschaft ein Token vorhanden ist. Dann können diese Sie beim Durchführen einer Abfrage über Entitäten Partition und Tabelle bewegen.

Bei der Abfrage möglicherweise ContinuationToken Parameter zwischen Objektinstanz Abfrage und die Rückruffunktion bereitgestellt:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Wenn Sie untersuchen das `continuationToken` Objekt finden Sie Eigenschaften wie `nextPartitionKey`, `nextRowKey` und `targetLocation`, die kann zum Durchlaufen der Ergebnisse.

Es gibt auch eine Fortsetzung Beispiel in Azure Storage Node.js Repo auf GitHub. Suchen Sie nach `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Arbeiten Sie mit SAS

SAS (SAS) sind eine sichere Möglichkeit zur präzise Zugriff auf Tabellen ohne Ihre Storage-Kontonamen oder Schlüssel. Eingeschränkter Zugriff auf Ihre Daten wie eine app Abfrage Datensätze werden häufig SAS verwendet.

Eine vertrauenswürdige Anwendung wie eine Cloud-basierte **GenerateSharedAccessSignature** von **TableService**mit SAS generiert und an eine nicht vertrauenswürdige oder teilweise vertrauenswürdige Anwendung wie eine app bietet. SAS wird mithilfe einer Richtlinie, die beschreibt die Start- und Enddaten, die SAS gültig ist, sowie die Zugriffsebene gewährt dem Inhaber SAS generiert.

Im folgende Beispiel generiert eine neue freigegebene Richtlinie, die Inhaber SAS-Abfrage ('R') der Tabelle ermöglichen und 100 Minuten nach der Erstellung läuft.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Beachten Sie, dass der Host auch Informationen müssen SAS Inhaber versucht der Tabelle erforderlich ist.

Die Clientanwendung anschließend verwendet die SAS mit **TableServiceWithSAS** Operationen für die Tabelle. Im folgenden Beispiel wird mit der Tabelle verbunden und führt eine Abfrage.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Da die SAS nur Abfragezugriff beim Versuch Einfügen war, aktualisieren und Löschen von Entitäten wurden, wird ein Fehler zurückgegeben.

### <a name="access-control-lists"></a>Zugriffssteuerungslisten

Eine Zugriffssteuerungsliste (ACL) können Sie die Zugriffsrichtlinie für SAS festgelegt. Dies ist nützlich, wenn mehrere Clients auf die Tabelle zugreifen, aber verschiedene Richtlinien für jeden Client ermöglichen möchten.

Eine ACL erfolgt mit einem Array von Richtlinien, mit der ID jeder Richtlinie zugeordnet. Das folgende Beispiel definiert zwei Richtlinien für "user1" und für "Benutzer2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Im folgende Beispiel wird die aktuelle ACL für die **Hometasks** -Tabelle und fügt neue **SetTableAcl**mit. Dieser Ansatz ermöglicht:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Sobald die ACL festgelegt wurde, können Sie anhand einer Richtlinie SAS erstellen. Das folgende Beispiel erstellt eine neue SAS für "Benutzer2":

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen.

-   [Azure-Speicher-Teamblog][].
-   [Azure Storage SDK für Knoten][] Repository auf GitHub.
-   [Node.js-Entwicklercenter](/develop/nodejs/)

  [Azure-Speicher SDK für Knoten]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js WebApp mit Azure Tabelle Service]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md
