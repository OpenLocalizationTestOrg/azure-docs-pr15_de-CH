<properties 
    pageTitle="Verwendung von Servicebuswarteschlangen mit Python | Microsoft Azure" 
    description="Informationen Sie zum Verwenden von Azure Service Bus-Warteschlangen aus Python." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>Wie Servicebuswarteschlangen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dieser Artikel beschreibt, wie Sie Servicebuswarteschlangen. Die Proben sind in Python geschrieben und [Python Azure Service Bus-Paket][]verwenden. Die Szenarios enthalten **Warteschlangen erstellen, senden und Empfangen von Nachrichten**und **Löschen von Warteschlangen**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Python oder [Python Azure Service Bus-Paket][]finden Sie unter [Python-Installationshandbuch](../python-how-to-install.md).

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Das **ServiceBusService** -Objekt können Sie mit Warteschlangen arbeiten. Fügen Sie folgenden Code im oberen Bereich einer Python-Datei, in der Sie programmgesteuert Service Bus zugreifen möchten:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

Der folgende Code erstellt ein **ServiceBusService** -Objekt. Ersetzen Sie `mynamespace`, `sharedaccesskeyname`, und `sharedaccesskey` mit Namespace, gemeinsamen Zugriff Signatur (SAS) Schlüssel Namen und Wert.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Die Werte für den Schlüsselnamen SAS und Wert in [Azure-Verwaltungsportal][] Verbindungsinformationen oder im Visual Studio- **Eigenschaften** finden Sie beim Service Bus-Namespace im Server-Explorer auswählen (siehe vorherigen Abschnitt).

```
bus_service.create_queue('taskqueue')
```

**Create_queue** unterstützt auch zusätzliche Optionen Warteschlange Standardeinstellungen Nachricht Gültigkeitsdauer (TTL) oder maximale Warteschlangengröße überschreiben können. Im folgende Beispiel wird die maximale Warteschlangengröße 5GB und den TTL-Wert auf 1 Minute:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Nachrichten in einer Warteschlange

Eine Nachricht an eine Warteschlange Service Bus, ruft die Anwendung die **Senden\_Warteschlange\_Nachricht** Methode für das **ServiceBusService** -Objekt.

Im folgenden Beispiel wird veranschaulicht, wie die Warteschlange *Taskqueue verwenden* eine Testnachricht an **Senden\_Warteschlange\_Meldung**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Servicebuswarteschlangen unterstützt eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB im [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Gibt es keine Beschränkung für die Anzahl der Nachrichten in einer Warteschlange, jedoch ist es die Gesamtgröße der Nachrichten von einer Warteschlange. Diese Größe wird zum Zeitpunkt der Erstellung mit einem oberen Grenzwert von 5 GB definiert. Weitere Informationen zu Kontingenten finden Sie unter [Service Bus Kontingente][].

## <a name="receive-messages-from-a-queue"></a>Empfangen von Nachrichten aus einer Warteschlange

Nachrichten mit der **wird\_Warteschlange\_Nachricht** Methode des **ServiceBusService** -Objekts:

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Nachrichten aus der Warteschlange gelöscht, sie beim Lesen der Parameter **Peek\_Sperren** auf **False**festgelegt ist. Lesen (Peek) und die Nachricht ohne durch Festlegen des Parameters aus der Warteschlange löschen Sperren **Blick\_Sperren** auf **True**.

Das Verhalten lesen und Löschen der Nachricht als Teil der Receive-Methode ist die einfachste und am besten für Szenarios, in denen eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus die Nachricht als verbraucht markiert haben wird, wird dann die Anwendung neu gestartet und beginnt, Nachrichten erneut es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Wenn die **Blick\_Sperren** -Parameter auf **True**festgelegt ist, erhalten wird ein zweistufiger Vorgang unterstützt Clientanwendungen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nach die Anwendung Verarbeitung der Meldung beendet (oder zuverlässig zur späteren Verarbeitung speichert) abgeschlossen die zweite Phase des Prozesses empfangen durch Aufrufen der **delete** -Methode des Objekts **angezeigt** wird. Die **delete** -Methode die Nachricht als verbraucht und aus der Warteschlange entfernt.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendungsabstürzen und unlesbar Nachrichten

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Ist eine empfängeranwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht können sie die **unlock** -Methode im **Message** -Objekt aufrufen. Dadurch Service Bus zu entsperren der Nachricht in der Warteschlange zur wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht in der Warteschlange gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Timeout für die Sperre abläuft (z. B. wenn die Anwendung abstürzt), Service Bus wird die Nachricht automatisch entsperrt und Verfügbarmachen erneut empfangen werden.

Die Anwendung stürzt nach dem Verarbeiten der Nachricht, aber bevor die **delete** -Methode aufgerufen wird, wird dann die Meldung an die Anwendung zurückgeliefert werden beim Neustart. Wird oft genannt **Mindestens einmal verarbeiten**, d. h. jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler ihre Anwendung doppelte Nachrichtenübermittlung behandeln zusätzlichen Logik hinzufügen. Dies geschieht häufig mithilfe der **MessageId** -Eigenschaft der Nachricht über Übermittlungsversuche konstant bleibt.

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Servicebuswarteschlangen bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

-   [Warteschlangen, Themen und Abonnements][]anzeigen

[Azure-Verwaltungsportal]: https://manage.windowsazure.com
[Python Azure Service Bus-Paket]: https://pypi.python.org/pypi/azure-servicebus  
[Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
[Service Bus Kontingente]: service-bus-quotas.md
 
