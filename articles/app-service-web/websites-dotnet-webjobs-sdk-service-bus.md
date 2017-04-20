<properties 
    pageTitle="Verwendung von Azure Service Bus mit WebJobs SDK" 
    description="Informationen Sie zum Azure Service Bus-Warteschlangen und Themen WebJobs SDK verwenden." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Verwendung von Azure Service Bus mit WebJobs SDK

## <a name="overview"></a>Übersicht

Dieses Handbuch bietet C# Codebeispiele, die zeigen, wie einen Prozess ausgelöst wird, wenn eine von Azure Service Bus Nachricht. Die Codebeispiele verwenden [Webaufträge SDK](websites-dotnet-webjobs-sdk.md) -Version 1.x.

Das Handbuch setzt [ein Webauftrag Projekt in Visual Studio mit Verbindungszeichenfolgen erstellt das Speicherkonto darauf](websites-dotnet-webjobs-sdk-get-started.md)vertraut.

Die Codeausschnitte nur Funktionen nicht den erstellten Code der `JobHost` Objekt wie in diesem Beispiel:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

Ein [vollständiges Codebeispiel Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) ist im Repository Azure-Webaufträge-Sdk-Beispielen auf GitHub.com.

## <a id="prerequisites"></a>Erforderliche Komponenten

Mit Service Bus müssen Sie [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet-Paket neben anderen WebJobs SDK Pakete installieren. 

Sie müssen die Verbindungszeichenfolge Speicherverbindungszeichenfolgen neben Verbindungszeichenfolgen Speicher festgelegt.  Sie können dabei die `connectionStrings` Abschnitt der Datei App.config, wie im folgenden Beispiel gezeigt:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Ein Beispielprojekt, die Einstellung für die Service Bus Verbindungszeichenfolge in der Datei App.config enthält, finden Sie unter [Service Bus-Beispiel](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Die Verbindungszeichenfolgen können auch in Azure Runtime-Umgebung festgelegt werden die App.config Einstellungen dann überschrieben, wenn der Webauftrag in Azure ausgeführt wird. Weitere Informationen finden Sie unter [Erste Schritte mit dem SDK Webaufträge](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Wie eine Funktion ausgelöst wird, wenn ein Service Bus Warteschlange Mitteilung

Verwenden, um eine Funktion zu schreiben, das WebJobs SDK aufruft, wenn eine Warteschlange Nachricht die `ServiceBusTrigger` Attribut. Der Attributkonstruktor hat einen Parameter mit dem Namen der Warteschlange abgefragt.

### <a name="how-servicebustrigger-works"></a>Funktionsweise von ServiceBusTrigger

Das SDK Meldung in `PeekLock` Modus und ruft `Complete` angezeigt, wenn die Funktion erfolgreich abgeschlossen wird oder Aufrufe `Abandon` Wenn die Funktion fehlschlägt. Läuft die Funktion mehr als das `PeekLock` Timeout die Sperre automatisch erneuert.

Service Bus ist die eigene Behandlung durch Gift Warteschlange gesteuert oder WebJobs SDK konfiguriert werden. 

### <a name="string-queue-message"></a>Zeichenfolgennachricht Warteschlange

Das folgende Codebeispiel liest eine warteschlangennachricht, die eine Zeichenfolge und schreibt die Zeichenfolge WebJobs SDK-Dashboard.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Hinweis:** Beim Erstellen der Warteschlangennachrichten in einer Anwendung, die das WebJobs SDK verwendet, stellen Sie [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) auf "Text/Plain".

### <a name="poco-queue-message"></a>POCO-warteschlangennachricht

Das SDK automatisch eine warteschlangennachricht mit JSON für ein POCO deserialisiert [(Plain Old CLR-Objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) geben. Das folgende Codebeispiel liest eine warteschlangennachricht mit einem `BlobInformation` Objekt hat eine `BlobName` Eigenschaft:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Verwendung der POCO Eigenschaften von Blobs und Tabellen in derselben Funktion Codebeispiele finden Sie unter [Speicher Warteschlangen-Version dieses Artikels](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Wenn Code, der die Warteschlange Nachricht WebJobs SDK nicht, verwenden Sie Code ähnlich dem folgenden Beispiel:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typen ServiceBusTrigger arbeitet mit

Neben `string` und POCO-Typen können Sie die `ServiceBusTrigger` -Attribut mit einem Bytearray oder `BrokeredMessage` Objekt.

## <a id="create"></a>Service Bus Warteschlangennachrichten erstellen

Eine Funktion schreiben, eine neue Nachricht Verwendung erstellt, der `ServiceBus` -Attribut und den Namen der Warteschlange an den Attributkonstruktor übergeben. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Erstellen Sie eine einzelne warteschlangennachricht nicht Async-Funktion

Das folgende Codebeispiel verwendet einen Output-Parameter zum Erstellen einer neuen Nachricht in der Warteschlange mit dem Namen "Outputqueue" mit denselben Inhalt wie die Meldung in der Warteschlange mit dem Namen "Inputqueue".

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Der Ausgabeparameter für eine einzelne warteschlangennachricht erstellen möglich der folgenden Typen:

* `string`
* `byte[]`
* `BrokeredMessage`
* Ein serialisierbarer POCO-Typ, den Sie definieren. Automatisch serialisiert als JSON.

POCO-Typparameter wird eine warteschlangennachricht immer nach Beendigung der Funktion erstellt. Wenn der Parameter null ist, erstellt das SDK eine warteschlangennachricht, die null zurückgibt, wenn die Meldung empfangen und deserialisiert wird. Wenn der Parameter null ist, wird für die anderen Arten keine warteschlangennachricht erstellt.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Erstellen mehrerer Warteschlangennachrichten oder Async-Funktionen

Um mehrere Nachrichten zu erstellen, verwenden Sie die `ServiceBus` Attribut mit `ICollector<T>` oder `IAsyncCollector<T>`, wie im folgenden Beispiel gezeigt:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Jede warteschlangennachricht sofort erstellt bei der `Add` wird aufgerufen.

## <a id="topics"></a>Arbeiten mit Service Bus-Themen

Verwenden, um eine Funktion schreiben, die das SDK beim Thema Service Bus eine Nachricht Aufrufen der `ServiceBusTrigger` Attribut mit Konstruktors, Thema und Namen, wie im folgenden Beispiel gezeigt:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Um eine Meldung zu einem Thema zu erstellen, verwenden Sie die `ServiceBus` Attribut mit einem Thema Namen wie Sie es mit einem Warteschlangennamen.

## <a name="features-added-in-release-11"></a>Neue Funktionen in Version 1.1

Die folgenden Funktionen wurden in Version 1.1 hinzugefügt:

* Ermöglichen die Anpassung der Verarbeitung über Tiefen `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`unterstützt die Anpassung des Service Bus `MessagingFactory` und `NamespaceManager`.
* Ein `MessageProcessor` Strategiemuster können Sie einen Prozessor pro Warteschlange/Thema angeben.
* Meldung Verarbeitung Parallelität wird standardmäßig unterstützt. 
* Einfache Anpassung der `OnMessageOptions` über `ServiceBusConfiguration.MessageOptions`.
* [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) in angegeben werden können `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (für Szenarios, in denen Sie möglicherweise keine Berechtigungen verwalten). 

## <a id="queues"></a>Der Speicher Warteschlangen Artikel Verwandte Themen

Informationen zu WebJobs SDK Szenarien nicht spezifisch für Service Bus finden Sie unter [Azure Warteschlangenspeicher Webaufträge SDK verwenden](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

In diesem Artikel behandelten Themen umfassen Folgendes:

* Async-Funktion
* Mehrere Instanzen
* Ordnungsgemäßes Herunterfahren
* WebJobs SDK Attribute im Hauptteil einer Funktion
* Die SDK-Verbindungszeichenfolgen im Code festlegen
* Legen Sie Werte für WebJobs SDK Parameter im code
* Eine Funktion manuell auslösen
* Schreiben von Protokollen

## <a id="nextsteps"></a>Nächste Schritte

Dieses Handbuch hat Codebeispiele, die veranschaulichen, wie häufige Szenarios für die Arbeit mit Azure Service Bus. Weitere Informationen zur Verwendung von Azure Webaufträge und WebJobs SDK finden Sie unter [Azure Webaufträge empfohlene Ressourcen](http://go.microsoft.com/fwlink/?linkid=390226).
 
