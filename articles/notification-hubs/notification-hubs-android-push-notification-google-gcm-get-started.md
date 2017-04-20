<properties
    pageTitle="Senden von Pushbenachrichtigungen Android mit Azure Notification Hubs | Microsoft Azure"
    description="In diesem Lernprogramm lernen Sie Azure Notification Hubs Pushbenachrichtigungen, Android-Geräte verwenden."
    services="notification-hubs"
    documentationCenter="android"
    keywords="Pushbenachrichtigungen Push-Benachrichtigung android Push-Benachrichtigung"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Senden von Pushbenachrichtigungen Android mit Azure Notification Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Übersicht

> [AZURE.IMPORTANT] Dieses Thema veranschaulicht Pushbenachrichtigungen mit Google Cloud Messaging (GCM). Bei Verwendung Googles FB Cloud Messaging (FCM) finden Sie unter [Senden von Pushbenachrichtigungen Android Azure Notification Hubs mit FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).

In diesem Lernprogramm wird veranschaulicht, wie mit Azure Notification Hubs Pushbenachrichtigungen Android Anwendung senden.
Erstellen Sie eine leere Android, die Pushbenachrichtigungen mit Google Cloud Messaging (GCM) erhält.

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Der vollständige Code für dieses Lernprogramm kann von GitHub heruntergeladen werden [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).


##<a name="prerequisites"></a>Erforderliche Komponenten

> [AZURE.IMPORTANT] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

Dieses Lernprogramm erfordert zusätzlich eine aktive Azure erwähnt nur die neueste Version von [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).

Dieses Lernprogramm ist eine Voraussetzung für alle anderen Notification Hubs Lernprogramme für apps.

##<a name="creating-a-project-that-supports-google-cloud-messaging"></a>Erstellen eines Projekts unterstützt Google Cloud Messaging

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Konfigurieren Sie einen neue benachrichtigungshub


[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6 Wählen Sie im Blatt **Einstellungen** **Notification Services** und **Google (GCM)**. Geben Sie den API-Schlüssel, und klicken Sie auf **Speichern**.

&emsp;&emsp;![Azure Notification Hubs - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Der benachrichtigungshub ist jetzt konfiguriert GCM und haben die Verbindungszeichenfolgen für sowohl Ihre Anwendung zum Empfangen und Senden von Pushbenachrichtigungen registrieren.

##<a id="connecting-app"></a>Ihre app Hub Benachrichtigung verbinden

###<a name="create-a-new-android-project"></a>Erstellen eines neuen Projekts Android

1. Starten Sie in Android Studio ein neues Android Studio-Projekt.

    ![Android Studio - neues Projekt][13]

2. Wählen Sie den Formularfaktor **Telefon- und Tablet** und **Minimum SDK** , die Sie unterstützen möchten. Klicken Sie auf **Weiter**.

    ![Android Studio - Projekt erstellen workflow][14]

3. **Leere** Aktivität für die Hauptaktivität, klicken Sie auf **Weiter**und dann auf **Fertig stellen**.

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


1. Zur Unterstützung der GCM müssen wir Listenerdienst eine Instanz-ID in unserem Code implementieren [Registrierung Token erhalten](https://developers.google.com/cloud-messaging/android/client#sample-register) mit [Google Instanz-ID-API](https://developers.google.com/instance-id/)verwendet wird. In diesem Lernprogramm nennen wir die Klasse `MyInstanceIDService`. 
 
    Fügen Sie die folgende Service Definition innerhalb der Datei AndroidManifest.xml die `<application>` Tag. Ersetzen der `<your package>` Platzhalter mit der Ihren tatsächlichen Paketnamen am oberen Rand der `AndroidManifest.xml` Datei.

        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>


2. Nach unserer GCM Registrierung Token aus der Instanz-ID-API Erhalt verwenden wir es [bei Azure Notification Hub](notification-hubs-push-notification-registration-management.md)registrieren. Wir unterstützen diese Registrierung im Hintergrund mit einer `IntentService` mit dem Namen `RegistrationIntentService`. Dieser Dienst wird auch zum [Aktualisieren unserer GCM Registrierung Token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens)sein.
 
    Fügen Sie die folgende Service Definition innerhalb der Datei AndroidManifest.xml die `<application>` Tag. Ersetzen der `<your package>` Platzhalter mit der Ihren tatsächlichen Paketnamen am oberen Rand der `AndroidManifest.xml` Datei. 

        <service
            android:name="<your package>.RegistrationIntentService"
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



4. Fügen Sie den folgenden erforderlichen GCM bezogene Berechtigungen unten die `</application>` Tag. Ersetzen Sie `<your package>` mit Paketnamen am oberen Rand der `AndroidManifest.xml` Datei.

    Weitere Informationen zu Berechtigungen finden Sie unter [GCM Clientanwendung für Android einrichten](https://developers.google.com/cloud-messaging/android/client#manifest).

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>


### <a name="adding-code"></a>Hinzufügen von code


1. Erweitern Sie in der Projektansicht **app** > **Src** > **wichtigsten** > **Java**. Der Paketordner unter **Java**Maustaste klicken Sie auf **neu**und klicken Sie auf **Java-Klasse**. Fügen Sie eine neue Klasse mit dem Namen `NotificationSettings`. 

    ![Android Studio - neue Java-Klasse][6]

    Diese drei aktualisieren Sie Platzhalter in den folgenden Code für die `NotificationSettings` Klasse:
    * **SenderId**: die Projektnummer in der [Google-Cloud-Konsole](http://cloud.google.com/console)erhalten.
    * **HubListenConnectionString**: die **DefaultListenAccessSignature** -Verbindungszeichenfolge für den Hub. Diese Verbindungszeichenfolge können **Richtlinien** auf Blatt **Einstellungen** des Haupt- [Azure-Portal].
    * **HubName**: Verwenden Sie den Namen des Haupt-Benachrichtigung, die in das Blade Hub [Azure-Portal]angezeigt.

    `NotificationSettings`Code:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Your default listen connection string>";
        }

2. Mit den Schritten fügen Sie eine weitere Klasse mit dem Namen `MyInstanceIDService`. Instanz-ID Listener Service Implementierung werden.

    Der Code für diese Klasse aufrufen unserer `IntentService` im Hintergrund [GCM-Token zu aktualisieren](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) .

        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;
        
        
        public class MyInstanceIDService extends InstanceIDListenerService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.i(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Fügen Sie eine weitere Klasse dem Projekt mit dem Namen `RegistrationIntentService`. Wird die Implementierung für unserer `IntentService` behandelt, die [GCM-Token zu aktualisieren](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) und [mit der Benachrichtigung registrieren](notification-hubs-push-notification-registration-management.md).

    Verwenden Sie den folgenden Code für diese Klasse.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
        
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
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
        
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {      
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting to register with NH using token : " + token);

                        regID = hub.register(token).getRegistrationId();

                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();

                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);       
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete token refresh", e);
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
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. Fügen Sie die folgenden privaten Member am Anfang der Klasse. Diese [Verfügbarkeit von Google Play Services wie Google empfohlen](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk)werden.

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
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


7. In der `MainActivity` Klasse, fügen Sie folgenden Code für Google Play Services vor dem Aufrufen überprüft die `IntentService` zu Ihrer Registrierung GCM token mit der Benachrichtigung registrieren.

        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
    
            if (checkPlayServices()) {
                // Start IntentService to register this application with GCM.
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

##<a name="sending-push-notifications"></a>Push-Benachrichtigung senden

Sie können testen, Pushbenachrichtigungen in Ihrer Anwendung über das [Azure-Portal] - suchen im Abschnitt **Problembehandlung** Blade-Hub senden empfangen, wie unten dargestellt.

![Azure Notification Hubs - Test senden](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Optional) Push-Benachrichtigung direkt

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

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";

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

    ![Testen auf Android - Registrierung][18]

2. Geben Sie eine Benachrichtigung an alle Android-Geräte, die mit dem Hub registriert haben.

    ![Testen auf Android - Senden einer Nachricht][19]

3. Drücken Sie die **Benachrichtigung senden**. Zeigt alle Geräte, die app ein `AlertDialog` mit Push-Benachrichtigung. Geräte, die nicht die app registriert wurden für Pushbenachrichtigungen erhalten eine Benachrichtigung im Benachrichtigungssystem Android. Diese können Wischen Sie von der oberen linken Ecke angezeigt werden.

    ![Testen auf Android - Benachrichtigung][21]

##<a name="next-steps"></a>Nächste Schritte

Wir empfehlen Lernprogramm [Verwendet Notification Hubs Push-Benachrichtigung an Benutzer] im nächsten Schritt. Es zeigt Ihnen wie Benachrichtigungen ein ASP.NET Backend mit Tags auf bestimmte Benutzer.

Ggf. Benutzer Interessengruppen segment checken Sie Lernprogramm [Verwendet Notification Hubs Nachrichten senden] .

Weitere allgemeine Informationen zu Notification Hubs finden Sie unter [Notification Hubs Anleitung].

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Benachrichtigung Hubs Anleitung]: http://msdn.microsoft.com/library/jj927170.aspx
[Benachrichtigungshubs mit der Push-Benachrichtigung an Benutzer]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Verwenden Sie Benachrichtigungshubs zum Senden von Nachrichten]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure-Portal]: https://portal.azure.com
