<properties 
    pageTitle="AMQP 1.0-Unterstützung für Service Bus partitioniert Warteschlangen und Themen | Microsoft Azure" 
    description="Informationen Sie über erweiterte Message Queuing Protocol (AMQP) 1.0 mit Service Bus partitioniert Warteschlangen und Themen." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>AMQP 1.0-Unterstützung für partitionierte Servicebuswarteschlangen und Themen 

Azure Service Bus unterstützt jetzt erweiterte Message Queuing Protocol (**AMQP**) 1.0 Service Bus **Themen und Warteschlangen partitioniert.**

**AMQP** ist ein offenen Standards Message queuing, die für die plattformübergreifende Anwendungsentwicklung mit verschiedenen Programmiersprachen ermöglicht. Allgemeine Informationen zum AMQP in Service Bus unterstützen Sie, finden Sie [AMQP 1.0 Service Bus unterstützen](service-bus-amqp-overview.md).

**Partitionierte Warteschlangen und Themen**auch *partitionierte Entitäten*bieten höhere Verfügbarkeit, Zuverlässigkeit und Durchsatz als herkömmliche nicht partitioniert Warteschlangen und Themen. Weitere Informationen über partitionierte Entitäten anzeigen Sie [Partitionierte Messaging Entitäten](service-bus-partitioning.md)

AMQP 1.0 als Protokoll für die Kommunikation mit partitionierten Warteschlangen und Themen können interoperable Clientanwendungen erstellen, die höhere Verfügbarkeit, Zuverlässigkeit und nutzen und im gesamten angebotenen Service Bus partitioniert Entitäten.

Eine detaillierte auf niedriger Ebene AMQP 1.0 Protokoll Guide erläutert das Service Bus implementiert und basiert auf der OASIS AMQP technische Spezifikation, finden Sie unter [AMQP 1.0 in Azure Service Bus und Ereignis Hubs Protokoll Guide](service-bus-amqp-protocol-guide.md).    

## <a name="use-amqp-with-partitioned-queues"></a>AMQP mit partitionierten Warteschlangen

Warteschlangen sind nützlich für Szenarien, die zeitliche Entkopplung laden Abgleich, Lastenausgleich und lose Kopplung erfordern. Eine Warteschlange Herausgeber Nachrichten an die Warteschlange gesendet und Consumer Nachrichten aus der Warteschlange in Situationen, in denen eine Nachricht nur einmal empfangen werden kann. Ein gutes Beispiel hierfür ist ein Lagersystem in der POS-Terminals Daten an eine Warteschlange nicht direkt an das Lagerverwaltungssystem veröffentlichen. Das Lagerverwaltungssystem verwendet dann die Daten jederzeit Teile verwalten. Dies hat mehrere Vorteile, einschließlich das Lagerverwaltungssystem nicht online sein müssen. Weitere Informationen zu Servicebuswarteschlangen finden Sie unter [Erstellen Applications, die Servicebuswarteschlangen verwenden](service-bus-create-queues.md). 

Eine partitionierte Warteschlange weiter wächst Verfügbarkeit, Zuverlässigkeit und Durchsatz für Applikationen diese Warteschlangen Nachrichtenmakler und messaging Speicher partitioniert werden.     

### <a name="create-partitioned-queues"></a>Erstellen von partitionierten Warteschlangen

Sie können eine partitionierte Warteschlange [Azure-Verwaltungsportal][] und Service Bus SDK erstellen. Eine partitionierte Warteschlange erstellen, legen Sie die [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) -Eigenschaft auf **true** in der [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) -Instanz. Im folgenden Codebeispiel wird das Erstellen eine partitionierte Warteschlange Service Bus SDK. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Senden und Empfangen von Nachrichten AMQP

Sie können Nachrichten senden und Empfangen einer partitionierten Warteschlange AMQP als Protokoll, indem [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) -Eigenschaft auf [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) , wie im folgenden Code gezeigt.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>AMQP mit partitionierten Themen

Themen sind vom Konzept her ähnlich wie Warteschlangen Themen können eine Kopie der gleichen Nachricht an mehrere *Abonnements*weiterleiten In einem Thema Herausgeber Nachrichten zum Thema und Verbraucher erhalten Nachrichten von Subskriptionen. Im Szenario POS-System Lager veröffentlichen Terminals Daten zum Thema. Das Lagerverwaltungssystem erhält Nachrichten von einem Abonnement. Darüber hinaus kann ein Überwachungssystem dieselbe Nachricht ein anderes Abonnement erhalten. Weitere Informationen zu Service Bus-Themen und Abonnements finden Sie unter [Erstellen Applications, die Service Bus-Themen und Abonnements verwenden](service-bus-create-topics-subscriptions.md). 

Mit Warteschlangen erhöhen partitionierte Themen finden Sie weitere Verfügbarkeit, Zuverlässigkeit und Durchsatz für Clientanwendungen wie diese Themen und ihre Abonnements Nachrichtenmakler und messaging Speicher partitioniert werden. 

### <a name="create-partitioned-topics"></a>Erstellen partitionierte Themen

Sie können einem partitionierten Thema [Azure-Verwaltungsportal][] und Service Bus SDK erstellen. Erstellen partitionierter Thema legen Sie die [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) -Eigenschaft auf **true** in der [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) -Instanz. Im folgenden Codebeispiel wird das Erstellen einer partitionierten Thema Service Bus SDK.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Senden und Empfangen von Nachrichten AMQP

Sie können senden und Empfangen von Nachrichten aus einer partitionierten Thema mit AMQP als Protokoll [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) -Eigenschaft auf [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx)einstellen, wie im folgenden Code gezeigt.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Nächste Schritte

Finden Sie die folgende zusätzliche Informationen partitionierte messaging Entitäten sowie AMQP erfahren.

*    [Partitionierte messaging Entitäten](service-bus-partitioning.md)
*    [OASIS Advanced Message Queuing Protocol (AMQP), Version 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [AMQP 1.0-Unterstützung in Service Bus](service-bus-amqp-overview.md)
*    [AMQP 1.0 in Azure Service Bus und Ereignis Hubs Protokoll-Handbuch](service-bus-amqp-protocol-guide.md)
*    [Verwendung von Java Message Service (JMS) API mit Service Bus und AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Verwendung von AMQP 1.0 mit Service Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)

[Azure-Verwaltungsportal]: http://manage.windowsazure.com
