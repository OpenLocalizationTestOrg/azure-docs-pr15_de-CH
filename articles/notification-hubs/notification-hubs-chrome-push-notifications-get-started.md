<properties
    pageTitle="Push-Benachrichtigung Chrome Apps mit Azure Notification Hubs | Microsoft Azure"
    description="Informationen Sie zum Azure Notification Hubs senden Pushbenachrichtigungen Chrome App."
    services="notification-hubs"
    keywords="Mobile Pushbenachrichtigungen Pushbenachrichtigungen Pushbenachrichtigung Chrom Pushbenachrichtigungen"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Chrome apps Azure Notification Hubs Push-Benachrichtigung

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In diesem Thema veranschaulicht die Azure Notification Hubs verwenden Chrome-App Push-Benachrichtigung an die im Rahmen des Google Chrome-Browsers angezeigt wird. In diesem Lernprogramm erstellen wir Chrome-app, die Pushbenachrichtigungen mit [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/)erhält. 

>[AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

Das Lernprogramm führt Sie diese grundlegenden Schritte Pushbenachrichtigungen aktivieren:

* [Aktivieren Sie Google Cloud Messaging](#register)
* [Konfigurieren Sie Ihren Notification hub](#configure-hub)
* [Chrome App Hub Benachrichtigung verbinden](#connect-app)
* [Senden Sie eine Push-Benachrichtigung für Ihre Anwendung Chrom](#send)
* [Zusätzliche Funktionalität und Funktionen](#next-steps)

>[AZURE.NOTE] Chrome app Pushbenachrichtigungen nicht generischen im Browser Benachrichtigungen - sind bestimmte Browser Erweiterbarkeit (siehe [Chrome Apps Übersicht] Details). Neben dem Desktopbrowser durchlaufen Chrome apps auf Mobile (Android und iOS) Apache Cordova. [Chrome Apps Mobile] mehr anzeigen

Konfigurieren von GCM und Azure Notification Hubs entspricht für Android, [Google Chrome Cloud Messaging] veraltet und dieselbe GCM unterstützt Geräte und Chrom Instanzen konfigurieren.

##<a id="register"></a>Aktivieren Sie Google Cloud Messaging

1. Navigieren Sie zu der Website [Google Cloud-Konsole] die Google-Anmeldeinformationen melden Sie an und klicken Sie auf **Projekt erstellen** . Sie einen geeigneten **Namen**, und klicken Sie dann auf die Schaltfläche **Erstellen** .

    ![Google Cloud Console - Projekt erstellen][1]

2. Notieren Sie die **Nummer** auf der Seite **Projekte** für das Projekt, das Sie gerade erstellt haben. Sie verwenden als **GCM Sender ID** in Chrome-App diese GCM registriert.

    ![Google Cloud Console - Projekt][2]

3. Im linken Bereich auf **APIs & Auth**, und blättern Sie und auf Umschalten um **Google Cloud Messaging für Android**zu aktivieren. Sie müssen **Google Chrome Cloud Messaging**aktivieren.

    ![Google Cloud Console - Server-Schlüssel][3]

4. Klicken Sie im linken Bereich auf **Anmeldeinformationen** > **Erstellen neuer Schlüssel** > **Server-Schlüssel** > **Erstellen**.

    ![Google Cloud-Konsole - Anmeldeinformationen][4]

5. Notieren Sie sich den Server **API-Schlüssel**. Dies konfigurieren im Notification Hub als Nächstes Sie Pushbenachrichtigungen GCM senden können.

    ![Google Cloud-Konsole - API-Schlüssel][5]

##<a id="configure-hub"></a>Konfigurieren Sie Ihren Notification hub

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6 Wählen Sie im Blatt **Einstellungen** **Notification Services** und **Google (GCM)**. API-Schlüssel eingeben und speichern.

&emsp;&emsp;![Azure Notification Hubs - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>Chrome App Hub Benachrichtigung verbinden

Der benachrichtigungshub ist jetzt konfiguriert GCM und haben die Verbindungszeichenfolgen für Ihre Anwendung zum Empfangen und Senden von Pushbenachrichtigungen registrieren. LK

###<a name="create-a-new-chrome-app"></a>Erstellen einer neuen Chrom

Im folgenden Beispiel wird basierend auf den [Chrome App GCM-Beispiel] und empfohlen, Chrome-App erstellen verwendet. Wir werden die Schritte im Zusammenhang mit Azure Notification Hubs hervorgehoben. 

>[AZURE.NOTE] Wir empfehlen die Quelle Chrome App aus [Chrom App Notification Hub]herunterzuladen.

Chrome-App mit JavaScript erstellt und können Sie Ihre bevorzugten Word-Editoren erstellen. Unten wird diese Chrom aussehen.

![Google Chrome-App][15]

1. Erstellen Sie einen Ordner und nennen Sie sie `ChromePushApp`. Der Name ist natürlich beliebig - Wenn Sie einen anderen Namen, sicherzustellen, dass Sie den Pfad in den erforderlichen Code ersetzen.

2. [Crypto-Js-Bibliothek] im zweiten Schritt erstellten Ordner herunterladen. Diese Bibliotheksordner enthält zwei Unterordner: `components` und `rollups`.

3. Erstellen einer `manifest.json` Datei. Chrome-Apps unterstützt, durch eine Manifestdatei, die app-Metadaten und die meisten enthält, alle Berechtigungen, die der Anwendung gewährt werden, wenn der Benutzer installiert.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Beachten Sie die `permissions` Element gibt an, dass diese Chrom GCM Push-Benachrichtigung erhalten können. Es muss auch Azure Notification Hubs URI angeben, Chrome-App REST registrieren aufrufen können.
    Beispiel-app verwendet auch eine `gcm_128.png`, das finden Sie an der Quelle, die aus der ursprünglichen GCM wiederverwendet. Sie können sie für jedes Bild ersetzen, die [Symbol Kriterien](https://developer.chrome.com/apps/manifest/icons)entspricht.

4. Erstellen Sie eine Datei namens `background.js` durch den folgenden Code:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    Dies ist die Datei, die die Chrome-App Fenster HTML (**register.html**) und definiert die Ereignishandler **MessageReceived** eingehende Pushbenachrichtigung verarbeiten.

5. Erstellen Sie eine Datei namens `register.html` -UI Chrome-App definiert. 

   >[AZURE.NOTE] **CryptoJS v3.1.2**verwendet. Wenn Sie eine andere Version der Bibliothek heruntergeladen haben, vergewissern Sie sich ordnungsgemäß ersetzen Sie die Version in der `src` Pfad.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Erstellen Sie eine Datei namens `register.js` durch den folgenden Code. Diese Datei legt das Skript hinter `register.html`. Chrome-Apps zulassen nicht Inlineausführung haben eine separate Sicherung Skript für Ihre Benutzeroberfläche erstellen.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Das obige Skript hat folgende wichtige Parameter:
    - **Window.OnLoad** definiert die Click-Ereignisse der zwei Schaltflächen auf der Benutzeroberfläche. Eine GCM registriert, und die andere verwendet die Identifikationsnummer, die danach mit GCM Azure Notification Hubs registriert zurückgegeben wird.
    - **UpdateLog** ist die Funktion, die einfache Protokollierungsfunktionen behandeln kann.
    - **RegisterWithGCM** ist die ersten Schaltflächenklick-Ereignishandler wird der `chrome.gcm.register` GCM sich die aktuelle Chrome-App-Instanz aufrufen.
    - **RegisterCallback** ist die Rückruffunktion, die aufgerufen wird, gibt die GCM Registrierungsaufruf.
    - **RegisterWithNH** ist die zweite Button.Click-Handler mit Benachrichtigung registriert. Es wird `hubName` und `connectionString` (die der Benutzer angegeben) und Handwerk Notification Hubs Registrierung REST-API-Aufruf.
    - **SplitConnectionString** und **GenerateSaSToken** sind Hilfsprogramme, die JavaScript-Implementierung von einem token SaS-Erstellungsprozess darstellen, die in allen REST-API-Aufrufen verwendet werden. Weitere Informationen finden Sie unter [Allgemeine Konzepte](http://msdn.microsoft.com/library/dn495627.aspx).
    - **SendNHRegistrationRequest** ist eine Funktion, die eine HTTP-REST aufrufen Azure Notification Hubs macht.
    - **RegistrationPayload** definiert die Registrierung XML-Nutzlast. Weitere Informationen finden Sie unter [Registrierung NH REST API erstellen]. Wir aktualisieren die Identifikationsnummer darin mit was wir von GCM erhalten.
    - **Client** ist eine Instanz von **XMLHttpRequest** verwenden wir die HTTP POST-Anforderung. Anmerkung Wir aktualisieren die `Authorization` Header mit `sasToken`. Abschluss des Aufrufs wird diese Instanz Chrome-App mit Azure Notification Hubs registriert.


Die gesamte Ordnerstruktur für dieses Projekt sollte wie folgt aussehen:     ![Google Chrome App - Ordnerstruktur][21]

###<a name="set-up-and-test-your-chrome-app"></a>Richten Sie ein und Testen Sie Ihrer Anwendung Chrom

1. Öffnen Sie Ihren Browser Chrom. **Chrome-Erweiterungen** öffnen und **den Entwicklermodus**zu aktivieren.

    ![Google Chrome - Entwickler-Modus aktivieren][16]

2. Klicken Sie auf **entpackt Erweiterung laden** und den Ordner, in dem die Dateien erstellt. Optional können Sie **Chrome Apps & Entwicklertool Extensions**. Dieses Tool ist ein Chrom selbst (Chrome Web Store installiert) und bietet erweiterte Debugfunktionen für die Chrom-Anwendungsentwicklung.

    ![Google Chrome - entpackt Erweiterung laden][17]

3. Wenn Chrome-Anwendung ohne Fehler erstellt wird, werden Sie Chrome App angezeigt angezeigt.

    ![Google Chrome - Chrom App-Anzeige][18]

4. Geben Sie die **Nummer** , die Sie von **Google Cloud-Konsole** als Absender-ID erhalten und auf **GCM registrieren**. Sehen Sie die Meldung **Anmeldung erfolgreich GCM.**

    ![Google Chrome - Chrom App Anpassung][19]

5. Geben Sie Ihren **Namen Hub** und **DefaultListenSharedAccessSignature** , die Sie zuvor aus dem Portal erhalten und auf **mit Azure Benachrichtigung registrieren**. Sehen Sie die Meldung **Benachrichtigung Hub Registrierung erfolgreich!** und die Details der Registrierungsantwort enthält die Registrierung Azure Notification Hubs-ID.

    ![Google Chrome - Benachrichtigung Hub Details angeben][20]  

##<a name="send"></a>Sendet eine Benachrichtigung an Ihre App Chrom

Zu Testzwecken senden wir Chrome Pushbenachrichtigungen mit .NET Anwendung Konsole. 

>[AZURE.NOTE] Sie können aus jeder Backend über öffentliche <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-Schnittstelle</a>Pushbenachrichtigungen mit Benachrichtigung senden. Sehen Sie sich unsere [dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/) Weitere Beispiele für die plattformübergreifende.

1. In Visual Studio im Menü **Datei** wählen Sie **neu** und anschließend **Projekt**. Unter **Visual C#**auf **Windows** und **Konsolenanwendungsprojekt**, und klicken Sie dann auf **OK**.  Dies erstellt ein neues Konsolenanwendungsprojekt.

2. Klicken Sie im Menü **Extras** auf **Bibliothek Paket-Manager** und **Paket-Manager-Konsole**. Der Paket-Manager-Konsole angezeigt.

3. Im Konsolenfenster Befehl:

        Install-Package Microsoft.Azure.NotificationHubs

    Einen Verweis auf den Azure Service Bus SDK <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet-Paket</a>hinzugefügt.

4. Open `Program.cs` und fügen Sie den folgenden `using` Anweisung:

        using Microsoft.Azure.NotificationHubs;

5. In der `Program` Klasse, fügen Sie folgende Methode:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Ersetzen Sie die `<hub name>` Platzhalter mit dem Namen des Notification Hubs, die im [Portal](https://portal.azure.com) in Ihrem Notification Hub Blade angezeigt wird. Auch die Verbindungszeichenfolge aufgerufen Connection String Platzhalter ersetzen `DefaultFullSharedAccessSignature` , die unter Benachrichtigung Hub-Konfiguration ermittelt.

    >[AZURE.NOTE] Achten Sie die Verbindungszeichenfolge mit **vollständigen** Zugang, nicht **hören** . Die Verbindungszeichenfolge Zugriff **Überwachen** gewährt keine Berechtigungen zum Senden von Pushbenachrichtigungen.

5. Fügen Sie die folgenden Aufrufe in die `Main` Methode:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Chrome ausgeführt wird, und führen Sie das Konsolenanwendungsprojekt.

7. Die folgende Benachrichtigung sollte Popup auf dem Desktop angezeigt werden.

    ![Google Chrome - Benachrichtigung][13]

8. Sie sehen auch alle Benachrichtigungen mit Chrome Benachrichtigungen Fenster in der Taskleiste (unter Windows) Wenn Chrome ausgeführt wird.

    ![Google Chrome - Benachrichtigungsliste][14]

>[AZURE.NOTE] Sie müssen der Chrome-App ausgeführt oder im Browser (obwohl sich Chrome-Browsers ausgeführt werden muss). Sie erhalten auch eine konsolidierte Ansicht der Benachrichtigung im Fenster Benachrichtigungen Chrom.

## <a name="next-steps"> </a>Nächste Schritte

Erfahren Sie mehr über Benachrichtigungshubs [Übersicht über Notification Hubs].

Um auf bestimmte Benutzer, finden Sie in [Azure Notification Hubs benachrichtigen Benutzer] -Lernprogramm. 

Ggf. Benutzer Interessengruppen segment können Sie [Azure Notification Hubs Neuigkeiten] Lernprogramm folgen.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome App Benachrichtigung Hub (Beispiel)]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud-Konsole]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Übersicht über Notification Hubs]: notification-hubs-push-notification-overview.md
[Chrome Apps (Übersicht)]: https://developer.chrome.com/apps/about_apps
[Chrome App GCM-Beispiel]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome Apps Mobile]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Registrierung NH REST API erstellen]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[Crypto-Js-Bibliothek]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging für Chrom]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs benachrichtigen Benutzer]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Notification Hubs Nachrichten]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
