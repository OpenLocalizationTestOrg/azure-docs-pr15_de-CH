<properties 
    pageTitle="Verwendung von Service Bus-Themen mit PHP | Microsoft Azure" 
    description="Erfahren Sie, wie PHP in Azure Service Bus-Themen mit." 
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
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Zum Service Bus Topics und Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieser Artikel beschreibt, wie Service Bus-Themen und Abonnements. Die Beispiele in PHP geschrieben und [Azure SDK für PHP](../php-download-sdk.md)verwenden. Die Szenarios enthalten **Themen und Abonnements erstellen**, **Abonnement Filter** **Nachrichten zu einem Thema**, **Empfangen von Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Erstellen Sie eine PHP-Anwendung

Voraussetzung für das Erstellen einer PHP-Anwendung, die Azure BLOB-Dienst zugreift, werden Klassen in [Azure SDK für PHP](../php-download-sdk.md) aus im Code verweisen. Alle Entwicklungstools können Sie Ihre Anwendung oder Editor erstellen.

> [AZURE.NOTE] PHP-Installation muss auch die [OpenSSL-Erweiterung](http://php.net/openssl) installiert und aktiviert haben.

Dieser Artikel beschreibt die Funktionen verwenden, die innerhalb einer PHP-Anwendung lokal oder in einer Webrolle Azure Worker-Rolle oder Website Code aufgerufen werden kann.

## <a name="get-the-azure-client-libraries"></a>Erhalten von Azure Client-Bibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Sie die Anwendung für Service Bus

Service Bus-APIs verwenden:

1. Die Autoloader Datei [per] Verweis[ require-once] Anweisung.
2. Verweisen auf alle Klassen, die Sie verwenden können.

Im folgenden Beispiel wird veranschaulicht, wie Includedatei Autoloader und die **ServiceBusService** -Klasse.

> [AZURE.NOTE] In diesem Beispiel (und andere Beispiele in diesem Artikel) wird angenommen, dass PHP-Clientbibliotheken für Azure über Composer installiert haben. Wenn die Bibliotheken manuell oder als PEAR-Paket installiert, müssen Sie die Datei **WindowsAzure.php** Autoloader verweisen.

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In den folgenden Beispielen der `require_once` -Anweisung wird immer angezeigt, aber nur die notwendigen Klassen für das Beispiel ausgeführt wird.

## <a name="set-up-a-service-bus-connection"></a>Service Bus Verbindung

Um einen Client Service Bus instanziieren müssen Sie zunächst eine gültige Verbindungszeichenfolge in diesem Format:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Wo `Endpoint` ist in der Regel das Format `https://[yourNamespace].servicebus.windows.net`.

Azure Webdienstclient erstellen müssen Sie die **ServicesBuilder** -Klasse verwenden. Sie können:

* Übergeben Sie die Verbindungszeichenfolge direkt.
* Verwenden Sie **CloudConfigurationManager (CCM)** externe Quellen für die Verbindungszeichenfolge zu überprüfen:
    * Standardmäßig kommt es mit Unterstützung für eine externe Quelle - Umgebungsvariablen.
    * Neue Datenquellen können durch Erweitern der **ConnectionStringSource** -Klasse.

Die hier aufgeführten Beispiele wird die Verbindungszeichenfolge direkt übergeben.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Erstellen eines Themas

Sie können Management Service Bus Themen über die Klasse **ServiceBusRestProxy** Operationen. Ein **ServiceBusRestProxy** -Objekt wird über die **ServicesBuilder::createServiceBusService** -Factory-Methode mit einer Verbindungszeichenfolge erstellt, die token Berechtigungen verwalten kapselt.

Im folgenden Beispiel wird veranschaulicht, wie instanziieren, **ServiceBusRestProxy** und **ServiceBusRestProxy -> CreateTopic** , um ein Thema erstellen `mytopic` in einem `MySBNamespace` Namespace:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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

> [AZURE.NOTE] Können die `listTopics` -Methode `ServiceBusRestProxy` Objekte zu überprüfen, ob eine Thema mit einem bestimmten Namen in einem Namespace Service bereits.

## <a name="create-a-subscription"></a>Erstellen eines Abonnements

Thema Abonnements werden auch mit der **ServiceBusRestProxy -> CreateSubscription** -Methode erstellt. Abonnements können werden benannt und einen optionalen Filter, der Nachrichten an das Abonnement virtuelle Warteschlange übergeben beschränkt.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen eines Abonnements mit der Standardfilter (MatchAll)

**MatchAll** Filter ist der Filter, der verwendet wird, kein Filter festgelegt wird, wenn ein neues Abonnement erstellt wird. Bei der **MatchAll** Filter befinden alle veröffentlichte Nachrichten zu dem Thema virtuelle Warteschlange für das Abonnement. Im folgenden Beispiel wird ein Abonnement mit dem Namen "Mysubscription" erstellt und den **MatchAll** Filter verwendet.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Abonnements mit Filtern erstellen

Sie können auch festlegen, mit denen Sie angeben, welche Nachrichten zu einem Thema sollten Filter innerhalb eines bestimmten Themas Abonnements angezeigt. Die flexibelste Art von Abonnements unterstützt Filter ist **SqlFilter**, das wenigen SQL92 implementiert. SQL-Filter werden für die Eigenschaften der Nachrichten, die das Thema veröffentlicht werden. Weitere Informationen zu SqlFilters finden Sie unter [SqlFilter.SqlExpression-Eigenschaft][sqlfilter].

> [AZURE.NOTE] Jede Regel ein Abonnement verarbeitet eingehende Nachrichten, das Abonnement ihrer Nachrichten Ergebnis hinzufügen. Darüber hinaus hat jedes neues Abonnement ein Standardobjekt **Regel** mit einem Filter, der alle Nachrichten aus dem Thema mit dem Abonnement hinzugefügt. Um nur die gefilterten Nachrichten empfangen, müssen Sie die Standardregel entfernen. Entfernen die Standard-Regel mithilfe der `ServiceBusRestProxy->deleteRule` Methode.

Das folgende Beispiel erstellt eine Subskription namens **HighMessages** mit **SqlFilter** , die nur Nachrichten, die eine benutzerdefinierte **MessageNumber** -Eigenschaft größer als 3 ausgewählt (Informationen finden Sie unter [Nachrichten zu einem Thema](#send-messages-to-a-topic) zum Hinzufügen von benutzerdefinierten Eigenschaften zu Nachrichten):

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Beachten Sie, dass dieser Code die Verwendung zusätzlicher Namespace erfordert: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Entsprechend wird im folgenden Beispiel wird ein Abonnement mit dem Namen **LowMessages** mit **SqlFilter** , die nur Nachrichten ausgewählt, die eine **MessageNumber** -Eigenschaft kleiner oder gleich 3 erstellt:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Wenn eine Nachricht an die `mytopic` Thema immer erfolgt an Empfänger abonniert die `mysubscription` Abonnement und selektiv Empfänger abonniert die `HighMessages` und `LowMessages` Abonnements (je nach Inhalt der Nachricht).

## <a name="send-messages-to-a-topic"></a>Nachrichten zu einem Thema

Um eine Nachricht zu einem Thema Service Bus Ruft die Anwendung die Methode **ServiceBusRestProxy -> SendTopicMessage** . Der folgende Code zeigt, wie Sie eine Nachricht senden der `mytopic` Thema erzeugten innerhalb der `MySBNamespace` Service-Namespace.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Nachrichten Service Bus-Themen sind Instanzen der Klasse **BrokeredMessage** . **BrokeredMessage** -Objekte haben standard Eigenschaften und Methoden (z. B. **GetLabel**, **GetTimeToLive**, **SetLabel**und **SetTimeToLive**) sowie Eigenschaften, benutzerdefinierte anwendungsspezifische Eigenschaften. Im folgenden Beispiel wird veranschaulicht, wie 5 Test Nachrichten an die `mytopic` Thema bereits erstellt. Die **SetProperty** -Methode wird verwendet, um eine benutzerdefinierte Eigenschaft hinzufügen (`MessageNumber`) jeder Nachricht. Beachten Sie, dass die `MessageNumber` Eigenschaft variiert für jede Nachricht (Sie können diesen Wert fest, welche Abonnements erhalten, wie im Abschnitt [Erstellen eines Abonnements](#create-a-subscription) ):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Service Bus-Themen unterstützen eine maximale Nachrichtengröße [Standard Tier](service-bus-premium-messaging.md) 256 KB und 1 MB in [Premium-Ebene](service-bus-premium-messaging.md). Der Header die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält können maximal 64 KB. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einem Thema jedoch gibt die Gesamtgröße der Nachrichten durch ein Thema ist. Diese Obergrenze für Thema ist 5 GB. Weitere Informationen zu Kontingenten finden Sie unter [Service Bus Kontingente][].

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten über ein Abonnement

Die beste Möglichkeit, ein Abonnement Nachrichten ist eine **ServiceBusRestProxy -> ReceiveSubscriptionMessage** -Methode verwenden. Nachrichten können in zwei verschiedenen Modi arbeiten: **ReceiveAndDelete** (Standard) und **PeekLock**.

Mit dem Modus **ReceiveAndDelete** empfangen ist ein Einzelbild Betrieb; also wenn Service Bus in einem Abonnement eine Leseoperation für eine Nachricht empfängt, es markiert die Nachricht als verbraucht wird und an die Anwendung zurückgegeben. **ReceiveAndDelete** ist das einfachste und am besten für Szenarios, in denen eine Anwendung tolerieren kann eine Meldung im Falle eines Fehlers nicht verarbeitet. Um dies zu verstehen, betrachten Sie ein Szenario der Verbraucher gibt die empfangsanforderung und stürzt vor der Verarbeitung. Da Service Bus die Nachricht als verbraucht markiert haben wird, wird dann die Anwendung neu gestartet und beginnt, Nachrichten erneut es die Meldung verpasst haben, die vor dem Absturz verbraucht wurde.

Im **PeekLock** -Modus wird das Empfangen einer Nachricht ein zweistufiger Vorgang unterstützt Clientanwendungen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Service Bus eine Anforderung empfängt, sucht nach der nächsten Nachricht verbraucht werden, sperren, um zu verhindern, dass andere Verbraucher empfangen und an die Anwendung zurückgibt. Nach die Anwendung Verarbeitung der Meldung beendet (oder zuverlässig zur späteren Verarbeitung speichert) abgeschlossen die zweite Phase des Prozesses empfangen die empfangene Nachricht zu **ServiceBusRestProxy DeleteMessage**übergeben wird. Service Bus rufen **DeleteMessage** sieht, wird die Nachricht als verbraucht und aus der Warteschlange entfernt.

Im folgenden Beispiel wird veranschaulicht, wie empfangen und Verarbeiten von Nachrichten mit **PeekLock** Modus (nicht die Standardeinstellung). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Gewusst wie: Behandeln von Anwendungsabstürzen und unlesbar Nachrichten

Service Bus bietet Funktionen zum ordnungsgemäß Fehler in Ihrer Anwendung oder Probleme bei der Verarbeitung einer Nachricht wiederherzustellen. Ist eine empfängeranwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht können sie die Methode **UnlockMessage** in der empfangenen Nachricht (nicht **DeleteMessage** ) aufrufen. Dadurch Service Bus zu entsperren der Nachricht in der Warteschlange zur wieder dieselbe verwendeten Anwendung oder einer anderen verwendeten Anwendung empfangen werden.

Auch ein Timeout einer Nachricht in der Warteschlange gesperrt ist und wenn die Anwendung zum Verarbeiten der Nachricht vor dem Timeout für die Sperre abläuft (z. B. wenn die Anwendung stürzt ab), Service Bus wird die Nachricht automatisch entsperrt und Verfügbarmachen erneut empfangen werden.

Die Anwendung stürzt nach dem Verarbeiten der Nachricht, aber vor Anforderung **DeleteMessage** , wird dann die Meldung an die Anwendung zurückgeliefert werden beim Neustart. Dies wird häufig **Mindestens einmal verarbeiten**bezeichnet; Jede Nachricht mindestens einmal verarbeitet jedoch in bestimmten Situationen dieselbe Nachricht kann neuzugestellt. Wenn das Szenario doppelte Verarbeitung tolerieren, sollten Entwickler Anwendung doppelte Nachrichtenübermittlung behandeln zusätzlichen Logik hinzufügen. Dies geschieht häufig mit der **GetMessageId** -Methode der Nachricht über Übermittlungsversuche konstant bleibt.

## <a name="delete-topics-and-subscriptions"></a>Themen und Abonnements löschen

Um ein Thema oder ein Abonnement zu löschen, verwenden Sie die **ServiceBusRestProxy -> DeleteTopic** oder **ServiceBusRestProxy -> DeleteSubscripton** -Methoden. Beachten Sie, dass ein Thema auch Abonnements gelöscht, die mit dem Thema registriert sind.

Im folgenden Beispiel wird veranschaulicht, wie ein Thema löschen `mytopic` und registrierte Abonnements.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Mithilfe der **DeleteSubscription** -Methode können Sie ein Abonnement einzeln löschen.

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Servicebuswarteschlangen bearbeitet haben, finden Sie weitere Informationen [Warteschlangen, Themen und Abonnements][] .

[Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Service Bus Kontingente]: service-bus-quotas.md
