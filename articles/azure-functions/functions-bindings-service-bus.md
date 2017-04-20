<properties
    pageTitle="Azure Service Bus Funktionen Trigger und Bindungen | Microsoft Azure"
    description="Verstehen Sie, wie Azure Service Bus-Trigger und Bindungen in Azure-Funktionen verwenden."
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

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure Service Bus Funktionen Trigger und Bindungen Warteschlangen und Themen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel beschreibt konfigurieren und Code Azure Service Bus Trigger und Bindungen in Azure-Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Service Bus-Warteschlange oder Thema trigger

#### <a name="functionjson"></a>Function.JSON

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`Der Variablenname in den Code für die Warteschlange oder Thema oder die Warteschlange oder Thema Nachricht verwendet. 
- `queueName`Warteschlange für trigger, der Name der Warteschlange abgefragt.
- `topicName`: Für Thema trigger, der Name des Themas abzurufen.
- `subscriptionName`: Für Thema trigger, der Name des Abonnements.
- `connection`: Der Name einer App festlegen, enthält eine Verbindungszeichenfolge Service Bus. Die Verbindungszeichenfolge muss für einen Service Bus-Namespace nicht auf eine bestimmte Warteschlange oder ein Thema beschränkt. Wenn die Verbindungszeichenfolge nicht verwalten haben Rechte, die `accessRights` Eigenschaft. Wenn Sie lassen `connection` leer, Trigger oder Bindung funktioniert mit Service Bus-Verbindungszeichenfolge für Funktion app Speicherverbindungszeichenfolgen app-Einstellung angegeben.
- `accessRights`: Gibt die Zugriffsrechte für die Verbindungszeichenfolge. Standardwert ist `manage`. Legen Sie auf `listen` Berechtigungen verwalten, verwenden Sie eine Verbindungszeichenfolge, die zur Verfügung steht. Verwaltung von Runtime versuchen Funktionen und nicht für Aufgaben, die erfordern Rechte.
- `type`: Muss auf *ServiceBusTrigger*festgelegt werden.
- `direction`: Muss *in*festgelegt werden. 

Beispiel *function.json* Service Bus Warteschlange Trigger:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C# -[NULL] Codebeispiel, das eine Nachricht Service Bus-Warteschlange verarbeitet

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F# Codebeispiel, die Verarbeitung einer Nachricht Service Bus-Warteschlange

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Node.js-Codebeispiel, das eine Nachricht Service Bus-Warteschlange verarbeitet

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Unterstützte Typen

Message Queue Service Bus kann einer der folgenden Typen deserialisiert werden:

* Objekt (JSON)
* Zeichenfolge
* Byte-array 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>PeekLock-Verhalten

Funktionen Runtime Meldung in `PeekLock` Modus und ruft `Complete` angezeigt, wenn die Funktion erfolgreich abgeschlossen wird oder Aufrufe `Abandon` Wenn die Funktion fehlschlägt. Läuft die Funktion mehr als das `PeekLock` Timeout die Sperre automatisch erneuert.

#### <a id="sbpoison"></a>Handhabung beschädigter Nachrichten

Service Bus ist eigene nicht verarbeitbare Nachricht behandeln, die nicht gesteuert oder Konfiguration von Azure Funktionen oder Code konfiguriert. 

#### <a id="sbsinglethread"></a>Single-threading

Laufzeit verarbeitet mehrere Warteschlangennachrichten standardmäßig Funktionen gleichzeitig. Legen Sie direkt die Laufzeit nur eine einzige Warteschlange oder Thema Nachricht verarbeiten, `serviceBus.maxConcurrrentCalls` 1 in der Datei *host.json* . Informationen über die *host.json* -Datei finden Sie unter [Ordnerstruktur](functions-reference.md#folder-structure) im Referenzartikel für Entwickler und [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) im WebJobs.Script Repository-Wiki.

## <a id="sboutput"></a>Service Bus-Warteschlange oder ein Thema Bindung ausgeben

#### <a name="functionjson"></a>Function.JSON

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`Der Variablenname Funktionscode für die Warteschlange oder warteschlangennachricht verwendet. 
- `queueName`Warteschlange für trigger, der Name der Warteschlange abgefragt.
- `topicName`: Für Thema trigger, der Name des Themas abzurufen.
- `subscriptionName`: Für Thema trigger, der Name des Abonnements.
- `connection`: Wie Service Bus auslösen.
- `accessRights`: Gibt die Zugriffsrechte für die Verbindungszeichenfolge. Standardwert ist `manage`. Legen Sie auf `send` Berechtigungen verwalten, verwenden Sie eine Verbindungszeichenfolge, die zur Verfügung steht. Verwaltung von Runtime versuchen Funktionen und nicht für Aufgaben, die erfordern wie Warteschlangen erstellt.
- `type`: Muss auf *ServiceBus*festgelegt werden.
- `direction`: *Out*muss festgelegt werden. 

Beispiel- *function.json* mit einem Timer Trigger Service Bus Warteschlangennachrichten schreiben:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Unterstützte Typen

Azure Funktionen können eine Service Bus warteschlangennachricht aus den folgenden Typen erstellen.

* Objekt (immer eine JSON-Nachricht erstellt, die Nachricht mit einem null-Objekt erstellt, wenn der Wert null ist, wenn die Funktion beendet)
* Zeichenfolge (eine Nachricht erstellt, wenn der Wert nicht Null ist, nach Beendigung der Funktion)
* Bytearray (funktioniert wie) 
* `BrokeredMessage`(C#, wie String)

Für das Erstellen mehrerer Nachrichten in einer C#-Funktion können Sie `ICollector<T>` oder `IAsyncCollector<T>`. Wird eine Nachricht erstellt, beim Aufrufen der `Add` Methode.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# Codebeispiele, in denen Service Bus Warteschlangennachrichten erstellen

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F# Codebeispiel, das einer Service Bus Warteschlange Nachricht

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Node.js-Codebeispiel, das einer Service Bus Warteschlange Nachricht

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
