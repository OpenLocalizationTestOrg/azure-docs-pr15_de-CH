<properties
    pageTitle="Benachrichtigungshubs Nachrichten Tutorial - Android"
    description="Informationen Sie zum Azure Service Bus Notification Hubs verwenden, um wichtige Nachrichten Meldungen an Geräte senden."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>Übersicht

In diesem Thema veranschaulicht die Azure Notification Hubs Breaking News Benachrichtigungen ein Android Übertragung verwenden. Zum Abschluss werden Sie registrieren für breaking News-Kategorien, die, denen Sie interessieren, und nur Pushbenachrichtigungen für diese Kategorien erhalten. Dieses Szenario ist ein allgemeines Muster für viele apps wo Benachrichtigung an Gruppen von Benutzern, die zuvor interessieren, z. B. RSS-Reader, apps für Musik usw. deklariert haben.

Broadcast-Szenarien werden durch ein oder mehrere _Tags_ einschließlich der Erstellung einer Registrierung im Notification Hub aktiviert. Benachrichtigung zu einem Tag gesendet, erhalten alle Geräte, die Registrierung für das Tag der Benachrichtigung. Da Tags einfach Zeichenfolgen sind, haben sie nicht vorab bereitgestellt werden. Weitere Informationen zu Tags finden Sie in der [Benachrichtigung Hubs und Tag-Ausdrücke](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Erforderliche Komponenten

Dieses Thema baut auf der app in [Erste Schritte mit Notification Hubs]erstellt[get-started]. Bevor Sie dieses Lernprogramm starten, Sie müssen bereits abgeschlossen haben [Erste Schritte mit Notification Hubs][get-started].

##<a name="add-category-selection-to-the-app"></a>Auswahl der app hinzufügen

Der erste Schritt ist Ihre vorhandenen Hauptaktivität, mit denen den Benutzer Kategorien registrieren UI-Elemente hinzufügen. Die vom Benutzer ausgewählten Kategorien werden auf dem Gerät gespeichert. Beim Starten der Anwendung wird eine geräteregistrierung als Tags im Notification Hub mit den ausgewählten Kategorien erstellt.

1. Öffnen Sie die Datei res/layout/activity_main.xml, und Ersetzen Sie den Inhalt durch Folgendes:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Öffnen Sie die Datei res/values/strings.xml, und fügen Sie folgende Zeilen:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Main_activity.xml grafisch Layout sollte jetzt folgendermaßen aussehen:

    ![][A1]

3. Erstellen Sie nun eine Klasse **Benachrichtigungen** in demselben Paket wie die **MainActivity** -Klasse.

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Diese Klasse verwendet den lokalen Speicher Kategorien von Nachrichten speichern, die dieses Gerät erhalten. Es enthält auch Methoden für diese Kategorien registrieren.


4. Entfernen Sie in der **MainActivity** -Klasse private Felder für **NotificationHub** und **GoogleCloudMessaging**, und fügen Sie ein Feld für **Benachrichtigung**:

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Entfernen Sie in der Methode **OnCreate** die Initialisierung **Hub** und die **RegisterWithNotificationHubs** -Methode. Fügen Sie die folgenden Zeilen die Initialisierung einer Instanz der Klasse **benachrichtigt** . 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`und `HubListenConnectionString` mit bereits festgelegt werden die `<hub name>` und `<connection string with listen access>` Platzhalter Namen der Hub mit der Verbindungszeichenfolge für *DefaultListenSharedAccessSignature* , die Sie zuvor erworben haben.

    > [AZURE.NOTE] Da Anmeldeinformationen, die mit einer Clientanwendung verteilt werden nicht im Allgemeinen sicher sind, sollten Sie nur den Schlüssel auf Listen mit der Clientanwendung verteilen. Access ermöglicht Ihrer Anwendung für Benachrichtigungen, aber vorhandene Registrierung registrieren geändert werden kann, hören und Benachrichtigung nicht gesendet werden. Vollzugriff-Schlüssel wird in einem sicheren Back-End-Dienst für Benachrichtigungen und Ändern der vorhandenen Registrierung verwendet.


6. Fügen Sie dann die folgenden Einfuhren und `subscribe` Methode zum Behandeln der Schaltfläche "Anmelden" klicken Sie auf Ereignis:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Diese Methode erstellt eine Liste der Kategorien und verwendet die **Benachrichtigung** -Klasse zum Speichern der Liste im lokalen Speicher und die entsprechenden Tags mit Ihrem Haupt-Benachrichtigung registrieren. Wenn Kategorien geändert werden, wird die Registrierung mit neuen Kategorien neu erstellt.

Ihre Anwendung kann jetzt Kategorien im lokalen Speicher auf dem Gerät speichern und Änderung der Benutzer die Auswahl von Kategorien mit der Benachrichtigung registrieren.

##<a name="register-for-notifications"></a>Registrieren Sie sich für Benachrichtigung

Folgendermaßen registrieren mit der Benachrichtigung beim Start der Kategorien, die im lokalen Speicher gespeichert.

> [AZURE.NOTE] Da Attribute zugewiesen von Google Cloud Messaging (GCM) jederzeit ändern kann, sollten Sie für Benachrichtigungen häufig zu Fehlern Benachrichtigung registrieren. Dieses Beispiel registriert für Benachrichtigung bei jedem Starten der Anwendung. Für apps, die häufig ausgeführt werden überspringen mehrmals täglich wahrscheinlich Sie Registrierung um Bandbreite zu sparen, wenn Sie länger als ein Tag nach der vorherigen Registrierung verstrichen ist.


1. Fügen Sie den folgenden Code am Ende der **OnCreate** -Methode in der **MainActivity** -Klasse:

        notifications.subscribeToCategories(notifications.retrieveCategories());

    Dadurch wird sichergestellt, dass bei jedem Start die Anwendung die Kategorien aus dem lokalen Speicher ruft und eine Registeration für diese Kategorien fordert. 

2. Aktualisieren der `onStart()` Methode der `MainActivity` -Klasse:

    @OverrideProtected void onStart() {super.onStart();      IsVisible = True;

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    Basierend auf dem Status der gespeicherten Kategorien Hauptaktivität aktualisiert.

Die Anwendung ist jetzt vollständig und Kategorien im lokalen Speicher Geräts mit dem benachrichtigungshub registriert Änderung der Benutzer die Auswahl von Kategorien speichern kann. Als Nächstes definieren wir Backend, die Kategorie Benachrichtigungen an diese Anwendung senden können.

##<a name="sending-tagged-notifications"></a>Markierte Benachrichtigungen senden

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Führen Sie die Anwendung und generieren Benachrichtigungen

1. Android Studio erstellen Sie die Anwendung, und starten sie auf einem Gerät oder Emulator.

    Beachten Sie, dass app-UI mehrere schaltet bereitstellt, die Sie können Kategorien abonnieren.

2. Aktivieren Sie eine oder mehrere Kategorien schaltet dann auf **Abonnieren**.

    Die app konvertiert die ausgewählten Kategorien in Tags und fordert eine neue geräteregistrierung für den ausgewählten Tags von Notification Hub. Registrierten Kategorien werden zurückgegeben und in eine Popupbenachrichtigung angezeigt.

4. Senden Sie eine neue Benachrichtigung mit .NET Konsole app.  Alternativ können Sie mithilfe der Registerkarte Debuggen des Haupt-Benachrichtigung in [Azure-Verwaltungsportal]markierten Vorlage Benachrichtigung senden.

    Benachrichtigung für die ausgewählten Kategorien werden als Popupbenachrichtigungen.

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir gelernt übertragen von Nachrichten nach Kategorie. Sollten Sie einen der folgenden Lernprogramme, in denen andere erweiterten Benachrichtigungshubs Szenarien vorgestellt:

+ [Verwenden Sie Notification Hubs lokalisierte Nachrichten übertragen]

    Informationen Sie zum Erweitern der aktuelle News app senden lokalisierte Notifications zu aktivieren.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Verwenden Sie Notification Hubs lokalisierte Nachrichten übertragen]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure-Verwaltungsportal]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
