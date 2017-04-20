<properties
    pageTitle="Senden von sicheren Pushbenachrichtigungen mit Azure Notification Hubs"
    description="Erfahren Sie, wie ein Android Azure sichere Pushbenachrichtigungen an. Beispiele in Java und C#."
    documentationCenter="android"
    keywords="Push-Benachrichtigung Pushbenachrichtigungen push Nachrichten android Pushbenachrichtigungen"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Senden von sicheren Pushbenachrichtigungen mit Azure Notification Hubs

> [AZURE.SELECTOR]
- [Universal Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>Übersicht

> [AZURE.IMPORTANT] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Push-Benachrichtigung-Unterstützung in Microsoft Azure ermöglicht eine einfach zu verwendende für mehrere Plattformen, skalierte Push Nachricht Infrastruktur die Implementierung von Pushbenachrichtigungen für Verbraucher und Unternehmen für mobile Plattformen vereinfacht zugreifen.

Durch behördliche oder Einschränkungen, eine Anwendung ggf. etwas in der Benachrichtigung enthalten, die über die standard-Push Notification Infrastruktur übertragen werden können. In diesem Lernprogramm beschrieben genauso erreichen vertraulichen Informationen über eine sichere, authentifizierte Verbindung zwischen Android Clientgerät und Backend-Anwendung.

Auf einer hohen Ebene ist der Fluss wie folgt:

1. App-Back-End:
    - Sichere Nutzlast im Back-End-Datenbank speichert.
    - Übermittelt die ID dieser Benachrichtigung Android-Gerät (keine sichere Informationen gesendet).
2. Die Anwendung auf dem Gerät bei der Benachrichtigung:
    - Android Gerät kontaktiert Back-End-secure Payload anfordern.
    - Die Anwendung kann die Nutzlast als Benachrichtigung an das Gerät anzeigen

Es ist wichtig zu beachten, dass im vorherigen Fluss und in diesem Lernprogramm wird angenommen, dass das Gerät ein Authentifizierungstoken im lokalen Speicher, speichert nachdem der Benutzer anmeldet. Dies garantiert eine vollständig nahtlose Gerät die Benachrichtigung sichere Nutzlast mit diesem Token abrufen kann. Wenn Ihre Anwendung Authentifizierungstoken nicht auf dem Gerät speichern oder diese Token abgelaufen sein können, sollten app Gerät beim Empfang von Push-Benachrichtigung eine generische Meldung Benutzereingabe Starten anzeigen Die Anwendung authentifiziert den Benutzer und zeigt die benachrichtigungsnutzlast.

Dieses Lernprogramm zeigt, wie sichere Pushbenachrichtigungen zu senden. Damit die in diesem Lernprogramm ersten Schritte sollten Sie ggf. erstellt auf das Lernprogramm [Benachrichtigen Benutzer](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) .

> [AZURE.NOTE] Diese praktische Einführung geht, erstellt und den benachrichtigungshub konfiguriert unter [Erste Schritte mit Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Android Projekt ändern

Ihr app-Back-End nur die *Id* eine Push-Benachrichtigung senden geändert, besitzen Sie ändern Ihre Android, Benachrichtigung und Rückruf der Backend-rufen Sie die sichere Nachricht angezeigt werden.
Um dieses Ziel zu erreichen, müssen Sie sicherstellen, dass Ihre Android weiß authentifizieren sich mit Ihrem Back-End Push-Benachrichtigung erhält.

Wir werden nun *Login* -Fluss sparen Authentication Header-Wert in den freigegebenen Voreinstellungen der Anwendung ändern. Analoge Mechanismen können alle Authentifizierungstoken (z.B. OAuth-Token) speichern, die die Anwendung verwenden, ohne Anmeldeinformationen verwendet werden.

1. Fügen Sie im Projekt Android Schreibzugriff am Anfang der **MainActivity** -Klasse:

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Noch in der **MainActivity** -Klasse aktualisieren die `getAuthorizationHeader()` -Methode enthält den folgenden Code:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Fügen Sie den folgenden `import` -Anweisung am Anfang der Datei **MainActivity** :

        import android.content.SharedPreferences;

Jetzt werden wir den Handler ändern, der aufgerufen wird, wenn die Benachrichtigung empfangen wird.

4. **MyHandler** Klasse ändern die `OnReceive()` -Methode enthalten:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Fügen Sie die `retrieveNotification()` -Methode ersetzt den Platzhalter `{back-end endpoint}` mit dem Back-End-Endpunkt beim Bereitstellen der Back-End abgerufen:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Diese Methode ruft die app Backend-rufen Sie die Anmeldeinformationen in den Voreinstellungen für freigegebenen Inhalt der Benachrichtigung und als normale Benachrichtigung angezeigt. Benachrichtigung sieht die Anwendung Benutzer genau wie andere Push-Benachrichtigung.

Beachten Sie, dass die Fälle fehlender Authentication Header oder Ablehnung von Back-End vorzuziehen. Spezifische Behandlung dieser Fälle wird meist angenehmer Ziel abhängig. Eine Möglichkeit ist, eine Benachrichtigung mit einer generischen Meldung für den Benutzer zu authentifizieren, zum Abrufen der aktuellen Benachrichtigung angezeigt.

## <a name="run-the-application"></a>Führen Sie die Anwendung

Führen Sie folgende Schritte aus, um die Anwendung auszuführen:

1. Stellen Sie sicher, dass **AppBackend** in Azure bereitgestellt wird. Wenn Sie Visual Studio verwenden, führen Sie **AppBackend** Web API-Anwendung aus. Eine ASP.NET Web angezeigt wird.

2. Führen Sie in Eclipse die Anwendung auf einem physischen Android Gerät oder Emulator.

3. Android UI Geben Sie Benutzername und Kennwort ein. Dies können eine Zeichenfolge sein, aber sie müssen dem Wert entsprechen.

4. Android UI klicken Sie auf **Anmelden**. Klicken Sie auf **Push senden**.
