<properties 
    pageTitle="Bewährte Methoden zur Verbesserung der Leistung mit Service Bus | Microsoft Azure"
    description="Beschreibt das Azure Service Bus um zu optimieren, wenn vermittelte Nachrichten austauschen."
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
    ms.date="10/25/2016"
    ms.author="sethm" />

# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Best Practices zur Steigerung der Leistung mit Service Bus Messaging

Dieses Thema beschreibt, wie mit Azure Service Bus Messaging vermittelten Nachrichten beim optimieren. Der erste Teil dieses Thema beschreibt die unterschiedlichen Mechanismen, die zur Leistungssteigerung beitragen, angeboten werden. Der zweite Teil enthält Hinweise zum Service Bus nutzen, die die beste Leistung in einem bestimmten Szenario bietet.

In diesem Thema bezieht sich der Begriff "Client" auf eine Entität, die Service Bus zugreift. Ein Client kann die Rolle von einem Absender oder Empfänger benötigen. Der Begriff "Absender" wird ein Service Bus-Warteschlange oder Thema Client verwendet, die Nachrichten an eine Service Bus-Warteschlange oder ein Thema sendet. Der Begriff "Receiver" bezieht sich auf eine Service Bus Warteschlange oder Abonnement-Client, der Nachrichten aus einer Warteschlange Service Bus oder Abonnement empfängt.

Diese Abschnitte erläutern einige Konzepte Service Bus verwendet, um die Performance zu steigern.

## <a name="protocols"></a>Protokolle

Service Bus ermöglicht Clients das Senden und Empfangen von Nachrichten über drei Protokolle

1. Erweiterte Message Queuing Protocol (AMQP)

2. Servicebus Messaging Protocol (SBMP)

3. HTTP

AMQP und SBMP sind effizienter, da sie die Verbindung zum Service Bus verwalten, solange die Messaging-Factory. Es implementiert auch Batchverarbeitung und Prefetch. Sofern nicht ausdrücklich erwähnt, davon aus, dass alle Informationen in diesem Thema die AMQP oder SBMP.

## <a name="reusing-factories-and-clients"></a>Wiederverwenden von Factorys und clients

Service Bus werden Client, [QueueClient][] oder [MessageSender][]durch ein Objekt [Fall][] Objekte mit internen Verbindungen. Sie sollte messaging Fabriken oder Warteschlange, Thema und Abonnement-Clients nicht schließen, nachdem Sie eine Nachricht senden und dann neu erstellen die nächste Nachricht senden. Die Verbindung mit dem Service Bus löscht messaging Factory schließen und eine neue Verbindung hergestellt wird, beim Neuerstellen Factory. Herstellen einer Verbindung ist ein aufwändiger Vorgang, den Sie erneut mit der gleichen Factory Clientobjekte für mehrere Operationen vermeiden können. Das [QueueClient][] -Objekt können sicher zum Senden von Nachrichten von gleichzeitige asynchrone Vorgänge und mehrere Threads. 

## <a name="concurrent-operations"></a>Gleichzeitige Vorgänge

Eine Operation (senden, empfangen, löschen usw.) dauert einige Zeit. Diesmal enthält der Vorgang mit Service Bus neben die Wartezeit der Anforderung und der Antwort. Erhöhen Sie die Anzahl der Vorgänge pro müssen Vorgänge gleichzeitig ausgeführt werden. Sie erreichen dies auf verschiedene Arten:

-   **Asynchrone Vorgänge**: Client plant Vorgänge durch asynchrone Vorgänge. Die nächste Anforderung wird gestartet, bevor die Anfrage abgeschlossen ist. Folgendes ist ein Beispiel für einen asynchronen Sendevorgang:

    ```
    BrokeredMessage m1 = new BrokeredMessage(body);
    BrokeredMessage m2 = new BrokeredMessage(body);
    
    Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #1");
      });
    Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #2");
      });
    Task.WaitAll(send1, send2);
    Console.WriteLine("All messages sent");
    ```

    Dies ist ein Beispiel für eine asynchrone receive-Methode:
    
    ```
    Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    
    Task.WaitAll(receive1, receive2);
    Console.WriteLine("All messages received");
    
    async void ProcessReceivedMessage(Task<BrokeredMessage> t)
    {
      BrokeredMessage m = t.Result;
      Console.WriteLine("{0} received", m.Label);
      await m.CompleteAsync();
      Console.WriteLine("{0} complete", m.Label);
    }
    ```

-   **Mehrere Fabriken**: alle Clients (Absender neben Empfänger), die vom selben Unternehmen erstellt eine TCP-Verbindung verwenden. Der maximale Durchsatz ist durch die Anzahl der Vorgänge begrenzt, die durch diese TCP-Verbindung gehen. Der Durchsatz, der mit einer Factory unterschiedlich mit TCP-Roundtrip-Zeiten und Nachrichtengröße. Höhere Durchsatzraten erhalten, sollten Sie mehrere messaging Factorys verwenden.

## <a name="receive-mode"></a>Empfangsmodus

Beim Erstellen eines Warteschlange oder Abonnement-Clients können Sie eine Empfangsmodus: *Peek Sperren* oder *empfangen und gelöscht*. Standardmäßig erhalten ist [PeekLock][]. Wenn dieser Modus sendet der Client eine Anforderung zum Empfangen einer Nachricht von Service Bus. Nachdem der Client die Nachricht erhalten hat, sendet er eine Anforderung zum Abschließen der Nachricht.

Wenn [ReceiveAndDelete][]Empfangsmodus festlegen, werden beide Schritte in einer Anforderung kombiniert. Dies reduziert die Anzahl der Vorgänge und verbessern den Gesamtdurchsatz von Nachrichten. Diese Leistungssteigerung kommt Gefahr gehen Nachrichten verloren.

Service Bus unterstützt keine Transaktionen für empfangen und löschen. Darüber hinaus sind Peek-Sperre Semantik für Szenarios, in denen der Client verschieben möchte, oder [unzustellbare](service-bus-dead-letter-queues.md) eine Nachricht.

## <a name="client-side-batching"></a>Clientseitige Batchverarbeitung

Clientseitige Batchverarbeitung kann ein Warteschlange oder Thema Client das Senden einer Nachricht für einen bestimmten Zeitraum. Der Client in diesem Zeitraum weitere Nachrichten sendet, sendet die Nachrichten in einem einzigen Batch. Clientseitige Batchverarbeitung wird auch einen Warteschlange-Abonnement Client mehrere Anfragen **vollständig** in einer einzelnen Anforderung Batch. Batchverarbeitung ist nur verfügbar für asynchrone Vorgänge **Senden** und **abgeschlossen** . Synchrone Vorgänge werden sofort an den Service Bus-Dienst gesendet. Batchverarbeitung für Peek auftreten oder nicht Empfangsvorgänge und Batchverarbeitung über Clients erfolgt.

Stapel die Maximalgröße überschreitet, die letzte Meldung aus dem Stapel entfernt und der Client sendet den Stapel sofort. Die letzte Nachricht wird die erste Nachricht im nächsten Batch. Standardmäßig verwendet ein Client eine Stapelverarbeitung Intervall von 20 ms. [BatchFlushInterval][] -Eigenschaft festlegen, bevor die messaging Factory erstellt, um Stapel Intervall zu ändern. Diese Einstellung wirkt sich auf alle Clients, die von dieser Factory erstellt werden.

Um Batchverarbeitung zu deaktivieren, legen Sie die [BatchFlushInterval][] -Eigenschaft auf **TimeSpan.Zero**. Zum Beispiel:

```
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Batchverarbeitung wirkt sich nicht auf die Anzahl der berechenbaren messaging-Abläufen und steht nur für Service Bus-Clientprotokoll. Das HTTP-Protokoll unterstützt keine Batchverarbeitung.

## <a name="batching-store-access"></a>Batchverarbeitung Speicherzugriff

Erhöhen Sie den Durchsatz der Warteschlange/Thema/Abonnement batches Service Bus mehrere Nachrichten internen Speicher schreibt. Aktiviert eine Warteschlange oder ein Thema verarbeitet Nachrichten in den Speicher geschrieben werden. Wenn eine Warteschlange oder das Abonnement aktiviert werden Nachrichten aus dem Speicher löschen zusammengefasst werden. Wenn für eine Entität im Batchmodus Speicherzugriff aktiviert, verzögert Service Bus einen Shop-Schreibvorgang zu dieser Entität von bis zu 20 ms. Weitere Speichervorgänge, die auftreten, während dieses Intervalls werden zum Stapel hinzugefügt. Im Batchmodus Speicherzugriff betrifft nur Vorgänge **Senden** und **abgeschlossen** . Empfangen von Operationen sind nicht betroffen. Im Batchmodus Speicherzugriff ist eine Eigenschaft einer Entität. Batchverarbeitung tritt für alle Personen, die im Batchmodus Speicher ermöglichen.

Beim Erstellen einer neuen Warteschlange, Thema oder Abonnement ist im Batchmodus Speicherzugriff standardmäßig aktiviert. Deaktivieren Sie im Batchmodus Speicherzugriff wird vor dem Erstellen der Entität die [EnableBatchedOperations][] -Eigenschaft auf **false** festgelegt. Zum Beispiel:

```
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Im Batchmodus Speicherzugriff wirkt sich nicht auf die Anzahl der berechenbaren messaging Operationen und eine Eigenschaft einer Warteschlange, Thema oder Abonnement. Es ist unabhängig vom Empfangsmodus und das Protokoll, das zwischen einem Client und Service Bus-Dienst verwendet wird.

## <a name="prefetching"></a>Prefetch-Vorgänge

Prefetch ermöglicht den Warteschlange oder das Abonnement Client weitere Nachrichten vom Dienst geladen, wenn einen Receive-Vorgang ausführt. Der Client speichert die Nachrichten im lokalen Cache. Die Größe des Caches bestimmt die Eigenschaften [QueueClient.PrefetchCount][] und [SubscriptionClient.PrefetchCount][] . Jeder Client, der Prefetch ermöglicht verwaltet einen eigenen Cache. Ein Cache ist auf Clients verwendet werden. Der Client initiiert eine Receive-Methode und der Cache leer ist, sendet der Dienst Nachrichten. Die Größe des Stapels entspricht der Größe des Cache oder 256KB, je nachdem, was kleiner ist. Der Client initiiert einen Empfangsvorgang Cache eine Meldung enthält, ist die Nachricht aus dem Cache abgerufen.

Der Dienst gesperrt Prefetch Nachricht vorab eine Nachricht. Dadurch kann nicht Prefetch Nachricht von einem anderen Empfänger empfangen werden. Wenn der Empfänger die Nachricht durchführen kann, vor Ablauf die Sperre, wird die Nachricht an andere Empfänger verfügbar. Prefetch Kopie der Nachricht im Cache bleibt. Receiver, die abgelaufene zwischengespeicherte Kopie verwendet erhalten eine Ausnahme, wenn er versucht, die Nachricht abgeschlossen. Standardmäßig läuft die Nachricht sperren nach 60 Sekunden. Dieser Wert kann auf 5 Minuten verlängert. Um den Verbrauch von abgelaufenen Nachrichten zu verhindern, sollte die Cachegröße immer kleiner als die Anzahl der Nachrichten, die von einem Client innerhalb der Sperre Timeoutintervall verwendet werden können.

Bei Standard Sperre Ablaufzeit von 60 Sekunden ist gut [SubscriptionClient.PrefetchCount][] 20 Mal die maximale Verarbeitungsraten aller Empfänger der Factory. Beispielsweise eine Factory erstellt 3 Empfänger und Empfänger kann bis zu 10 Nachrichten pro Sekunde verarbeiten. Die Anzahl der Prefetch darf 20 nicht überschreiten\*3\*10 = 600. Standardmäßig ist [QueueClient.PrefetchCount][] auf 0 festgelegt, dass keine weiteren Nachrichten vom Dienst abgerufen werden.

Den Gesamtdurchsatz für eine Warteschlange oder das Abonnement Prefetch Nachrichten erhöht werden, da die Anzahl der Nachricht bzw. Schleifen verringert. Abrufen der ersten Nachricht, aber dauert (durch die erhöhte Nachrichtengröße) länger. Abgerufene Nachrichten werden schneller diese Nachrichten vom Client heruntergeladen wurden.

Time to live (TTL)-Eigenschaft einer Nachricht wird vom Server gleichzeitig überprüft die Nachricht an den Client senden. Der Client überprüft nicht die TTL-Eigenschaft beim Empfang der Meldung. Stattdessen kann die Nachricht auch empfangen werden Gültigkeitsdauer der Nachricht übergeben wurde, während die Nachricht vom Client zwischengespeichert wurde.

Prefetch wirkt sich nicht auf die Anzahl der berechenbaren messaging-Abläufen und steht nur für Service Bus-Clientprotokoll. Das HTTP-Protokoll unterstützt keine Prefetch. Prefetch ist für synchrone und asynchrone Operationen erhalten.

## <a name="express-queues-and-topics"></a>Express-Warteschlangen und Themen

Express Entitäten ermöglichen einen hohen Durchsatz und Wartezeit Szenarien reduziert. Mit express Wenn eine an eine Warteschlange oder ein Thema Nachricht ist die Nachricht nicht sofort in der Nachrichtenspeicher gespeichert. Stattdessen wird er im Arbeitsspeicher zwischengespeichert. Bleibt eine Meldung in der Warteschlange für mehr als ein paar Sekunden, wird es automatisch, damit Schutz vor Datenverlust aufgrund eines Ausfalls stabilen geschrieben. Schreiben in den Speichercache erhöht den Durchsatz und Wartezeit reduziert, da kein Zugriff bei stabilen, die die Nachricht gesendet wird. Nachrichten, die innerhalb von Sekunden verbraucht werden nicht im Nachrichtenspeicher geschrieben. Das folgende Beispiel erstellt ein express-Thema.

```
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Wenn eine Nachricht mit wichtigen Informationen nicht verloren gehen dürfen express Entität gesendet wird, kann der Absender Service Bus weiterhin, dass sofort Nachricht stabilen durch die [ForcePersistence][] -Eigenschaft auf **true**festlegen erzwingen.

## <a name="use-of-partitioned-queues-or-topics"></a>Verwendung von partitionierten Warteschlangen oder Themen

Intern Service Bus verwendet denselben Knoten und messaging zum Verarbeiten und Speichern aller Nachrichten für eine Messaging-Entität (Warteschlange oder Thema) speichern. Eine partitionierte Warteschlange oder ein Thema wird auf der anderen Seite mehrere Knoten und messaging Informationsspeicher verteilt. Partitionierte Warteschlangen und Themen nicht nur einen höheren Durchsatz als reguläre Warteschlangen und Themen finden, sie weisen eine hohe Verfügbarkeit. Legen Sie zum Erstellen einer partitionierten Entität [EnablePartitioning][] -Eigenschaft auf **true**im folgenden Beispiel. Weitere Informationen über partitionierte Entitäten finden Sie unter [messaging Entitäten partitioniert][].

```
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Verwenden mehrerer Warteschlangen

Wenn die erwartete Last nicht kann eine einzelne partitionierte Warteschlange oder ein Thema behandeln nicht möglich ist, eine partitionierte Warteschlange oder ein Thema verwenden, müssen Sie mehrere Entitäten messaging verwenden. Wenn mehrere Entitäten verwenden, erstellen Sie einen dedizierten Client für jede Entität, anstatt den gleichen Client für alle Entitäten.

## <a name="development--testing-features"></a>Entwicklung und Testfunktionen

Service Bus ist eine Funktion, die speziell für die Entwicklung verwenden die **nie in Produktionskonfigurationen verwendet werden soll**.

[TopicDescription.EnableFilteringMessagesBeforePublishing](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing.aspx)
- Wenn neue Regeln oder Filter zum Thema hinzugefügt, kann EnableFilteringMessagesBeforePublishing verwendet werden, zu überprüfen, ob neue Filterausdruck erwartungsgemäß.

## <a name="scenarios"></a>Szenarien

Die folgenden Abschnitte beschreiben normalerweise Messagingszenarien und Gliederung Service Bus-Einstellungen. Durchsatzraten werden so gering (weniger als 1 Nachricht pro Sekunde), Mittel (1 Nachricht pro Sekunde oder mehr, aber weniger als 100 Nachrichten pro Sekunde) und (100 Nachrichten/zweite oder größer). Die Anzahl der Clients gelten als klein (5 oder weniger), Mittel (mehr als 5 jedoch kleiner oder gleich 20), und Groß (mehr als 20).

### <a name="high-throughput-queue"></a>Warteschlange mit hohem Durchsatz

Ziel: Maximieren Sie den Durchsatz einer einzelnen Warteschlange. Die Anzahl von Sendern und Empfängern ist klein.

-   Verwenden einer partitionierten Warteschlange für verbesserte Performance und Verfügbarkeit.

-   Um die Gesamtrate senden in die Warteschlange zu erhöhen, verwenden Sie mehrere Nachricht Factorys Absendern erstellen. Verwenden Sie für jeden Absender asynchrone Vorgänge oder mehrere Threads.

-   Um die Gesamtrate empfangen aus der Warteschlange zu erhöhen, verwenden Sie mehrere Nachricht Factorys Empfänger erstellen.

-   Verwenden Sie asynchrone Vorgänge zu clientseitigen Batchverarbeitung.

-   Batchverarbeitung Intervall zu 50 ms Service Bus Client Protokoll Datenübertragungen Zahl. Bei mehreren Absendern erhöhen Sie das Batchverarbeitung Intervall 100 ms.

-   Lassen Sie im Batchmodus Speicherzugriff aktiviert. Dies erhöht die Gesamtrate, mit der Nachrichten in die Warteschlange geschrieben werden können.

-   Die Anzahl der Prefetch zu 20-Mal Max. alle Empfänger einer Factory festgelegt. Dies reduziert die Anzahl der Service Bus Client Protokoll Datenübertragungen.

### <a name="multiple-high-throughput-queues"></a>Mehrere Warteschlangen hohem Durchsatz

Ziel: Maximieren Sie Gesamtdurchsatz mehrere Warteschlangen. Eine einzelne Warteschlange ist Mittel oder hoch.

Um maximalen Durchsatz über mehrere Warteschlangen verwenden Sie die Einstellung um eine einzelne Warteschlange liegenden beschrieben. Darüber hinaus verwenden Sie verschiedene Factorys Clients erstellen, die senden oder Empfangen von anderen Warteschlangen.

### <a name="low-latency-queue"></a>Niedrige Latenz Warteschlange

Ziel: Minimieren Sie-Ende einer Warteschlange oder ein Thema. Die Anzahl von Sendern und Empfängern ist klein. Die Warteschlange ist klein oder Mittel.

-   Verwenden einer partitionierten Warteschlange für eine verbesserte Verfügbarkeit.

-   Deaktivieren Sie die clientseitige Batchverarbeitung. Der Client sendet eine Nachricht sofort.

-   Deaktivieren Sie im Batchmodus Speicherzugriff. Der Dienst hat die Nachricht sofort in den Speicher geschrieben.

-   Bei einem einzelnen Client festgelegt Anzahl der Prefetch auf 20 Mal die Verarbeitungsrate des Empfängers. Wenn mehrere Nachrichten gleichzeitig in der Warteschlange eintreffen, überträgt Service Bus-Clientprotokoll sie alle gleichzeitig. Der Client die nächste Nachricht empfängt, wird die Nachricht bereits im lokalen Cache. Der Cache sollte klein sein.

-   Wenn mehrere Clients, legen Sie die Anzahl der Prefetch auf 0. Dadurch kann der zweite Client die zweite Meldung wird während der erste Client noch die erste Nachricht verarbeitet.

### <a name="queue-with-a-large-number-of-senders"></a>Warteschlange mit einer großen Anzahl von Absendern

Ziel: Maximieren des Durchsatzes von eine Warteschlange oder ein Thema mit einer großen Anzahl von Absendern. Jeder Sender sendet Nachrichten mit mittleren. Die Anzahl der Empfänger ist klein.

Service Bus können bis zu 1000 gerätebasierte messaging Entität (oder 5000 mit AMQP). Dieses Limit wird auf Namespace-Ebene erzwungen und Warteschlangen, Themen/Abonnements maximal gleichzeitige Verbindungen pro Namespace Obergrenze. Diese Zahl ist für Warteschlangen zwischen Absender und Empfänger freigegeben. Wenn alle 1000 Verbindungen für Absender erforderlich sind, ersetzen Sie die Warteschlange ein Thema mit einem Abonnement. Ein Thema akzeptiert bis zu 1000 gleichzeitige Verbindungen von Absendern, während das Abonnement eine zusätzliche 1000 gerätebasierte Empfänger akzeptiert. Wenn mehr als 1000 gleichzeitige Absender erforderlich sind, sollten die Absender Service Bus Protokoll über HTTP Nachrichten senden.

Gehen Sie wie folgt vor, um maximalen Durchsatz zu erzielen:

-   Verwenden einer partitionierten Warteschlange für verbesserte Performance und Verfügbarkeit.

-   Verwenden Sie aller Absender in einem anderen Prozess befindet, eine einzige Factory pro Prozess.

-   Verwenden Sie asynchrone Vorgänge zu clientseitigen Batchverarbeitung.

-   Verwenden Sie die Batchverarbeitung Intervall von 20 ms Service Bus Client Protokoll Datenübertragungen Zahl.

-   Lassen Sie im Batchmodus Speicherzugriff aktiviert. Dies erhöht die Gesamtrate, mit der Nachrichten in der Warteschlange oder das Thema geschrieben werden können.

-   Die Anzahl der Prefetch zu 20-Mal Max. alle Empfänger einer Factory festgelegt. Dies reduziert die Anzahl der Service Bus Client Protokoll Datenübertragungen.

### <a name="queue-with-a-large-number-of-receivers"></a>Warteschlange mit einer großen Anzahl von Empfängern

Ziel: Maximieren Sie die Empfangsrate einer Warteschlange oder Abonnement mit einer großen Anzahl von Empfängern. Jeder Empfänger erhält Nachrichten mit einer mittleren Geschwindigkeit. Anzahl der Absender ist klein.

Service Bus können bis zu 1000 gleichzeitige Verbindungen zu einer Entität. Eine Warteschlange mehr als 1000 Empfänger benötigt, ersetzen Sie die Warteschlange mehrere Abonnements mit einem Thema. Jedes Abonnement kann bis zu 1000 gleichzeitige Verbindungen unterstützt. Alternativ können Empfänger die Warteschlange über HTTP zugreifen.

Gehen Sie wie folgt vor, um maximalen Durchsatz zu erzielen:

-   Verwenden einer partitionierten Warteschlange für verbesserte Performance und Verfügbarkeit.

-   Verwenden Sie Empfänger in einem anderen Prozess befindet, eine einzige Factory pro Prozess.

-   Empfänger können synchron oder asynchron verwenden. Angesichts der Mittel erhalten einen einzelnen Empfänger, wirkt clientseitige Batchverarbeitung eines vollständigen Antrags Empfänger Durchsatz sich nicht.

-   Lassen Sie im Batchmodus Speicherzugriff aktiviert. Dies reduziert die Gesamtauslastung der Entität. Es verringert auch die Gesamtrate, mit der Nachrichten in der Warteschlange oder das Thema geschrieben werden können.

-   Legen Sie die Anzahl die Prefetch auf einen niedrigen Wert (z. B. PrefetchCount = 10). Dadurch wird verhindert, dass Empfänger Leerlauf, während andere Empfänger zahlreicher Nachrichten zwischengespeichert haben.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Mit einer kleinen Anzahl von Abonnements

Ziel: Maximieren des Durchsatzes eines Themas mit einer kleinen Anzahl von Abonnements. Eine Meldung durch viele Abonnements, was bedeutet, dass die kombinierten Empfangsrate über alle Abonnements größer als die Übertragungsrate. Anzahl der Absender ist klein. Die Anzahl der Empfänger pro Abonnement ist klein.

Gehen Sie wie folgt vor, um maximalen Durchsatz zu erzielen:

-   Verwenden Sie partitionierter Thema für verbesserte Performance und Verfügbarkeit.

-   Um die Gesamtrate Senden zum Thema zu erhöhen, verwenden Sie mehrere Nachricht Factorys Absendern erstellen. Verwenden Sie für jeden Absender asynchrone Vorgänge oder mehrere Threads.

-   Um die Gesamtrate empfangen ein Abonnement zu erhöhen, verwenden Sie mehrere Nachricht Factorys Empfänger erstellen. Verwenden Sie für jeden Empfänger asynchrone Vorgänge oder mehrere Threads.

-   Verwenden Sie asynchrone Vorgänge zu clientseitigen Batchverarbeitung.

-   Verwenden Sie die Batchverarbeitung Intervall von 20 ms Service Bus Client Protokoll Datenübertragungen Zahl.

-   Lassen Sie im Batchmodus Speicherzugriff aktiviert. Dies erhöht die Gesamtrate, mit der Nachrichten in das Thema geschrieben werden können.

-   Die Anzahl der Prefetch zu 20-Mal Max. alle Empfänger einer Factory festgelegt. Dies reduziert die Anzahl der Service Bus Client Protokoll Datenübertragungen.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Mit einer großen Anzahl von Abonnements

Ziel: Maximieren des Durchsatzes eines Themas mit einer großen Anzahl von Abonnements. Eine Nachricht wird durch viele Abonnements kombinierten Empfangsrate über alle Abonnements größer als die Übertragungsrate ist. Anzahl der Absender ist klein. Die Anzahl der Empfänger pro Abonnement ist klein.

Themen mit einer großen Anzahl von Abonnements verfügbar in der Regel niedrige Gesamtdurchsatz, wenn alle Nachrichten an alle Abonnements weitergeleitet werden. Dies wird dadurch verursacht, die jeder Meldung oft und alle Nachrichten, die in einem Hilfethema enthalten sind und alle Abonnements im gleichen Speicher gespeichert. Es wird angenommen, dass die Anzahl der Absender und die Anzahl der Empfänger pro Abonnement klein ist. Servicebus unterstützt bis zu 2.000 Abonnements pro Thema.

Gehen Sie wie folgt vor, um maximalen Durchsatz zu erzielen:

-   Verwenden Sie partitionierter Thema für verbesserte Performance und Verfügbarkeit.

-   Verwenden Sie asynchrone Vorgänge zu clientseitigen Batchverarbeitung.

-   Verwenden Sie die Batchverarbeitung Intervall von 20 ms Service Bus Client Protokoll Datenübertragungen Zahl.

-   Lassen Sie im Batchmodus Speicherzugriff aktiviert. Dies erhöht die Gesamtrate, mit der Nachrichten in das Thema geschrieben werden können.

-   Anzahl der Prefetch auf 20 Mal die erwartete Empfangsrate in Sekunden festgelegt. Dies reduziert die Anzahl der Service Bus Client Protokoll Datenübertragungen.

## <a name="next-steps"></a>Nächste Schritte

Um weitere Informationen zum Optimieren der Leistung des Service Bus Siehe [partitionierte messaging Entitäten][].

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [Fall]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [PeekLock]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [ReceiveAndDelete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [BatchFlushInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval.aspx
  [EnableBatchedOperations]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations.aspx
  [QueueClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.prefetchcount.aspx
  [SubscriptionClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.prefetchcount.aspx
  [ForcePersistence]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.forcepersistence.aspx
  [EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [Partitionierte messaging Entitäten]: service-bus-partitioning.md
  