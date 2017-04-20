<properties 
    pageTitle="Schreiben, verwenden Servicebuswarteschlangen | Microsoft Azure"
    description="Wie Sie eine einfache Warteschlange-Anwendung schreiben, die Service Bus verwendet."
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-queues"></a>Erstellen Sie, Servicebuswarteschlangen verwenden

Dieses Thema beschreibt Servicebuswarteschlangen und zeigt, wie eine einfache Warteschlange-Anwendung schreiben, die Service Bus verwendet.

Betrachten Sie ein Szenario aus der Einzelhandel in der Daten aus einzelnen Kasse POS-Terminals ein Lagerverwaltungssystem müssen, die Daten verwendet weitergeleitet werden, um zu bestimmen, wenn der Bestand aufgefüllt werden muss. Diese Lösung verwendet Service Bus messaging für die Kommunikation zwischen den Terminals und Inventory Managementsystem, wie in der folgenden Abbildung dargestellt:

![Servicebuswarteschlangen Bild 1](./media/service-bus-create-queues/IC657161.gif)

Jedes POS-Terminal meldet die Verkaufsdaten von Nachrichten an **DataCollectionQueue**senden. Diese Nachrichten verbleiben in dieser Warteschlange, bis sie durch das Lagerverwaltungssystem abgerufen werden. Dieses Muster wird häufig *asynchrones messaging*bezeichnet da POS-Terminal nicht warten, bis eine Antwort für das Lagerverwaltungssystem fortgesetzt.

## <a name="why-queuing"></a>Warum queuing?

Bevor wir den Code, die zum Einrichten dieser Anwendung erforderlich ist betrachten, sollten Sie die Vorteile der Verwendung einer Warteschlange in diesem Szenario anstatt den POS-Terminals sprechen direkt (synchron) das Lagerverwaltungssystem.

### <a name="temporal-decoupling"></a>Zeitliche Entkopplung

Mit der asynchronen messaging Hersteller und Verbraucher keinen gleichzeitig online sein. Die Messaginginfrastruktur speichert Nachrichten zuverlässig bis konsumierenden Partei empfangen werden. Dies bedeutet, dass die Komponenten einer verteilten Anwendung, entweder freiwillig getrennt werden können. z. B. für Wartung oder aufgrund eines Absturzes Komponente ohne Beeinträchtigung des gesamten Systems. Darüber hinaus haben die verarbeitende Anwendung nur während bestimmter Zeiten online sein. Beispielsweise in diesem Retail haben das Lagerverwaltungssystem nur nach dem Ende des Arbeitstages online geschaltet.

### <a name="load-leveling"></a>Abgleich laden

Häufig variiert Systemlast im Laufe der Zeit, während die Bearbeitungszeit für jede Arbeitseinheit normalerweise konstant ist. Vermittlung Nachricht Erzeugern und Verbrauchern mit einer Warteschlange bedeutet, dass nur die verarbeitende Anwendung (Arbeitskraft) bereitgestellt werden, um eine durchschnittliche Auslastung als auch service. Die Tiefe der Warteschlange wächst und wie eingehende Load variiert. Dies spart direkt hinsichtlich der Infrastruktur erforderlichen Anwendung geladen.

![Servicebuswarteschlangen Bild 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Lastenausgleich

Mit steigender Last können mehrere Arbeitsprozesse Lesen aus der Warteschlange hinzugefügt werden. Jede Nachricht wird nur von einem der Arbeitsprozesse verarbeitet. Außerdem ermöglicht dieses Pull-basierten Lastenausgleich für optimale Nutzung der Arbeitskraft Computer selbst wenn die Arbeitskraft Computer hinsichtlich Leistung, wie sie Nachrichten mit eigenen maximale Rate ziehen. Dieses Muster wird oft konkurrierenden Consumer-Muster bezeichnet.

![Servicebuswarteschlangen Bild 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Kopplung

Mit Message queuing Nachricht Erzeugern und Verbrauchern zwischen bietet einen integrierten Kopplung zwischen den Komponenten. Da der Erzeuger und der Verbraucher nicht gegenseitig kennen, kann Consumer ohne Auswirkung auf die Erzeuger aktualisiert werden. Darüber hinaus kann die Messagingtopologie entwickeln, ohne vorhandene Endpunkte. Wir diskutieren dies mehr veröffentlichen/abonnieren zu sprechen.

## <a name="show-me-the-code"></a>Code anzeigen

Im folgenden Abschnitt wird das Service Bus verwenden, um diese Anwendung zu erstellen.

### <a name="sign-up-for-an-azure-account"></a>Für ein Azure-Konto anmelden

Ein Azure-Konto benötigen um Arbeiten mit Service Bus. Wenn Sie nicht bereits eine haben, melde dich für ein kostenloses Konto [hier](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Erstellen eines Namespaces

Haben Sie ein Abonnement können Sie [einen neuen Namespace erstellen](service-bus-create-namespace-portal.md). Jeder Namespace fungiert als Bereichseinheit für eine Reihe von Service Bus Entitäten. Benennen Sie dem neuen Namespace eindeutig allen Service Bus-Konten. 

### <a name="install-the-nuget-package"></a>Das NuGet-Paket

Um den Service Bus-Namespace verwenden, muss eine Anwendung Service Bus Assembly ausdrücklich Microsoft.ServiceBus.dll verweisen. Finden Sie diese Assembly als Teil des Microsoft Azure SDK und der Download steht unter [Azure SDK-Downloadseite](https://azure.microsoft.com/downloads/). [Service Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) ist jedoch am einfachsten zu Service Bus-API und die Anwendung mit allen Service Bus Abhängigkeiten konfigurieren.

### <a name="create-the-queue"></a>Erstellen der Warteschlange

Verwaltungsvorgänge für Service Bus messaging Entitäten (Warteschlangen und Veröffentlichen/Abonnieren-Themen) erfolgt über die [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse. Service Bus verwendet eine Grundlage Sicherheitsmodell [Shared Access Signatur (SAS)](service-bus-sas-overview.md) . Die [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) -Klasse stellt ein Sicherheitstokenanbieter mit integrierten Factorymethoden einige bekannte Tokenanbieter zurückgeben. Wir verwenden eine [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) Methode der SAS-Anmeldeinformationen. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Instanz wird dann mit der Basisadresse des Service Bus-Namespace und der Tokenanbieter erstellt.

[NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse stellt Methoden zum Erstellen, auflisten und Löschen von Messaging-Entitäten. Der hier gezeigte Code zeigt wie [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Instanz erstellt und die **DataCollectionQueue** -Warteschlange erstellt.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Beachten Sie, dass [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) -Methode-Überladung, die Eigenschaften der Warteschlange abgestimmt werden können. Beispielsweise können Sie die Time-to-live (TTL) Standardwert für Nachrichten an die Warteschlange gesendet festlegen.

### <a name="send-messages-to-the-queue"></a>Nachrichten an die Warteschlange senden

Für Vorgänge zur Laufzeit Service Bus Entitäten; Beispielsweise muss Nachrichten senden und empfangen, eine Anwendung zuerst ein [Fall](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) -Objekt erstellen. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse ähnlich, wird die Instanz [Fall](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) von der Basisadresse Service-Namespace und der Tokenanbieter erstellt.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Nachrichten an und Empfangen von Service Bus Warteschlangen sind Instanzen der Klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Diese Klasse besteht aus einer Reihe von Standardeigenschaften (wie [Beschriftung](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) und [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) ein Wörterbuch Anwendungseigenschaften, und eine beliebige Daten enthält. Eine Anwendung kann [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) serialisiert das Objekt mit Text übergeben jedes serialisierbare Objekt (im folgende Beispiel wird ein **SalesData** -Objekt, das Daten vom POS-Terminal darstellt) festlegen. Alternativ kann ein [Stream](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -Objekt bereitgestellt werden.

Die einfachste Möglichkeit, Nachrichten an eine Warteschlange, in unserem Fall **DataCollectionQueue**ist mit [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) ein [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) Objekt direkt von der Instanz der [Fall](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Empfangen von Nachrichten aus der Warteschlange

Um Nachrichten aus der Warteschlange können Sie ein [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) -Objekt direkt aus der [Fall](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) mit [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)erzeugen. Empfänger der Nachricht können in zwei verschiedenen Modi arbeiten: **ReceiveAndDelete** und **PeekLock**. [Abschlussaufruf](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) wird festgelegt, wenn der Nachrichtenempfänger als Parameter für den Aufruf der [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) erstellt wird.


Bei Verwendung des Modus **ReceiveAndDelete** ist erhalten eine Operation Einzelbild. also wenn Service Bus die Anforderung empfängt, es markiert die Nachricht als verbraucht wird und an die Anwendung zurückgegeben. **ReceiveAndDelete** ist das einfachste und am besten für Szenarios, in denen die Anwendung tolerieren kann eine Nachricht nicht verarbeitet werden, falls ein Fehler auftritt. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus die Nachricht markiert als verbraucht, wenn Anwendungsneustarts und verbraucht Nachrichten erneut startet er die Nachricht verpasst haben wird, die vor dem Absturz verbraucht wurde.

Im **PeekLock** -Modus wird der Empfang ein zweistufiger Vorgang macht es möglich, die fehlende Nachrichten tolerieren unterstützen. Wenn Service Bus die Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nachdem die Anwendung beendet Verarbeitung der Nachricht (oder zuverlässig zur späteren Verarbeitung speichert), abgeschlossen die zweite Phase des Prozesses empfangen fordert die empfangene Nachricht [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ist. Beim Service Bus rufen Sie [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) sieht, markiert die Nachricht als verbraucht.

Zwei Ergebnisse sind möglich. Zuerst ist die Anwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht können sie [verlassen](https://msdn.microsoft.com/library/azure/hh181837.aspx) in der empfangenen Nachricht (statt [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)) aufrufen. Dadurch Service Bus Nachricht entsperren und es wieder denselben Verbraucher oder anderen abschließen Consumer empfangen werden. Zweitens ein Timeout der Sperre zugeordnet ist und die Anwendung die Nachricht nicht verarbeiten kann die Sperre Timeout abläuft, bevor (z. B. wenn die Anwendung stürzt ab), wenn Service Bus Nachricht entsperren und Verfügbarmachen erneut empfangen werden (im Grunde eines Vorgangs [aufgeben](https://msdn.microsoft.com/library/azure/hh181837.aspx) standardmäßig).

Beachten Sie, dass wenn die Anwendung abstürzt, nachdem die Nachricht verarbeitet, aber vor dem [abschließen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) der Anforderung wurde ausgegeben, die Nachricht an die Anwendung beim Neustart zugestellt wird. Dies wird häufig *Mindestens* Verarbeitung bezeichnet. Dies bedeutet, dass jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, ist zusätzlicher Logik der Anwendung Duplikate erkennen erforderlich. Dies kann anhand der [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) -Eigenschaft der Nachricht erreicht werden. Der Wert dieser Eigenschaft bleibt über Zustellversuche. Dies ist *Genau einmal* Verarbeitung bezeichnet.

Die hier gezeigte Code empfängt und verarbeitet eine Nachricht der **PeekLock** Modus ist, wenn kein [Abschlussaufruf](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) Wert explizit bereitgestellt wird.

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
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

### <a name="use-the-queue-client"></a>Verwenden Sie den Client Warteschlange

Die Beispiele in diesem Abschnitt erstellt Objekte [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) und [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) direkt von [Fall](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) zu senden bzw. Empfangen von Nachrichten aus der Warteschlange. Eine Alternative ist die Klasse [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) verwendet, welche unterstützt sowohl Sende- und Empfangsvorgänge neben erweiterte Funktionen, z. B..

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangen bearbeitet haben, finden Sie unter [Erstellen Applikationen, die Service Bus-Themen und Abonnements verwenden,](service-bus-create-topics-subscriptions.md) diese Diskussion mithilfe der Publish/subscribe-Funktionen der Service Bus-Themen und Abonnements.