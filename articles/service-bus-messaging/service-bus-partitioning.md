<properties 
    pageTitle="Partitionierte Warteschlangen und Themen | Microsoft Azure"
    description="Beschreibt die Servicebuswarteschlangen und Themen mit mehreren Nachrichtenmakler partitionieren."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Partitionierte Warteschlangen und Themen

Azure Service Bus verwendet mehrere Nachrichtenmakler Nachrichten verarbeiten und mehrere messaging speichert Nachrichten gespeichert. Eine konventionelle Warteschlange oder ein Thema wird von einer einzelnen nachrichtenbroker und in einem Nachrichtenspeicher gespeichert. Service Bus können auch Warteschlangen oder Themen Nachrichtenmakler und messaging Speicher partitioniert werden. Dies bedeutet, dass der Gesamtdurchsatz einer partitionierten Warteschlange oder das Thema nicht mehr durch die Leistung einer einzelnen nachrichtenbroker oder Nachrichtenspeicher begrenzt ist. Darüber hinaus wird ein zeitweilige Ausfall eines messaging keine partitionierte Warteschlange oder ein Thema nicht verfügbar gerendert. Partitionierte Warteschlangen und Themen können alle erweiterten Service Bus-Features wie die Unterstützung für Transaktionen und enthalten.

Weitere Informationen zu Service Bus Internals finden Sie unter [Service Bus-Architektur][] .

## <a name="how-it-works"></a>Funktionsweise

Jede partitionierte Warteschlange oder ein Thema besteht aus mehreren Fragmenten. Fragment ist in verschiedenen Nachrichtenspeicher gespeichert und von anderen Nachrichtenmakler. Wenn eine an eine partitionierte Warteschlange oder ein Thema Nachricht zugewiesen Service Bus Nachricht eines der Fragmente. Die Auswahl erfolgt nach dem Zufallsprinzip Service Bus oder Partitionsschlüssel Absender angeben kann.

Wenn ein Client einer Nachricht aus einer partitionierten Warteschlange oder ein Abonnement partitionierte Thema Service Bus Abfragen alle Fragmente für Nachrichten erhalten möchte, gibt die erste Nachricht an den Empfänger aus der Messaging-Speicher abgerufen wird. Service Bus-Caches die Nachrichten und gibt sie bei Erhalt weitere Anfragen. Empfangende Client ist nicht über die Partitionierung. Kunden Verhalten einer partitionierten Warteschlange oder ein Thema (z. B. lesen, ausführen, verzögern, für unzustellbare Nachrichten, Prefetch) entspricht dem Verhalten einer regulären Entität.

Es ist ohne zusätzliche Kosten eine Nachricht zu senden oder Empfangen einer Nachricht aus einer partitionierten Warteschlange oder ein Thema ein.

## <a name="enable-partitioning"></a>Teilung aktivieren

Verwendung von partitionierten Warteschlangen und Themen mit Azure Service Bus Azure SDK, Version 2.2 oder höher, oder geben `api-version=2013-10` in der HTTP-Anfragen.

Sie können Servicebuswarteschlangen und Themen 1, 2, 3, 4 oder 5 GB Größe erstellen (die Standardeinstellung ist 1 GB). Partitionierung aktiviert, erstellt Service Bus 16 Partitionen für jedes GB angeben. Wenn Sie eine Warteschlange, die 5 GB Größe erstellen, mit 16 Partitionen die maximale Warteschlangengröße wird (5 \* 16) = 80 GB. Die maximale Größe der partitionierten Warteschlange oder Thema sehen von [Azure-Portal][]auf der auf.

Es gibt mehrere Methoden zum Erstellen einer partitionierten Warteschlange oder ein Thema. Beim Erstellen der Warteschlange oder ein Thema aus der Anwendung können Sie Partitionierung für die Warteschlange oder das Thema bzw. [QueueDescription.EnablePartitioning][] oder [TopicDescription.EnablePartitioning][] -Eigenschaft auf **true**festlegen. Diese Eigenschaften müssen zum Zeitpunkt die Warteschlange oder Thema erstellt. Es ist nicht möglich, diese Eigenschaften auf eine vorhandene Warteschlange oder ein Thema ändern. Zum Beispiel:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternativ können Sie eine partitionierte Warteschlange oder ein Thema in Visual Studio oder in [Azure-Portal][]erstellen. Beim Erstellen einer neuen Warteschlange oder ein Thema im Portal die Option die **Partitionierung aktivieren** im Falz **General Settings** die Warteschlange oder das Thema **Einstellungsfenster auf **true**** . Klicken Sie auf das Kontrollkästchen **Partitionierung aktivieren** im Dialogfeld **Neue Warteschlange** oder **Neues Thema** in Visual Studio.

## <a name="use-of-partition-keys"></a>Partition-Schlüssel

Wenn eine Nachricht in die Warteschlange eingereiht in eine partitionierte Warteschlange oder ein Thema, prüft, ob der Partitionsschlüssel Service Bus. Wenn gefunden, wählt das Fragment basierend auf diesem Schlüssel. Wenn Partitionsschlüssel nicht findet, wird das Fragment basierend auf einem internen Algorithmus ausgewählt.

### <a name="using-a-partition-key"></a>Partitionsschlüssel verwenden

Einige Szenarien Sessions oder Transaktionen erfordern Nachrichten in einem bestimmten Fragment gespeichert werden. Alle Szenarien erfordern die Verwendung eines partitionsschlüssels. Gleichen Fragment werden alle Nachrichten mit der gleichen Partition Taste zugewiesen. Wenn das Fragment vorübergehend nicht verfügbar ist, zurückgegeben Service Bus ein Fehler.

Je nach Szenario werden andere Eigenschaften als Partitionsschlüssel verwendet:

**SessionId**: Wenn eine Nachricht hat die [BrokeredMessage.SessionId][] -Eigenschaft und Service Bus diese Eigenschaft als Partitionsschlüssel verwendet. Auf diese Weise werden alle Nachrichten, die zu derselben Sitzung gehören von der gleichen Nachrichtenmakler behandelt. Service Bus Bestellung sowie die Konsistenz des Sitzungsstatus Nachricht garantieren können.

**PartitionKey**: Wenn eine Nachricht hat die [BrokeredMessage.PartitionKey][] -Eigenschaft jedoch nicht die [BrokeredMessage.SessionId][] -Eigenschaft und Service Bus [PartitionKey][] Eigenschaft als Partitionsschlüssel verwendet. Wenn die Meldung [SessionId][] und die [PartitionKey][] festgelegt ist, müssen beide Eigenschaften identisch sein. Wenn die [PartitionKey][] -Eigenschaft auf einen anderen Wert als die [SessionId][] -Eigenschaft festgelegt ist, gibt Service Bus eine **InvalidOperationException** -Ausnahme. [PartitionKey][] -Eigenschaft sollte verwendet werden, ein Absender nicht Sitzung bewusst Transaktionsnachrichten gesendet. Der Partitionsschlüssel wird sichergestellt, dass alle innerhalb einer Transaktion gesendeten Nachrichten von derselben messaging Broker verarbeitet werden.

**MessageId**: die Warteschlange oder das Thema [QueueDescription.RequiresDuplicateDetection][] -Eigenschaft auf **true** festgelegt ist und die [BrokeredMessage.SessionId][] oder die [BrokeredMessage.PartitionKey][] -Eigenschaft nicht festgelegt, wenn die [BrokeredMessage.MessageId][] Eigenschaft dient als Partitionsschlüssel. (Beachten Sie, dass Microsoft .NET und AMQP-Bibliotheken eine Nachrichten-ID automatisch zugewiesen, wenn die sendende Anwendung nicht.) In diesem Fall werden alle Kopien der gleichen Nachricht vom gleichen Nachrichtenmakler behandelt. Dies ermöglicht Service Bus zu erkennen und zu beseitigen doppelte Nachrichten. Wenn die [QueueDescription.RequiresDuplicateDetection][] -Eigenschaft nicht auf **true**festgelegt ist, hält Service Bus [MessageId][] -Eigenschaft nicht als Partitionsschlüssel.

### <a name="not-using-a-partition-key"></a>Partitionsschlüssel verwenden nicht

In Ermangelung Partitionsschlüssel verteilt Service Bus Nachrichten eine Roundrobin auf alle Fragmente eines partitionierten Warteschlange oder Thema. Wenn das gewählte Fragment nicht verfügbar ist, weist Service Bus Nachricht zu einem anderen Fragment. Auf diese Weise ist der Sendevorgang trotz der vorübergehenden Ausfall eines messaging erfolgreich.

Service Bus geben muss ausreichend Zeit, wurde die Nachricht in einem anderen Fragment vom Client, der Senden der Nachricht angegebene [MessagingFactorySettings.OperationTimeout][] -Wert als 15 Sekunden liegen. Es wird empfohlen, dass Sie die [OperationTimeout][] -Eigenschaft auf den Standardwert von 60 Sekunden festlegen.

Beachten Sie, dass Partitionsschlüssel eine Nachricht zu einem bestimmten Fragment "fixiert". Wenn der Nachrichtenspeicher, die dieses Fragment enthält nicht verfügbar ist, zurückgegeben Service Bus ein Fehler. In Ermangelung Partitionsschlüssel können Service Bus verschiedene Fragment und der Vorgang erfolgreich ist. Daher wird empfohlen, dass Sie Partitionsschlüssel nicht angeben, wenn es erforderlich ist.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Erweiterte Themen: Verwenden von Transaktionen mit partitionierten

Als Teil einer Transaktion gesendeten Nachrichten müssen Partitionsschlüssel angeben. Dies kann eine der folgenden Eigenschaften: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]oder [BrokeredMessage.MessageId][]. Alle Nachrichten als Teil derselben Transaktion müssen denselben Partitionsschlüssel angeben. Wenn Sie versuchen, eine Nachricht ohne Partitionsschlüssel innerhalb einer Transaktion, gibt Service Bus eine **InvalidOperationException** -Ausnahme. Wenn Sie versuchen, mehrere Nachrichten innerhalb einer Transaktion senden, die andere Partition Schlüssel, gibt Service Bus eine **InvalidOperationException** -Ausnahme. Zum Beispiel:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Bei Eigenschaften, die als Partitionsschlüssel verwendet stattdessen pins Service Bus Nachricht zu einem bestimmten Fragment. Dieses Verhalten tritt auf, ob eine Transaktion verwendet wird. Es wird empfohlen, dass Sie Partitionsschlüssel nicht angeben, ist es nicht erforderlich.

## <a name="using-sessions-with-partitioned-entities"></a>Sessions verwenden mit partitionierten

Um eine transaktionale Meldung an einen sitzungsorientiert Thema oder eine Warteschlange senden, muss die Nachricht [BrokeredMessage.SessionId][] -Eigenschaft festgelegt. Wenn die [BrokeredMessage.PartitionKey][] -Eigenschaft angegeben ist, muss die [SessionId][] -Eigenschaft identisch sein. Andernfalls gibt Service Bus eine **InvalidOperationException** -Ausnahme.

Im Gegensatz zu regulären (nicht partitioniert) Warteschlangen oder Themen kann nicht mehrere Nachrichten zu anderen Sitzungen mit einer einzigen Transaktion. Wenn versucht, gibt Service Bus eine **InvalidOperationException **-Ausnahme. Zum Beispiel:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatische Weiterleitung mit partitionierten

Azure Service Bus unterstützt automatische Weiterleitung von, an oder zwischen partitionierten. Um automatische Nachricht weiterleiten zu aktivieren, legen Sie die Eigenschaft [QueueDescription.ForwardTo][] auf dem Quell- oder Abonnement. Wenn die Nachricht Partitionsschlüssel ([SessionId][], [PartitionKey][]oder [MessageId][]) gibt diesen Partitionsschlüssel Zielentität dient.

## <a name="considerations-and-guidelines"></a>Hinweise und Richtlinien

- **Konsistente Features**: Wenn eine Entität verwendet Features wie Sessions, Duplikaterkennungsregeln oder explizite Steuerung der Partitionierung Schlüssel, messaging Operationen immer zu bestimmten Fragmenten weitergeleitet. Wenn eines der Fragmente hohem Datenverkehr oder zugrunde liegenden Speicher fehlerhaft ist, die fehlschlägt und Verfügbarkeit reduziert. Insgesamt ist die Konsistenz noch viel höher als Entitäten nicht partitioniert. nur eine Teilmenge der Verkehr ist, den gesamten Datenverkehr Probleme.
- **Management**: wie erstellen, aktualisieren und Löschen für alle Fragmente des Unternehmens erfolgen müssen. Fragment problematisch ist kann es Fehler für diese Vorgänge führen. Für den Ladevorgang muss Informationen wie Anzahl von Nachrichten aus allen aggregiert werden. Fragment fehlerhaft ist, wird die Entität Verfügbarkeitsstatus Limited gemeldet.
- **Niedrige Lautstärke Nachricht Szenarien**: in solchen Situationen, besonders wenn das HTTP-Protokoll verwenden, müssen Sie möglicherweise mehrere ausführen Empfangsvorgänge um alle Nachrichten zu erhalten. Für Anfragen empfangen führt alle Fragmente empfangen und speichert alle empfangenen Antworten. Eine nachfolgende empfangen Anforderung auf derselben Verbindung von diesem Cache profitieren und erhalten würde Wartezeiten niedriger sein. Wenn Sie mehrere Anschlüsse oder HTTP verwenden, richtet, die für jede Anforderung eine neue Verbindung. Daher besteht keine Garantie, die auf demselben Knoten landen würden. Der Receive-Methode gibt **null**zurück, wenn alle vorhandene Nachrichten gesperrt und in einen anderen front-End zwischengespeichert. Nachrichten schließlich ablaufen und erneut empfangen werden. HTTP-Keep-alive wird empfohlen.
- **Nachrichten durchsuchen-einsehen**: PeekBatch gibt die Anzahl der Nachrichten in der [MessageCount Eigenschaft](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx)angegebenen nicht immer zurück. Es gibt zwei häufige Gründe dafür. Deshalb aggregierte Sammlung von Nachrichten die maximale Größe von 256 KB größer. Ein weiterer Grund ist, dass wenn die Warteschlange oder das Thema der [EnablePartitioning-Eigenschaft](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) auf **true**festgelegt ist, eine Partition nicht genügend Nachrichten an die angeforderte Anzahl von Nachrichten führen. Im Allgemeinen, wenn eine Anwendung eine bestimmte Anzahl von Nachrichten erhalten möchte, sollte aufgerufen [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) wiederholt bis die Anzahl der Nachrichten ruft, oder es keine weiteren Nachrichten sind einsehen. Weitere Informationen, einschließlich Codebeispiele finden Sie unter [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) oder [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Zuletzt hinzugefügten features

- Hinzufügen oder Entfernen von Regel jetzt mit partitionierten unterstützt. Andere Entitäten nicht partitioniert werden diese Vorgänge unter Transaktionen unterstützt. 
- AMQP ist jetzt zum Senden und Empfangen von Nachrichten aus einer partitionierten Entität unterstützt.
- AMQP wird nun für die folgenden Operationen unterstützt: [Senden](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Batch erhalten](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx) [Empfangen von Sequenznummer](https://msdn.microsoft.com/library/azure/hh330765.aspx), [einsehen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Sperren erneuern](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Zeitplannachricht](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Geplante Nachricht abbrechen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Regel hinzufügen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Regel entfernen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Sitzung erneuern Sperren](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Set Session State](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Get Sitzungszustand](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Sitzung Nachrichten einsehen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)und [Sessions auflisten](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Partitionierte Entitäten Grenzen

Service Bus stellt derzeit die folgenden Nachteile auf partitionierten Warteschlangen und Themen:

-   Partitionierte Warteschlangen und Themen unterstützt nicht sendende von Nachrichten, die zu anderen Sitzungen in einer einzigen Transaktion gehören.
-   Service Bus können derzeit bis zu 100 partitionierte Warteschlangen oder Themen pro Namespace. Jede partitionierte Warteschlange oder ein Thema wird maximal 10.000 Entities pro Namespace (gilt nicht für Premium-Ebene).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Beiträge [AMQP 1.0-Unterstützung für Service Bus partitioniert Warteschlangen und Themen][] erfahren Partitionierung messaging Entitäten. 

  [Service-Architektur]: service-bus-architecture.md
  [Azure-portal]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [AMQP 1.0-Unterstützung für partitionierte Servicebuswarteschlangen und Themen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
