1. Öffnen Sie die Datei in Ihrem Projekt **app** `AndroidManifest.xml`. Ersetzen Sie den Code in den nächsten beiden Schritten _`**my_app_package**`_ mit dem Namen des app-Pakets für das Projekt, das ist der Wert der `package` Attribut der `manifest` Tag.

2. Fügen Sie folgenden neuen Berechtigungen nach der vorhandenen `uses-permission` Element:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Fügen Sie folgenden Code nach dem `application` Starttag:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. Öffnen Sie die Datei *ToDoActivity.java*, und fügen Sie die folgenden Import-Anweisung:

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. Fügen Sie die folgenden privaten Variablen zur Klasse: Ersetzen _`<PROJECT_NUMBER>`_ mit der Projektnummer von Google für Ihre Anwendung in der vorangehenden Prozedur zugewiesen:

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. Ändern der *MobileServiceClient* von **private** **Öffentliche statische**damit nun folgendermaßen:

        public static MobileServiceClient mClient;

7. Als Nächstes müssen wir fügen Sie eine neue Klasse zum Behandeln von Benachrichtigungen. Öffnen Sie im Projekt-Explorer **Src** => **wichtigsten** => **Java** -Knoten und mit der rechten Maustaste den Namen Paketknoten: Klicken Sie auf **neue** **Java-Klasse**.

8. **Name** Typ `MyHandler`, klicken Sie auf **OK**.


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. Ersetzen Sie in der Datei MyHandler Klassendeklaration mit

        public class MyHandler extends NotificationsHandler {


10. Fügen Sie die folgende Import-Anweisung für die `MyHandler` Klasse:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. Fügen Sie dieses Mitglied auf der `MyHandler` Klasse:

        public static final int NOTIFICATION_ID = 1;


12. In der `MyHandler` Klasse, fügen Sie folgenden Code, um das Gerät mit mobile Service Notification Hub registriert die Methode **OnRegistered** überschreiben.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. In der `MyHandler` -Klasse verwenden, fügen Sie folgenden Code zur Methode **OnReceive** überschreiben, wodurch die Benachrichtigung beim Empfang angezeigt.

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. Aktualisieren Sie in der Datei TodoActivity.java **OnCreate** -Methode der Klasse *ToDoActivity* die Benachrichtigungsklasse Handler registrieren. Sollten Sie diesen Code hinzufügen, nachdem *MobileServiceClient* instanziiert wird.


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Die Anwendung wurde aktualisiert, um Pushbenachrichtigungen unterstützen.
