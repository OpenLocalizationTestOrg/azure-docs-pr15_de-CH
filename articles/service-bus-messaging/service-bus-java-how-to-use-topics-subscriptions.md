<properties
    pageTitle="Verwendung von Service Bus-Themen mit Java | Microsoft Azure"
    description="Informationen Sie zum Azure Service Bus Topics und Abonnements verwenden. Codebeispiele sind für Java Applikationen geschrieben."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Zum Service Bus Topics und Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieses Handbuch beschreibt das Service Bus-Themen und Abonnements verwenden. Die Proben sind in Java geschrieben und [Azure SDK für Java][]verwenden. Die Szenarios enthalten **Themen und Abonnements erstellen**, **Abonnement Filter** **Nachrichten zu einem Thema**, **Empfangen von Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Was sind Service Bus Topics und Abonnements

Service Bus-Themen und Abonnements unterstützen eine *Veröffentlichen/Abonnieren* Kommunikationsmodell messaging. Themen und Abonnements sind Komponenten einer verteilten Anwendung nicht direkt miteinander kommunizieren; Stattdessen tauschen sie Nachrichten über ein Thema, das als Vermittler fungiert.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Im Gegensatz zu Servicebuswarteschlangen, in denen jede Nachricht einen Verbraucher verarbeitet wird bieten Themen und Abonnements eine "1-n" Form der Kommunikation Veröffentlichen-Abonnieren-Musters. Es ist möglich, mehrere Abonnements zu einem Thema zu registrieren. Wenn eine mit einem Thema Nachricht sie dann für jedes Abonnement erfolgt Handle/unabhängig voneinander bearbeiten.

Ein Abonnement für ein Thema ähnelt eine virtuelle Warteschlange, die Nachrichten empfängt, die dem Thema gesendet wurden. Sie können optional Filterregeln zu einem Thema auf einer Basis pro Abonnement registrieren Filter/eingeschränkt zu einem Thema durch welche Abonnements Thema empfangen werden.

Service Bus-Themen und Abonnements können Sie eine große Anzahl von Nachrichten über eine sehr große Anzahl von Benutzern und Applikationen zu skalieren.

## <a name="create-a-service-namespace"></a>Erstellen eines Service-Namespaces

Azure Service Bus Topics und Abonnements bei zunächst muss einen Dienstnamespace erstellen. Ein Namespace bietet ein Bereichseinheit Service Bus Ressourcen in der Anwendung.

So erstellen Sie einen namespace

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Sie die Anwendung für Service Bus

Stellen Sie sicher, dass Sie [Azure SDK für Java][] installiert haben, bevor Sie dieses Beispiel erstellen. Wenn Sie Eclipse verwenden, können Sie der [Azure-Toolkit für Eclipse][] installieren, Azure SDK für Java enthält. Sie können das Projekt dann **Microsoft Azure Bibliotheken für Java** hinzufügen:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Am Anfang der Datei Java importieren Folgendes hinzufügen:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Fügen Sie Azure Bibliotheken für Java Buildpfad hinzu und in die Projektassembly Bereitstellung.

## <a name="create-a-topic"></a>Erstellen eines Themas

Verwaltungsvorgänge für Service Bus Topics erfolgt über die **ServiceBusContract** -Klasse. Ein **ServiceBusContract** -Objekt mit einer entsprechenden Konfiguration, die SAS-Token mit Berechtigungen zur Verwaltung kapselt erstellt und die **ServiceBusContract** -Klasse ist der einzige Kommunikation mit Azure.

Die **ServiceBusService** -Klasse stellt Methoden zum Erstellen, auflisten und Löschen von Themen. Das folgende Beispiel zeigt, wie ein **ServiceBusService** -Objekt zum Erstellen ein Thema `TestTopic`, mit einem Namespace namens `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Gibt es Methoden für **TopicInfo** Eigenschaften des Themas festgelegt werden (Beispiel: Festlegen der Time-to-live (TTL) Standardwert zum Thema gesendeten Nachrichten angewendet werden). Im folgenden Beispiel wird veranschaulicht, wie ein Thema erstellen `TestTopic` mit einer maximalen Größe von 5 GB:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Beachten Sie, dass Sie die **ListTopics** -Methode für **ServiceBusContract** Objekte um zu überprüfen, ob eine Thema mit einem bestimmten Namen in einem Namespace Service bereits.

## <a name="create-subscriptions"></a>Abonnements erstellen

Abonnements für Themen werden auch mit der **ServiceBusService** -Klasse erstellt. Abonnements können werden benannt und einen optionalen Filter, der Nachrichten an das Abonnement virtuelle Warteschlange übergeben beschränkt.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit der Standardfilter (MatchAll)

**MatchAll** Filter ist der Filter, der verwendet wird, kein Filter festgelegt wird, wenn ein neues Abonnement erstellt wird. Bei der **MatchAll** Filter befinden alle veröffentlichte Nachrichten zu dem Thema virtuelle Warteschlange für das Abonnement. Im folgenden Beispiel wird ein Abonnement mit dem Namen "AllMessages" erstellt und den **MatchAll** Filter verwendet.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Abonnements mit Filtern erstellen

Sie können auch Filter erstellen, mit denen Sie Bereich die Nachrichten zu einem Thema innerhalb eines bestimmten Themas Abonnements anzeigen soll.

Die flexibelste Art von Abonnements unterstützt Filter ist [SqlFilter][], das wenigen SQL92 implementiert. SQL-Filter werden für die Eigenschaften der Nachrichten, die das Thema veröffentlicht werden. Weitere Informationen über Ausdrücke, die mit SQL-Filter überprüfen Sie [SqlFilter.SqlExpression][] Syntax.

Im folgenden Beispiel wird ein Abonnement mit dem Namen `HighMessages` mit einem [SqlFilter][] -Objekt, das nur Nachrichten ausgewählt, die eine benutzerdefinierte **MessageNumber** -Eigenschaft größer als 3:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Im folgenden Beispiel wird auch ein Abonnement mit dem Namen `LowMessages` mit einem [SqlFilter][] -Objekt, das nur Nachrichten ausgewählt, die eine **MessageNumber** -Eigenschaft kleiner oder gleich 3:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Wenn eine Nachricht nun an gesendet `TestTopic`, es immer erhalten Empfänger abonniert die `AllMessages` Abonnement und selektiv Empfänger abonniert die `HighMessages` und `LowMessages` Abonnements (je nach Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Nachrichten zu einem Thema

Um eine Nachricht zu einem Thema Service Bus erhält die Anwendung ein **ServiceBusContract** -Objekt. Der folgende Code veranschaulicht, wie eine Nachricht für die `TestTopic` Thema zuvor erstellte innerhalb der `HowToSample` Namespace:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Nachrichten Service Bus Topics sind Instanzen der Klasse [BrokeredMessage][] . [BrokeredMessage][] *Objekte weisen eine Reihe von Methoden (z. B. * *SetLabel* * und * *TimeToLive**), ein Wörterbuch mit benutzerdefinierten anwendungsspezifische Eigenschaften und eine beliebige Anwendung Daten. Eine Anwendung lassen sich den Hauptteil der Nachricht serialisierbares Objekt an den Konstruktor von [BrokeredMessage][]und die entsprechenden übergeben * *DataContractSerializer* * wird zum Serialisieren des Objekts verwendet werden. Sie können auch eine * *java.io.InputStream** kann.

Im folgenden Beispiel wird veranschaulicht, wie fünf Testnachrichten zum Senden der `TestTopic` **MessageSender** es Codeausschnitt aus.
Hinweis wie **MessageNumber** Eigenschaftswert der Nachricht auf die Iteration der Schleife variiert (Dies legt fest, welche Abonnements erhalten):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB in [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einem Thema jedoch gibt die Gesamtgröße der Nachrichten durch ein Thema ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung mit einem oberen Grenzwert von 5 GB definiert.

## <a name="how-to-receive-messages-from-a-subscription"></a>Wie ein Abonnement Nachrichten

Um Nachrichten aus einem Abonnement erhalten möchten, verwenden Sie ein **ServiceBusContract** -Objekt. Nachrichten können in zwei verschiedenen Modi arbeiten: **ReceiveAndDelete** und **PeekLock**.

Erhalten mit dem Modus **ReceiveAndDelete** ist ein Betrieb Einzelbild - also wenn Service Bus eine Leseoperation für eine Nachricht empfängt, es markiert die Nachricht als verbraucht und an die Anwendung zurückgegeben. **ReceiveAndDelete** ist das einfachste und am besten für Szenarios, in denen eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus die Nachricht als verbraucht markiert haben wird, wird dann die Anwendung neu gestartet und beginnt, Nachrichten erneut es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Im **PeekLock** Modus empfangen wird ein zweistufiger Vorgang unterstützt Clientanwendungen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nach die Anwendung Verarbeitung der Meldung beendet (oder zuverlässig zur späteren Verarbeitung speichert) abgeschlossen empfangenen Nachricht fordert **Löschen** die zweite Phase des Prozesses empfangen wird. Service Bus **Delete** -Aufruf sieht, wird die Nachricht als verbraucht und aus dem Thema entfernen.

Das folgende Beispiel veranschaulicht, wie Nachrichten empfangen und verarbeitet **PeekLock** Modus (nicht die Standardeinstellung). Das folgende Beispiel führt eine Schleife verarbeitet Nachrichten in "HighMessages" Abonnement und wird beendet, wenn keine weiteren Nachrichten (auch sie können festgelegt werden auf neue Nachrichten warten).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Ist eine empfängeranwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht können sie die Methode **UnlockMessage** in der empfangenen Nachricht (nicht **DeleteMessage** ) aufrufen. Dadurch Service Bus Nachricht innerhalb des Themas zu entsperren und es wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht innerhalb eines Themas gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Timeout für die Sperre abläuft (z. B. wenn die Anwendung stürzt ab), Service Bus wird die Nachricht automatisch entsperrt und Verfügbarmachen erneut empfangen werden.

Die Anwendung stürzt nach dem Verarbeiten der Nachricht, aber vor Anforderung **DeleteMessage** , wird dann die Meldung an die Anwendung zurückgeliefert werden beim Neustart. Wird oft genannt **Mindestens einmal verarbeiten**, d. h. jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler ihre Anwendung doppelte Nachrichtenübermittlung behandeln zusätzlichen Logik hinzufügen. Dies geschieht häufig mit der **GetMessageId** -Methode der Nachricht über Übermittlungsversuche konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Themen und Abonnements löschen

Die gängigste Themen und Abonnements löschen ist ein **ServiceBusContract** -Objekt verwenden. Ein Thema wird auch alle Abonnements löschen, die mit dem Thema registriert sind. Abonnements können auch einzeln gelöscht werden.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Nächste Schritte

Grundlagen Servicebuswarteschlangen bearbeitet haben, finden Sie weitere Informationen [Servicebuswarteschlangen, Themen und Abonnements][] .

  [Azure SDK für Java]: http://azure.microsoft.com/develop/java/
  [Azure-Toolkit für Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Servicebuswarteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
