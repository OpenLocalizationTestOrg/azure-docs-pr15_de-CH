<properties 
    pageTitle="Verwendung von Service Bus-Themen mit Node.js | Microsoft Azure" 
    description="Informationen Sie zum Service Bus Topics und Abonnements in Azure Node.js-App verwenden." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Mit Service Bus-Themen zu Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieses Handbuch beschreibt das Service Bus-Themen und Abonnements Node.js-Anwendung verwenden. Die Szenarios enthalten **Themen und Abonnements erstellen**, **Abonnementfilter erstellen**, **Nachrichten** an ein Thema **Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements**. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt [Weiter](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Erstellen Sie eine Node.js-Anwendung

Erstellen einer leeren Node.js-Anwendung. Informationen zum Erstellen einer Anwendung Node.js finden Sie [Erstellen und Bereitstellen einer Anwendung Node.js eine Azure-Website], [Node.js-Clouddienst][] WebMatrix mit Windows PowerShell oder Website.

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Sie die Anwendung für Service Bus

Verwendung von Service Bus, herunterladen Sie Node.js Azure-Paket Dieses Paket umfasst eine Reihe von Bibliotheken, die mit den Service Bus REST-Diensten kommunizieren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden Sie das Paket zu Knoten Paket-Manager (NPM)

1.  Eine Befehlszeilenschnittstelle verwenden wie **PowerShell** (Windows) **Terminal** (Mac) oder **Bash** (Unix), navigieren Sie zu dem Ordner, in denen eine beispielanwendung erstellt.

2.  Geben Sie im Befehlsfenster die folgende Ausgabe wird **Npm Azure installieren** :

    ```
        azure@0.7.5 node_modules\azure
    ├── dateformat@1.0.2-1.2.3
    ├── xmlbuilder@0.4.2
    ├── node-uuid@1.2.0
    ├── mime@1.2.9
    ├── underscore@1.4.4
    ├── validator@1.1.1
    ├── tunnel@0.0.2
    ├── wns@0.5.3
    ├── xml2js@0.2.7 (sax@0.5.2)
    └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3.  Sie können manuell ausführen des Befehls **ls** überprüfen, ob ein **Knoten\_Module** wurde erstellt. Suchen Sie in diesem Ordner **Azure** Paket enthält die Bibliotheken auf Service Bus Topics müssen.

### <a name="import-the-module"></a>Das Modul importieren

Mit dem Editor oder einem anderen Text-Editor, fügen Sie Folgendes am Anfang der Datei **server.js** der Anwendung:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Service Bus Verbindung

Azure-Modul liest die Umgebungsvariablen AZURE\_SERVICEBUS\_NAMESPACE und AZURE\_SERVICEBUS\_Zugang\_für die Verbindung zum Service Bus erforderlich. Wenn diese Umgebungsvariablen nicht festgelegt werden, geben Sie die Kontoinformationen, wenn **CreateServiceBusService**aufgerufen.

Ein Beispiel für die Umgebungsvariable in einer Konfigurationsdatei Azure Cloud Service finden Sie unter [Node.js Cloud-Dienst mit][].

Beispielsweise der Umgebungsvariable im [klassischen Azure-Portal][] eine Azure-Website finden Sie unter [Node.js Web Application mit][].

## <a name="create-a-topic"></a>Erstellen eines Themas

Das **ServiceBusService** -Objekt können Sie mit Themen arbeiten. Der folgende Code erstellt ein **ServiceBusService** -Objekt. Fügen sie oben in der Datei **server.js** nach der Anweisung Azure-Modul importieren:

```
var serviceBusService = azure.createServiceBusService();
```

**CreateTopicIfNotExists** für das **ServiceBusService** -Objekt, das angegebene Thema (falls vorhanden) zurückgegeben oder ein neues Thema mit dem angegebenen Namen erstellt. Der folgende Code verwendet **CreateTopicIfNotExists** erstellen oder das Thema 'Meinthema':

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**CreateServiceBusService** unterstützt auch zusätzliche Optionen Thema Standardeinstellungen maximale Thema Größe oder Nachrichtzeit überschreiben können. Im folgenden Beispiel wird die maximale Thema Größe 5 GB mit 1 Minute Leben:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filter

Optionale Filteroperationen können Operationen mit **ServiceBusService**angewendet werden. Filteroperationen kann Protokollierung automatisch wiederholen usw. enthalten. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

```
function handle (requestOptions, next)
```

Nach Anforderung Optionen preprocessing durchführen, ruft die Methode `next` übergibt einen Rückruf mit der folgenden Signatur:

```
function (returnObject, finalCallback, next)
```

In diesem Rückruf und nach der Verarbeitung der **ReturnObject** (die Antwort auf die Anforderung an den Server) rufen den Rückruf muss entweder weiter ggf. um andere Filter verarbeitet oder einfach **FinalCallback** sonst zum Ende der Dienstaufruf aufrufen.

Zwei Filter, die Wiederholungslogik implementieren enthaltenen Azure SDK Node.js, **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**. Die folgenden erstellt ein **ServiceBusService** -Objekt, das **ExponentialRetryPolicyFilter**verwendet:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Abonnements erstellen

Thema Abonnements werden auch mit dem **ServiceBusService** -Objekt erstellt. Abonnements können werden benannt und einen optionalen Filter, der das Abonnement virtuelle Warteschlange übermittelt Nachrichten beschränkt.

> [AZURE.NOTE] Abonnements bleiben und weiterhin bis entweder sie oder das Thema sie sind, werden gelöscht. Enthält anwendungsspezifische Logik ein Abonnement erstellen, wird zunächst überprüft, wenn das Abonnement bereits vorhanden ist, mit der **GetSubscription** -Methode.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit der Standardfilter (MatchAll)

**MatchAll** Filter ist der Filter, der verwendet wird, kein Filter festgelegt wird, wenn ein neues Abonnement erstellt wird. Bei der **MatchAll** Filter befinden alle veröffentlichte Nachrichten zu dem Thema virtuelle Warteschlange für das Abonnement. Im folgenden Beispiel wird ein Abonnement mit dem Namen "AllMessages" erstellt und den **MatchAll** Filter verwendet.

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Abonnements mit Filtern erstellen

Sie können auch Filter erstellen, mit die Hilfe Sie Bereich Nachrichten an ein Thema innerhalb eines bestimmten Themas Abonnements anzeigen soll.

Die flexibelste Art von Abonnements unterstützt Filter ist **SqlFilter**, das wenigen SQL92 implementiert. SQL-Filter werden für die Eigenschaften der Nachrichten, die das Thema veröffentlicht werden. Für Weitere Informationen zu den Ausdrücken, die mit SQL-Filter verwendet werden können, überprüfen [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] Syntax.

Filter können mithilfe der **CreateRule** -Methode des **ServiceBusService** -Objekts in ein Abonnement hinzugefügt werden. Diese Methode können Sie ein vorhandenes Abonnement neue Filter hinzufügen.

> [AZURE.NOTE] Da der Filter automatisch auf alle neuen Abonnements angewendet wird, entfernen Sie zuerst den Standardfilter oder **MatchAll** überschreiben alle Filter, die Sie angeben können. Sie können die Default-Regel mithilfe der **DeleteRule** -Methode des **ServiceBusService** -Objekts entfernen.

Im folgenden Beispiel wird ein Abonnement mit dem Namen `HighMessages` mit **SqlFilter** , die nur Nachrichten ausgewählt, die eine benutzerdefinierte **Messagenumber** -Eigenschaft größer als 3:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Im folgenden Beispiel wird auch ein Abonnement mit dem Namen `LowMessages` mit **SqlFilter** , die nur Nachrichten ausgewählt, die eine **Messagenumber** -Eigenschaft kleiner oder gleich 3:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Wenn eine Nachricht nun an gesendet `MyTopic`, es immer erhalten Empfänger abonniert die `AllMessages` Thema Abonnement und selektiv Empfänger abonniert die `HighMessages` und `LowMessages` Thema Abonnements (je nach Inhalt der Nachricht).

## <a name="how-to-send-messages-to-a-topic"></a>Senden von Nachrichten zu einem Thema

Um eine Nachricht zu einem Thema Service Bus muss die Anwendung die **SendTopicMessage** -Methode des **ServiceBusService** -Objekts verwenden.
Nachrichten Service Bus-Themen sind **BrokeredMessage** Objekte.
**BrokeredMessage** -Objekte haben eine Reihe von Standardeigenschaften (wie **Beschriftung** und **TimeToLive**) ein Wörterbuch mit benutzerdefinierten Anwendung spezifischen Eigenschaften sowie ein Körper Zeichenfolge. Eine Anwendung kann den Hauptteil der Nachricht durch einen Zeichenfolgenwert an **SendTopicMessage** übergeben und alle erforderlichen standard Eigenschaften mit Standardwerten aufgefüllt.

Im folgenden Beispiel wird veranschaulicht, wie fünf Nachrichten 'Meinthema' Testen senden. Hinweis Die Iteration der Schleife **Messagenumber** Eigenschaftswert jeder Nachricht variieren (Dies legt fest, welche Abonnements erhalten):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB in [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einem Thema jedoch gibt die Gesamtgröße der Nachrichten durch ein Thema ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung mit einem oberen Grenzwert von 5 GB definiert.

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten über ein Abonnement

Nachrichten werden aus einem Abonnement mit der **ReceiveSubscriptionMessage** -Methode für das Objekt **ServiceBusService** . Standardmäßig werden Nachrichten aus dem Abonnement gelöscht gelesen werden; Sie können jedoch lesen (Peek) und sperren, die Nachricht ohne Löschen aus der durch die optionalen Parameter **IsPeekLock** auf **true**festlegen.

Das Standardverhalten lesen und Löschen der Nachricht als Teil der Receive-Methode ist die einfachste und am besten für Szenarios, in denen eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus die Nachricht als verbraucht markiert haben wird, wird dann die Anwendung neu gestartet und beginnt, Nachrichten erneut es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Wenn der Parameter **IsPeekLock** auf **true**festgelegt ist, wird erhalten ein zweistufiger Vorgang unterstützt Clientanwendungen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt.
Nach Anwendung Verarbeitung der Meldung beendet (oder zuverlässig zur späteren Verarbeitung speichert) schließt es die zweite Phase des Prozesses empfangen durch **DeleteMessage** -Methode aufrufen und Bereitstellen der Nachricht als Parameter gelöscht werden. **DeleteMessage** -Methode die Nachricht als verbraucht und aus dem Abonnement entfernen.

Im folgenden Beispiel wird veranschaulicht, wie Nachrichten empfangen und verarbeitet **ReceiveSubscriptionMessage**. Im Beispiel zuerst und Löschen einer Nachricht aus der "LowMessages" und dann eine Meldung aus der 'HighMessages' mit **IsPeekLock** auf True festgelegt ist. Anschließend wird die Nachricht **DeleteMessage**gelöscht:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendungsabstürzen und unlesbar Nachrichten

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Ist eine empfängeranwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht können sie die **UnlockMessage** -Methode des **ServiceBusService** -Objekts aufrufen. Dadurch Service Bus zu entsperren der Nachricht innerhalb des Abonnements zur wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht in das Abonnement gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Timeout für die Sperre abläuft (z. B. wenn die Anwendung stürzt ab), und Service Bus Nachricht automatisch entsperrt Verfügung erneut empfangen werden.

Die Anwendung stürzt nach der Verarbeitung der Nachricht aber vor **DeleteMessage** , wird dann die Meldung an die Anwendung zurückgeliefert werden beim Neustart. Wird oft genannt **Mindestens einmal verarbeiten**, d. h. jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler ihre Anwendung doppelte Nachrichtenübermittlung behandeln zusätzlichen Logik hinzufügen. Dies geschieht häufig mithilfe der **MessageId** -Eigenschaft der Nachricht über Übermittlungsversuche konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Themen und Abonnements löschen

Themen und Abonnements sind beständig und müssen über das [klassische Azure-Portal][] oder programmgesteuert explizit gelöscht werden.
Im folgenden Beispiel wird veranschaulicht, wie das Thema löschen `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Ein Thema wird auch alle Abonnements löschen, die mit dem Thema registriert sind. Abonnements können auch einzeln gelöscht werden. Im folgenden Beispiel wird veranschaulicht, wie ein Abonnement mit dem Namen löschen `HighMessages` aus der `MyTopic` Thema:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Service Bus Topics bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

-   [Warteschlangen, Themen und Abonnements][]anzeigen
-   API-Referenz für [SqlFilter][].
-   Besuchen Sie GitHub Repository [Azure SDK für Knoten][] .

  [Azure SDK für Knoten]: https://github.com/Azure/azure-sdk-for-node
  [Azure-Verwaltungsportal]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js Cloud-Dienst]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Erstellen und Bereitstellen einer Anwendung Node.js eine Azure-Website]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud-Dienst mit]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Node.js Web-Anwendung mit]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 
