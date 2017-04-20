<properties
    pageTitle="Verwendung von Node.js Warteschlangenspeicher | Microsoft Azure"
    description="Informationen Sie zum Verwenden des Azure-warteschlangendiensts erstellen und Löschen von Warteschlangen fügen Sie ein, abrufen Sie und löschen. In Node.js-Beispiele."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>Verwendung von Node.js Warteschlangenspeicher

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht die Szenarien mit Microsoft Azure-Warteschlangendienst ausgeführt. Die Beispiele wurden mithilfe der Node.js-API. Die Szenarios umfassen **Einfügen**, **einsehen**, **Abrufen**und **Löschen von** Nachrichten in Warteschlange sowie **Erstellen und Löschen von Warteschlangen**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Erstellen Sie eine Node.js-Anwendung

Erstellen einer leeren Node.js-Anwendung. Node.js-Anwendung erstellen Informationen finden Sie unter [Erstellen einer Node.js Web app in Azure App Service], [Erstellen und Bereitstellen eine Anwendung Node.js Azure Cloud Service] mithilfe von Windows PowerShell oder [Erstellen und Bereitstellen eine Node.js Web app in Azure mit Web Matrix].

## <a name="configure-your-application-to-access-storage"></a>Der Anwendung Zugriff

Um Azure-Speicher zu verwenden, benötigen Sie Azure Storage SDK für Node.js, die benutzerfreundliche Bibliotheken enthält, die mit Storage REST-Dienste kommunizieren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden Sie das Paket zu Knoten Paket-Manager (NPM)

1.  Eine Befehlszeilenschnittstelle verwenden wie **PowerShell** (Windows) **Terminal** (Mac) oder **Bash** (Unix), navigieren Sie zu dem Ordner, in denen eine beispielanwendung erstellt.

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

Mit dem Editor oder einem anderen Text-Editor, fügen Sie Folgendes am Anfang der Datei **server.js** der Anwendung Speicher verwendet werden soll:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Ein Azure-Speicher Verbindung

Azure-Modul liest die Umgebungsvariablen AZURE\_Speicher\_Konto und AZURE\_Speicher\_Zugang\_Schlüssel oder AZURE\_Speicher\_Verbindung\_Zeichenfolge für die Verbindung zu Ihrem Konto Azure-Speicher erforderlich. Wenn diese Umgebungsvariablen nicht festgelegt werden, geben Sie die Kontoinformationen, wenn **CreateQueueService**aufgerufen.

Ein Beispiel zum Festlegen von Umgebungsvariablen in [Azure-Portal](https://portal.azure.com) für Azure-Website finden Sie unter [Node.js WebApp mit Azure Tabelle Service].

## <a name="how-to-create-a-queue"></a>Gewusst wie: Erstellen einer Warteschlange

Der folgende Code erstellt ein **QueueService** -Objekt, das mit Warteschlangen arbeiten ermöglicht.

    var queueSvc = azure.createQueueService();

Verwenden Sie die Methode **CreateQueueIfNotExists** , ab der angegebenen Warteschlange bereits vorhanden ist oder eine neue Warteschlange mit dem angegebenen Namen erstellt, wenn es nicht bereits vorhanden ist.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Wenn die Warteschlange erstellt wird, `result.created` gilt. Wenn die Warteschlange vorhanden ist, `result.created` ist falsch.

### <a name="filters"></a>Filter

Optionale Filteroperationen können Operationen mit **QueueService**angewendet werden. Filteroperationen kann Protokollierung automatisch wiederholen usw. enthalten. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

    function handle (requestOptions, next)

Danach seine Vorverarbeitungsschritten Optionen Anforderung muss die Methode aufrufen "Weiter" einen Rückruf mit der folgenden Signatur:

    function (returnObject, finalCallback, next)

In diesem Rückruf und nach der Verarbeitung der ReturnObject (die Antwort auf die Anforderung an den Server) muss der Rückruf aufgerufen, anschließend zur weiteren Bearbeitung anderer Filter vorhanden oder einfach FinalCallback sonst zum Ende der Dienstaufruf aufrufen.

Zwei Filter, die Wiederholungslogik implementieren enthaltenen Azure SDK Node.js, **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**. Die folgenden erstellt ein **QueueService** -Objekt, das **ExponentialRetryPolicyFilter**verwendet:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Gewusst wie: Einfügen eine Nachricht in einer Warteschlange

Verwenden Sie zum Einfügen einer Nachricht in einer Warteschlange **CreateMessage** -Methode, um eine neue Nachricht erstellen und an die Warteschlange hinzufügen.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Gewusst wie: Einsehen der nächsten Nachricht

Sie können die Nachricht in einer Warteschlange einsehen, ohne durch Aufruf der **PeekMessages** -Methode aus der Warteschlange entfernt. Standardmäßig sieht **PeekMessages** einer einzelnen Nachricht.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

Die `result` die Nachricht enthält.

> [AZURE.NOTE] Mit **PeekMessages** , wenn es gibt keine Nachrichten in der Warteschlange kein Fehler zurückgegeben, werden jedoch keine Nachrichten zurückgegeben.

## <a name="how-to-dequeue-the-next-message"></a>Gewusst wie: Abrufen der nächsten Nachricht

Verarbeitung einer Nachricht ist ein zweistufiger Prozess:

1. Entfernen Sie die Nachricht.

2. Löschen der Nachricht.

Um eine Nachricht zu entfernen, verwenden Sie **GetMessages**. Diese unsichtbar Nachrichten in der Warteschlange keine Clients verarbeiten können. Sobald die Anwendung eine Nachricht verarbeitet, rufen Sie **DeleteMessage** aus der Warteschlange löschen. Im folgenden Beispiel wird eine Nachricht anschließend gelöscht:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Standardmäßig wird eine Nachricht nur für 30 Sekunden ausgeblendet nach dem anderen angezeigt wird. Geben Sie einen anderen Wert mit `options.visibilityTimeout` mit **GetMessages**.

> [AZURE.NOTE]
> **GetMessages** verwenden, wenn es gibt keine Nachrichten in der Warteschlange kein Fehler zurückgegeben, werden jedoch keine Nachrichten zurückgegeben.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Gewusst wie: Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht direkt in die Warteschlange **UpdateMessage**ändern. Das folgende Beispiel aktualisiert den Text einer Nachricht:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Gewusst wie: Zusätzliche Optionen für Warteschlangen Nachrichten

Es gibt zwei Arten anpassen aus einer Warteschlange abrufen:

* `options.numOfMessages`-Nachrichten (bis zu 32) abrufen
* `options.visibilityTimeout`-Festlegen Sie einen längere bzw. kürzere unsichtbarkeit Timeout.

Das folgende Beispiel verwendet die **GetMessages** -Methode zu 15 Nachrichten aufrufen. Anschließend verarbeitet jede Nachricht mit einer for-Schleife. Außerdem wird das unsichtbarkeit-Timeout fünf Minuten für alle Nachrichten, die von dieser Methode zurückgegebene festgelegt.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Gewusst wie: Abrufen die Warteschlangenlänge

**GetQueueMetadata** gibt Metadaten zur Warteschlange, einschließlich die ungefähre Anzahl der Nachrichten in der Warteschlange zurück.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Gewusst wie: Liste Warteschlangen

Um eine Liste mit Warteschlangen abzurufen, verwenden Sie **ListQueuesSegmented**. Verwenden Sie zum Abrufen einer Liste gefiltert nach einem bestimmten Präfix **ListQueuesSegmentedWithPrefix**.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Wenn alle Warteschlangen zurückgegeben werden, `result.continuationToken` als der erste Parameter der **ListQueuesSegmented** oder der zweite Parameter der **ListQueuesSegmentedWithPrefix** mehr Ergebnisse abrufen verwendet werden.

## <a name="how-to-delete-a-queue"></a>Gewusst wie: Löschen eine Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten **DeleteQueue** Methode für das Warteschlangenobjekt.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Um alle Nachrichten aus einer Warteschlange löschen, ohne Sie zu löschen, verwenden Sie **ClearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Gewusst wie: Arbeiten mit SAS

Freigegebene-Signaturen (SAS) sind auf sichere Weise zu präzise auf Warteschlangen zugreifen ohne Ihr speicherkontoname oder Schlüssel. SAS werden häufig verwendet, eingeschränkter Zugriff auf Warteschlangen, wie eine app, Nachrichten zu senden.

Eine vertrauenswürdige Anwendung wie eine cloudbasierte **GenerateSharedAccessSignature** des **QueueService**mit SAS generiert und an eine nicht vertrauenswürdige oder teilweise vertrauenswürdige Anwendung bietet. Angenommen, eine app. SAS wird mithilfe einer Richtlinie, die beschreibt die Start- und Enddaten, die SAS gültig ist, sowie die Zugriffsebene gewährt dem Inhaber SAS generiert.

Im folgenden Beispiel wird eine neue freigegebene Richtlinie, die SAS-Inhaber Nachrichten an die Warteschlange hinzufügen kann generiert und 100 Minuten nach der Erstellung läuft.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Beachten Sie, dass der Host auch Informationen müssen je nach Bedarf SAS Inhaber die Warteschlange zuzugreifen versucht.

Die Clientanwendung anschließend verwendet die SAS mit **QueueServiceWithSAS** Operationen der Warteschlange. Im folgenden Beispiel wird eine Verbindung mit der Warteschlange und erstellt eine Nachricht.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Da die SAS hinzufügen Zugriff generiert wurde und versucht wurde, lesen, aktualisieren oder löschen, wird ein Fehler zurückgegeben.

### <a name="access-control-lists"></a>Zugriffssteuerungslisten

Eine Zugriffssteuerungsliste (ACL) können Sie die Zugriffsrichtlinie für SAS festgelegt. Dies ist hilfreich, wenn Sie mehrere Clients zuzugreifen, aber verschiedene Richtlinien für jeden Client zulassen möchten.

Eine ACL erfolgt mit einem Array von Richtlinien, mit der ID jeder Richtlinie zugeordnet. Das folgende Beispiel definiert zwei Richtlinien. eine "Benutzer1" und für "Benutzer2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Im folgende Beispiel wird die aktuelle ACL für **Berichtnachrichten**und fügt neue **SetQueueAcl**verwenden. Dieser Ansatz ermöglicht:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Sobald die ACL festgelegt wurde, können Sie anhand einer Richtlinie SAS erstellen. Das folgende Beispiel erstellt eine neue SAS für "Benutzer2":

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links komplexer Speicheraufgaben erfahren.

-   Besuchen Sie den [Azure-Speicher-Teamblog][].
-   Besuchen Sie GitHub Repository [Azure Storage SDK für Knoten][] .

  [Azure-Speicher SDK für Knoten]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Erstellen Sie Node.js Web app in Azure App Service]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js WebApp mit Azure Tabelle Service]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Erstellen und Bereitstellen einer Anwendung Node.js Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Erstellen und Bereitstellen einer Node.js Web app in Azure mit Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
