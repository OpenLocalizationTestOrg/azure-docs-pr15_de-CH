<properties
    pageTitle="Senden von Pushbenachrichtigungen mit Azure Notification Hubs in Windows Phone | Microsoft Azure"
    description="In diesem Lernprogramm lernen Sie Azure Notification Hubs Pushbenachrichtigungen einer Windows Phone 8 oder Windows Phone 8.1 Silverlight-Anwendung verwenden."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="Push-Benachrichtigung, push-Benachrichtigung, Windows phone-push"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Senden von Pushbenachrichtigungen mit Azure Notification Hubs für Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Übersicht

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

Dieses Lernprogramm veranschaulicht mit Azure Notification Hubs Pushbenachrichtigungen einer Windows Phone 8 oder Windows Phone 8.1 Silverlight-Anwendung senden. Wenn Sie Windows Phone 8.1 (nicht Silverlight) abzielen, finden Sie die [Universal Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Version.
In diesem Lernprogramm erstellen Sie eine leere Windows Phone 8-app, die Pushbenachrichtigungen mit Microsoft Push Notification Service (MPNS) empfängt. Wenn Sie fertig sind, werden Sie mit Ihrem Haupt-Benachrichtigung Push-Benachrichtigung an alle Ihre App Geräte übertragen.

> [AZURE.NOTE] Benachrichtigung Hubs Windows Phone SDK unterstützt nicht Windows Phone 8.1 Silverlight-apps mit Windows Push Notification Service (WNS). WNS (statt MPNS) mit Silverlight für Windows Phone 8.1 apps, führen Sie die [Benachrichtigungshubs - Windows Phone Silverlight-Lernprogramm], der REST-APIs verwendet.

Das Lernprogramm demonstriert das einfache Übertragung Szenario mit Notification Hubs.

##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm ist Folgendes erforderlich:

+ [Visual Studio 2012 Express für Windows Phone]oder höher.

Dieses Lernprogramm ist eine Voraussetzung für alle anderen Notification Hubs Lernprogramme für apps für Windows Phone 8.

##<a name="create-your-notification-hub"></a>Den benachrichtigungshub erstellen

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Klicken Sie auf die <b>Notification Services-</b> Abschnitt ( <i>Settings</i>), <b>Windows Phone (MPNS)</b> auf, und klicken Sie auf das Kontrollkästchen <b>nicht authentifizierten Push aktivieren</b> .</p>
</li>
</ol>

&emsp;&emsp;![Azure-Portal - Unathenticated Push-Benachrichtigung aktivieren](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Der Hub erstellt und so konfiguriert, dass nicht authentifizierte Benachrichtigung für Windows Phone jetzt.

> [AZURE.NOTE] Diese praktische Einführung verwendet MPNS im nicht authentifizierten Modus. MPNS nicht authentifizierten Modus kommt mit auf die Nachrichten, die an jedem Kanal gesendet. Benachrichtigungshubs unterstützt [MPNS authentifizierten Modus](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) indem Ihr Zertifikat hochladen.

##<a name="connecting-your-app-to-the-notification-hub"></a>Ihre app verbinden Hub Benachrichtigung

1. Erstellen Sie in Visual Studio eine neue Windows Phone 8-Anwendung.

    ![Visual Studio - neues Projekt - Windows Phone App][13]

    In Visual Studio 2013 Update 2 oder höher, erstellen Sie stattdessen eine Windows Phone Silverlight-Anwendung.

    ![Visual Studio - neues Projekt - leere App - Windows Phone Silverlight][11]

2. Klicken Sie in Visual Studio die Projektmappe und dann auf **NuGet-Pakete verwalten**.

    Das Dialogfeld " **NuGet-Pakete verwalten** " angezeigt.

3. Suchen Sie nach `WindowsAzure.Messaging.Managed` klicken Sie auf **Installieren**und die Vereinbarung akzeptieren.

    ![Visual Studio - NuGet Paket-Manager][20]

    Diese downloads installiert und fügt einen Verweis auf die Bibliothek Azure Messaging für Windows mithilfe des <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet-Paket</a>.

4. Öffnen Sie die Datei App.xaml.cs, und fügen Sie Folgendes `using` Aussagen:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Fügen Sie den folgenden Code am Anfang der **Application_Launching** -Methode in "App.Xaml.cs":

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] Der Wert **MyPushChannel** ist ein Index verwendet wird, um einen vorhandenen Kanal in der [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) -Auflistung zu suchen. Wenn es gibt keines, erstellen Sie einen neuen Eintrag mit diesem Namen.
    
    Legen Sie den Namen des Haupt- und **DefaultListenSharedAccessSignature** Sie erhalten im vorherigen Abschnitt namens Verbindungszeichenfolge ein.
    Dieser Code Ruft den Channel-URI für die Anwendung von MPNS und Channel-URI mit der Benachrichtigung registriert. Außerdem, dass den Kanal URI im benachrichtigungshub jedes Mal registriert die Anwendung.

    >[AZURE.NOTE]In diesem Lernprogramm sendet Popupbenachrichtigung an das Gerät. Beim Senden einer kachelbenachrichtigung müssen Sie stattdessen die **BindToShellTile** -Methode auf dem Kanal aufrufen. Unterstützen beide Toast und kachelbenachrichtigungen, **BindToShellTile** und **BindToShellToast**aufrufen.

6. Erweitern Sie im Projektmappen-Explorer **Eigenschaften**öffnen Sie die `WMAppManifest.xml` Datei, klicken Sie auf **Funktionen** und stellen Sie sicher, dass die **ID_CAP_PUSH_NOTIFICATION** -Funktion aktiviert ist.

    ![Visual Studio - Windows Phone App-Funktionen][14]

    Dadurch Ihre app Pushbenachrichtigungen empfangen kann. Ohne fehl jeder Versuch, eine Push-Benachrichtigung an die Anwendung senden.

7. Drücken Sie die `F5` , um die Anwendung auszuführen.

    Eine Registrierungsnachricht wird in der Anwendung angezeigt.

8. Schließen Sie die Anwendung.  

   >[AZURE.NOTE] Um eine Push-Benachrichtigung Spruch zu erhalten, muss die Anwendung nicht im Vordergrund ausgeführt.

##<a name="send-push-notifications-from-your-backend"></a>Push-Benachrichtigung von Ihrem Back-End-

Push-Benachrichtigung zu senden mit Notification Hubs aus jeder Backend über öffentliche <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-Schnittstelle</a>. In diesem Lernprogramm haben Sie über eine Push-Benachrichtigung senden. 

Finden Sie ein Beispiel von einer ASP.NET WebAPI Back-End-Pushbenachrichtigungen senden, die Benachrichtigungshubs integriert [Azure Notification Hubs Benutzer benachrichtigen mit .NET Back-End](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Ein Beispiel für Push-Benachrichtigung mithilfe der [REST-APIs](https://msdn.microsoft.com/library/azure/dn223264.aspx) [wie Notification Hubs von Java](./notification-hubs-java-push-notification-tutorial.md) und [wie Benachrichtigungshubs von PHP](./notification-hubs-php-push-notification-tutorial.md).

1. Mit der rechten Maustaste der Lösung, wählen Sie **Hinzufügen** und **Neues Projekt...**klicken Sie unter **Visual C#**, **Windows** und **Console Application**, und klicken Sie auf **OK**.

    ![Visual Studio - neues Projekt - Konsolenanwendungsprojekt][6]

    Dadurch wird ein neues Visual C# Konsolenanwendungsprojekt zur Projektmappe hinzugefügt. Sie können auch in einer anderen Projektmappe dazu.

4. Klicken Sie auf **Extras**, klicken Sie auf **Bibliothek Paket-Manager**und klicken Sie auf **Paket-Manager-Konsole**.

    Der Paket-Manager-Konsole angezeigt.

5.  Im Konsolenfenster **Paket-Manager** **Project Standard** auf Ihr neues Konsolenanwendungsprojekt festgelegt, und führen Sie folgenden Befehl im Konsolenfenster angezeigt:

        Install-Package Microsoft.Azure.NotificationHubs

    Einen Verweis auf Azure Notification Hubs SDK mit <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-Paket</a>hinzugefügt.

6. Öffnen der `Program.cs` -Datei und fügen Sie den folgenden `using` Anweisung:

        using Microsoft.Azure.NotificationHubs;

6. In der `Program` Klasse, fügen Sie folgende Methode:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Ersetzen Sie die `<hub name>` Platzhalter mit dem Namen des Notification-Hubs, die im Portal angezeigt wird. Ersetzen Sie den Verbindung Zeichenfolge Platzhalter auch mit der Verbindungszeichenfolge **DefaultFullSharedAccessSignature** Sie erhalten im Abschnitt "Konfigurieren der benachrichtigungshub" aufgerufen

    >[AZURE.NOTE]Achten Sie die Verbindungszeichenfolge mit **vollständigen** Zugang, nicht **hören** . Hören Zugriffszeichenfolge ist nicht berechtigt, Pushbenachrichtigungen zu senden.

4. Fügen Sie folgende Zeile in Ihre `Main` Methode:

         SendNotificationAsync();
         Console.ReadLine();

5. Windows Phone-Emulator Ausführen Ihrer Anwendung geschlossen und das Konsolenanwendungsprojekt als Projekt, und drücken Sie dann die `F5` , um die Anwendung auszuführen.

    Sie erhalten eine Push-Benachrichtigung Spruch. Tippen auf das Banner Toast lädt die app.

Möglichen Nutzlasten finden Sie in den Themen [Toast Katalog] und [Katalog nebeneinander] auf MSDN.

##<a name="next-steps"></a>Nächste Schritte

In diesem einfachen Beispiel wird Push-Benachrichtigung an alle Windows Phone 8-Geräte übertragen. 

Um bestimmte Zielgruppe finden Sie [Mit Notification Hubs Push-Benachrichtigung an Benutzer] -Lernprogramm. 

Ggf. Benutzer Interessengruppen segment erhalten Sie [Mit Notification Hubs Nachrichten senden]. 

Erfahren Sie mehr zur Verwendung von Notification Hubs in [Notification Hubs Anleitung].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express für Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Benachrichtigung Hubs Anleitung]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Benachrichtigungshubs mit der Push-Benachrichtigung an Benutzer]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[Toast Katalog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[Kachel-Katalog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Benachrichtigungshubs - Windows Phone Silverlight-Lernprogramm]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

