<properties
    pageTitle="Azure Funktionen Event Hub Bindings | Microsoft Azure"
    description="Verstehen Sie, wie Azure Event Hub Bindings in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure Funktionen Event Hub-Bindung

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Erläutert das Konfigurieren und Code [Azure Event Hub](../event-hubs/event-hubs-overview.md) Bindings für Azure. Azure Funktionen unterstützen Trigger und Ausgabe Bindings für Azure Ereignis Hubs.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Wenn Sie neue Azure Ereignis Hubs sind, finden Sie [Azure Event Hub Overview](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure Event Hub Bindung auslösen

Ein Trigger Azure Event Hub kann verwendet werden, auf ein Ereignis Hub Ereignisstream gesendetes Ereignis reagieren. Sie müssen auf Ereignis Hub Trigger Bindung einzurichten.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Function.JSON Ereignis Hub auslösen Bindung

Die *function.json* -Datei für einen Trigger Azure Event Hub gibt die folgenden Eigenschaften:

- `type`: Muss auf *EventHubTrigger*festgelegt werden.
- `name`Der Variablenname zum Hub Ereignisnachricht Funktionscode. 
- `direction`: Muss *in*festgelegt werden. 
- `path`: Der Name des Ereignisses Hubs.
- `consumerGroup`: Dies ist eine optionale Eigenschaft, die zum Abonnieren von Ereignissen im Hub [Verbrauchergruppe](../event-hubs-overview.md#consumer-groups) festgelegt. Wenn nicht angegeben, die `$Default` Verbrauchergruppe verwendet. 
- `connection`: Der Name einer App festlegen, enthält die Verbindungszeichenfolge für den Namespace, dem in der Ereignis-Hub befindet. Kopieren Sie diese Verbindungszeichenfolge Schaltfläche **Verbindungsinformationen** für den Namespace nicht Ereignis Hub selbst.  Diese Verbindungszeichenfolge muss mindestens den Trigger aktivieren Leseberechtigungen.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Azure Event Hub Trigger C#-Beispiel
 
Das obige Beispiel function.json verwenden, wird der Hauptteil der Meldung protokolliert C#-Funktion folgenden Code:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Azure Event Hub Trigger F#-Beispiel

Das obige Beispiel function.json verwenden, wird der Hauptteil der Meldung protokolliert F#-Funktion folgenden Code:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure Event Hub Trigger Node.js-Beispiel
 
Das obige Beispiel function.json verwenden, wird der Hauptteil der Meldung protokolliert Node.js-Funktion folgenden Code:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure Event Hub Ausgabe Bindung

Azure Event Hub Ausgabe Bindung wird verwendet, um Ereignisse in ein Ereignis Hub Ereignisstream schreiben. Send-Berechtigung an einen Hub Ereignis Ereignisse schreiben müssen. 

#### <a name="functionjson-for-event-hub-output-binding"></a>Function.JSON Ereignis Hub Ausgabe Bindung

Die *function.json* für Azure Event Hub Ausgabedatei Bindung die folgenden Eigenschaften an:

- `type`: Muss auf *EventHub*festgelegt werden.
- `name`Der Variablenname zum Hub Ereignisnachricht Funktionscode. 
- `path`: Der Name des Ereignisses Hubs.
- `connection`: Der Name einer App festlegen, enthält die Verbindungszeichenfolge für den Namespace, dem in der Ereignis-Hub befindet. Kopieren Sie diese Verbindungszeichenfolge Schaltfläche **Verbindungsinformationen** für den Namespace nicht Ereignis Hub selbst.  Diese Verbindungszeichenfolge müssen Berechtigungen zum Senden der Nachricht in den Stream Event Hub senden.
- `direction`: *Out*muss festgelegt werden. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure Event Hub C# Code beispielsweise Ausgabe Bindung
 
Im folgende C#-Funktion Beispielcode veranschaulicht ein Ereignis Hub Ereignisstream ein Ereignis schreiben. Dieses Beispiel stellt C# Timer Trigger Event Hub Ausgabe oben angegebene Bindung zugewiesen.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure Event Hub F# Codebeispiel für Ausgabe Bindung

Der folgende F#-Funktion aus zeigt ein Ereignis Hub Ereignisstream ein Ereignis schreiben. Dieses Beispiel stellt C# Timer Trigger Event Hub Ausgabe oben angegebene Bindung zugewiesen.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure Event Hub Node.js-Codebeispiel für Ausgabe Bindung
 
Der folgende Beispielcode Funktion Node.js veranschaulicht ein Ereignis Hub Ereignisstream ein Ereignis schreiben. Dieses Beispiel stellt Node.js Timer Trigger Event Hub Ausgabe oben angegebene Bindung zugewiesen.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
