<properties
    pageTitle="Bindung Azure Funktionen Twilio | Microsoft Azure"
    description="Beschreiben Sie, wie Twilio Bindings Azure-Funktionen verwenden."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure Funktionen Twilio Ausgabe Bindung

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Erläutert das Konfigurieren und verwenden Twilio Bindings Azure-Funktion. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure Funktionen unterstützt Ausgabe Twilio Bindung, um die Funktionen zum Senden von SMS-Nachrichten mit wenigen Zeilen Code und einem [Twilio](https://www.twilio.com/) . 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Function.JSON für Azure Notification Hub Ausgabe Bindung

Die Datei function.json enthält die folgenden Eigenschaften:

- `name`: Variablenname in den Code für die Twilio SMS-Nachricht.
- `type`: muss auf *"TwilioSms"*festgelegt werden.
- `accountSid`: Dieser Wert muss auf den Namen einer App-Einstellung festgelegt, die die Twilio Konto-Sid enthält.
- `authToken`: Dieser Wert muss auf den Namen einer App-Einstellung festgesetzt, die Ihr Authentifizierungstoken Twilio enthält.
- `to`: Die Telefonnummer festgelegt ist, die die SMS gesendet wird.
- `from`: Dieser Wert wird der Rufnummer festgelegt, die an die SMS.
- `direction`: *"*Out" festgelegt werden.
- `body`: Dieser Wert kann verwendet werden, an die SMS-Nachricht codieren möchten Sie nicht dynamisch im Code für die Funktion festlegen. 

 
Beispiel function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Wird C# Warteschlange Trigger Twilio Ausgabe Bindung

#### <a name="synchronous"></a>Synchrone

Diese synchronen Beispielcode für einen Azure Storage Queue Trigger wird mit Out-Parameter eine Textnachricht an einen Kunden senden, die Bestellung.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Asynchrone

Diese asynchronen Beispielcode für einen Trigger Azure Storage Warteschlange sendet eine Textnachricht an einen Kunden, der eine Bestellung aufgegeben.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Beispiel Node.js Warteschlange Trigger Twilio Ausgabe Bindung

In diesem Beispiel Node.js sendet eine Textnachricht an einen Kunden, der eine Bestellung aufgegeben.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
