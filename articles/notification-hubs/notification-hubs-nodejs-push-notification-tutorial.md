<properties
    pageTitle="Pushbenachrichtigungen mit Azure Benachrichtigung und Node.js senden"
    description="Erfahren Sie mit Notification Hubs Pushbenachrichtigungen Node.js-Anwendung senden."
    keywords="Push-Benachrichtigung Push notifications,node.js Push Ios push"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Pushbenachrichtigungen mit Azure Benachrichtigung und Node.js senden
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>Übersicht

> [AZURE.IMPORTANT] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Dieses Handbuch wird direkt aus einer Anwendung Node.js Pushbenachrichtigungen mit Hilfe der Azure Benachrichtigung senden angezeigt. 

Die Szenarios umfassen das Senden von Pushbenachrichtigungen Clientanwendungen auf den folgenden Plattformen:

* Android
* iOS
* Windows Phone
* Universelle Windows-Plattform 

Weitere Informationen zu Notification Hubs finden Sie im Abschnitt [Weiter](#next) .

##<a name="what-are-notification-hubs"></a>Was sind Benachrichtigungshubs?

Azure Notification Hubs bieten eine einfach zu verwendende, Multi-Plattform und skalierbare Infrastruktur für Pushbenachrichtigungen an mobile Geräte senden. Finden Sie ausführliche Informationen über die Infrastruktur der [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) .

##<a name="create-a-nodejs-application"></a>Erstellen Sie eine Node.js-Anwendung

Der erste Schritt in diesem Lernprogramm wird eine neue leere Node.js-Anwendung erstellen. Informationen zum Erstellen einer Anwendung Node.js finden Sie [Erstellen und Bereitstellen einer Anwendung Node.js zu Azure-Website][nodejswebsite], [Node.js Cloud-Dienst] [ Node.js Cloud Service] mit Windows PowerShell oder [mit WebMatrix].

##<a name="configure-your-application-to-use-notification-hubs"></a>Konfigurieren Sie die Anwendung für Notification Hubs

Um Azure Notification Hubs verwenden, müssen Sie downloaden und verwenden das Node.js [Azure-Paket](https://www.npmjs.com/package/azure), das eine integrierte Hilfsbibliotheken enthält, die mit Push Notification REST-Dienste kommunizieren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden Sie das Paket zu Knoten Paket-Manager (NPM)

1.  Eine Befehlszeilenschnittstelle **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Linux) verwenden und den Ordner, in dem die leere Anwendung erstellt.

2.  Geben Sie im Befehlsfenster **Npm install Azure Sb** .

3.  Sie können manuell ausführen den Befehl **ls** oder **Dir** zu überprüfen, ob ein **Knoten\_Module** wurde erstellt. Suchen Sie in diesem Ordner **Azure** Paket enthält Bibliotheken, die Sie den Notification Hub müssen.

>[AZURE.NOTE] Sie können weitere Informationen zur Installation von NPM auf der offiziellen [NPM Blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 

### <a name="import-the-module"></a>Das Modul importieren

Verwenden einen Texteditor, fügen Sie Folgendes am Anfang der Datei **server.js** der Anwendung:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Ein Azure Notification Hub Verbindung

Das **NotificationHubService** -Objekt können Sie mit Notification arbeiten. Der folgende Code erstellt ein **NotificationHubService** -Objekt namens **Hubname**Meldung-Hub. Fügen sie oben in der Datei **server.js** nach der Anweisung Azure-Modul importieren:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

**Connectionstring** Verbindungswert [Azure-Portal] erhalten den folgenden Schritten:

1. Klicken Sie im linken Navigationsbereich auf **Durchsuchen**.

2. Wählen Sie **Notification Hubs**und finden Sie den Hub für das Beispiel verwenden möchten. Zum [Windows Store Einstieg Lernprogramm](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) finden Sie Hilfe zum Erstellen einer neuen Benachrichtigungshub benötigen.

3. **Wählen Sie.**

4. Klicken Sie auf **Richtlinien**. Sie sehen beide Verbindungszeichenfolgen freigegebenen und vollständigen Zugriff.

![Azure-Portal - Benachrichtigungshubs](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] Sie können auch mit **Get-AzureSbNamespace-** Cmdlet von [Azure PowerShell](../powershell-install-configure.md) oder den Befehl **show Azure Sb Namespace** mit der [Azure-Befehlszeilenschnittstelle (CLI Azure)](../xplat-cli-install.md)bereitgestellten Verbindungszeichenfolge abrufen.

##<a name="general-architecture"></a>Allgemeine Architektur

**NotificationHubService** -Objekt enthält die folgenden Instanzen für Push-Benachrichtigung an bestimmte Geräte und Programme senden:

* **Android** - verwenden Sie das **GcmService** -Objekt auf **notificationHubService.gcm**
* **iOS** - verwenden Sie das **ApnsService** -Objekt **notificationHubService.apns** zugänglich ist
* **Windows Phone** - verwenden Sie das **MpnsService** -Objekt auf **notificationHubService.mpns**
* **Universelle Windows-Plattform** - verwenden Sie das **WnsService** -Objekt auf **notificationHubService.wns**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Gewusst wie: Senden von Pushbenachrichtigungen Android Anwendung

Das Objekt **GcmService** Weise **Senden** , mit der Pushbenachrichtigungen Android Anwendung senden. Die **send** -Methode akzeptiert die folgenden Parameter:

* **Tags** - Transponder-IDs. Wenn kein Tag angegeben ist, wird die Benachrichtigung al-Clients gesendet.
* **Nutzlast** - JSON oder raw Zeichenfolge Nutzlast der Nachricht.
* **Rückruf** - Callback-Funktion.

Weitere Informationen über das Format Nutzlast finden Sie im Abschnitt **Nutzlast** des Dokuments [GCM-Server implementieren](http://developer.android.com/google/gcm/server.html#payload) .

Der folgende Code verwendet eine Push-Benachrichtigung an alle registrierten Clients senden **GcmService** Instanz von **NotificationHubService** verfügbar gemacht.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Gewusst wie: Senden von Pushbenachrichtigungen iOS-Anwendung

Dieselbe wie mit Android beschriebenen **ApnsService** Objekt **Senden** stellt eine Methode bereit, die zum Senden von Pushbenachrichtigungen iOS-Anwendung verwendet werden kann. Die **send** -Methode akzeptiert die folgenden Parameter:

* **Tags** - Transponder-IDs. Wenn kein Tag angegeben ist, wird die Benachrichtigung an alle Clients gesendet.
* **Nutzlast** - die Nachricht JSON oder Zeichenfolge Nutzlast.
* **Rückruf** - Callback-Funktion.

Weitere Informationen die Nutzlast-Format finden Sie in Abschnitt **Benachrichtigungsnutzlast** des [lokalen und Push Notification Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) .

Der folgende Code verwendet eine Warnung an alle Kunden senden **ApnsService** Instanz von **NotificationHubService** verfügbar gemacht werden:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Gewusst wie: Senden von Pushbenachrichtigungen Windows Phone-Anwendung

Das **MpnsService** -Objekt enthält eine **Senden** -Methode, die zum Senden von Pushbenachrichtigungen Windows Phone-Anwendung verwendet werden kann. Die **send** -Methode akzeptiert die folgenden Parameter:

* **Tags** - Transponder-IDs. Wenn kein Tag angegeben ist, wird die Benachrichtigung an alle Clients gesendet.
* **Nutzlast** - XML-Nutzlast der Nachricht.
* **Zielname**  -  `toast` für Popupbenachrichtigungen. `token`für kachelbenachrichtigungen.
* **NotificationClass** - die Priorität der Benachrichtigung. Siehe Abschnitt **HTTP-Headerelementen** des Dokuments [Pushbenachrichtigungen von einem Server](http://msdn.microsoft.com/library/hh221551.aspx) auf gültige Werte.
* **Optionen** - optional Anforderungsheader.
* **Rückruf** - Callback-Funktion.

Eine Liste der gültigen Optionen **Zielname**, **NotificationClass** und Header findest [Pushbenachrichtigungen von einem Server](http://msdn.microsoft.com/library/hh221551.aspx) .

Der folgende Code verwendet ein Toast Push-Benachrichtigung **MpnsService** Instanz von **NotificationHubService** verfügbar gemacht:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Gewusst wie: Senden von Pushbenachrichtigungen Clientanwendungen universelle Windows-Plattform (UWP)

Das Objekt **WnsService** Weise **Senden** , die universelle Windows-Plattform Pushbenachrichtigungen an verwendet werden können.  Die **send** -Methode akzeptiert die folgenden Parameter:

* **Tags** - Transponder-IDs. Wenn kein Tag angegeben ist, wird die Benachrichtigung an alle registrierten Clients gesendet.
* **Nutzlast** - XML-Nachrichtennutzlast.
* **Typ** der Benachrichtigungstyp.
* **Optionen** - optional Anforderungsheader.
* **Rückruf** - Callback-Funktion.

[Push Notification Service-Anforderung und Antwort-Headern](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx)finden Sie eine Liste der gültigen Typen und Anforderungsheader.

Der folgende Code verwendet eine UWP app eine Push-Benachrichtigung Spruch an **WnsService** Instanz von **NotificationHubService** verfügbar gemacht werden:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Nächste Schritte

Die obigen Beispiel Ausschnitte können Sie problemlos Infrastruktur zum Übermitteln von Pushbenachrichtigungen an eine Vielzahl von Geräten. Die Grundlagen von node.js Benachrichtigungshubs mit bearbeitet haben, folgen Sie diesen Links erfahren Sie, wie Sie diese Funktionen weiter erweitern können.

-   Finden Sie im MSDN [Azure Notification](https://msdn.microsoft.com/library/azure/jj927170.aspx)Hubs.
-   Weitere Beispiele und Implementierungsdetails finden Sie auf [Azure SDK für Knoten] Repository auf GitHub.

  [Azure SDK für Knoten]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Website mit WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Azure-Portal]: https://portal.azure.com
