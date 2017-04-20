<properties
    pageTitle="Verwendung von Servicebuswarteschlangen mit Java | Microsoft Azure"
    description="Erfahren Sie in Azure Service Bus-Warteschlangen verwenden. Codebeispiele in Java geschrieben."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Wie Servicebuswarteschlangen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dieser Artikel beschreibt, wie Sie Servicebuswarteschlangen. Die Proben sind in Java geschrieben und [Azure SDK für Java][]verwenden. Die Szenarios enthalten **Warteschlangen erstellen**, **Senden und Empfangen von Nachrichten**und **Warteschlangen löschen**.

## <a name="what-are-service-bus-queues"></a>Was sind Service Bus-Warteschlangen?

Servicebuswarteschlangen unterstützen **vermittelten messaging** Kommunikationsmodell. Wenn Warteschlangen kommunizieren Komponenten einer verteilten Anwendung nicht direkt; Stattdessen tauschen sie Nachrichten über eine Warteschlange fungiert als Vermittler (Broker). Nachricht Erzeuger (Absender) gibt eine Meldung an die Warteschlange und dann die Verarbeitung.
Asynchrone Nachricht Verbraucher (Empfänger) Ruft die Nachricht aus der Warteschlange und verarbeitet. Der Hersteller muss nicht warten auf eine Antwort vom Consumer um verarbeiten und weitere Nachrichten senden. Warteschlangen bieten **erste, erste Out (FIFO)** Nachrichten über einen oder mehrere konkurrierende Consumer. D. h. Nachrichten normalerweise empfangen und vom Empfänger in der Reihenfolge, in der sie der Warteschlange hinzugefügt wurden und jede Nachricht empfangen und verarbeitet nur eine Nachricht Verbraucher, verarbeitet.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Servicebuswarteschlangen sind eine allgemeine Technologie für eine Vielzahl von Szenarios verwendet werden kann:

- Kommunikation zwischen Web- und Workerrollen Rollen in einem mehrstufigen Azure-Anwendung.
- Kommunikation zwischen lokalen apps und Azure gehostet apps in einer Hybrid-Lösung.
- Kommunikation zwischen Komponenten einer verteilten Anwendung in verschiedenen Organisationen oder Abteilung einer Organisation lokal ausgeführt.

Mit Warteschlangen können Sie Ihre Anwendung problemlos skalieren und Stabilität Ihrer Architektur.

## <a name="create-a-service-namespace"></a>Erstellen eines Service-Namespaces

Zunächst in Azure Service Bus-Warteschlangen verwenden, müssen Sie zuerst einen Namespace erstellen. Ein Namespace bietet ein Bereichseinheit Service Bus Ressourcen in der Anwendung.

So erstellen Sie einen namespace

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Sie die Anwendung für Service Bus

Stellen Sie sicher, dass Sie [Azure SDK für Java][] installiert haben, bevor Sie dieses Beispiel erstellen. Wenn Sie Eclipse verwenden, können Sie der [Azure-Toolkit für Eclipse][] installieren, Azure SDK für Java enthält. Sie können das Projekt dann **Microsoft Azure Bibliotheken für Java** hinzufügen:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Fügen Sie den folgenden `import` -Anweisung am Anfang der Datei Java:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Verwaltungsvorgänge für Servicebuswarteschlangen erfolgt über die **ServiceBusContract** -Klasse. Ein **ServiceBusContract** -Objekt mit einer entsprechenden Konfiguration, die SAS-Token mit Berechtigungen zur Verwaltung kapselt erstellt und die **ServiceBusContract** -Klasse ist der einzige Kommunikation mit Azure.

Die **ServiceBusService** -Klasse stellt Methoden zum Erstellen, auflisten und Löschen von Warteschlangen. Das folgende Beispiel zeigt, wie ein **ServiceBusService** -Objekt zum Erstellen einer Warteschlange mit dem Namen "TestQueue" mit einem Namespace namens "HowToSample":

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Gibt es Methoden für **QueueInfo** , Eigenschaften der Warteschlange zu optimierende (Beispiel: festlegen den Standardwert Time to live (TTL) an die Warteschlange gesendeten Nachrichten angewendet werden). Im folgende Beispiel veranschaulicht das Erstellen einer Warteschlange mit dem Namen `TestQueue` mit einer maximalen Größe von 5GB:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Beachten Sie, dass Sie die **ListQueues** -Methode für **ServiceBusContract** Objekte um zu überprüfen, ob eine Warteschlange mit einem bestimmten Namen in einem Namespace Service bereits.

## <a name="send-messages-to-a-queue"></a>Nachrichten in einer Warteschlange

Zum Senden einer Nachricht an eine Warteschlange Service Bus erhält die Anwendung ein **ServiceBusContract** -Objekt. Der folgende Code zeigt, wie eine Nachricht für die `TestQueue` Warteschlange zuvor erstellte der `HowToSample` Namespace:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Nachrichten an und Empfangen von Service Bus Warteschlangen sind Instanzen der Klasse [BrokeredMessage][] . [BrokeredMessage][] -Objekte haben eine Reihe von Standardeigenschaften (wie [Beschriftung](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) und [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) ein Wörterbuch mit benutzerdefinierten Anwendung bestimmte Eigenschaften sowie eine beliebige Anwendung Daten. Eine Anwendung kann Hauptteil der Nachricht serialisierbares Objekt an den Konstruktor des [BrokeredMessage][]übergeben, und das entsprechende Serialisierungsprogramm dann zum Serialisieren des Objekts verwendet. Alternativ können Sie Java **bereitstellen. IO. InputStream** Objekt.

Im folgenden Beispiel wird veranschaulicht, wie fünf Testnachrichten zum Senden der `TestQueue` **MessageSender** im vorherigen Codeausschnitt erzielten:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Servicebuswarteschlangen unterstützt eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB im [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Gibt es keine Beschränkung für die Anzahl der Nachrichten in einer Warteschlange, jedoch ist es die Gesamtgröße der Nachrichten von einer Warteschlange. Diese Größe wird zum Zeitpunkt der Erstellung mit einem oberen Grenzwert von 5 GB definiert.

## <a name="receive-messages-from-a-queue"></a>Empfangen von Nachrichten aus einer Warteschlange

Das Hauptverfahren zum Empfangen von Nachrichten aus einer Warteschlange ist ein **ServiceBusContract** -Objekt verwenden. Nachrichten können in zwei verschiedenen Modi arbeiten: **ReceiveAndDelete** und **PeekLock**.

Erhalten mit dem Modus **ReceiveAndDelete** ist ein Betrieb Einzelbild - also wenn Service Bus eine Leseoperation für eine Nachricht in einer Warteschlange empfängt, es markiert die Nachricht als verbraucht und an die Anwendung zurückgegeben. **ReceiveAndDelete** (der Standardmodus) ist das einfachste und am besten für Szenarios, in denen eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung.
Da Service Bus die Nachricht als verbraucht markiert haben wird, wird dann die Anwendung neu gestartet und beginnt, Nachrichten erneut es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Im **PeekLock** Modus empfangen wird ein zweistufiger Vorgang unterstützt Clientanwendungen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nach die Anwendung Verarbeitung der Meldung beendet (oder zuverlässig zur späteren Verarbeitung speichert) abgeschlossen empfangenen Nachricht fordert **Löschen** die zweite Phase des Prozesses empfangen wird. Service Bus **Delete** -Aufruf sieht, wird die Nachricht als verbraucht und aus der Warteschlange entfernt.

Im folgenden Beispiel wird veranschaulicht, wie Nachrichten empfangen und verarbeitet **PeekLock** Modus (nicht die Standardeinstellung). Im folgenden Beispiel wird eine unendliche Schleife und verarbeitet Nachrichten eintreffen in unsere "TestQueue":

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendungsabstürzen und unlesbar Nachrichten

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Ist eine empfängeranwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht können sie die Methode **UnlockMessage** in der empfangenen Nachricht (nicht **DeleteMessage** ) aufrufen. Dadurch Service Bus zu entsperren der Nachricht in der Warteschlange zur wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht in der Warteschlange gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Timeout für die Sperre abläuft (z. B. wenn die Anwendung abstürzt), Service Bus wird die Nachricht automatisch entsperrt und Verfügbarmachen erneut empfangen werden.

Die Anwendung stürzt nach dem Verarbeiten der Nachricht, aber vor Anforderung **DeleteMessage** , wird dann die Meldung an die Anwendung zurückgeliefert werden beim Neustart. Wird oft genannt **Mindestens einmal verarbeiten**, d. h. jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler ihre Anwendung doppelte Nachrichtenübermittlung behandeln zusätzlichen Logik hinzufügen. Dies geschieht häufig mit der **GetMessageId** -Methode der Nachricht über Übermittlungsversuche konstant bleibt.

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Servicebuswarteschlangen bearbeitet haben, finden Sie weitere Informationen [Warteschlangen, Themen und Abonnements][] .

Weitere Informationen finden Sie unter [Java Developer Center](/develop/java/).


  [Azure SDK für Java]: http://azure.microsoft.com/develop/java/
  [Azure-Toolkit für Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

