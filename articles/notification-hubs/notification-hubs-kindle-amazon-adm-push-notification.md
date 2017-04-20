<properties
    pageTitle="Erste Schritte mit Azure Notification Hubs für apps Kindle | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie, wie mit Azure Notification Hubs Pushbenachrichtigungen Kindle Anwendung senden."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Erste Schritte mit Notification Hubs für Kindle apps

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Übersicht

In diesem Lernprogramm wird veranschaulicht, wie mit Azure Notification Hubs Pushbenachrichtigungen Kindle Anwendung senden.
Erstellen Sie eine leere Kindle app, die Pushbenachrichtigungen mit Amazon Gerät Messaging (ADM) erhält.

##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm ist Folgendes erforderlich:

+ <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android-Website</a>Android SDK (vorausgesetzt Sie Eclipse verwenden) erhalten.
+ Führen Sie unter <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Setting Up Your Development Environment</a> Umgebung für Kindle eingerichtet.

##<a name="add-a-new-app-to-the-developer-portal"></a>Hinzufügen einer neuen Applikation zum Entwicklerportal

1. Erstellen Sie zunächst eine app im [Amazon-Entwicklerportal].

    ![][0]

2. Kopieren Sie die **ANWENDUNGSTASTE**.

    ![][1]

3. Im Portal auf den Namen der Anwendung und klicken Sie auf die Registerkarte **Device Messaging** .

    ![][2]

4. Klicken Sie auf **Erstellen eines neuen Sicherheitsprofils**und erstellen Sie ein neues Sicherheitsprofil (z. B. **TestAdm-Sicherheitsprofil**). Klicken Sie auf **Speichern**.

    ![][3]

5. Klicken Sie auf **Sicherheitsprofile** um das Sicherheitsprofil anzuzeigen, das Sie gerade erstellt haben. Kopieren Sie die **Client-ID** und **Geheimen** Werte zur späteren Verwendung.

    ![][4]

## <a name="create-an-api-key"></a>Erstellen eines API-Schlüssels

1. Öffnen Sie ein Eingabeaufforderungsfenster mit Administratorrechten.
2. Navigieren Sie zum Ordner Android SDK.
3. Geben Sie den folgenden Befehl ein:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  Geben Sie das Kennwort **Schlüsselspeicher** **android**.

5.  **MD5** -Fingerabdruck zu kopieren.
6.  Im Entwicklerportal auf der Registerkarte **Messaging** auf **Android-Kindle** Geben Sie den Namen des Pakets für Ihre Anwendung (z. B. **com.sample.notificationhubtest**) und der **MD5** -Wert und klicken Sie dann auf **API-Schlüssel zu generieren**.

## <a name="add-credentials-to-the-hub"></a>Hinzufügen von Anmeldeinformationen zum hub

Fügen Sie im Portal geheimen und Client-ID zur Registerkarte **Konfigurieren** des Notification Haupt.

## <a name="set-up-your-application"></a>Einrichten der Anwendung

> [AZURE.NOTE] Wenn Sie eine Anwendung erstellen, verwenden Sie mindestens API-Ebene 17.

Das Eclipse Projekt ADM-Bibliotheken hinzufügen:

1. ADM-Bibliothek, [Laden Sie das SDK]zu erhalten. Die SDK-Zip-Datei zu extrahieren.
2. In Eclipse Maustaste auf das Projekt und klicken Sie dann auf **Eigenschaften**. Wählen Sie **Java erstellen Pfad** auf der linken Seite und wählen Sie die Registerkarte **Bibliotheken **oben. Klicken Sie auf **Externe Jar hinzufügen**und wählen Sie die Datei `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` aus dem Verzeichnis in der Amazon-SDK extrahiert.
3. Downloaden Sie NotificationHubs Android SDK (Link).
4. Entpacken Sie das Paket und die Datei `notification-hubs-sdk.jar` in die `libs` Ordner in Eclipse.

Bearbeiten Sie Ihre app-Manifest ADM Unterstützung:

1. Das manifest Stammelement Amazon-Namespace hinzugefügt:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Berechtigungen als erstes Element unter dem manifest-Element hinzufügen. Ersetzen Sie **[IHR PAKETNAME]** mit dem Paket zur Erstellung Ihrer app.

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Fügen Sie das folgende Element als das erste untergeordnete Element des Anwendungselements. **[IHR DIENSTNAME]** mit dem Namen der ADM-Message Handler ersetzt werden, die im nächsten Abschnitt (einschließlich Paket) erstellen müssen Sie und der Paketname, mit dem die Anwendung erstellt, durch **[IHR PAKETNAME]** .

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Erstellen Sie Ihre ADM-Meldungshandler

1. Erstellen Sie eine neue Klasse erbt `com.amazon.device.messaging.ADMMessageHandlerBase` Namen `MyADMMessageHandler`, wie in der folgenden Abbildung dargestellt:

    ![][6]

2. Fügen Sie den folgenden `import` Aussagen:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Fügen Sie folgenden Code in die erstellte Klasse. Beachten Sie die Hub und die Verbindungszeichenfolge Zeichenfolge (Empfang) ersetzen:

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Fügen Sie den folgenden Code in die `OnMessage()` Methode:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Fügen Sie den folgenden Code in die `OnRegistered` Methode:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Fügen Sie den folgenden Code in die `OnUnregistered` Methode:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. In der `MainActivity` Methode die folgenden Import-Anweisung hinzufügen:

        import com.amazon.device.messaging.ADM;

8. Fügen Sie folgenden Code am Ende der `OnCreate` Methode:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Ihre Anwendung den API-Schlüssel hinzufügen

1. Erstellen Sie eine neue Datei namens **api_key.txt** im Verzeichnis Ressourcen des Projekts in Eclipse.
2. Öffnen Sie die Datei, und kopieren Sie den API-Schlüssel, den in der Amazon-Entwicklerportal generiert.

## <a name="run-the-app"></a>Führen Sie die Anwendung

1. Starten Sie den Emulator.
2. Wischen Sie im Emulator vom oberen und klicken Sie auf **Optionen**, und klicken Sie auf **Mein Konto** und mit einem gültigen Amazon-Konto registrieren.
3. In Eclipse, führen Sie die Anwendung.

> [AZURE.NOTE] Wenn ein Problem auftritt, überprüfen Sie die Emulation (oder Gerät). Der Wert muss genau sein. Um die Kindle Emulator ändern, führen Sie den folgenden Befehl Android SDK Plattform-Tools-Verzeichnis:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Senden einer Nachricht

So senden Sie eine Nachricht mit .NET

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon-Entwicklerportal]: https://developer.amazon.com/home.html
[Das SDK herunterladen]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
