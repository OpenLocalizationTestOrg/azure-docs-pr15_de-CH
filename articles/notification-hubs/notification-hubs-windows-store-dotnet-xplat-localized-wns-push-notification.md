<properties
    pageTitle="Benachrichtigungshubs lokalisiert Breaking News-Lernprogramm"
    description="Erfahren Sie, wie Azure Notification Hubs mit lokalisierten aktuelle News-Benachrichtigung senden."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Verwenden Sie Notification Hubs lokalisierte Nachrichten senden

> [AZURE.SELECTOR]
- [Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>Übersicht

In diesem Thema veranschaulicht die Funktion **Vorlage** von Azure Notification Hubs übertragen Gerät mit Sprache lokalisiert wurden wichtige Nachrichten Meldungen. In diesem Lernprogramm beginnen Sie mit der Windows Store-app [Mit Notification Hubs zum Senden von Nachrichten]erstellt. Nach Abschluss des Vorgangs wird möglicherweise für Kategorien, die Sie registrieren, eine Sprache, die Benachrichtigungen erhalten und nur Pushbenachrichtigungen für ausgewählten Kategorien in dieser Sprache angeben.


Es gibt zwei Teile dieses Szenarios:

- Windows Store-app kann Client Geräte an einer Sprache und andere wichtige News-Kategorien abonnieren.

- Back-End sendet die Benachrichtigung mit dem **Tag-** und **Vorlage** Funktionen Azure Notification Hubs.



##<a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen bereits abgeschlossen haben das Lernprogramm [Verwendet Notification Hubs Nachrichten senden] und den Code zur Verfügung, da diese Anleitung direkt auf Code aufbaut.

Sie benötigen auch Visual Studio 2012 oder höher.


##<a name="template-concepts"></a>Konzepte

[Mit Notification] Hubs Nachrichten senden erstellt Sie eine Anwendung, **Tags** zum Abonnieren von Benachrichtigungen für verschiedene neue Kategorien.
Viele apps jedoch mehrere Märkte und Lokalisierung. Dies bedeutet, dass der Inhalt der Benachrichtigung selbst lokalisiert und an den richtigen Satz an Geräte übermittelt.
In diesem Thema zeigen wir, wie Sie die **Vorlage** Funktion der Benachrichtigung einfach lokalisierte Breaking News Benachrichtigungen.

Hinweis: können lokalisierte Benachrichtigungen werden mehrere Versionen jedes Tag erstellen. Beispielsweise unterstützen Mandarin, Englisch und Französisch, müssten drei verschiedenen Tags für Nachrichten: "World_en", "World_fr" und "World_ch". Wir müssen eine lokalisierte Version der World News einzelnen Tags an. In diesem Thema verwenden wir die Verbreitung von Tags und die Senden mehrerer Nachrichten zu Vorlagen.

Auf einer hohen Ebene sind Vorlagen angeben, wie ein bestimmtes Gerät eine Benachrichtigung erhalten sollen. Die Vorlage gibt das genaue Nutzlast Format Teil der Nachricht Ihre app Backend sind-Eigenschaften auf. In diesem Fall werden wir eine Gebietsschema unabhängig, alle unterstützte Sprachen Nachricht:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Dann stellen wir sicher, dass Geräte mit einer Vorlage auf die richtige Eigenschaft zu registrieren. Beispielsweise wird eine Windows Store-app, die eine einfache Toast Meldung für die folgende Vorlage mit entsprechenden Tags registrieren:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Vorlagen sind ein sehr leistungsfähiges Feature, das Sie im Artikel [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md) erfahren können. 


##<a name="the-app-user-interface"></a>Der app-Benutzeroberfläche

Wir werden jetzt im Thema [Mit Notification Hubs Nachrichten senden] lokalisierte Vorlagen Nachrichten senden erstellte Breaking News app ändern.

In Windows Store-Apps:

Ändern Sie Ihre MainPage.xaml auf ein Gebietsschema Kombinationsfeld:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>Erstellen von Windows Store-Clientanwendung

1. Die Methoden *StoreCategoriesAndSubscribe* und *SubscribeToCateories* in der Klasse Benachrichtigungen fügen Sie Gebietsschemaparameter hinzu.

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Anstatt die Methode *RegisterNativeAsync* wir *RegisterTemplateAsync*aufrufen: wir eine Benachrichtigung Format die Vorlage davon Gebietsschema hängt registrieren. Wir geben Sie einen Namen für die Vorlage ("LocalizedWNSTemplateExample"), da wir mehr als eine Vorlage (zum Beispiel eine Popupbenachrichtigungen) und für Kacheln registrieren möchten und wir benennen zu aktualisieren oder zu löschen.

    Beachten Sie, dass ein Gerät mehrere Vorlagen mit demselben Tag registriert, eine eingehende Nachricht verwendet, die mehrere Benachrichtigungen Tag führt das Gerät (eine für jede Vorlage) übermittelt. Dieses Verhalten ist nützlich, wenn dieselbe logische Nachricht verfügt über mehrere visuelle Benachrichtigung zeigt beispielsweise ein Logo und einen Toast in einer Windows Store-Anwendung führen.

2. Fügen Sie die folgende Methode gespeicherte Gebietsschema abrufen:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. Aktualisieren Sie Ihre MainPage.xaml.cs Schaltfläche Behandlungsroutine durch den aktuellen Wert des Kombinationsfelds Gebietsschema und dem Aufruf der Klasse Benachrichtigungen bereitstellen, wie dargestellt:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. In der Datei App.xaml.cs stellen aktualisieren die `InitNotificationsAsync` -Methode das Gebietsschema abrufen und bei:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Lokalisierte Benachrichtigung von Ihrem Back-end

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
