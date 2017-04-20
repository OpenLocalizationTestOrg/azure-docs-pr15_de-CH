<properties
    pageTitle="Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten (Universal Windows)"
    description="Verwenden Sie Azure Notification Hubs mit Tags in der Registrierung zum Senden von Nachrichten universelle Windows-App."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Übersicht

In diesem Thema veranschaulicht, wie mit Azure Notification Hubs Breaking News Benachrichtigungen zu einer Windows Store app für Windows Phone 8.1 (nicht Silverlight) übertragen. Wenn Sie Windows Phone 8.1 Silverlight abzielen, finden Sie in der [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) -Version. Zum Abschluss werden Sie registrieren für breaking News-Kategorien, die, denen Sie interessieren, und nur Pushbenachrichtigungen für diese Kategorien erhalten. Dieses Szenario ist ein allgemeines Muster für viele apps wo Benachrichtigung an Gruppen von Benutzern, die Interesse, z. B. RSS-Reader apps für Musikfans zuvor deklariert und. 

Broadcast-Szenarien werden durch ein oder mehrere _Tags_ einschließlich der Erstellung einer Registrierung im Notification Hub aktiviert. Benachrichtigung zu einem Tag gesendet, erhalten alle Geräte, die Registrierung für das Tag der Benachrichtigung. Da Tags einfach Zeichenfolgen sind, haben sie nicht vorab bereitgestellt werden. Weitere Informationen zu Tags finden Sie in der [Benachrichtigung Hubs und Tag-Ausdrücke](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Erforderliche Komponenten

Dieses Thema baut auf der app in [Erste Schritte mit Notification Hubs]erstellt[get-started]. Bevor Sie dieses Lernprogramm starten, Sie müssen bereits abgeschlossen haben [Erste Schritte mit Notification Hubs][get-started].

##<a name="add-category-selection-to-the-app"></a>Auswahl der app hinzufügen

Zunächst ist das Hinzufügen der Benutzeroberflächenelemente Ihrer vorhandenen Hauptseite, die dem Benutzer ermöglichen, sich Kategorien auswählen. Die vom Benutzer ausgewählten Kategorien werden auf dem Gerät gespeichert. Beim Starten der Anwendung wird eine geräteregistrierung als Tags im Notification Hub mit den ausgewählten Kategorien erstellt.

1. Öffnen Sie "MainPage.xaml" Projektdatei, und kopieren Sie folgenden Code in das **Grid** -Element:

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Wählen Sie die **Shared** fügen eine neue Klasse namens **Benachrichtigungen**fügen Sie den **public** -Modifizierer der Klassendefinition hinzu, und neue Codedatei fügen Sie Folgendes **verwenden hinzu** :

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Kopieren Sie folgenden Code in die neue **Benachrichtigung** -Klasse:

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Diese Klasse verwendet den lokalen Speicher Kategorien von Nachrichten speichern, die dieses Gerät erhalten. Beachten Sie, dass nicht die *RegisterNativeAsync* -Methode wir *RegisterTemplateAsync* für die Kategorien mit einer vorlagenregistrierung registrieren aufrufen. 
    
    Wir geben Sie einen Namen für die Vorlage ("SimpleWNSTemplateExample"), da wir mehr als eine Vorlage (zum Beispiel eine Popupbenachrichtigungen) und für Kacheln registrieren möchten und wir benennen zu aktualisieren oder zu löschen.

    Beachten Sie, dass ein Gerät mehrere Vorlagen mit demselben Tag registriert, eine eingehende Nachricht verwendet, die mehrere Benachrichtigungen Tag führt das Gerät (eine für jede Vorlage) übermittelt. Dieses Verhalten ist nützlich, wenn dieselbe logische Nachricht verfügt über mehrere visuelle Benachrichtigung zeigt beispielsweise ein Logo und einen Toast in einer Windows Store-Anwendung führen.

    Weitere Informationen zu Vorlagen finden Sie unter [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md).




4. Der **App** -Klasse in App.xaml.cs-Projektdatei fügen Sie die folgende Eigenschaft hinzu:

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Diese Eigenschaft wird erstellt und eine **Benachrichtigung** Instanz verwendet.

    Ersetzen Sie im obigen Code die `<hub name>` und `<connection string with listen access>` Platzhalter Namen der Hub mit der Verbindungszeichenfolge für *DefaultListenSharedAccessSignature* , die Sie zuvor erworben haben.

    > [AZURE.NOTE] Da Anmeldeinformationen, die mit einer Clientanwendung verteilt werden nicht im Allgemeinen sicher sind, sollten Sie nur den Schlüssel auf Listen mit der Clientanwendung verteilen. Access ermöglicht Ihrer Anwendung für Benachrichtigungen, aber vorhandene Registrierung registrieren geändert werden kann, hören und Benachrichtigung nicht gesendet werden. Vollzugriff-Schlüssel wird in einem sicheren Back-End-Dienst für Benachrichtigungen und Ändern der vorhandenen Registrierung verwendet.

5. Fügen Sie die folgende Zeile in Ihre MainPage.xaml.cs:

        using Windows.UI.Popups;

6. Fügen Sie in der Projektdatei MainPage.xaml.cs die folgende Methode:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Diese Methode erstellt eine Liste der Kategorien und verwendet die **Benachrichtigung** -Klasse zum Speichern der Liste im lokalen Speicher und die entsprechenden Tags mit Ihrem Haupt-Benachrichtigung registrieren. Wenn Kategorien geändert werden, wird die Registrierung mit neuen Kategorien neu erstellt.

Ihre Anwendung kann jetzt Kategorien im lokalen Speicher auf dem Gerät speichern und Änderung der Benutzer die Auswahl von Kategorien mit der Benachrichtigung registrieren.

##<a name="register-for-notifications"></a>Registrieren Sie sich für Benachrichtigung

Folgendermaßen registrieren mit der Benachrichtigung beim Start der Kategorien, die im lokalen Speicher gespeichert.

> [AZURE.NOTE] Da der Channel-URI zugewiesen durch Windows Notification Service (WNS) jederzeit ändern kann, sollten Sie für Benachrichtigungen häufig zu Fehlern Benachrichtigung registrieren. Dieses Beispiel registriert für Benachrichtigung bei jedem Starten der Anwendung. Für apps, die häufig ausgeführt werden überspringen mehrmals täglich wahrscheinlich Sie Registrierung um Bandbreite zu sparen, wenn Sie länger als ein Tag nach der vorherigen Registrierung verstrichen ist.

1. App.xaml.cs öffnen und aktualisieren die **InitNotificationsAsync** Methode der `notifications` Klasse abonnieren auf Kategorien basiert.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    Dadurch wird sichergestellt, dass bei jedem Start die Anwendung die Kategorien aus dem lokalen Speicher ruft und eine Registeration für diese Kategorien fordert. **InitNotificationsAsync** -Methode wurde als Teil der [Erste Schritte mit Notification Hubs] [ get-started] Tutorial.

3. In der Projektdatei MainPage.xaml.cs *OnNavigatedTo* -Methode fügen Sie den folgenden Code hinzu:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }

    Die Hauptseite basierend auf dem Status der gespeicherten Kategorien aktualisiert.

Die Anwendung ist jetzt vollständig und Kategorien im lokalen Speicher Geräts mit dem benachrichtigungshub registriert Änderung der Benutzer die Auswahl von Kategorien speichern kann. Als Nächstes definieren wir Backend, die Kategorie Benachrichtigungen an diese Anwendung senden können.

##<a name="sending-tagged-notifications"></a>Markierte Benachrichtigungen senden

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Führen Sie die Anwendung und generieren Benachrichtigungen

1. In Visual Studio mit F5 kompilieren und die Anwendung zu starten.

    ![][1]

    Beachten Sie, dass app-UI mehrere schaltet bereitstellt, die Sie können Kategorien abonnieren.

2. Aktivieren Sie eine oder mehrere Kategorien schaltet dann auf **Abonnieren**.

    Die app konvertiert die ausgewählten Kategorien in Tags und fordert eine neue geräteregistrierung für den ausgewählten Tags von Notification Hub. Die registrierten Kategorien werden zurückgegeben und in einem Dialogfeld angezeigt.

    ![][19]

4. Senden Sie eine neue Benachrichtigung aus dem Back-End auf folgenden Arten:

    + **Konsole:** starten die Konsole app.

    + **Java-PHP:** Ihre app-Skript ausführen.

    Benachrichtigung für die ausgewählten Kategorien werden als Popupbenachrichtigungen.

    ![][14]

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir gelernt übertragen von Nachrichten nach Kategorie. Sollten Sie einen der folgenden Lernprogramme, in denen andere erweiterten Benachrichtigungshubs Szenarien vorgestellt:

+ [Verwenden Sie Notification Hubs lokalisierte Nachrichten übertragen]

    Informationen Sie zum Erweitern der aktuelle News app senden lokalisierte Notifications zu aktivieren.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Verwenden Sie Notification Hubs lokalisierte Nachrichten übertragen]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
