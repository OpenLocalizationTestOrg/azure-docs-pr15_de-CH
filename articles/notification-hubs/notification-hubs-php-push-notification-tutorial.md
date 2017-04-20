<properties 
    pageTitle="Verwendung von Notification Hubs mit PHP" 
    description="Informationen Sie zum Azure Notification Hubs ein PHP-Backend verwenden." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Wie Sie Notification Hubs von PHP
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Sie können Java, PHP/Ruby Back-End mit der Benachrichtigung Hub REST-Schnittstelle wie der MSDN [Benachrichtigung Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx)beschrieben alle Notification Hubs Features zugreifen.

In diesem Thema zeigen wir wie:

* Erstellen eines weiteren Clients Benachrichtigungshubs Features in PHP;
* Führen Sie die [Get gestartet Lernprogramm](notification-hubs-ios-apple-push-notification-apns-get-started.md) für Ihre mobilen Plattform, Implementieren von Back-End-Bereich in PHP.

## <a name="client-interface"></a>Client-Schnittstelle
Die grundlegende Schnittstelle bieten dieselben Methoden in [.NET Benachrichtigung Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx)verfügbar sind, dadurch können Sie die Lernprogramme und Beispiele, die auf dieser Website derzeit direkt zu übersetzen und von der Gemeinschaft im Internet.

Gesamten Code in [PHP REST Wrapper Beispiel]finden.

Wenn Sie beispielsweise einen Client erstellen:

    $hub = new NotificationHub("connection string", "hubname"); 

IOS-Betriebssystemen systemeigenen Benachrichtigung:
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementierung
Wenn Sie dies nicht bereits geschehen, führen Sie unsere [Get gestartet Lernprogramm] bis zum letzten Abschnitt Sie Back-End implementieren müssen.
Auch wenn Sie möchten, können Sie mithilfe der [PHP-REST Wrapper Beispiel] und direkt zum Abschnitt [des Lernprogramms abgeschlossen](#complete-tutorial) .

Die Details einen vollständigen REST Wrapper implementieren finden Sie auf [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). In diesem Abschnitt beschreiben wir PHP-Implementierung der wichtigsten Schritte Benachrichtigung Hubs REST-Endpunkte auf:

1. Analysieren der Verbindungszeichenfolge
2. Das Authentifizierungstoken generieren
3. HTTP-Aufruf auszuführen

### <a name="parse-the-connection-string"></a>Analysieren der Verbindungszeichenfolge

Hier ist die Hauptklasse Implementieren des Clients, deren Konstruktor, der die Zeichenfolge analysiert:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Sicherheitstoken erstellen
Token Erstellen des Details finden Sie [hier](http://msdn.microsoft.com/library/dn495627.aspx).
Die folgende Methode hat der **NotificationHub** -Klasse erstellt basierte auf dem URI, der die aktuelle Anforderung und die Anmeldeinformationen aus der Verbindungszeichenfolge extrahiert Token hinzugefügt werden.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Benachrichtigung senden
Zuerst definieren Sie wir eine Klasse, die eine Benachrichtigung darstellt.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Diese Klasse ist ein Container für eine Stelle systemeigene Benachrichtigung oder eine Reihe von Eigenschaften auf eine vorlagenbenachrichtigung und mehrere Header enthält plattformspezifische Eigenschaften (wie Apple Ablaufdatum und WNS-Header) und Format (systemeigene Plattform oder Vorlage).

Finden Sie in der [Benachrichtigung Hubs REST APIs Dokumentation](http://msdn.microsoft.com/library/dn495827.aspx) und die Benachrichtigung Plattformen Formate für alle verfügbaren Optionen.

Ausgestattet mit dieser Klasse können wir jetzt senden Benachrichtigungsmethoden innerhalb der **NotificationHub** -Klasse schreiben.

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Methoden senden HTTP POST-Anforderung an /messages Endpunkt des Notification Haupt, mit den richtigen Header der Benachrichtigung.

##<a name="complete-tutorial"></a>Arbeiten Sie das Lernprogramm
Jetzt können Sie das erste Schritte-Lernprogramm durch Senden der Benachrichtigung von PHP Back-End abschließen.

Initialisieren Sie den Client Benachrichtigungshubs (Ersatz der Verbindungsname Zeichenfolge und Hub wie [Get gestartet Lernprogramm]):

    $hub = new NotificationHub("connection string", "hubname"); 

Fügen Sie je nach Ziel mobilen Plattform Code senden.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store und Windows Phone 8.1 (nicht Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 und 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Feuer Anzünden
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

PHP-Code ausführen soll nun eine Benachrichtigung auf Ihrem Zielgerät.


## <a name="next-steps"></a>Nächste Schritte
In diesem Thema werden zum Erstellen eines einfachen Java REST Clients für Notification Hubs vorgestellt. Hier können Sie:

* Downloaden Sie das vollständige [PHP REST Wrapper Beispiel]den obige Code enthält.
* Lernen Sie über Benachrichtigungshubs tagging Feature [Breaking News Lernprogramms]
* Erfahren Sie mehr über Benachrichtigungen für einzelne Benutzer [Benutzer benachrichtigen Tutorial] drücken

Weitere Informationen finden Sie auch die [PHP-Entwicklercenter](/develop/php/).

[PHP REST Wrapper-Beispiel]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Gestartete Get-Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
