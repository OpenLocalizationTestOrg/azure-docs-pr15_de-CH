<properties 
    pageTitle="Servicebuswarteschlangen, Themen und Abonnements | Microsoft Azure"
    description="Überblick über Service Bus messaging Entitäten."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="service-bus-queues-topics-and-subscriptions"></a>Servicebuswarteschlangen, Themen und Abonnements

Microsoft Azure Service Bus unterstützt eine Reihe von Cloud-basierten Message oriented Middleware Technologien einschließlich reliable Message queuing und dauerhaften Veröffentlichen/Abonnieren messaging. Diese vermittelten Messagingfunktionen sozusagen entkoppelten Unterstützung Veröffentlichen-Abonnieren Messagingfunktionen zeitliche Entkopplung und Lastenausgleich Szenarien mit Fabric messaging Service-Bus. Entkoppelte Kommunikation hat viele Vorteile. Beispielsweise können Clients und Server Bedarf verbinden und ihre Vorgänge asynchron ausführen.

Messaging-Entitäten, die den Kern der vermittelten Messagingfunktionen in Service Bus bilden sind Warteschlangen, Themen-Abonnements und Regeln-Aktionen.

## <a name="queues"></a>Warteschlangen

Warteschlangen bieten erste, Nachrichtenübermittlung an einen oder mehrere konkurrierende Consumer erste Out (FIFO). D. h. Nachrichten normalerweise sollen empfangen und vom Empfänger in der Reihenfolge, in der sie der Warteschlange hinzugefügt wurden und jede Nachricht empfangen und verarbeitet nur eine Nachricht Verbraucher, verarbeitet werden. Der Hauptvorteil von Warteschlangen ist "zeitliche Entkopplung" der Anwendungskomponenten. Also müssen Erzeugern (Absender) und Verbrauchern (Empfänger) keinen senden und Empfangen von Nachrichten gleichzeitig, da Nachrichten dauerhaft in der Warteschlange gespeichert werden. Darüber hinaus muss der Hersteller keine Antwort vom Consumer warten, um weiterhin verarbeiten und Nachrichten.

Verwandter Vorteil ist "load abgleichen" ermöglicht es Erzeugern und Verbrauchern zum Senden und Empfangen von Nachrichten unterschiedlich. Häufig variiert die Systemlast Zeitverlauf; die Bearbeitungszeit für jede Arbeitseinheit ist jedoch normalerweise konstant. Vermittlung Nachricht Erzeugern und Verbrauchern mit einer Warteschlange bedeutet, dass nur die verarbeitende Anwendung bereitgestellt werden, um durchschnittliche Last statt Belastung. Die Tiefe der Warteschlange wächst und Verträge, wie eingehende laden. Dies spart direkt hinsichtlich der Infrastruktur erforderlichen Anwendung geladen. Mit steigender Last können mehrere Arbeitsprozesse zum Lesen aus der Warteschlange hinzugefügt werden. Jede Nachricht wird nur von einem der Arbeitsprozesse verarbeitet. Darüber hinaus ermöglicht dieses Pull-basierten Lastenausgleich für eine optimale Nutzung der Arbeitskraft Computer selbst wenn die Arbeitskraft Computer hinsichtlich Leistung, wie sie Nachrichten mit eigenen maximale Rate ziehen. Dieses Muster ist oft das Muster "Wettbewerb Consumer" bezeichnet.

Warteschlangen, Message Erzeugern und Verbrauchern zwischen bietet einen inhärenten Kopplung zwischen den Komponenten. Da der Erzeuger und der Verbraucher nicht gegenseitig kennen, kann Consumer ohne Auswirkung auf die Erzeuger aktualisiert werden.

Erstellen einer Warteschlange ist ein Prozess mit mehreren Schritten. Sie Operationen Management Service Bus messaging Entitäten (Warteschlangen und Themen) über die [Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse, die mit der Basisadresse des Service Bus-Namespace und die Anmeldeinformationen des Benutzers erstellt wird. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) bietet Methoden zum Erstellen, auflisten und Löschen von Messaging-Entitäten. Die [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) -Methode können Sie nach dem Erstellen einer [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) aus dem Namen SAS und Schlüssel und ein Dienstobjekt Namespace Management, die Warteschlange erstellen. Zum Beispiel:

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Sie können dann ein Warteschlangenobjekt und messaging Factory Service Bus URI als Argument erstellen. Zum Beispiel:

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Sie können dann Nachrichten an die Warteschlange senden. Beispielsweise haben Sie eine Liste der brokernachrichten aufgerufen `MessageList`, wird der Code ähnlich dem folgenden angezeigt:

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Sie können dann wie folgt Nachrichten aus der Warteschlange empfangen:

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Der Empfangsvorgang wird im Modus [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) Einzelbild; also wenn Service Bus die Anforderung empfängt, es markiert die Nachricht als verbraucht wird und an die Anwendung zurückgegeben. [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) ist das einfachste und am besten für Szenarios, in denen die Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus die Nachricht markiert als verbraucht, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, wird es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Im [PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) -Modus wird der Receive-Methode zwei Stufen ermöglicht die Unterstützung von Clientanwendungen, die fehlende Nachrichten tolerieren. Wenn Service Bus die Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nachdem die Anwendung beendet Verarbeitung der Nachricht (oder zuverlässig zur späteren Verarbeitung speichert), abgeschlossen die zweite Phase des Prozesses empfangen fordert die empfangene Nachricht [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ist. Beim Service Bus rufen Sie [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) sieht, markiert die Nachricht als verbraucht.

Ist die Anwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht können sie die [Abandon](https://msdn.microsoft.com/library/azure/hh181837.aspx) -Methode in der empfangenen Nachricht (statt [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)) aufrufen. Service Bus Nachricht entsperren und es wieder durch den gleichen Verbraucher oder anderen konkurrierenden Verbraucher empfangen werden können. Zweitens ein Timeout der Sperre zugeordnet ist und wenn die Anwendung zum Verarbeiten der Nachricht die Sperre Timeout abläuft, bevor (z. B. wenn die Anwendung stürzt ab) und Service Bus Nachricht entsperrt und erneut empfangen werden verfügbar gemacht (im Wesentlichen eines Vorgangs [aufgeben](https://msdn.microsoft.com/library/azure/hh181837.aspx) standardmäßig).

Beachten Sie, dass, stürzt die Anwendung nach dem Verarbeiten der Nachricht, aber vor Anforderung [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ist, die Nachricht an die Anwendung erneut beim Neustart zugestellt wird. Dies wird häufig *Mindestens *Verarbeitung aufgerufen. Jede Nachricht wird also mindestens einmal verarbeitet. Jedoch kann in bestimmten Situationen die gleiche Nachricht zugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren zusätzlicher Logik erforderlich ist, in der Anwendung erkennen Duplikate erreicht werden **MessageId** -Eigenschaft der Nachricht, auf die über Übermittlungsversuche konstant bleibt. Dies wird als *Genau einmal* verarbeitet.

Weitere Informationen und ein funktionsfähiges Beispiel für das Erstellen und Senden von Nachrichten an und von Warteschlangen finden Sie unter [Service Bus messaging .NET Lernprogramm vermittelt](service-bus-brokered-tutorial-dotnet.md).

## <a name="topics-and-subscriptions"></a>Themen und Abonnements

*Themen* und *Abonnements* bieten im Gegensatz zu Warteschlangen, in denen jede Nachricht einen Verbraucher verarbeitet wird eine 1: n-Form der Kommunikation im *Veröffentlichen-Abonnieren* -Musters. Nützlich für die Skalierung für eine große Anzahl von Empfängern wird jede veröffentlichte Nachricht für jedes Abonnement registriert das Thema verfügbar. Nachrichten werden gesendet, um ein Thema und an mindestens zugeordneten Abonnements je nach Regeln, die auf pro festgelegt werden können. Zusätzliche Filter können die Abonnements die Nachrichten beschränken, die sie erhalten möchten. Nachrichten zu einem Thema auf die gleiche Weise werden sie an eine Warteschlange gesendet, aber Nachrichten werden nicht direkt vom Thema empfangen. Stattdessen erhalten sie Abonnements. Ein Thema Abonnement ähnelt eine virtuelle Warteschlange, die Nachrichten erhält, die das Thema an. Aus einem Abonnement sind identisch wie Nachrichten, die sie aus einer Warteschlange empfangen werden.

Zum Vergleich Funktionen senden Nachricht einer Warteschlange Karten direkt zu einem Thema und seine Funktionalität Nachricht empfangen ordnet ein Abonnement. Unter anderem also Abonnements in diesem Abschnitt für Warteschlangen beschriebenen dieselben Muster unterstützen: konkurrierende Consumer, zeitliche Entkopplung laden Abgleich und Lastenausgleich.

Erstellen eines Themas ähnelt dem Erstellen einer Warteschlange wie im Beispiel im vorherigen Abschnitt. Erstellen Sie Dienst-URI und verwenden Sie dann den Namespace Client [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse. Anschließend können Sie ein Thema mithilfe der [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) -Methode erstellen. Zum Beispiel:

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Fügen Sie Abonnements nach Bedarf:

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Sie können dann einen Thema Client erstellen. Zum Beispiel:

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Absender der Nachricht können Sie senden und empfangen Nachrichten dem Thema, wie im vorherigen Abschnitt gezeigt. Zum Beispiel:

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Ähnlich wie Warteschlangen erhalten Nachrichten aus einem Abonnement mit einem [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) -Objekt anstelle eines [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) -Objekts. Erstellen des Abonnement-Clients den Namen des Themas, den Namen des Abonnements und (optional) Empfangsmodus als Parameter übergeben. Beispielsweise mit dem **Lager** -Abonnement:

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Regeln und Aktionen

In vielen Szenarios müssen Nachrichten, die bestimmte Merkmale unterschiedlich verarbeitet werden. Dazu können Sie Abonnements Nachrichten finden, die Eigenschaften gewünscht führen Sie bestimmte Änderungen an den Eigenschaften konfigurieren. Service Bus-Abonnements alle Nachrichten zum Thema sehen, können Sie nur eine Teilmenge dieser Nachrichten Warteschlange virtuellen Abonnement kopieren. Dies geschieht mithilfe der Abonnementfilter. Diese Modifikationen werden *Filteraktionen*bezeichnet. Wenn ein Abonnement erstellt wird, können Sie einen Filterausdruck angeben, der auf die Eigenschaften der Nachricht die Systemeigenschaften (z. B. **Label**) und die benutzerdefinierte Anwendungseigenschaften (z. B. **StoreName**.) In diesem Fall ist der SQL-Filterausdruck optional. ohne einen SQL-Filterausdruck wird Filteraktion für ein Abonnement definiert alle Nachrichten für dieses Abonnement ausgeführt werden.

Im vorherigen Beispiel, zum Filtern von Nachrichten aus **1**würden Sie das Dashboard Abonnement wie folgt erstellen:

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Mit diesem Abonnement an, nur Nachrichten, die die `StoreName` -Eigenschaft festgelegt `Store1` kopiert die virtuelle Warteschlange für die `Dashboard` Abonnement.

Weitere Informationen zu möglichen Filterwerte finden Sie in der Dokumentation für die [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) und [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) . Siehe auch die [Messaging vermittelte: erweiterter Filter](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) und [Thema filtern](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) .

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter folgenden Themen Weitere Informationen und Beispiele zur Verwendung von messaging Service Bus vermittelte Entitäten erweitert.

- [Übersicht über Service Bus](service-bus-messaging-overview.md)
- [Service Bus messaging .NET Lernprogramm vermittelt](service-bus-brokered-tutorial-dotnet.md)
- [Service Bus vermittelte messaging REST-Lernprogramm](service-bus-brokered-tutorial-rest.md)
- [Thema Filter-Beispiel](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters)
- [Vermittelte Messaging: Erweiterte Filter-Beispiel](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

