<properties
    pageTitle="Apache Cordova App Azure mobiler Apps Pushbenachrichtigungen hinzufügen | Azure App Service"
    description="Informationen Sie zum Azure Mobile Apps mithilfe Ihrer Apache Cordova Anwendung Pushbenachrichtigungen senden."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Apache Cordova app Pushbenachrichtigungen hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Übersicht

In diesem Lernprogramm hinzugefügt Pushbenachrichtigungen Projekt [Apache Cordova schnell starten] , damit eine Push-Benachrichtigung an das Gerät gesendet wird, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, benötigen Sie das Erweiterungspaket Push Notification. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Erforderliche Komponenten

Dieses Lernprogramm umfasst eine Apache Cordova Anwendung mit Visual Studio auf Google Android Emulator, Android-Gerät ein Windows-Gerät und ein Gerät iOS-2015.

Um dieses Lernprogramm müssen wie folgt vor:

* Ein PC mit [Visual Studio Community 2015] oder höher.
* [Visual Studio-Tools für Apache Cordova].
* Ein [Aktives Azure-Konto](https://azure.microsoft.com/pricing/free-trial/).
* Ein abgeschlossenes Projekt [Apache Cordova Schnellstart] .
* (Android) Ein [Konto] mit einem verifizierten e-Mail-Adresse.
* (iOS) Eine Mitgliedschaft Apple Developer und iOS-Gerät (iOS Simulator unterstützt keine Push).
* (Windows) Windows Store-Entwicklerkonto und ein Windows 10.

##<a name="configure-hub"></a>Konfigurieren eines Benachrichtigung Hubs

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[In diesem Video zeigt Schritte in diesem Abschnitt](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Aktualisieren von Project Server zum Senden von Pushbenachrichtigungen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Ändern Sie Ihre Cordova app Push-Benachrichtigung

Sie müssen sicherstellen, dass das Apache-Cordova-app-Projekt Pushbenachrichtigungen mit Cordova Push Plug-in plus plattformspezifische Push Dienste behandeln kann.

#### <a name="update-the-cordova-version-in-your-project"></a>Cordova Version im Projekt zu aktualisieren.

Sie sollten das Clientprojekt auf Cordova 6.1.1 aktualisieren, wenn das Projekt mit einer älteren Version konfiguriert ist. Klicken Sie zum Aktualisieren des Projekts config.xml Konfigurations-Designer öffnen. Wählen Sie die Registerkarte Plattformen und 6.1.1 im Textfeld **Cordova CLI** .

Wählen Sie **Erstellen**und **Projektmappe** , das Projekt zu aktualisieren.

#### <a name="install-the-push-plugin"></a>Push-Plugin installieren

Apache Cordova Applikationen behandelt Gerät oder Netzwerk-Funktionen nicht direkt.  Diese Funktionen werden von Plug-Ins bereitgestellt, die [Npm](https://www.npmjs.com/) oder auf GitHub veröffentlicht werden.  Die `phonegap-plugin-push` Plugin zum Netzwerk Pushbenachrichtigungen behandeln.

Sie können das Push-Plugin folgendermaßen installieren:

**Über die Befehlszeile:**

Führen Sie den folgenden Befehl ein:

    cordova plugin add phonegap-plugin-push

**Von innerhalb von Visual Studio:**

1.  Öffnen Sie im Projektmappen-Explorer die `config.xml` Datei klicken Sie auf **Plug-Ins** > **benutzerdefinierte** **Git** als Installationsquelle auswählen, und geben Sie `https://github.com/phonegap/phonegap-plugin-push` als Quelle.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Klicken Sie auf den Pfeil neben der Installationsquelle.

3. In **SENDER_ID**Wenn Sie bereits eine numerische Projektnummer für Google Developer Console-Projekt können Sie es hier hinzufügen. Geben Sie andernfalls einen Platzhalterwert wie 777777, und wenn Android richten diesen Wert in config.xml später aktualisieren.

4. Klicken Sie auf **Hinzufügen**.

Push-Plugin ist jetzt installiert.

####<a name="install-the-device-plugin"></a>Gerät Plug-in installieren

Gehen Sie zum Push-Plugin installieren, aber finden Sie in der Liste der Core-Plugins Plugin Gerät (klicken Sie auf **Plug-Ins** > **Kern** zu finden). Sie benötigen dieses Plugin zu den Plattformnamen (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Registrieren Sie Ihr Gerät auf Start

Zunächst werden wir minimalen Code für Android enthalten. Später machen wir kleinen Änderungen auf iOS oder Windows 10.

1. Fügen Sie einen Aufruf von **RegisterForPushNotifications** während des Rückrufs für die Anmeldung oder am Ende der **OnDeviceReady** -Methode:

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    Dieses Beispiel zeigt **RegisterForPushNotifications** nach erfolgreicher Authentifizierung Aufrufen der empfiehlt beim Push-Benachrichtigung und die Authentifizierung in Ihrer Anwendung verwenden.

2. Fügen Sie die neue **RegisterForPushNotifications** -Methode:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android) Ersetzen Sie im obigen Code `Your_Project_ID` mit numerischen project ID für Ihre Anwendung in der [Entwicklerkonsole Google].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Optional) Konfigurieren und Ausführen der Anwendung für Android

Führen Sie diesen Abschnitt, um Pushbenachrichtigungen für Android aktivieren.

####<a name="enable-gcm"></a>Aktivieren Sie FB Cloud Messaging

Da wir zunächst die Google Android-Plattform verwenden, müssen Sie die FB Cloud Messaging aktivieren. Wenn Sie Microsoft Windows-Geräte ausgerichtet waren, würde Sie WNS-Unterstützung aktivieren.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>Konfigurieren Sie das Mobile Anwendung Backend FCM mit Push-Anforderung senden

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>Konfigurieren Sie Ihre Cordova-app für Android

In Ihrer Anwendung Cordova öffnen config.xml, und Ersetzen Sie `Your_Project_ID` mit numerischen project ID für Ihre Anwendung in der [Entwicklerkonsole Google].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Öffnen Sie index.js und aktualisieren Sie den Code, um die numerische Projekt-ID verwenden.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>Konfigurieren Sie das Android Gerät für USB-debugging

Vor dem Bereitstellen der Anwendung Android Geräts müssen Sie USB-Debugging aktivieren.  Führen Sie die folgenden Schritte auf Ihrem Handy:

1. **Standardeinstellungen**zum > **über Telefon**, und tippen Sie auf die **Build-Nummer** bis Entwicklermodus (7-Mal) aktiviert ist.

2. In **Einstellung** > **Developer Optionen** **USB-debugging**aktivieren und anschließend auf Ihrem Handy auf Ihrem Entwicklungs-PC mit einem USB-Kabel verbinden.

Wir getestet mit einem Google Nexus 5 X Gerät mit Android 6.0 (Kaugummi).  Jedoch gelten die Verfahren in allen modernen Android-Versionen.

#### <a name="install-google-play-services"></a>Google Play Services installieren

Push-Plugin basiert auf Android Google Play Services für Pushbenachrichtigungen.  

1.  Klicken Sie in **Visual Studio** **Tools** > **Android** > **Android SDK Manager**, erweitern Sie den Ordner **Extras** und Kontrollkästchen sicherstellen, dass alle folgenden SDKs installiert ist.
    * Android 2.3 oder höher
    * Google Repository Revision 27 oder höher
    * Google Play Services 9.0.2 oder höher

2.  Klicken Sie auf **Pakete installieren** und warten Sie, bis die Installation abgeschlossen.

Die aktuelle erforderlichen Bibliotheken in der [Phonegap-Plugin-Push-Installationsdokumentation]aufgeführt sind.

#### <a name="test-push-notifications-in-the-app-on-android"></a>Pushbenachrichtigungen Test in der app für Android

Sie können jetzt testen Pushbenachrichtigungen durch die Anwendung und Einfügen von Elementen in der TodoItem-Tabelle. Dasselbe Gerät oder ein zweites Gerät dabei als dieselbe Backend verwenden. Testen Sie Ihrer Anwendung Cordova Android-Plattform auf eine der folgenden Arten:

- **Auf einem physischen Gerät:**  
Entwicklungscomputer mit einem USB-Kabel Android Gerät zuordnen.  Wählen Sie statt **Google Android Emulator** **Gerät**. Visual Studio in das Gerät bereitzustellen und auszuführen.  Sie können dann mit der Anwendung auf dem Gerät interagieren.  
Verbessern Sie der Entwicklung.  Bildschirm Anwendung wie [Mobizen] unterstützen Sie Android Anwendungsentwicklung durch Projizieren Android Bildschirms auf einem Web-Browser auf Ihrem PC.

- **Einem Android Emulator:**  
Es sind zusätzliche Konfigurationsschritte erforderlich, wenn auf einem Emulator ausgeführt.

    Stellen Sie sicher, dass Sie zum Bereitstellen oder auf ein virtuelles Gerät mit Google APIs angestrebt Debuggen, wie folgt Android Virtual Device (AVD)-Manager.

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Wenn Sie schneller X86 möchten Emulator, [HAXM installieren](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) und konfigurieren Sie den Emulator verwenden.

    Android-Gerät ein Konto hinzufügen, indem Sie **Apps**auf > **Settings** > **Konto hinzufügen**und dann auf eine vorhandene Google hinzufügen Konto auf dem Gerät (Wir empfehlen ein Konto verwenden, anstatt eine neue) folgen.

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Führen Sie die Aufgabenliste app vor und fügen Sie ein neues Todo-Element ein. Dieses Mal wird ein Symbol im Infobereich angezeigt. Öffnen Sie die Schublade Benachrichtigung um den vollständigen Text der Benachrichtigung.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Optional) Konfigurieren und Ausführen von IOS

Dieser Abschnitt ist für die Ausführung des Projekts Cordova auf iOS-Geräten. Wenn Sie nicht mit iOS-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Installieren und Ausführen des iOS-Remotebuild-Agents auf einem Mac oder Cloud-Dienst

Vor dem Ausführen von Cordova-app für iOS mit Visual Studio gehen Sie durch die Schritte in [iOS Handbuch](http://taco.visualstudio.com/en-us/docs/ios-guide/) zum Installieren und Ausführen des Remotebuild-Agents.

Sicherstellen Sie, dass die app für iOS erstellen können. Die Schritte im Handbuch für das iOS von Visual Studio erstellen müssen. Wenn Sie keinen Mac kann für iOS mit dem Remotebuild-Agent von einem Dienst wie MacInCloud erstellen. Weitere Informationen finden Sie unter [iOS-app in der Cloud ausgeführt](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/).

>[AZURE.NOTE] XCode 7 oder höher muss auf iOS Push-Plug-in verwenden.

####<a name="find-the-id-to-use-as-your-app-id"></a>Suchen Sie die ID als Ihre App-ID verwenden

Bevor Sie Ihre Anwendung für Pushbenachrichtigungen öffnen config.xml in Ihrer Anwendung Cordova registrieren finden die `id` Attributwert im Widget-Element und zur späteren Verwendung kopieren. Im folgenden XML wird die ID `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Verwenden Sie dieser Bezeichner später, wenn eine App-ID auf der Apple Developer Portal erstellen. (Wenn Sie eine andere App ID Developer Portal erstellen und verwenden möchten, müssen Sie später in diesem Lernprogramm mit dieser ID in config.xml ändern einige zusätzliche Schritte ausführen müssen. ID im Widget-Element muss die ID App Entwicklerportal übereinstimmen.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Registrieren Sie die app für Pushbenachrichtigungen Apple Developer Portal

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[In diesem Video zeigt ähnliche Schritte](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Konfigurieren von Azure Push-Benachrichtigung senden

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Überprüfen Sie, ob Ihre App-ID Ihrer app Cordova entspricht

Wenn Ihr Konto Apple Developer erstellten App-ID die ID des Elements Widget in config.xml übereinstimmt, können Sie diesen Schritt überspringen. Falls die IDs übereinstimmen, gehen Sie folgendermaßen vor:

1. Plattformen Ordner aus dem Projekt gelöscht.

2. Der Ordner "Plug-Ins" aus dem Projekt gelöscht.

3. Löschen Sie den Ordner Node_modules aus dem Projekt.

4. Aktualisieren Sie das ID-Attribut des Elements Widget in config.xml App-ID verwenden, die Sie in Ihrem Apple Developer-Konto erstellt.

5. Erstellen Sie das Projekt neu.

#####<a name="test-push-notifications-in-your-ios-app"></a>Test Pushbenachrichtigungen in iOS-app

1. In Visual Studio **iOS** als Bereitstellungsziel ausgewählt ist, und wählen Sie die **Geräte** auf Ihrem Gerät verbundenen iOS.

    Sie können auf einem iOS-Gerät mit dem PC verbunden ausführen mit iTunes. IOS-Simulator unterstützt keine Push-Benachrichtigung.

2. Drücken Sie die Schaltfläche **Ausführen** oder **F5** in Visual Studio das Projekt und die app in iOS-Gerät und klicken Sie auf **OK** , um Push-Benachrichtigung zu übernehmen.

    >[AZURE.NOTE] Sie müssen explizit Pushbenachrichtigungen aus Ihrer Anwendung akzeptieren. Diese Anforderung tritt nur beim ersten, der die Anwendung ausgeführt wird.

3. In der Anwendung geben Sie eine Aufgabe und klicken Sie dann auf das Pluszeichen (+) Symbol.

4. Stellen Sie sicher, dass eine Benachrichtigung empfangen wird, klicken Sie auf OK, um die Benachrichtigung zu schließen.

##<a name="optional-configure-and-run-on-windows"></a>(Optional) Konfigurieren und Ausführen von Windows

Dieser Abschnitt ist zum Ausführen von Apache Cordova-app-Projekt auf Windows 10 Geräte (PhoneGap Push-Plugin wird auf Windows 10 unterstützt). Wenn Sie nicht mit Windows arbeiten, können Sie diesen Abschnitt überspringen.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrieren Sie Ihre Windows-Anwendung für Pushbenachrichtigungen WNS

Shop-Optionen in Visual Studio verwenden, wählen Sie Windows Ziel aus Projektmappenplattformen **Windows X64** oder **Windows-X86** ( **Windows AnyCPU** für Pushbenachrichtigungen vermeiden).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[In diesem Video zeigt ähnliche Schritte](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>Konfigurieren des Benachrichtigung Hubs für WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Konfigurieren Sie Ihre app Cordova unterstützen Windows Pushbenachrichtigungen

Öffnen Sie den Designer Konfiguration (Rechtsklick auf "config.xml" und wählen Sie **Ansicht-Designer**) Wählen Sie die Registerkarte **Windows** und **Windows 10** unter **Windows Zielversion**auswählen.

>[AZURE.NOTE] Bei Verwendung eine Cordova Version vor Cordova 5.1.1 (6.1.1 empfohlen) müssen auch Toast kann Flag true in config.xml festlegen.

Unterstützung Push Notifications in Ihrem standardmäßigen (Debug), öffnen build.json Datei erstellt. Kopieren Sie die Konfiguration "Release" auf die Debugkonfiguration.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Nach der Aktualisierung muss dieser Code wie folgt aussehen.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Erstellen Sie die Anwendung und stellen Sie sicher, dass keine Fehler auftreten. -Anwendung sollte hierbei vom Back-End Mobile-Anwendung registrieren. Wiederholen Sie diesen Abschnitt für alle Windows-Projekt in der Projektmappe.

####<a name="test-push-notifications-in-your-windows-app"></a>Pushbenachrichtigungen in Ihrer Windows-Anwendung testen

Stellen Sie in Visual Studio sicher, dass Windows-Plattform als Bereitstellungsziel **Windows X64** oder **Windows X86**ausgewählt ist. Führen Sie die Anwendung auf einem PC mit Windows 10 hosting Visual Studio wählen Sie **Lokaler Computer**.

Drücken Sie auf Ausführen, um das Projekt und die Anwendung starten.

In der Anwendung einen Namen für eine neue Todoitem und klicken Sie dann auf das Pluszeichen (+) Symbol hinzufügen.

Stellen Sie sicher, dass eine Benachrichtigung empfangen, wenn das Element hinzugefügt wird.

##<a name="next-steps"></a>Nächste Schritte

* Informationen Sie zu [Notification Hubs] Pushbenachrichtigungen erfahren.
* Wenn nicht bereits geschehen, weiterhin im Lernprogramm von [Authentifizierung hinzufügen] zu Ihrer Apache Cordova Anwendung.

Dazu verwenden Sie die SDKs.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs -->
[Authentifizierung hinzufügen]: app-service-mobile-cordova-get-started-users.md
[Apache Cordova-Schnellstart]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Konto]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Google Entwicklerkonsole]: https://console.developers.google.com/home/dashboard
[PhoneGap-Plugin-Push-Installation-Dokumentation]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio-Tools für Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Benachrichtigungshubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
