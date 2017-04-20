<properties
    pageTitle="Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten (Windows Phone)"
    description="Verwenden Sie Azure Notification Hubs mit Tags in Registrierung senden Nachrichten an eine Windows Phone-app."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Übersicht

In diesem Thema veranschaulicht die Azure Notification Hubs Übertragung Breaking News Benachrichtigung einer Windows Phone 8.0-8.1 Silverlight-Anwendung verwenden. Wenn Sie Windows Store oder Windows Phone 8.1 app abzielen, finden Sie die [Universal Windows](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) Version. Zum Abschluss werden Sie registrieren für breaking News-Kategorien, die, denen Sie interessieren, und nur Pushbenachrichtigungen für diese Kategorien erhalten. Dieses Szenario ist ein allgemeines Muster für viele apps wo Benachrichtigung an Gruppen von Benutzern, die zuvor interessieren, z. B. RSS-Reader, apps für Musik usw. deklariert haben.

Broadcast-Szenarien werden durch ein oder mehrere _Tags_ einschließlich der Erstellung einer Registrierung im Notification Hub aktiviert. Benachrichtigung zu einem Tag gesendet, erhalten alle Geräte, die Registrierung für das Tag der Benachrichtigung. Da Tags einfach Zeichenfolgen sind, haben sie nicht vorab bereitgestellt werden. Weitere Informationen zu Tags finden Sie in der [Benachrichtigung Hubs und Tag-Ausdrücke](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Erforderliche Komponenten

Dieses Thema baut auf in [Erste Schritte mit Notification Hubs]erstellten Anwendung. Bevor Sie dieses Lernprogramm starten, müssen bereits [Erste Schritte mit Notification Hubs]abgeschlossen sein.

##<a name="add-category-selection-to-the-app"></a>Auswahl der app hinzufügen

Zunächst ist das Hinzufügen der Benutzeroberflächenelemente Ihrer vorhandenen Hauptseite, die dem Benutzer ermöglichen, sich Kategorien auswählen. Die vom Benutzer ausgewählten Kategorien werden auf dem Gerät gespeichert. Beim Starten der Anwendung wird eine geräteregistrierung als Tags im Notification Hub mit den ausgewählten Kategorien erstellt.

1. Öffnen Sie die Projektdatei MainPage.xaml wieder ein **Raster** Elemente mit dem Namen `TitlePanel` und `ContentPanel` mit dem folgenden Code:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. Im Projekt eine neue Klasse namens **Benachrichtigungen**der **public** -Modifizierer auf die Klassendefinition und dann fügen Sie Folgendes **verwenden** neue Codedatei hinzu:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Kopieren Sie folgenden Code in die neue **Benachrichtigung** -Klasse:

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Diese Klasse verwendet den isolierten Speicher, die Kategorien von Nachrichten speichern dieses Gerät erhalten. Es enthält auch Methoden für diese Kategorien mithilfe einer [Vorlage](notification-hubs-templates-cross-platform-push-messages.md) Benachrichtigung Registrierung registrieren.


4. Der **App** -Klasse in App.xaml.cs-Projektdatei fügen Sie die folgende Eigenschaft hinzu. Ersetzen Sie die `<hub name>` und `<connection string with listen access>` Platzhalter Namen der Hub mit der Verbindungszeichenfolge für *DefaultListenSharedAccessSignature* , die Sie zuvor erworben haben.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Da Anmeldeinformationen, die mit einer Clientanwendung verteilt werden nicht im Allgemeinen sicher sind, sollten Sie nur den Schlüssel auf Listen mit der Clientanwendung verteilen. Access ermöglicht Ihrer Anwendung für Benachrichtigungen, aber vorhandene Registrierung registrieren geändert werden kann, hören und Benachrichtigung nicht gesendet werden. Vollzugriff-Schlüssel wird in einem sicheren Back-End-Dienst für Benachrichtigungen und Ändern der vorhandenen Registrierung verwendet.

5. Fügen Sie die folgende Zeile in Ihre MainPage.xaml.cs:

        using Windows.UI.Popups;

6. Fügen Sie in der Projektdatei MainPage.xaml.cs die folgende Methode:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Diese Methode erstellt eine Liste der Kategorien und verwendet die **Benachrichtigung** -Klasse zum Speichern der Liste im lokalen Speicher und die entsprechenden Tags mit Ihrem Haupt-Benachrichtigung registrieren. Wenn Kategorien geändert werden, wird die Registrierung mit neuen Kategorien neu erstellt.

Ihre Anwendung kann jetzt Kategorien im lokalen Speicher auf dem Gerät speichern und Änderung der Benutzer die Auswahl von Kategorien mit der Benachrichtigung registrieren.

##<a name="register-for-notifications"></a>Registrieren Sie sich für Benachrichtigung

Folgendermaßen registrieren mit der Benachrichtigung beim Start der Kategorien, die im lokalen Speicher gespeichert.

> [AZURE.NOTE] Da der Channel-URI zugewiesen durch Microsoft Push Notification Service (MPNS) jederzeit ändern kann, sollten Sie für Benachrichtigungen häufig zu Fehlern Benachrichtigung registrieren. Dieses Beispiel registriert für Benachrichtigung bei jedem Starten der Anwendung. Für apps, die häufig ausgeführt werden überspringen mehrmals täglich wahrscheinlich Sie Registrierung um Bandbreite zu sparen, wenn Sie länger als ein Tag nach der vorherigen Registrierung verstrichen ist.


1. Öffnen Sie App.xaml.cs **Application_Launching** Methode fügen Sie **asynchron** Modifizierer hinzu und Ersetzen Sie den Bestätigungscode Benachrichtigungshubs, den Sie in [Erste Schritte mit Notification Hubs] mit dem folgenden Code hinzugefügt:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    Dadurch wird sichergestellt, dass bei jedem Start die Anwendung die Kategorien aus dem lokalen Speicher ruft und eine Registrierung für diese Kategorien fordert.

2. Fügen Sie den folgenden Code, der die **OnNavigatedTo** -Methode implementiert, in der Projektdatei MainPage.xaml.cs:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
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

    ![][2]

3. Führen Sie nach der Bestätigung, dass Ihre Kategorien Abonnement abgeschlossen wurden die Konsole app Benachrichtigungen für jede Kategorie. Stellen Sie sicher, dass nur eine Benachrichtigung für die Kategorien angezeigt, die Sie abonniert haben.

    ![][3]

In diesem Thema haben.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Erste Schritte mit Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

