<properties 
    pageTitle="Verwendung Servicebuswarteschlangen PHP mit | Microsoft Azure" 
    description="Erfahren Sie in Azure Service Bus-Warteschlangen verwenden. Codebeispiele in PHP geschrieben." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Wie Servicebuswarteschlangen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dieses Handbuch veranschaulicht die Servicebuswarteschlangen verwenden. Die Beispiele in PHP geschrieben und [Azure SDK für PHP](../php-download-sdk.md)verwenden. Die Szenarios enthalten **Warteschlangen erstellen**, **Senden und Empfangen von Nachrichten**und **Warteschlangen löschen**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Erstellen Sie eine PHP-Anwendung

Voraussetzung für das Erstellen einer PHP-Anwendung, die Azure BLOB-Dienst zugreift, ist das Verweisen auf Klassen in [Azure SDK für PHP](../php-download-sdk.md) aus im Code. Alle Entwicklungstools können Sie Ihre Anwendung oder Editor erstellen.

> [AZURE.NOTE] PHP-Installation muss auch die [OpenSSL-Erweiterung](http://php.net/openssl) installiert und aktiviert haben.

In diesem Handbuch Verwenden Sie Funktionen, die von innerhalb einer PHP-Anwendung lokal oder in einer Webrolle Azure Worker-Rolle oder Website Code aufgerufen werden kann.

## <a name="get-the-azure-client-libraries"></a>Erhalten von Azure Client-Bibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Sie die Anwendung für Service Bus

Um die Service Bus-Warteschlange APIs verwenden, führen Sie folgende Schritte aus:

1. Die Autoloader Datei [per] Verweis[ require_once] Anweisung.
2. Verweisen auf alle Klassen, die Sie verwenden können.

Im folgenden Beispiel wird veranschaulicht, wie Includedatei Autoloader und die **ServicesBuilder** -Klasse.

> [AZURE.NOTE] In diesem Beispiel (und andere Beispiele in diesem Artikel) wird angenommen, dass PHP-Clientbibliotheken für Azure über Composer installiert haben. Wenn die Bibliotheken manuell oder als PEAR-Paket installiert, müssen Sie die Datei **WindowsAzure.php** Autoloader verweisen.

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In den folgenden Beispielen der `require_once` -Anweisung wird immer angezeigt, aber nur die notwendigen Klassen für das Beispiel ausgeführt wird.

## <a name="set-up-a-service-bus-connection"></a>Service Bus Verbindung

Um einen Client Service Bus zu instanziieren, müssen Sie zunächst eine gültige Verbindungszeichenfolge in diesem Format:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

**Endpunkt** ist normalerweise das Format `[yourNamespace].servicebus.windows.net`.

Azure Webdienstclient erstellen müssen Sie die **ServicesBuilder** -Klasse verwenden. Sie können:

* Übergeben Sie die Verbindungszeichenfolge direkt.
* Verwenden Sie **CloudConfigurationManager (CCM)** externe Quellen für die Verbindungszeichenfolge zu überprüfen:
    * Standardmäßig bietet es Unterstützung für eine externe Quelle - Umgebungsvariablen
    * Neue Datenquellen können durch Erweitern der **ConnectionStringSource** -Klasse

Die hier aufgeführten Beispiele wird die Verbindungszeichenfolge direkt übergeben.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Gewusst wie: Erstellen einer Warteschlange

Verwaltungsvorgänge für Servicebuswarteschlangen über die **ServiceBusRestProxy** -Klasse möglich. Ein **ServiceBusRestProxy** -Objekt wird über die **ServicesBuilder::createServiceBusService** -Factory-Methode mit einer Verbindungszeichenfolge erstellt, die token Berechtigungen verwalten kapselt.

Im folgenden Codebeispiel wird veranschaulicht, wie ein **ServiceBusRestProxy** instanziiert und rufen Sie zum Erstellen einer Warteschlange mit dem Namen **ServiceBusRestProxy -> CreateQueue** `myqueue` in einem `MySBNamespace` Service-Namespace:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] Können die `listQueues` -Methode `ServiceBusRestProxy` Objekte zu überprüfen, ob eine Warteschlange mit einem bestimmten Namen in einem Namespace bereits vorhanden ist.

## <a name="how-to-send-messages-to-a-queue"></a>Gewusst wie: Senden von Nachrichten an eine Warteschlange

Zum Senden einer Nachricht an eine Warteschlange Service Bus Ruft die Anwendung die Methode **ServiceBusRestProxy -> SendQueueMessage** . Der folgende Code zeigt, wie Sie eine Nachricht senden der `myqueue` Warteschlange zuvor erstellte innerhalb der `MySBNamespace` Service-Namespace.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Nachrichten sind an (und erhielt von) Servicebuswarteschlangen Instanzen der Klasse **BrokeredMessage** . **BrokeredMessage** -Objekte haben eine Reihe von Standardmethoden (wie **GetLabel**, **GetTimeToLive**, **SetLabel**und **SetTimeToLive**) und Eigenschaften, mit denen anwendungsspezifische Datenfelder, und eine beliebige Daten enthalten.

Servicebuswarteschlangen unterstützt eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB im [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Gibt es keine Beschränkung für die Anzahl der Nachrichten in einer Warteschlange, jedoch ist es die Gesamtgröße der Nachrichten von einer Warteschlange. Diese Obergrenze auf Größe ist 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Nachrichten aus einer Warteschlange empfangen

Die beste Möglichkeit, Nachrichten aus einer Warteschlange empfangen werden **ServiceBusRestProxy -> ReceiveQueueMessage** -Methode verwendet. Nachrichten können in zwei verschiedenen Modi: **ReceiveAndDelete** (Standard) und **PeekLock**.

Erhalten mit dem Modus **ReceiveAndDelete** ist ein Betrieb Einzelbild - also wenn Service Bus eine Leseoperation für eine Nachricht in einer Warteschlange empfängt, es markiert die Nachricht als verbraucht und an die Anwendung zurückgegeben. **ReceiveAndDelete** ist das einfachste und am besten für Szenarios, in denen eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus die Nachricht als verbraucht markiert haben wird, wird dann die Anwendung neu gestartet und beginnt, Nachrichten erneut es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Im **PeekLock** -Modus wird das Empfangen einer Nachricht ein zweistufiger Vorgang unterstützt Clientanwendungen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nach die Anwendung Verarbeitung der Meldung beendet (oder zuverlässig zur späteren Verarbeitung speichert) abgeschlossen die zweite Phase des Prozesses empfangen die empfangene Nachricht zu **ServiceBusRestProxy DeleteMessage**übergeben wird. Service Bus rufen **DeleteMessage** sieht, wird die Nachricht als verbraucht und aus der Warteschlange entfernt.

Das folgende Beispiel zeigt, wie eine Nachricht empfangen und verarbeitet **PeekLock** Modus (nicht die Standardeinstellung).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Gewusst wie: Behandeln von Anwendungsabstürzen und unlesbar Nachrichten

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Ist eine empfängeranwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht können sie die Methode **UnlockMessage** in der empfangenen Nachricht (nicht **DeleteMessage** ) aufrufen. Dadurch Service Bus zu entsperren der Nachricht in der Warteschlange zur wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht in der Warteschlange gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Timeout für die Sperre abläuft (z. B. wenn die Anwendung stürzt ab), Service Bus wird die Nachricht automatisch entsperrt und Verfügbarmachen erneut empfangen werden.

Die Anwendung stürzt nach dem Verarbeiten der Nachricht, aber vor Anforderung **DeleteMessage** , wird dann die Meldung an die Anwendung zurückgeliefert werden beim Neustart. Dies wird häufig **Mindestens einmal verarbeiten**bezeichnet; Jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Szenario doppelte Verarbeitung tolerieren, ist zusätzlichen Logik für Anträge auf doppelte Nachrichtenübermittlung behandeln hinzufügen sollten. Dies geschieht häufig mit der **GetMessageId** -Methode der Nachricht über Übermittlungsversuche konstant bleibt.

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Servicebuswarteschlangen bearbeitet haben, finden Sie weitere Informationen [Warteschlangen, Themen und Abonnements][] .

Weitere Informationen finden Sie auch im [PHP Developer Center](/develop/php/).

[Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
