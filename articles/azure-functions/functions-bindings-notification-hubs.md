<properties
    pageTitle="Azure Funktionen Notification Hub Bindung | Microsoft Azure"
    description="Verstehen Sie, wie Azure Notification Hub Bindung in Azure-Funktionen verwenden."
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
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure Funktionen Notification Hub Ausgabe Bindung

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel beschreibt das Konfigurieren und Code Azure Notification Hub Bindings in Azure Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Funktionen können mit einem konfigurierten Azure Notification Hub mit wenigen Codezeilen Pushbenachrichtigungen senden. Jedoch muss Azure Notification Hub für Plattform Benachrichtigungen Services (PNS) konfiguriert, die Sie verwenden möchten. Weitere Informationen zum Konfigurieren einer Azure Notification Hub und eine Benachrichtigung registrieren Clientanwendungen entwickeln, finden Sie unter [Erste Schritte mit Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) und Client Zielplattform oben auf.

Gesendete Benachrichtigung kann systemeigene Benachrichtigungen oder Vorlage Benachrichtigung. Systemeigene Benachrichtigungen Plattform eine Benachrichtigung gemäß der Konfiguration der `platform` -Eigenschaft der Bindung Ausgabe. Eine vorlagenbenachrichtigung kann für mehrere Plattformen verwendet werden.   

## <a name="notification-hub-output-binding-properties"></a>Benachrichtigung Hub Ausgabeeigenschaften Bindung

Die Datei function.json enthält die folgenden Eigenschaften:

- `name`: Variablenname in den Code für die Hub-Nachricht.
- `type`: muss auf *"NotificationHub"*festgelegt werden.
- `tagExpression`: Tag-Ausdrücke können Sie festlegen, dass Benachrichtigungen an verschiedene Geräte übermittelt werden registrierte Benachrichtigungen, die den Tag-Ausdruck übereinstimmen.  Weitere Informationen finden Sie unter [Routing und Tag-Ausdrücke](../notification-hubs/notification-hubs-tags-segment-push-message.md).
- `hubName`: Der Name der Benachrichtigung Hub Ressource in Azure-Portal.
- `connection`Diese Verbindungszeichenfolge muss eine **Anwendungseinstellung** Verbindungszeichenfolge *DefaultFullSharedAccessSignature* Wert für Ihren Notification Hub.
- `direction`: *"*Out" festgelegt werden. 
- `platform`Die Platform-Eigenschaft gibt Plattform Benachrichtigung Benachrichtigungsziele an. Einer der folgenden Werte muss sein:
    - `template`: Dies ist die Standardplattform Platform-Eigenschaft aus der Ausgabe Bindung fehlt. Vorlage Benachrichtigung können auf jeder Zielplattform konfiguriert Azure Notification Hubs verwendet werden. Finden Sie weitere Informationen im Allgemeinen mit Vorlagen mit einer Azure Notification plattformübergreifende Benachrichtigungen [Vorlagen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Apple Push Notification Service. Weitere Informationen zum Konfigurieren der benachrichtigungshub APN und Mitteilung in einer Clientanwendung finden Sie unter [Pushbenachrichtigungen IOS mit Azure Benachrichtigung senden](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Amazon Gerät Messaging](https://developer.amazon.com/device-messaging). Weitere Informationen zum Konfigurieren des Benachrichtigung Hubs für ADM und Mitteilung in Kindle-Anwendung finden Sie unter [Erste Schritte mit Notification Hubs für Kindle apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). FB Cloud Messaging ist die neue Version der GCM wird ebenfalls unterstützt. Weitere Informationen zum Konfigurieren von Notification Hub GCM-FCM und Mitteilung in einer app von Android-Client finden Sie unter [Pushbenachrichtigungen Android mit Azure Benachrichtigung senden](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) für Windows-Plattformen. Windows Phone 8.1 und höher unterstützt auch WNS. Weitere Informationen zum Konfigurieren der benachrichtigungshub WNS und Mitteilung in einer app universelle Windows-Plattform (UWP) finden Sie unter [Erste Schritte mit Notification Hubs für Windows universelle Plattform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Diese Plattform unterstützt Windows Phone 8 und frühere Windows Phone-Plattformen. Weitere Informationen zum Konfigurieren von MPNS und Mitteilung in eine Windows Phone app benachrichtigungshub finden Sie unter [Senden Pushbenachrichtigungen mit Azure Notification Hubs in Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Beispiel function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Benachrichtigung Hub Verbindung Einrichtung

Um eine Benachrichtigung Hub Ausgabe Bindung verwenden, müssen Sie die Verbindungszeichenfolge für den Hub konfigurieren. Auf der Registerkarte *Integration* können dies von Ihrem Haupt-Benachrichtigung auswählen oder eine neue erstellen. 

Sie können auch manuell eine Verbindungszeichenfolge für eine vorhandene Hub durch Hinzufügen einer Verbindungszeichenfolge für den *DefaultFullSharedAccessSignature* auf Ihrem Haupt-Benachrichtigung hinzufügen. Diese Verbindungszeichenfolge enthält die Funktion Berechtigungen zum Senden von Nachrichten. Der Zeichenfolgenwert *DefaultFullSharedAccessSignature* Verbindung erfolgt über **Schlüssel** auf die wichtigsten Blade Notification Hub Ressource in Azure-Portal. Gehen Sie folgendermaßen vor, um manuell eine Verbindungszeichenfolge für den Hub: 

1. Klicken Sie auf die **Funktion app** Azure-Portal, auf **Appeinstellungen > App Service Einstellungen**.

2. Klicken Sie im Blatt **Einstellungen** **Application Settings**.

3. Abschnitt **Verbindungszeichenfolgen** scrollen und benannten Eintrag für *DefaultFullSharedAccessSignature* Wert für Ihren Notification Hub. In **Benutzerdefiniert**ändern.
4. Verweisen Sie der Verbindungszeichenfolgenname Ausgabe Bindungen. Ähnlich wie **MyHubConnectionString** im obigen Beispiel verwendet.

## <a name="native-notification-examples"></a>Beispiele für systemeigenen Benachrichtigung

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>APN systemeigene Benachrichtigungen mit C# Warteschlange

Dieses Beispiel zeigt, wie Sie in [Microsoft Azure Notification Hubs Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierte Typen native APN-Benachrichtigung. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>Systemeigene Benachrichtigungen mit C# Warteschlange GCM

Dieses Beispiel zeigt, wie Sie in [Microsoft Azure Notification Hubs Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierte Typen native GCM-Benachrichtigung. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS systemeigene Benachrichtigungen mit C# Warteschlange

In diesem Beispiel wird veranschaulicht, wie mit [Microsoft Azure Notification Hubs Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierten Typen native WNS Popupbenachrichtigung senden. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Beispiele für Steuerelementvorlagen Benachrichtigung

#### <a name="template-example-for-nodejs-timer-triggers"></a>Vorlage beispielsweise Node.js Timer Trigger 

In diesem Beispiel sendet eine Benachrichtigung für eine [Registrierung der Dokumentvorlage](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , die enthält `location` und `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Vorlage wird für F# Timer Trigger

In diesem Beispiel sendet eine Benachrichtigung für eine [Registrierung der Dokumentvorlage](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , die enthält `location` und `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Vorlage wird mit Out-parameter 

In diesem Beispiel sendet eine Benachrichtigung für eine [Registrierung der Dokumentvorlage](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , die enthält eine `message` Platzhalter in der Vorlage.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Vorlage wird mit asynchronen

Bei Verwendung von asynchronem Code sind Parameter nicht zulässig. In diesem Fall `IAsyncCollector` vorlagenbenachrichtigung zurückgegeben. Der folgende Code ist ein asynchroner Beispiel des obigen Codes. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Vorlage Beispiel JSON 

In diesem Beispiel sendet eine Benachrichtigung für eine [Registrierung der Dokumentvorlage](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , die enthält eine `message` Platzhalter in der Vorlage eine gültige JSON-Zeichenfolge verwenden.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Vorlage Beispiel Bibliothekstypen Benachrichtigungshubs

Dieses Beispiel zeigt, wie mit [Microsoft Azure Notification Hubs Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)definierten Typen. 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
