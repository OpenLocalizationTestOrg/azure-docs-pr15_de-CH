<properties
    pageTitle="iOS Pushbenachrichtigungen mit Notification Hubs für apps Xamarin | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie, wie Sie Azure Notification Hubs Pushbenachrichtigungen zu einer Xamarin iOS-Anwendung senden."
    services="notification-hubs"
    keywords="IOS Pushbenachrichtigungen, Push Nachrichten Pushbenachrichtigungen, push Nachricht"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS Pushbenachrichtigungen mit Benachrichtigung Xamarin Apps

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Übersicht
> [AZURE.IMPORTANT] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Dieses Lernprogramm zeigt wie Azure Notification Hubs Pushbenachrichtigungen iOS-Anwendung an.
Erstellen Sie eine leere Xamarin.iOS app, die Pushbenachrichtigungen mit [Apple Push Notification Service (APN)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)erhält. Wenn Sie fertig sind, werden Sie mit Ihrem Haupt-Benachrichtigung Push-Benachrichtigung an alle Ihre App Geräte übertragen. Der fertige Code ist in [NotificationHubs app] [ GitHub] Beispiel.

Dieses Lernprogramm demonstriert die Nachricht einfach broadcast Szenario mit Notification Hubs.

##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm ist Folgendes erforderlich:

+ [Xcode 6.0][Install Xcode]
+ Ein iOS 7.0 (oder höher)-Gerät
+ iOS Entwickler Mitgliedschaft
+ [Xamarin Studio]

   > [AZURE.NOTE] Aufgrund Konfiguration für iOS Benachrichtigungen drücken müssen Sie bereitstellen und testen die Beispiel-Anwendung auf einem physischen iOS-Gerät (iPhone und iPad) statt im Simulator.

Dieses Lernprogramm ist eine Voraussetzung für alle anderen Notification Hubs Lernprogramme für Xamarin iOS-apps.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>Konfigurieren Sie Ihren Notification hub

Dieser Abschnitt führt Sie durch eine neue benachrichtigungshub erstellen und Konfigurieren von Authentifizierung mit APN mit dem **P12** Push-Zertifikat, das Sie erstellt haben. Möchten Sie Notification Hub verwenden, den Sie bereits erstellt haben, können Sie zu Schritt 5 überspringen.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Wie wir im Azure-Portal APN-Verbindung konfigurieren möchten, Öffnen der Benachrichtigungshub, und <b>Notification Services</b>auf, und klicken Sie auf der <b>Apple (APN)</b> Element in der Liste. Danach klicken Sie auf <b>Hochladen</b> und wählen Sie <b>P12</b> -Zertifikat, das Sie früher sowie das Kennwort für das Zertifikat exportiert.</p>
<p>Stellen Sie sicher <b>geschützten</b> Modus auswählen, da Sie Push-Nachrichten in einem senden. Verwenden Sie Einstellung der <b>Produktion</b> nur Push-Benachrichtigung an Benutzer senden, die Ihre Anwendung aus dem Speicher bereits gekauft werden soll.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Der benachrichtigungshub ist jetzt konfiguriert APN und haben die Verbindungszeichenfolgen für Ihre Anwendung registrieren und Push-Benachrichtigung zu senden.


##<a name="connect-your-app-to-the-notification-hub"></a>Ihre app Hub Benachrichtigung verbinden

#### <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

1. In Xamarin Studio erstellen ein neues iOS-Projekt, und wählen Sie die **Einheitliche API** > **einzelne** Anwendungsvorlage anzeigen.

    ![Xamarin Studio - Anwendung auswählen][31]

2. Fügen Sie einen Verweis auf die Azure Messaging-Komponente. In der Ansicht Projektmappen Maustaste Ordner **Komponenten** für Ihr Projekt, und wählen Sie **Weitere Komponenten**. **Azure** -Messagingkomponente suchen und die Komponente dem Projekt hinzufügen.

3. **AppDelegate.cs**, fügen Sie folgende Anweisung verwenden:

        using WindowsAzure.Messaging;

4. Deklarieren Sie eine Instanz des **SBNotificationHub**:

        private SBNotificationHub Hub { get; set; }

5. Erstellen Sie eine **Constants.cs** -Klasse die folgenden Variablen:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. Aktualisieren Sie in **AppDelegate.cs** **FinishedLaunching()** entsprechend dem folgenden Beispiel:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. Überschreiben Sie die Methode **RegisteredForRemoteNotifications()** in **AppDelegate.cs**:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. Überschreiben Sie die Methode **ReceivedRemoteNotification()** in **AppDelegate.cs**:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. Erstellen Sie die folgende **ProcessNotification()** -Methode in **AppDelegate.cs**:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] Sie können **FailedToRegisterForRemoteNotifications()** für Situationen wie keine Verbindung zum Netzwerk zu überschreiben. Dies ist besonders wichtig, Benutzer beginnen die Anwendung im Offlinemodus (z.B. Flugzeug) wobei Messagingszenarios für Ihre app Push verarbeiten soll.


10. Führen Sie die Anwendung auf dem Gerät.


## <a name="sending-push-notifications"></a>Push-Benachrichtigung senden


Sie können testen, Benachrichtigungen Push in Ihrer Anwendung in [Azure-Portal] **Testen senden** Funktion im Toolset **Problembehandlung** , direkt in die Benachrichtigungsseite Hub Benachrichtigungen senden wie im folgenden Bildschirm dargestellt.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Pushbenachrichtigungen sind normalerweise über einen Back-End wie Mobile Dienste oder ASP.NET eine kompatible Bibliothek gesendet. Die REST-API können direkt mit Push Nachrichten senden, wenn eine Bibliothek nicht in Ihrem Szenario verfügbar ist. 

In diesem Lernprogramm erstellen wir einfach und nur demonstrieren Testen der Clientanwendung Benachrichtigungen mit .NET SDK für Notification Hubs ein Konsolenanwendungsprojekt kein Back-End-Dienst. Empfohlen Lernprogramm [Verwendet Notification Hubs Push-Benachrichtigung an Benutzer](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) im nächsten Schritt für das Senden von Benachrichtigungen aus einer ASP.NET Backend. Die folgenden Ansätze können jedoch für Benachrichtigungen verwendet werden:

* **REST-Schnittstelle**: Push-Benachrichtigung kann auf allen Back-End-Plattformen mithilfe der [REST-Schnittstelle](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)unterstützen.

* **Microsoft Azure Notification Hubs.NET SDK**: In der Nuget Paket-Manager für Visual Studio [- Installationspaket Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)ausführen.

* **Node.js** : [wie Notification Hubs von Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobile Apps**: finden Sie ein Beispiel aus einem Backend Azure App Service Mobile Apps Benachrichtigungen, die Benachrichtigungshubs integriert [Pushbenachrichtigungen für Ihre mobilen Anwendung hinzufügen](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: beispielsweise eine Push-Benachrichtigung senden mithilfe der REST-APIs finden Sie unter "Benachrichtigungshubs von Java-PHP verwenden" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Optional) Push-Benachrichtigung von einer Konsole.

In diesem Abschnitt senden wir Pushbenachrichtigungen mithilfe einer einfachen .NET Konsolenanwendung. Für dieses Beispiel wechseln wir zu einer windowsumgebung, die Visual Studio installiert ist.

1. Erstellen Sie in Visual Studio eine neue Visual C#:

    ![Visual Studio – erstellen Sie eine neue][213]

2. Klicken Sie in Visual Studio auf **Extras**, klicken Sie auf **NuGet Paket-Manager**und klicken Sie **Paket-Manager Konsole**.

    Die Paket-Manager-Konsole erscheint am unteren Rand der Visual Studio-Arbeitsbereich angedockt.

3. Im Konsolenfenster Paket-Manager **Project Standard** auf Ihr neues Konsolenanwendungsprojekt festgelegt, und führen Sie folgenden Befehl im Konsolenfenster angezeigt:

        Install-Package Microsoft.Azure.NotificationHubs

    Einen Verweis auf Azure Notification Hubs SDK mit <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-Paket</a>hinzugefügt.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Öffnen der `Program.cs` -Datei und fügen Sie den folgenden `using` -Anweisung wir Azure Klassen und Funktionen innerhalb der Hauptklasse verwenden können:

        using Microsoft.Azure.NotificationHubs;

3. In der `Program` Klasse, fügen Sie die folgende Methode hinzu (vergessen Sie nicht, die **Verbindungszeichenfolge** und **Hubnamen**ersetzen):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Fügen Sie folgende Zeilen in der `Main` Methode:

         SendNotificationAsync();
         Console.ReadLine();

5. Drücken Sie F5, um die Anwendung auszuführen. Innerhalb von Sekunden erhalten Sie eine Push-Benachrichtigung auf Ihrem Gerät angezeigt. Ob WLAN oder ein Mobilfunknetz stellen Sie sicher, dass eine Internetverbindung auf dem Gerät.

Alle möglichen Nutzlasten finden Sie im Apple [lokale und Push Notification Programming Guide].

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Optional) Benachrichtigung von einem mobilen Service

In diesem Abschnitt werden wir Pushbenachrichtigungen verwenden mobilen Service Knoten Skripts senden.

Um eine Benachrichtigung zu senden, mithilfe eines mobilen Diensts führen Sie [Erste Schritte mit Mobile Dienste], und:

1. Melden Sie sich bei [Azure-Verwaltungsportal]und wählen Sie den mobilen Dienst.

2. Wählen Sie auf der Registerkarte **Planer** .

    ![Klassische Azure-Portal - Planer][215]

3. Einen neuen geplanten Auftrag erstellen, geben Sie einen Namen und wählen **bei Bedarf**.

    ![Azure Classic-Portal - neues Projekt erstellen][216]

4. Wenn der Auftrag erstellt wurde, klicken Sie auf den Auftragsnamen. Klicken Sie auf die Registerkarte **Skript** in der oberen Leiste.

5. Fügen Sie das folgende Skript in der Planer-Funktion. Stellen Sie sicher, die Platzhalter Namen der Hub mit der Verbindungszeichenfolge *DefaultFullSharedAccessSignature* ersetzen, die Sie zuvor erworben haben. Klicken Sie auf **Speichern**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Klicken Sie in der Leiste unten auf **Ausführen** . Sie erhalten eine Benachrichtigung auf Ihrem Gerät.

##<a name="next-steps"></a>Nächste Schritte

In diesem einfachen Beispiel wird Push-Benachrichtigung an alle iOS-Geräte übertragen. Um bestimmte Zielgruppe finden Sie das Lernprogramm [Verwendet Notification Hubs Push-Benachrichtigung an Benutzer]. Ggf. Benutzer Interessengruppen segment erhalten Sie [Mit Notification Hubs Nachrichten senden]. Erfahren Sie mehr zur Verwendung von Notification Hubs [Benachrichtigung Hubs Anleitung] und das- [Benachrichtigung Hubs Vorgehensweisen für iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Erste Schritte mit Mobile Dienste]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure-Verwaltungsportal]: https://manage.windowsazure.com/
[Benachrichtigung Hubs Anleitung]: http://msdn.microsoft.com/library/jj927170.aspx
[Benachrichtigung Hubs Vorgehensweisen für iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Benachrichtigungshubs mit der Push-Benachrichtigung an Benutzer]: /manage/services/notification-hubs/notify-users-aspnet
[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]: /manage/services/notification-hubs/breaking-news-dotnet

[Lokale und Benachrichtigung Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure-Portal]: https://portal.azure.com
