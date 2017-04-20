<properties
    pageTitle="Bindings Azure Funktionen DocumentDB | Microsoft Azure"
    description="Verstehen Sie, wie Azure DocumentDB Bindings in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktionen, Funktionen, Verarbeitung von Ereignissen, dynamische Compute serverlose Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-documentdb-bindings"></a>Azure Funktionen DocumentDB Bindung

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel beschreibt das Konfigurieren und Code Azure DocumentDB Bindings in Azure Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB Eingabe Bindung

Eingabebindungen können ein Dokument aus DocumentDB laden und direkt an die Bindung übergeben. Die Dokument-Id kann ermittelt werden, basierend auf dem Auslöser, der die Funktion aufgerufen. In einem C#-Funktion werden Änderungen an den Datensatz automatisch zur Auflistung gesendet, wenn die Funktion erfolgreich beendet wird.

#### <a name="functionjson-for-documentdb-input-binding"></a>Function.JSON für die DocumentDB input Bindung

Die Datei *function.json* enthält die folgenden Eigenschaften:

- `name`: Variablenname in den Code für das Dokument.
- `type`: muss auf "Documentdb" festgelegt werden.
- `databaseName`Die Datenbank des Dokuments.
- `collectionName`Die Auflistung mit dem Dokument.
- `id`Die Id des Dokuments abgerufen. Diese Eigenschaft unterstützt die Bindung "{QueueTrigger}" ähnelt dem verwendet des Zeichenfolgenwert der warteschlangennachricht als Dokument-ID
- `connection`Diese Zeichenfolge muss eine Anwendung Einrichtung der Endpunkt für Ihr Konto DocumentDB. Wenn Sie Ihr Konto auf der Registerkarte Integration auswählen, wird eine neue Einstellung für die Anwendung Sie mit einem Namen erstellt, die das Formular YourAccount_DOCUMENTDB akzeptiert. Möchten Sie die App-Einstellung manuell erstellen, müssen die tatsächlichen Verbindungszeichenfolge Formular AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- "Richtung: *"*in "festgelegt werden.

Beispiel *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB input Beispiels für C# Warteschlange trigger
 
Mit dem obigen Beispiel function.json, DocumentDB input Bindung das Dokument mit der Id, die Warteschlange Meldungszeichenfolge entspricht, abruft und 'Document'-Parameter übergeben. Wenn das Dokument nicht gefunden wird, wird der Parameter 'Dokument' null sein. Das Dokument wird mit dem neuen Textwert dann aktualisiert, wenn die Funktion beendet wird.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB input Beispiels für einen Trigger F#-Warteschlange

Mit dem obigen Beispiel function.json, DocumentDB input Bindung das Dokument mit der Id, die Warteschlange Meldungszeichenfolge entspricht, abruft und 'Document'-Parameter übergeben. Wenn das Dokument nicht gefunden wird, wird der Parameter 'Dokument' null sein. Das Dokument wird mit dem neuen Textwert dann aktualisiert, wenn die Funktion beendet wird.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

Sie müssen eine `project.json` Datei mit NuGet an der `FSharp.Interop.Dynamic` und `Dynamitey` Pakete als Paket abhängig davon:

    {
      "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
          }
        }
      }
    }

Diese NuGet zum Abrufen der Abhängigkeiten und verweisen sie in Ihrem Skript.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB input Beispiels für Node.js Warteschlange trigger
 
Mit dem obigen Beispiel function.json, DocumentDB input Bindung wird das Dokument mit der Id, die Warteschlange Meldungszeichenfolge entspricht, abrufen und übergeben diese an die `documentIn` Eigenschaft binden. Node.js Funktionen erhalten aktualisierte Dokumente nicht zur Auflistung. Sie können jedoch übergeben input Bindung an eine DocumentDB Ausgabe Bindung benannten `documentOut` Updates unterstützen. In diesem Codebeispiel wird die Text-Eigenschaft des Eingabedokuments aktualisiert und als Dokument festgelegt.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB Ausgabe Bindungen

Funktionen können JSON schreiben Dokumente mit einer Azure DocumentDB Datenbank **Azure DocumentDB Dokument** Ausgabe Bindung. Überprüfen Sie weitere Azure DocumentDB [Einführung in DocumentDB](../documentdb/documentdb-introduction.md) und [Erste Schritte-Lernprogramm](../documentdb/documentdb-get-started.md).

#### <a name="functionjson-for-documentdb-output-binding"></a>Function.JSON für DocumentDB Ausgabe Bindung

Die Datei function.json enthält die folgenden Eigenschaften:

- `name`: Variablenname in den Code für das neue Dokument.
- `type`: muss auf *"Documentdb"*festgelegt werden.
- `databaseName`Die Datenbank der Auflistung, in das neue Dokument erstellt werden.
- `collectionName`Die Auflistung, in das neue Dokument erstellt werden.
- `createIfNotExists`: Dies ist ein boolescher Wert, der angibt, ob die Auflistung erstellt wird, wenn er nicht vorhanden ist. Der Standardwert ist *false*. Der Grund ist neue Sammlungen werden erstellt mit reserviert, die Preise auswirken. Besuchen Sie für Weitere Informationen die [Preisseite](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`Diese Zeichenfolge muss eine **Anwendungseinstellung** Endpunkt Ihres DocumentDB-Kontos festgelegt sein. Wenn Sie Ihr Konto auf der Registerkarte **Integration** auswählen, eine neue Anwendung Einstellung erstellt werden Sie mit einem Namen, der folgende Form annimmt `yourAccount_DOCUMENTDB`. Möchten Sie die App-Einstellung manuell erstellen, müssen die aktuelle Verbindungszeichenfolge folgende Form `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: *"*Out" festgelegt werden. 
 
Beispiel function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB Ausgabe Codebeispiel Node.js Warteschlange Trigger

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Dokument:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB Ausgabe Codebeispiel für einen Trigger F#-Warteschlange

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB Ausgabe Codebeispiel für C# Warteschlange trigger


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB Code Beispiel Einstellung Ausgabedateiname

Wenn Sie den Namen des Dokuments in der Funktion festlegen möchten, legen Sie nur die `id` Wert.  Wenn beispielsweise JSON für einen Mitarbeiter in der Warteschlange ähnlich der folgenden gelöscht wurde:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

Im folgenden C#-Code können in einer Warteschlange Trigger: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Oder der entsprechende F#-Code:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Ausgabe:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
