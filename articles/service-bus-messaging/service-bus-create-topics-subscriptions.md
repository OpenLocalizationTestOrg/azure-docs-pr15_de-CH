<properties 
    pageTitle="Erstellen, Service Bus Topics und Abonnements verwenden | Microsoft Azure"
    description="Einführung in das Veröffentlichen-Abonnieren-Funktionen von Service Bus-Themen und Abonnements."
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Erstellen Sie, Service Bus Topics und Abonnements verwenden

Azure Service Bus unterstützt eine Reihe von Cloud-basierten, nachrichtenbasierte Middleware Technologie reliable Message queuing und dauerhaften Veröffentlichen/Abonnieren messaging. Dieser Artikel baut auf die Angaben [Erstellen Applikationen, die Servicebuswarteschlangen verwenden,](service-bus-create-queues.md) und bietet eine Einführung in das Veröffentlichen/Abonnieren-Funktionen Service Bus Topics.

## <a name="evolving-retail-scenario"></a>Szenario: Einzelhandel entwickelt

In diesem Artikel weiterhin das Szenario: Einzelhandel eingesetzt [erstellen, die Servicebuswarteschlangen verwenden](service-bus-create-queues.md). Denken Sie daran, dass Daten aus einzelnen Punkt Verkaufsort (POS) Terminals ein Lagerverwaltungssystem müssen die Daten verwendet weitergeleitet werden, um zu bestimmen, wann der Bestand aufgefüllt werden muss. Jedes POS-Terminal meldet seine Daten durch Senden von Nachrichten an die Warteschlange **DataCollectionQueue** , bleibt sie bis sie Inventory Management System sind wie folgt:

![Servicebus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Um dieses Szenario zu entwickeln, wurde eine neue Anforderung zum System hinzugefügt: Shop-Besitzer können Leistung Speicher in Echtzeit überwachen möchte.

Um dieser Anforderung zu entsprechen, muss das System "aus den Daten tippen". Wir möchten weiterhin jede Nachricht von den POS-Terminals an das Lagerverwaltungssystem als vor, aber wir wollen eine weitere Kopie jeder Nachricht, mit dem wir die Übersichtsansicht Shop-Besitzer angezeigt.

In jeder Situation, in der jede Nachricht von mehreren Parteien verwendet werden muss, können Sie Service Bus- *Themen*verwenden. Themen ein Veröffentlichen-Abonnieren-Muster mit jeder veröffentlichten Nachricht für Abonnements registriert das Thema verfügbar ist. Im Gegensatz dazu wird jede Nachricht mit Warteschlangen ein Verbraucher empfangen.

Nachrichten werden auf die gleiche Weise zu einem Thema gesendet, sie an eine Warteschlange gesendet werden. Allerdings werden Nachrichten nicht aus dem Thema direkt empfangen; Sie erhalten von Abonnements. Sie können ein Abonnement für ein Thema als virtuelle Warteschlange vorstellen, die Nachrichten empfängt, die in diesem Thema an. Nachrichten aus einem Abonnement genauso erhalten sie aus einer Warteschlange empfangen werden.

Zurück zu dem Szenario: Einzelhandel, die Warteschlange erhält ein Thema und ein Abonnement wird hinzugefügt, können Sie die Komponente Inventory Management System. Das System sieht nun folgendermaßen aus:

![Servicebus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Die Konfiguration verhält wie die vorherige Warteschlange Entwurf. Also werden Nachrichten zum Thema **Lager** Abonnement weitergeleitet aus denen Sie das **Lagerverwaltungssystem** verwendet.

Zur Unterstützung der Managementkonsole erstellen ein zweites Abonnement zum Thema, wie hier gezeigt:

![Servicebus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Bei dieser Konfiguration wird jede Nachricht von den POS-Terminals **Dashboards** und **Lager** Abonnements zur Verfügung gestellt.

## <a name="show-me-the-code"></a>Code anzeigen

Artikel [Erstellen Applikationen, die Servicebuswarteschlangen verwenden,](service-bus-create-queues.md) beschreibt ein Azure-Konto und erstellen Sie einen Service-Namespace. Um einen Service Bus-Namespace zu verwenden, muss eine Anwendung Service Bus Assembly ausdrücklich Microsoft.ServiceBus.dll verweisen. Auf Service Bus Abhängigkeiten am einfachsten Service Bus [Nuget-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus/)installieren. Sie finden die Assembly als Teil des Azure SDK. Der Download steht unter [Azure SDK-Downloadseite](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Thema und Abonnements erstellen

Verwaltungsvorgänge für Service Bus messaging Entitäten (Warteschlangen und Veröffentlichen/Abonnieren-Themen) erfolgt über die [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse. Entsprechende Anmeldeinformationen werden benötigt, um einen [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Instanz für einen bestimmten Namespace erstellen. Service Bus verwendet eine Grundlage Sicherheitsmodell [Shared Access Signatur (SAS)](service-bus-sas-overview.md) . Die [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) -Klasse stellt ein Sicherheitstokenanbieter mit integrierten Factorymethoden einige bekannte Tokenanbieter zurückgeben. Wir verwenden eine [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) Methode der SAS-Anmeldeinformationen. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Instanz wird dann mit der Basisadresse des Service Bus-Namespace und der Tokenanbieter erstellt.

[NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse stellt Methoden zum Erstellen, auflisten und Löschen von Messaging-Entitäten. Die hier gezeigte Code zeigt, wie [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Instanz erstellt und zum Thema **DataCollectionTopic** erstellen.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Beachten Sie, dass Überladung der Methode [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) , mit denen Eigenschaften des Themas festlegen. Sie können z. B. den Standard Time to live (TTL) für Nachrichten zum Thema festlegen. Fügen Sie die **Lager-** und **Dashboard** -Abonnements an.

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Nachrichten zum Thema

Für Vorgänge zur Laufzeit Service Bus Entitäten; Beispielsweise muss Nachrichten senden und empfangen, eine Anwendung zuerst ein [Fall](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) -Objekt erstellen. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse ähnlich, wird die Instanz [Fall](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) von der Basisadresse Service-Namespace und der Tokenanbieter erstellt.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Nachrichten an und von Service Bus-Themen erhalten Instanzen der Klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Diese Klasse besteht aus einer Reihe von Standardeigenschaften (wie [Beschriftung](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) und [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) ein Wörterbuch Anwendungseigenschaften, und eine beliebige Daten enthält. Eine Anwendung kann [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) serialisiert das Objekt mit Text übergeben jedes serialisierbare Objekt (im folgende Beispiel wird ein **SalesData** -Objekt, das Daten vom POS-Terminal darstellt) festlegen. Alternativ kann ein [Stream](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -Objekt bereitgestellt werden.

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

Die einfachste Möglichkeit, Nachrichten zum Thema ist mit [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) ein [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) Objekt direkt von der Instanz der [Fall](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten über ein Abonnement

Ähnlich wie Warteschlangen zum Empfangen von Nachrichten aus einem Abonnement Verwendung [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) Objekt direkt aus der [Fall](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) mit [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)erzeugen. Verwenden Sie eine der zwei verschiedenen Modi,**ReceiveAndDelete** und **PeekLock**angezeigt, wie in [Erstellen von Clientanwendungen, die Servicebuswarteschlangen verwenden](service-bus-create-queues.md).

Beachten Sie, dass beim Erstellen einer [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) für Abonnements der *EntityPath* -Parameter des Formulars `topicPath/subscriptions/subscriptionName`. Daher erstellen Sie eine [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) für das Abonnement **Inventory** **DataCollectionTopic** Thema *EntityPath* muss festgelegt werden `DataCollectionTopic/subscriptions/Inventory`. Der Code folgt wie:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Subskriptions-filtern

In diesem Szenario werden alle Nachrichten im Thema, alle registrierten Abonnements zur Verfügung gestellt. Das Schlüsselwort "zur Verfügung." Service Bus-Abonnements alle Nachrichten zum Thema sehen, können Sie nur eine Teilmenge dieser Nachrichten an die Warteschlange virtuellen Abonnement kopieren. Abonnement *Filter*erfolgt. Beim Erstellen eines Abonnements können Sie einen Filterausdruck in Form eines Prädikats SQL92 Stil angeben, die die Eigenschaften der Nachricht Systemeigenschaften (z. B. [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) und die Anwendungseigenschaften, wie im vorherigen Beispiel **StoreName** arbeitet.

Das Szenario zur Veranschaulichung entwickelt, ist ein zweiter Retail Szenario hinzugefügt werden. Daten aus allen POS-Terminals aus beiden Datenspeichern noch an das zentrale Management-System weitergeleitet werden, jedoch einen Geschäftsführer mit dem Dashboard-Tool nur die Leistung dieses Speichers interessiert. Abonnement dazu filtern können. Beachten Sie, dass beim POS-Terminals Nachrichten veröffentlichen sie die **StoreName** Anwendung auf die Nachricht. Erhalten zwei Speicher, beispielsweise **Redmond** und **Seattle**speichern POS-Terminals in Redmond Stempel **Seattle**ihre Verkaufsdaten Nachrichten mit einem **StoreName** gleich **Redmond**Seattle Speicher POS-Terminals **StoreName** verwenden gleich. Speicher möchte des Informationsspeichers Redmond nur Daten von den POS-Terminals. Das System wird folgendermaßen angezeigt:

![Service-Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Zum Einrichten dieser Arbeitsplan erstellen Sie das **Dashboard** -Abonnement:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Mit diesem Abonnementfilter kopiert nur Nachrichten mit **StoreName** Eigenschaftensatz **beschloss** , die virtuelle Warteschlange für das Abonnement **Dashboard** . Es gibt viel mehr Filterung Abonnement jedoch. Clientanwendungen können mehrere Filterregeln pro Abonnement die Möglichkeit zum Ändern der Eigenschaften einer Nachricht als ein Abonnement virtuelle Warteschlange übergeben.

## <a name="summary"></a>Zusammenfassung

Alle Gründe mit queuing beschrieben [erstellen, die Servicebuswarteschlangen verwenden,](service-bus-create-queues.md) gelten auch für Themen, insbesondere:

- Zeitliche Entkopplung – Nachricht Erzeugern und Verbrauchern müssen nicht gleichzeitig online sein.

- Laden Abgleich-Peaks laden das Thema verwendeten Anwendung für durchschnittliche Auslastung statt Spitzenlast bereitgestellt werden geglättet.

- Lastenausgleich – ähnlich einer Warteschlange können Sie mehreren konkurrierende Consumern überwacht ein einzelnes Abonnement mit jeder Nachricht an nur einen Verbraucher und Last übergeben.

- Kopplung – entwickeln messaging-Netzwerk ohne vorhandene Endpunkte; z. B. Abonnements hinzufügen oder Ändern von Filtern zu einem Thema zu neuer Verbraucher.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie Informationen zur Verwendung von Warteschlangen im Szenario Retail POS [erstellen, die Servicebuswarteschlangen verwenden](service-bus-create-queues.md) .