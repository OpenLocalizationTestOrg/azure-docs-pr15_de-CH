<properties
    pageTitle="Azure Funktionen Trigger und Bindungen | Microsoft Azure"
    description="Verstehen Sie, wie Trigger und Bindungen in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktionen, Funktionen, Verarbeitung, Webhooks, dynamische Compute, serverlose Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure Funktionen Trigger und Bindings-Entwicklerreferenz


Dieses Thema enthält allgemeine Referenz für Trigger und Bindung. Es enthält erweiterte Bindungsfunktionen und Syntax von allen Bindung unterstützt.  

Wenn konfigurieren und einen bestimmten Typ eines Triggers oder einer Bindung Code Informationen suchen, sollten Sie auf einen Trigger oder Bindungen stattdessen unten klicken:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Diese Artikel davon ausgehen, dass die [Entwicklerreferenz Azure Funktionen](functions-reference.md)und [C#](functions-reference-csharp.md), [F#](functions-reference-fsharp.md)oder [Node.js](functions-reference-node.md) Developer Reference Artikeln gelesen haben.



## <a name="overview"></a>Übersicht

Trigger werden verwendet, um den benutzerdefinierten Code Ereignis antworten. Sie können der Azure-Plattform oder lokal auf Ereignisse reagieren. Bindungen stellen die erforderlichen Metadaten zum gewünschten Trigger oder zugeordneten Eingabe- oder Ausgabedaten Code herstellen.

Um einen besseren Überblick über die anderen Bindungen zu erhalten Sie Ihre Azure-Funktion App integrieren können, finden Sie in der folgenden Tabelle.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Besseres Verständnis auslösen und Bindungen im Allgemeinen möchten Sie Code Prozess ausführen ein neues Element in einer Warteschlange Azure-Speicher abgelegt. Azure Funktionen bietet einen Trigger Azure-Warteschlange zu unterstützen. Sie müssten, die folgende Informationen zur Warteschlange überwachen:
 
- Das Speicherkonto, in dem die Warteschlange vorhanden ist.
- Der Name der Warteschlange.
- Ein Variablenname, mit denen Code an das neue Element zu verweisen, die in der Warteschlange abgelegt wird.  
 
Warteschlange Trigger Bindung enthält diese Informationen für eine Azure-Funktion. Die *function.json* für jede Funktion enthält alle zugehörigen Bindungen.  Hier ist ein Beispiel mit einer Warteschlange *function.json* Bindung auslösen. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Code kann verschiedene Ausgabe je nach das neuen Warteschlangenelement Verarbeitung senden. Sie möchten z. B. einen neuen Datensatz einer Tabelle Azure-Speicher zu schreiben.  Zu diesem Zweck können Sie eine Bindung Ausgabe einer Azure Storage-Tabelle einrichten. Hier ist ein Beispiel- *function.json* mit einer Bindung Speicher Tabelle Ausgabe, die Warteschlange Trigger verwendet werden kann. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


Die folgende C#-Funktion reagiert auf ein neues Element in der Warteschlange abgelegt und schreibt einen neuen Benutzereintrag in Azure Storage-Tabelle.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Weitere Codebeispiele und Weitere Informationen zu Azure-Speicher, die unterstützt werden, finden Sie unter [Azure Funktionen Trigger und Bindungen für Azure-Speicher](functions-bindings-storage.md).


Klicken Sie erweiterten Funktionen der Bindung im Azure-Portal die Option **Erweiterter Editor** auf der Registerkarte **Integration** Ihrer Funktion. Der erweiterte Editor können Sie die *function.json* direkt im Portal bearbeiten.

## <a name="dynamic-parameter-binding"></a>Dynamische Parameter binden 

Statt eine statische Konfiguration für Ausgabeeigenschaften binden können Sie die Einstellungen dynamisch an Daten gebunden werden, die Teil des Auslösers input Bindung konfigurieren. Beispielszenario: neue Aufträge werden mit einer Azure Storage-Warteschlange verarbeitet. Jede neue Warteschlangenelement ist eine JSON-Zeichenfolge, die mindestens der folgenden Eigenschaften:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

Sie möchten dem Kunden senden eine SMS-Nachricht mit Ihrem Konto Twilio als Aktualisierung der Bestellung.  Konfigurieren Sie die `body` und `to` Feld der Twilio Ausgabe Bindung dynamisch gebunden werden die `name` und `mobileNumber` , waren Teil der Eingabe wie folgt.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
Jetzt muss der Funktionscode nur Ausgabeparameter zu initialisieren. Während der Ausführung werden die Ausgabeeigenschaften mit den gewünschten Eingabedaten gebunden.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Zufällige GUIDs

Azure Funktionen bietet eine Syntax mit der zufälligen GUIDs generieren. Die folgenden Bindungssyntax schreibt Ausgabe in ein neues BLOB mit einem eindeutigen Namen in einem Container Azure-Speicher: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Eine einzelne Ausgabe zurückgeben

Bei der Funktionscode eine einzelne Ausgabe zurück können Sie eine Ausgabe-Bindung mit dem Namen `$return` eine natürlichere Signatur im Code beibehalten. Dies kann nur mit Sprachen verwendet werden, die einen Rückgabewert (C#, Node.js, F#) unterstützen. Die Bindung wäre Blob Ausgabe Bindung ähnlich, die Warteschlange Trigger verwendet wird.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


Im folgende C#-Code gibt die Ausgabe natürlich ohne ein `out` Parameter in der Signatur.


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Diese Vorgehensweise ist unten mit Node.js veranschaulicht.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F# Beispiel unten.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

Dies kann auch verwendet werden, mit mehreren Ausgabeparametern durch Festlegen einer einzelnen Ausgabe mit `$return`.


## <a name="route-support"></a>Route-Unterstützung

Beim Erstellen einer Funktion für eine HTTP-Trigger oder WebHook, ist die Funktion standardmäßig mit einer Route des Formulars angesprochen:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Sie können diese Route mit dem optionalen `route` -Eigenschaft der HTTP-Trigger Benutzereingabe Bindung. Beispielsweise die folgende *function.json* Datei definiert eine `route` -Eigenschaft für eine HTTP-Trigger:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Mit dieser Konfiguration ist die Funktion nun mit den folgenden anstelle des ursprünglichen Arbeitsplans adressierbar.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Dadurch wird den Funktionscode unterstützt zwei Parameter in der Adresse `category` und `id`. Alle [Web API Route Einschränkung](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) können die Parameter. Der folgende C#-Funktionscode beide Parameter verwendet.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Hier steht Node.js-Funktion mit Routenparametern.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Standardmäßig sind alle Funktion Routen *api*vorangestellt. Sie anpassen oder entfernen Sie das Präfix mit der `http.routePrefix` -Eigenschaft in der Datei *host.json* . Das folgende Beispiel entfernt das Routenpräfix *api* mit einer leeren Zeichenfolge für das Präfix in der Datei *host.json* .

    {
      "http": {
        "routePrefix": ""
      }
    }

Ausführliche Informationen zum Aktualisieren der Datei *host.json* Funktion finden Sie unter [Funktion app aktualisieren Dateien](functions-reference.md#fileupdate). 

Bei dieser Konfiguration hinzufügen, ist die Funktion adressierbare mit den folgenden:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Informationen zu anderen Eigenschaften, die in der Datei *host.json* konfigurieren können, finden Sie unter [host.json Verweis](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in folgenden Ressourcen:

* [Testen einer Funktion](functions-test-a-function.md)
* [Skalieren einer Funktion](functions-scale.md)
