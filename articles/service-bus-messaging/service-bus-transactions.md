<properties 
    pageTitle="Service Bus Transaktionen | Microsoft Azure" 
    description="Übersicht über Azure Service Bus atomaren Transaktionen und per" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Überblick über Service Bus-Transaktion

Dieser Artikel beschreibt die Transaktionsfunktionen von Azure Service Bus. Die Diskussion wird von [Atomaren Transaktionen mit Service Bus](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)dargestellt. Dieser Artikel ist beschränkt Überblick Transaktion und die Funktion *per* im Service Bus während atomaren Transaktionen Beispiel größere und komplexere im Bereich.

## <a name="transactions-in-service-bus"></a>Transaktionen in Servicebus

Eine [Transaktion](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) gruppiert mindestens zwei Vorgänge zu einem *Ausführungsbereich*. Naturgemäß müssen eine Transaktion alle Vorgänge für eine bestimmte Gruppe von Operationen gelingen oder Fehlschlagen gemeinsam. In dieser Hinsicht fungieren Transaktionen als eine Einheit *Unteilbarkeit*genannt wird. 

Service Bus Transaktionsnachricht Broker und Transaktionsintegrität für alle internen Vorgänge der Nachrichtenspeicher. Alle überträgt Nachrichten in Service Bus wie das Verschieben von Nachrichten in einer [Warteschlange](service-bus-dead-letter-queues.md) oder [die automatische Weiterleitung](service-bus-auto-forwarding.md) von Nachrichten zwischen Entitäten sind transaktional. So Service Bus eine Nachricht akzeptiert, es bereits gespeichert und mit einer laufenden Nummer gekennzeichnet. Danach alle Nachrichtenübermittlungen Service Bus sind aufeinander abgestimmte Vorgänge über Entitäten und führt nicht zu (erfolgreich Quelle und Ziel fehlschlägt) oder Duplizierung (nicht Quelle und Ziel erfolgreich) der Nachricht.

Service Bus unterstützt Gruppierung für Messaging-Einheit (Warteschlange, Thema Abonnement) im Rahmen einer Transaktion. Beispielsweise mehrere Nachrichten an eine Warteschlange im Transaktionsbereich senden und Nachrichten nur werden in die Warteschlange Protokoll Wenn die Transaktion erfolgreich abgeschlossen wurde.

## <a name="operations-within-a-transaction-scope"></a>Operationen innerhalb eines Transaktionsbereichs 

Innerhalb eines Transaktionsbereichs Operationen sind wie folgt:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: senden, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: abgeschlossen, CompleteAsync verlassen, AbandonAsync, für unzustellbare Nachrichten, DeadletterAsync, verzögern, DeferAsync, RenewLock, RenewLockAsync 

Empfangen von Operationen sind nicht enthalten, da davon ausgegangen wird, dass die Anwendung Nachrichten erhält [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) Modus, oder in einigen Empfangsschleife mit einem [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) Rückruf und nur dann öffnet einen Transaktionsbereich für die Verarbeitung der Nachricht.

Die Disposition der Nachricht (Abschluss Abandon unzustellbare, verzögern) tritt im Rahmen von abhängigen auf das Gesamtergebnis der Buchung.

## <a name="transfers-and-send-via"></a>Übertragung und "per"

Transaktionale Übergabe von Daten aus einer Warteschlange auf einem Prozessor und einer anderen Warteschlange unterstützt, um Service Bus *übertragen*. Bei einem Übertragungsvorgang Absender zuerst sendet eine Nachricht an eine Warteschlange"Transfer" und der Übertragungswarteschlange verschiebt die Nachricht sofort in angegebene Zielwarteschlange mit derselben stabile Übertragung-Implementierung, der die Funktion Automatische Weiterleitung verwendet. Die Nachricht ist nie verpflichtet die Schlange Protokoll so, dass der Übertragungswarteschlange Verbraucher sichtbar.

Macht dies transaktionale wird deutlich, wenn die Schlange sich die Quelle für eingehende Nachrichten des Absenders ist. Also Service Bus können übermittelt die Nachricht an die Zielwarteschlange "via" die Schlange beim vollständigen (oder verzögern, oder unzustellbare) auf die Eingabenachricht in einem atomaren Vorgang. 

### <a name="see-it-in-code"></a>Siehe Code

Um die Übertragung einzurichten, erstellen Sie Nachrichtensender, die Zielwarteschlange über die Schlange abzielt. Sie müssen auch einen Empfänger, der Nachrichten aus dieser dieselbe Warteschlange abruft. Zum Beispiel:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Eine einfache Transaktion dann verwendet diese Elemente wie im folgenden Beispiel:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Nächste Schritte

Finden Sie in folgenden Artikeln Informationen Servicebuswarteschlangen:

- [Verkettung Service Bus Entitäten mit automatische Weiterleitung](service-bus-auto-forwarding.md)
- [Automatische Weiterleitung-Beispiel](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Atomare Transaktionen mit Service Bus](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure Warteschlangen und Service Bus Warteschlangen im Vergleich](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Wie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)