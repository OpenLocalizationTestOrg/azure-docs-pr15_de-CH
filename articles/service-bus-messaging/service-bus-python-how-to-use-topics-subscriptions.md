<properties 
    pageTitle="Verwendung von Service Bus-Themen mit Python | Microsoft Azure" 
    description="Informationen Sie zum Verwenden von Azure Service Bus Topics und Abonnements von Python." 
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
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Zum Service Bus Topics und Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieser Artikel beschreibt, wie Service Bus-Themen und Abonnements. Die Proben werden Python geschrieben und [Python Azure-Paket][]verwenden. Die Szenarios enthalten **Themen und Abonnements erstellen**, **Abonnement Filter** **Nachrichten zu einem Thema**, **Empfangen von Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements**. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt [Weiter](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Hinweis:** Wenn Sie Python oder [Python Azure-Paket][]installieren möchten, finden Sie unter [Python-Installationshandbuch](../python-how-to-install.md).

## <a name="create-a-topic"></a>Erstellen eines Themas

Das **ServiceBusService** -Objekt können Sie mit Themen arbeiten. Fügen Sie am oberen Rand jeder Service Bus programmgesteuerten Zugriff auf gewünschte Python-Datei Folgendes:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Der folgende Code erstellt ein **ServiceBusService** -Objekt. Ersetzen Sie `mynamespace`, `sharedaccesskeyname`, und `sharedaccesskey` mit den aktuellen Namespace Shared Access Signatur (SAS) Schlüssel Namen und Schlüssel-Wert.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Die Werte für den SAS Schlüsselnamen und den Wert erhalten der [Azure-Portal][]Sie.

```
bus_service.create_topic('mytopic')
```

**Erstellen\_Thema** unterstützt auch zusätzliche Optionen Thema Standardeinstellungen maximale Thema Größe oder Nachrichtzeit überschreiben können. Im folgende Beispiel wird die maximale Thema Größe 5 GB und nacheinander live (TTL) für 1 Minute:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Abonnements erstellen

Abonnements für Themen werden auch mit dem **ServiceBusService** -Objekt erstellt. Abonnements können werden benannt und einen optionalen Filter, der das Abonnement virtuelle Warteschlange übermittelt Nachrichten beschränkt.

> [AZURE.NOTE] Abonnements bleiben und weiterhin bis sie oder Thema, die sie abonniert haben, werden gelöscht.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit der Standardfilter (MatchAll)

**MatchAll** Filter ist der Filter, der verwendet wird, kein Filter festgelegt wird, wenn ein neues Abonnement erstellt wird. Bei der **MatchAll** Filter befinden alle veröffentlichte Nachrichten zu dem Thema virtuelle Warteschlange für das Abonnement. Im folgenden Beispiel wird ein Abonnement mit dem Namen "AllMessages" erstellt und den **MatchAll** Filter verwendet.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Abonnements mit Filtern erstellen

Sie können auch Filter, mit denen Sie angeben, welche Nachrichten zu einem Thema sollte innerhalb eines bestimmten Themas Abonnements angezeigt.

Die Flexibilität von Abonnements unterstützt Filter ist ein **SqlFilter**, das wenigen SQL92 implementiert. SQL-Filter werden für die Eigenschaften der Nachrichten, die das Thema veröffentlicht werden. Weitere Informationen zu Ausdrücken, die mit SQL-Filter finden Sie unter [SqlFilter.SqlExpression][] Syntax.

Ein Abonnement Filter hinzufügen möchten, mithilfe der **Erstellen\_Regel** Methode des **ServiceBusService** -Objekts. Diese Methode können Sie ein vorhandenes Abonnement neue Filter hinzufügen.

> [AZURE.NOTE] Da der Filter automatisch auf alle neuen Abonnements angewendet wird, entfernen Sie zuerst den Standardfilter oder **MatchAll** überschreiben alle Filter, die Sie angeben können. Entfernen die Standard-Regel mithilfe der **Löschen\_Regel** Methode des **ServiceBusService** -Objekts.

Im folgenden Beispiel wird ein Abonnement mit dem Namen `HighMessages` mit **SqlFilter** , die nur Nachrichten ausgewählt, die eine benutzerdefinierte **Messagenumber** -Eigenschaft größer als 3:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Im folgenden Beispiel wird auch ein Abonnement mit dem Namen `LowMessages` mit **SqlFilter** , die nur Nachrichten ausgewählt, die eine **Messagenumber** -Eigenschaft kleiner oder gleich 3:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Wenn eine Nachricht an `mytopic` wird immer **AllMessages** Thema Abonnement abonniert und selektiv Empfänger abonniert die **HighMessages** und **LowMessages** Thema Abonnements (abhängig vom Inhalt Nachricht) an Empfänger übermittelt.

## <a name="send-messages-to-a-topic"></a>Nachrichten zu einem Thema

Um eine Nachricht zu einem Thema Service Bus Ihrer Anwendung muss die **Senden\_Thema\_Nachricht** Methode des **ServiceBusService** -Objekts.

Im folgenden Beispiel wird veranschaulicht, wie fünf Testnachrichten, senden `mytopic`. Hinweis Die Iteration der Schleife **Messagenumber** Eigenschaftswert jeder Nachricht variieren (Dies bestimmt, welche Abonnements erhalten):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB in [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einem Thema jedoch gibt die Gesamtgröße der Nachrichten durch ein Thema ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung mit einem oberen Grenzwert von 5 GB definiert. Weitere Informationen zu Kontingenten finden Sie unter [Service Bus Kontingente][].

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten über ein Abonnement

Nachrichten aus einem Abonnement mithilfe der **empfangen\_Abonnement\_Nachricht** Methode des **ServiceBusService** -Objekts:

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Nachrichten werden aus dem Abonnement gelöscht, wenn gelesen werden die Parameter **Blick\_Sperren** auf **False**festgelegt ist. Lesen (Peek) und die Nachricht ohne durch Festlegen des Parameters aus der Warteschlange löschen Sperren **Blick\_Sperren** auf **True**.

Das Verhalten lesen und Löschen der Nachricht als Teil der Receive-Methode ist die einfachste und am besten für Szenarios, in denen eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus die Nachricht als verbraucht markiert haben wird, wird dann die Anwendung neu gestartet und beginnt, Nachrichten erneut es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Wenn die **Blick\_Sperren** -Parameter auf **True**festgelegt ist, erhalten wird ein zweistufiger Vorgang unterstützt Clientanwendungen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nachdem die Anwendung beendet Verarbeitung der Nachricht (oder zuverlässig zur späteren Verarbeitung speichert), abgeschlossen die zweite Phase des Prozesses empfangen durch Aufrufen der **delete** -Methode des Objekts **angezeigt** wird. Die **delete** -Methode markiert die Nachricht als verbraucht und aus dem Abonnement entfernt.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendungsabstürzen und unlesbar Nachrichten

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Ist eine empfängeranwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht können sie die **unlock** -Methode im **Message** -Objekt aufrufen. Dadurch Service Bus zu entsperren der Nachricht innerhalb des Abonnements zur wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht in das Abonnement gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Timeout für die Sperre abläuft (z. B. wenn die Anwendung stürzt ab), und Service Bus Nachricht automatisch entsperrt Verfügung erneut empfangen werden.

Die Anwendung stürzt nach dem Verarbeiten der Nachricht, aber bevor die **delete** -Methode aufgerufen wird, wird dann die Meldung an die Anwendung zurückgeliefert werden beim Neustart. Wird oft genannt **Mindestens einmal verarbeiten**, d. h. jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler ihre Anwendung doppelte Nachrichtenübermittlung behandeln zusätzlichen Logik hinzufügen. Dies geschieht häufig mithilfe der **MessageId** -Eigenschaft der Nachricht über Übermittlungsversuche konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Themen und Abonnements löschen

Themen und Abonnements sind beständig und müssen über das [Azure-Portal][] oder programmgesteuert explizit gelöscht werden. Im folgenden Beispiel wird veranschaulicht, wie das Thema löschen `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Ein Thema löschen auch Abonnements mit dem Thema registriert sind. Abonnements können auch einzeln gelöscht werden. Der folgende Code zeigt ein Abonnement mit dem Namen löschen `HighMessages` aus der `mytopic` Thema:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Service Bus Topics bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

-   [Warteschlangen, Themen und Abonnements][]anzeigen
-   Referenz für [SqlFilter.SqlExpression][].

[Azure-portal]: https://portal.azure.com
[Python Azure-Paket]: https://pypi.python.org/pypi/azure  
[Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Service Bus Kontingente]: service-bus-quotas.md 
