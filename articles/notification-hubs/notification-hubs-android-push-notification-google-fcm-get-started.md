<properties
    pageTitle="Senden von Pushbenachrichtigungen Android mit Azure Notification Hubs FB Cloud Messaging | Microsoft Azure"
    description="In diesem Lernprogramm lernen Sie Azure Notification Hubs und FB Cloud Messaging Pushbenachrichtigungen, Android-Geräte verwenden."
    services="notification-hubs"
    documentationCenter="android"
    keywords="Push Notifications Push-Benachrichtigung android Push-Benachrichtigung, Fcm, FB cloud messaging"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Senden von Pushbenachrichtigungen Android mit Azure Notification Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Übersicht

> [AZURE.IMPORTANT] Dieses Thema veranschaulicht Pushbenachrichtigungen mit Google FB Cloud Messaging (FCM). Wenn Sie Google Cloud Messaging (GCM) weiterhin verwenden möchten, finden Sie unter [Senden von Pushbenachrichtigungen Android Azure Notification Hubs mit GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).

In diesem Lernprogramm wird veranschaulicht, wie Azure Notification Hubs und FB Cloud Messaging mit Push-Benachrichtigung an eine Android Anwendung senden.
Erstellen Sie eine leere Android, die Pushbenachrichtigungen mit FB Cloud Messaging (FCM) erhält.



[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Der vollständige Code für dieses Lernprogramm kann von GitHub heruntergeladen werden [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).


##<a name="prerequisites"></a>Erforderliche Komponenten

> [AZURE.IMPORTANT] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

- Dieses Lernprogramm erfordert zusätzlich eine aktive Azure erwähnt die neueste Version von [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).
- Android 2.3 oder höher für FB Cloud Messaging.
- Google Repository Revision 27 oder höher ist erforderlich für FB Cloud Messaging.
- Google Play Services 9.0.2 oder höher für FB Cloud Messaging.
- Dieses Lernprogramm ist eine Voraussetzung für alle anderen Notification Hubs Lernprogramme für apps.


##<a name="create-a-new-android-studio-project"></a>Erstellen eines neuen Projekts für Android Studio

1. Starten Sie in Android Studio ein neues Android Studio-Projekt.

    ![Android Studio - neues Projekt](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)

2. Wählen Sie den Formularfaktor **Telefon- und Tablet** und **Minimum SDK** , die Sie unterstützen möchten. Klicken Sie auf **Weiter**.

    ![Android Studio - Projekt erstellen workflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)

3. **Leere** Aktivität für die Hauptaktivität, klicken Sie auf **Weiter**und dann auf **Fertig stellen**.


##<a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Erstellen Sie ein Projekt, das FB Cloud Messaging unterstützt

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Konfigurieren Sie einen neue benachrichtigungshub

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6 Wählen Sie im Blatt **Einstellungen** des Notification Haupt **Notification Services** und **Google (GCM)**. Geben Sie Schlüssel des FCM zuvor kopierten [FB-Konsole](https://firebase.google.com/console/) und klicken Sie auf **Speichern**.

&emsp;&emsp;![Azure Notification Hubs - Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Der benachrichtigungshub ist jetzt konfiguriert FB Cloud Messagin und haben die Verbindungszeichenfolgen für sowohl Ihre Anwendung zum Empfangen und Senden von Pushbenachrichtigungen registrieren.

##<a id="connecting-app"></a>Ihre app Hub Benachrichtigung verbinden

###<a name="add-google-play-services-to-the-project"></a>Google Play Dienste zum Projekt hinzufügen

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Azure Notification Hubs Bibliotheken hinzufügen


1. In der `Build.Gradle` für die **app**-Datei, fügen Sie die folgenden Zeilen im Abschnitt **Dependencies** .

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Fügen Sie das folgende Repository nach Abschnitt **Dependencies** .

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>Aktualisiert die AndroidManifest.xml.


1. FCM Unterstützung müssen wir einen Instanz-ID Listenerdienst in unserem Code implementieren [Registrierung Token erhalten](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) mit [Google FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId)verwendet wird. In diesem Lernprogramm nennen wir die Klasse `MyInstanceIDService`. 
 
    Fügen Sie die folgende Service Definition innerhalb der Datei AndroidManifest.xml die `<application>` Tag. 

        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>



2. Nach unserer FCM Registrierung Token aus der FirebaseInstanceId-API Erhalt wird es sich [mit Azure Notification Hub](notification-hubs-push-notification-registration-management.md)verwendet. Wir unterstützen diese Registrierung im Hintergrund mit einer `IntentService` mit dem Namen `RegistrationIntentService`. Dieser Dienst wird auch zum Aktualisieren unserer FCM Registrierung Token sein.
 
    Fügen Sie die folgende Service Definition innerhalb der Datei AndroidManifest.xml die `<application>` Tag. 

        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>



3. Definieren wir auch einen Receiver Benachrichtigungen. Fügen Sie die folgende Empfänger Definition innerhalb der Datei AndroidManifest.xml die `<application>` Tag. Ersetzen der `<your package>` Platzhalter mit der Ihren tatsächlichen Paketnamen am oberen Rand der `AndroidManifest.xml` Datei.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Fügen Sie den folgenden erforderlichen FCM bezogene Berechtigungen unten die `</application>` Tag. Ersetzen Sie `<your package>` mit Paketnamen am oberen Rand der `AndroidManifest.xml` Datei.

    Weitere Informationen zu Berechtigungen finden Sie unter [Setup GCM Clientanwendung für Android](https://developers.google.com/cloud-messaging/android/client#manifest) und [Migration GCM Clientanwendung für Android FB Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />


### <a name="adding-code"></a>Hinzufügen von code


1. Erweitern Sie in der Projektansicht **app** > **Src** > **wichtigsten** > **Java**. Der Paketordner unter **Java**Maustaste klicken Sie auf **neu**und klicken Sie auf **Java-Klasse**. Fügen Sie eine neue Klasse mit dem Namen `NotificationSettings`. 

    ![Android Studio - neue Java-Klasse](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)

    Diese drei aktualisieren Sie Platzhalter in den folgenden Code für die `NotificationSettings` Klasse:
    * **SenderId**: die Sender-Id in der **Cloud Messaging** Registerkarte Projekt Einstellungen [FB Konsole](https://firebase.google.com/console/)abgerufen.
    * **HubListenConnectionString**: die **DefaultListenAccessSignature** -Verbindungszeichenfolge für den Hub. Diese Verbindungszeichenfolge können **Richtlinien** auf Blatt **Einstellungen** des Haupt- [Azure-Portal].
    * **HubName**: Verwenden Sie den Namen des Haupt-Benachrichtigung, die in das Blade Hub [Azure-Portal]angezeigt.

    `NotificationSettings`Code:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
        }

2. Mit den Schritten fügen Sie eine weitere Klasse mit dem Namen `MyInstanceIDService`. Instanz-ID Listener Service Implementierung werden.

    Der Code für diese Klasse aufrufen unserer `IntentService` im Hintergrund [FCM-Token zu aktualisieren](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) .

        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;
        
        
        public class MyInstanceIDService extends FirebaseInstanceIdService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.d(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Fügen Sie eine weitere Klasse dem Projekt mit dem Namen `RegistrationIntentService`. Wird die Implementierung für unserer `IntentService` behandelt, die [FCM-Token zu aktualisieren](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) und [mit der Benachrichtigung registrieren](notification-hubs-push-notification-registration-management.md).

    Verwenden Sie den folgenden Code für diese Klasse.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {
        
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
        
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
        
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
        
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
        
                    // Check if the token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
        
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
        
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
        
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete registration", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. In der `MainActivity` Klasse, fügen Sie den folgenden `import` Aussagen über der Klassendeklaration.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. Fügen Sie die folgenden privaten Member am Anfang der Klasse. Diese [Verfügbarkeit von Google Play Services wie Google empfohlen](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk)werden.

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. In der `MainActivity` Klasse, fügen Sie die folgende Methode für die Verfügbarkeit von Google Play Services. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. In der `MainActivity` Klasse, fügen Sie folgenden Code für Google Play Services vor dem Aufrufen überprüft die `IntentService` zu Ihrer Registrierung FCM token mit der Benachrichtigung registrieren.

        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService to register this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. In der `OnCreate` Methode der `MainActivity` Klasse, fügen Sie folgenden Code, um die Registrierung zu starten, wenn Aktivität erstellt wird.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. Fügen Sie diese zusätzlichen Methoden, die `MainActivity` app-Status und Status in Ihrer Anwendung zu überprüfen.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. Die `ToastNotify` -Methode verwendet *"Hello World"* `TextView` Steuerelement Berichtstatus und Benachrichtigung dauerhaft in der app. Fügen Sie in Ihr Layout activity_main.xml die folgende Id für das Steuerelement.

        android:id="@+id/text_hello"

11. Anschließend fügen wir eine Unterklasse für unsere Empfänger, die wir in der AndroidManifest.xml definiert. Fügen Sie eine weitere Klasse dem Projekt mit dem Namen `MyHandler`.

12. Hinzufügen die folgende Import-Anweisung am Anfang der `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Fügen Sie folgenden Code für die `MyHandler` Klasse macht eine Unterklasse von `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Dieser Code überschreibt die `OnReceive` -Methode auf, sodass der Handler melden Benachrichtigung empfangen werden. Der Handler auch sendet die Push-Benachrichtigung an Android Notification Manager der `sendNotification()` Methode. Die `sendNotification()` -Methode ausgeführt werden soll, wenn die Anwendung nicht ausgeführt wird und eine Benachrichtigung erhalten.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. Android Studio in der Menüleiste klicken Sie auf **Erstellen** > **Projekt neu erstellen** , um sicherzustellen, dass keine Fehler im Code vorhanden sind.
15. Führen Sie die Anwendung auf dem Gerät und überprüfen Sie, ob er erfolgreich mit der Benachrichtigung registriert. 

    > [AZURE.NOTE] Registrierung beim anfänglichen Start bis zum Fehlschlagen der `onTokenRefresh()` Methode der Instanz-ID-Dienst aufgerufen. Die Aktualisierung sollte Intiate eine Anmeldung mit der Benachrichtigung.

##<a name="sending-push-notifications"></a>Push-Benachrichtigung senden

Sie können testen, Pushbenachrichtigungen in Ihrer Anwendung über das [Azure-Portal] - suchen im Abschnitt **Problembehandlung** Blade-Hub senden empfangen, wie unten dargestellt.

![Azure Notification Hubs - Test senden](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Optional) Push-Benachrichtigung direkt

>[AZURE.IMPORTANT] In diesem Beispiel Senden von Benachrichtigungen von der Clientanwendung erfolgt nur zu. Da dies erfordert die `DefaultFullSharedAccessSignature` um auf die Clientanwendung, macht Ihre benachrichtigungshub das Risiko, dass Benutzer nicht autorisierte Benachrichtigungen an die Clients zugreifen kann.

Normalerweise senden Sie Nachrichten mit einem Back-End-Server. In einigen Fällen möchten Sie Push-Benachrichtigung direkt von der Clientanwendung gesendet werden. Dieser Abschnitt erläutert die Benachrichtigungen vom Client mithilfe der [Azure Notification Hub REST-API](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. Erweitern Sie in Android Studio Projektansicht **App** > **Src** > **wichtigsten** > **Res** > **Layout**. Öffnen der `activity_main.xml` Layout-Datei und klicken Sie auf Registerkarte **Text** aktualisieren den Text-Inhalt der Datei. Aktualisieren mit dem folgenden Code die neue hinzufügt `Button` und `EditText` Steuerelemente für Push Nachrichten an den benachrichtigungshub senden. Fügen Sie diesen Code unten vor `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. Erweitern Sie in Android Studio Projektansicht **App** > **Src** > **wichtigsten** > **Res** > **Werte**. Öffnen der `strings.xml` und fügen die neuen referenziert Zeichenfolgenwerte `Button` und `EditText` Steuerelemente. Fügen Sie diese am Ende der Datei unmittelbar vor dem `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. In der `NotificationSetting.java` -Datei, fügen Sie die folgende Einstellung der `NotificationSettings` Klasse.

    Update `HubFullAccess` mit **DefaultFullSharedAccessSignature** -Verbindungszeichenfolge für den Hub. Diese Verbindungszeichenfolge kann aus dem [Azure-Portal] Blatt **Einstellungen** für Ihren Notification Hub auf **Richtlinien** kopiert werden.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";

4. In der `MainActivity.java` Datei, fügen Sie den folgenden `import` Aussagen über die `MainActivity` Klasse.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. In der `MainActivity.java` -Datei, fügen Sie die folgenden Elemente am Anfang von der `MainActivity` Klasse.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. Erstellen Sie eine Software Zugriff (SaS) Signaturtoken um eine POST-Anforderung der benachrichtigungshub Nachrichten authentifizieren. Dies erfolgt durch Analyse der wichtigsten Daten aus der Verbindungszeichenfolge und zum Erstellen von SaS-Token wie [Allgemeine Konzepte](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST-API-Referenz. Der folgende Code ist ein beispielimplementierung.

    In `MainActivity.java`, fügen Sie die folgende Methode, um die `MainActivity` Klasse Zeichenfolge analysiert.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. In `MainActivity.java`, fügen Sie die folgende Methode, um die `MainActivity` -Klasse ein Authentifizierungstoken SaS erstellt.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. In `MainActivity.java`, fügen Sie folgende Methode, die `MainActivity` Klasse den Klick auf die **Benachrichtigung senden** und mit der integrierten REST API Push-Benachrichtigung an den Hub senden.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Testen Ihre Anwendung

####<a name="push-notifications-in-the-emulator"></a>Pushbenachrichtigungen im emulator

Wenn Sie Pushbenachrichtigungen in einem Emulator testen möchten, stellen Sie sicher, dass der Emulator Google API-Ebene unterstützt, die Sie für Ihre Anwendung haben. Wenn das Bild nicht systemeigene Google APIs unterstützt, Sie erhalten die **SERVICE\_nicht\_verfügbar** Ausnahme.

Darüber hinaus sicherstellen, dass Ihre laufenden Emulator unter **Einstellungen**Ihr Google-Konto hinzugefügt haben > **Konten**. Andernfalls möglicherweise Versuch GCM registrieren in der **Authentifizierung\_Fehler** Ausnahme.

####<a name="running-the-application"></a>Ausführen der Anwendung

1. Führen Sie die Anwendung und beachten Sie, dass die Identifikationsnummer für eine erfolgreiche Registrierung gemeldet wird.

    ![Testen auf Android - Registrierung](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)

2. Geben Sie eine Benachrichtigung an alle Android-Geräte, die mit dem Hub registriert haben.

    ![Testen auf Android - Senden einer Nachricht](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)

3. Drücken Sie die **Benachrichtigung senden**. Zeigt alle Geräte, die app ein `AlertDialog` mit Push-Benachrichtigung. Geräte, die nicht die app registriert wurden für Pushbenachrichtigungen erhalten eine Benachrichtigung im Benachrichtigungssystem Android. Diese können Wischen Sie von der oberen linken Ecke angezeigt werden.

    ![Testen auf Android - Benachrichtigung](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

##<a name="next-steps"></a>Nächste Schritte

Wir empfehlen Lernprogramm [Verwendet Notification Hubs Push-Benachrichtigung an Benutzer] im nächsten Schritt. Es zeigt Ihnen wie Benachrichtigungen ein ASP.NET Backend mit Tags auf bestimmte Benutzer.

Ggf. Benutzer Interessengruppen segment checken Sie Lernprogramm [Verwendet Notification Hubs Nachrichten senden] .

Weitere allgemeine Informationen zu Notification Hubs finden Sie unter [Notification Hubs Anleitung].

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Benachrichtigung Hubs Anleitung]: notification-hubs-push-notification-overview.md
[Benachrichtigungshubs mit der Push-Benachrichtigung an Benutzer]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure-Portal]: https://portal.azure.com
