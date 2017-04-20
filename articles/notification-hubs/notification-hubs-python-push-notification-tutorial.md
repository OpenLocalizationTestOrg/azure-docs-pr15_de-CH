<properties 
    pageTitle="Verwendung von Notification Hubs mit Python" 
    description="Informationen Sie zum Azure Notification Hubs aus einem Python-Backend verwenden." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Wie Sie Notification Hubs aus Python
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Sie können Java, PHP/Python-/ Ruby Back-End mit der Benachrichtigung Hub REST-Schnittstelle wie der MSDN [Benachrichtigung Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx)beschrieben alle Notification Hubs Features zugreifen.

> [AZURE.NOTE] Dies ist eine Implementierung des Verweis Beispiel für die Implementierung der Benachrichtigung sendet in Python und nicht offiziell unterstützte Benachrichtigungen Hub Python SDK.
>
> In diesem Beispiel werden Python 3.4 mit geschrieben.

In diesem Thema zeigen wir wie:

* REST-Client Benachrichtigungshubs Python-Features zu erstellen.
* Benachrichtigung über die Python-Benutzeroberfläche Benachrichtigung Hub REST APIs. 
* Ein Abbild der REST der HTTP-Anforderung/Antwort für debugging/Ausbildungszwecke abrufen. 

Sie können [Get gestartet Lernprogramm](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) für Ihre mobilen Plattform der Wahl folgen Implementieren von Back-End-Bereich in Python.

> [AZURE.NOTE] Umfang der Probe wird nur zum Senden von Benachrichtigungen, und keine Registrierung Management.

## <a name="client-interface"></a>Client-Schnittstelle
Die grundlegende Schnittstelle bieten dieselben Methoden, die im [.NET Benachrichtigung Hubs SDK](http://msdn.microsoft.com/library/jj933431.aspx)verfügbar sind. Dadurch können Sie die Lernprogramme und Beispiele, die auf dieser Website derzeit direkt zu übersetzen und von der Gemeinschaft im Internet bereitgestellt.

Sie können den [REST Python Wrapper Beispiel]für Code finden.

Wenn Sie beispielsweise einen Client erstellen:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
So senden Sie eine Windows-Popupbenachrichtigung
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Implementierung
Wenn Sie dies nicht bereits geschehen, führen Sie unsere [Get gestartet Lernprogramm] bis zum letzten Abschnitt Sie Back-End implementieren müssen.

Die Details einen vollständigen REST Wrapper implementieren finden Sie auf [MSDN](http://msdn.microsoft.com/library/dn530746.aspx). In diesem Abschnitt beschreiben wir die Python-Implementierung der wichtigsten Schritte Benachrichtigung Hubs REST-Endpunkte und Benachrichtigung

1. Analysieren der Verbindungszeichenfolge
2. Das Authentifizierungstoken generieren
3. Benachrichtigung über HTTP REST-API

### <a name="parse-the-connection-string"></a>Analysieren der Verbindungszeichenfolge

Hier ist die Hauptklasse Implementieren des Clients, dessen Konstruktor die Verbindungszeichenfolge analysiert:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Sicherheitstoken erstellen
Token Erstellen des Details finden Sie [hier](http://msdn.microsoft.com/library/dn495627.aspx).
Die folgenden Methoden der **NotificationHub** -Klasse erstellt basierte auf dem URI, der die aktuelle Anforderung und die Anmeldeinformationen aus der Verbindungszeichenfolge extrahiert Token hinzugefügt werden müssen.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Benachrichtigung über HTTP REST-API
Erstens können mit definieren Sie eine Klasse, die eine Benachrichtigung darstellt.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Diese Klasse ist ein Container für eine systemeigene Benachrichtigung oder eine Reihe von Eigenschaften bei einer vorlagenbenachrichtigung mehrere Header enthält plattformspezifische Eigenschaften (wie Apple Ablaufdatum und WNS-Header) und Format (systemeigene Plattform oder Vorlage).

Finden Sie in der [Benachrichtigung Hubs REST APIs Dokumentation](http://msdn.microsoft.com/library/dn495827.aspx) und die Benachrichtigung Plattformen Formate für alle verfügbaren Optionen.

Jetzt können mit dieser Klasse senden Benachrichtigungsmethoden innerhalb der **NotificationHub** -Klasse schreiben.

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Methoden senden HTTP POST-Anforderung an /messages Endpunkt des Notification Haupt, mit den richtigen Header der Benachrichtigung.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Debug-Eigenschaft verwenden ausführliche Protokollierung
Debug-Eigenschaft beim Initialisieren der Hub Benachrichtigung aktivieren, detaillierte Informationen über die HTTP-Anforderung schreibt und Antwort Dump sowie detaillierte Benachrichtigung senden Ergebnis. Zuletzt hinzugefügten wir diese Eigenschaft namens [Benachrichtigung Hubs TestSend-Eigenschaft](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) die detaillierte Informationen über die Benachrichtigung senden Ergebnis zurückgibt. -Verwendung initialisieren Sie folgendermaßen:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Die Benachrichtigung Hub Anforderung HTTP-URL wird daher "Test" Querystring angehängt. 

##<a name="complete-tutorial"></a>Arbeiten Sie das Lernprogramm
Jetzt können Sie das erste Schritte-Lernprogramm durch Senden der Benachrichtigung aus einem Python-Backend abschließen.

Initialisieren Sie den Client Benachrichtigungshubs (Ersatz der Verbindungsname Zeichenfolge und Hub wie [Get gestartet Lernprogramm]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Fügen Sie je nach Ziel mobilen Plattform Code senden. Dieses Beispiel fügt auch höhere Methoden zum sendende von Benachrichtigungen basierend auf der Plattform z. B. Send_windows_notification für Windows aktivieren; Send_apple_notification (für Apple) usw.. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Store und Windows Phone 8.1 (nicht Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 und 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Feuer Anzünden
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Ausführen des Python-Codes soll eine Benachrichtigung auf Ihrem Zielgerät.

## <a name="examples"></a>Beispiele:

### <a name="enabling-debug-property"></a>Aktivieren von Debug-Eigenschaft
Wann können Sie Debugflag beim Initialisieren der NotificationHub, dann sehen Sie detaillierte HTTP-Anforderung und Antwort Dump sowie NotificationOutcome wie folgt, können Sie verstehen, welche HTTP-Header in der Anforderung übergeben werden und welche HTTP-Antwort vom Hub Benachrichtigung empfangen wurde:    ![][1]

Sehen Sie z.B. detaillierte Notification Hub-Ergebnis 

- Wenn die Nachricht Push Notification Service gesendet wird. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Wenn keine Ziele für jede Push-Benachrichtigung gefunden wurden dann soll wahrscheinlich Folgendes in der Antwort angezeigt (Dies bedeutet, dass keine Registrierung zum Übermitteln der Benachrichtigung wahrscheinlich hatte die Registrierung nicht übereinstimmende Tags gefunden wurden)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Broadcast-Popupbenachrichtigung Windows 

Beachten Sie die Header, die beim Senden von broadcast Popupbenachrichtigung Windows-Client, gesendet werden. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Benachrichtigung einen Tag (oder Tag-Ausdruck)

Beachten Sie den Tags-HTTP-Header der HTTP-Anforderung hinzugefügt wird (im folgenden Beispiel wir senden die Benachrichtigung nur auf Registrierung mit "Sport")

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Benachrichtigung mehrere Tags angeben

Beachten Sie, wie Tags HTTP-Header ändern, wenn mehrere Tags gesendet werden. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Vorlagen-Benachrichtigung

Hinweis Format http-Header ändert und der Nutzlast wird als Teil des HTTP-Anforderungstexts gesendet:

**Clientseitig - registrierten Vorlage**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Serverseitige - Nutzlast senden**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Nächste Schritte
In diesem Thema vorgestellt wie ein einfaches Python REST für Notification Hubs erstellen. Hier können Sie:

* Downloaden Sie das vollständige [Python REST Wrapper Beispiel]den obige Code enthält.
* Lernen Sie über Benachrichtigungshubs tagging Feature [Breaking News-Lernprogramm]
* [Lokalisieren von News-Lernprogramm] lernen Hubs Benachrichtigungsvorlagen Feature fortsetzen

<!-- URLs -->
[Python REST Wrapper-Beispiel]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Gestartete Get-Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Breaking News-Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Lokalisieren von News-Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
