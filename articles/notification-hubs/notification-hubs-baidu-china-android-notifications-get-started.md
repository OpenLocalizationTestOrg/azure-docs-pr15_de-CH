<properties
    pageTitle="Erste Schritte mit Azure Notification Hubs mit Baidu | Microsoft Azure"
    description="In diesem Lernprogramm lernen Sie Azure Notification Hubs Pushbenachrichtigungen auf Geräte mit Baidu verwenden."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Erste Schritte mit Notification Hubs mit Baidu

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Übersicht

Baidu Cloud Push ist eine chinesische Clouddienst, auf Pushbenachrichtigungen an mobile Geräte senden. Dieser Service ist besonders in China Android Pushbenachrichtigungen bereitzustellen ist komplexer aufgrund von verschiedenen app-Stores und Push-Dienste neben der Verfügbarkeit der Geräte in der Regel nicht mit GCM (Google Cloud Messaging) verbunden sind.

##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm ist Folgendes erforderlich:

+ Android SDK (vorausgesetzt Sie Eclipse verwenden), die von <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android-Website</a> herunterladen können
+ [Mobile Dienste Android SDK]
+ [Baidu Push Android SDK]

>[AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##<a name="create-a-baidu-account"></a>Erstellen eines Kontos Baidu

Um Baidu verwenden zu können, benötigen Sie ein Konto Baidu. Wenn Sie bereits [Baidu Portal] anmelden und mit dem nächsten Schritt fortfahren. Andernfalls finden Sie unter wie ein Baidu-Konto erstellen.  

1. Zum [Baidu-Portal] , und klicken Sie auf**登录**(**Login**). Klicken Sie auf**立即注册**um die Konto-Registrierung zu starten.

    ![][1]

2. Geben Sie die erforderlichen Informationen, Telefon oder e-Mail-Adresse, Kennwort und Überprüfung – und klicken Sie auf **Anmeldung**.

    ![][2]

3. Sie werden eine e-Mail-Adresse e-Mail mit einem Link zur Aktivierung Ihres Kontos Baidu eingegeben.

    ![][3]

4. Melden Sie sich bei Ihrem e-Mail-Konto und klicken Sie auf den Aktivierungslink aktivieren Ihres Kontos Baidu Baidu Aktivierung e-Mail.

    ![][4]

Haben Sie eine aktivierte Baidu-Konto, melden Sie sich [Baidu Portal].

##<a name="register-as-a-baidu-developer"></a>Als Entwickler Baidu registrieren

1. Sobald Sie [Baidu Portal]angemeldet haben, klicken Sie auf**更多 >>** (**mehr**).

    ![][5]

2. Bildlauf im Abschnitt**站长与开发者服务 (Webmaster und Developer Services)** und**百度开放云平台**(**Baidu Cloudplattform öffnen**) klicken.

    ![][6]

3. Klicken Sie auf der nächsten Seite auf**开发者服务**(**Developer Services**) in der oberen rechten Ecke.

    ![][7]

4. Klicken Sie auf der nächsten Seite im Menü in der rechten oberen Ecke auf**注册开发者**(**Entwickler registriert**).

    ![][8]

5. Geben Sie Ihren Namen, eine Beschreibung und eine Mobiltelefonnummer für eine Überprüfung empfangen und dann auf**送验证码**(**Überprüfungscode senden**). Beachten Sie, dass bei internationalen Rufnummern den Ländercode in Klammern einschließen. Angenommen, für eine Anzahl USA werden **(1) 1234567890**.

    ![][9]

6. Sie erhalten eine SMS mit einem Bestätigungscode dann wie im folgenden Beispiel gezeigt:

    ![][10]

7. Geben Sie den Bestätigungscode aus der Nachricht in**验证码**(**Bestätigungscode**).

8. Schließlich führen Sie entwicklerregistrierung der Zustimmung Baidu und**提交**(**Senden**) klicken. Nach erfolgreichem Abschluss der Registrierung wird die folgende Seite angezeigt werden:

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Erstellen Sie ein Baidu Cloud Push-Projekt

Beim Erstellen eines Baidu Cloud Push erhalten Ihre ID app API-Schlüssel und geheime Schlüssel.

1. Sobald Sie [Baidu Portal]angemeldet haben, klicken Sie auf**更多 >>** (**mehr**).

    ![][5]

2. Bildlauf im Abschnitt**站长与开发者服务**(**Webmaster und Developer Services**) und**百度开放云平台**(**Baidu Cloudplattform öffnen**) klicken.

    ![][6]

3. Klicken Sie auf der nächsten Seite auf**开发者服务**(**Developer Services**) in der oberen rechten Ecke.

    ![][7]

4. Klicken Sie auf der nächsten Seite auf**云推送**(**Cloud Push**) im Abschnitt**云服务**(**Cloud-Dienste**).

    ![][12]

5. Sobald Sie registrierte Entwickler sind, sehen Sie**管理控制台**(**Verwaltungskonsole**) im oberen Menü. Klicken Sie auf**开发者服务管理**(**Entwickler Service Management**).

    ![][13]

6. Klicken Sie auf der nächsten Seite auf**创建工程**(**Projekt erstellen**).

    ![][14]

7. Geben Sie einen Namen, und klicken Sie auf**创建**(**Erstellen**).

    ![][15]

8. Nach der erfolgreichen Erstellung eines Baidu Cloud Push sehen Sie eine Seite mit **AppID**, **API-Schlüssel**und **Geheimschlüssel**. Notieren Sie sich den API-Schlüssel und geheime Schlüssel, den wir später verwenden möchten.

    ![][16]

9. Konfigurieren des Projekts für Pushbenachrichtigungen**云推送**(**Cloud Push**) im linken Bereich auf.

    ![][31]

10. Klicken Sie auf der nächsten Seite auf**推送设置**(**Push Settings**).

    ![][32]  

11. Fügen Sie auf der Konfigurationsseite der Paketname, dass Sie in Ihrem Android im Feld**应用包名**(**Antrag**) verwenden, und klicken Sie auf**保存设置**(**Speichern**).  

    ![][33]

Wird der**保存成功!** (**wurde erfolgreich gespeichert!**) angezeigt.

##<a name="configure-your-notification-hub"></a>Konfigurieren Sie Ihren Notification hub

1. [Azure-Verwaltungsportal]melden Sie an und klicken Sie dann auf **+ neu** am unteren Bildschirmrand.

2. Klicken Sie auf **Anwendungsdienste**, klicken Sie auf **Service Bus**klicken Sie **Notification Hub**und klicken Sie auf **Quick erstellen**.

3. Geben Sie einen Namen für Ihre **Benachrichtigungshub**, **Region** und wo diese benachrichtigungshub erstellte **Namespace** auswählen und dann auf **eine neue Benachrichtigungshub erstellen**.  

    ![][17]

4. Klicken Sie auf den Namespace in dem benachrichtigungshub erstellt und dann auf **Benachrichtigungshubs** oben.

    ![][18]

5. Wählen Sie benachrichtigungshub, den Sie erstellt und klicken Sie im oberen Menü auf **Konfigurieren** .

    ![][19]

6. **Baidu Benachrichtigung** Einstellungen scrollen und API-Schlüssel und geheime Schlüssel Baidu Konsole zuvor für das Projekt Baidu Cloud Push erworbenen eingeben. Klicken Sie auf **Speichern**.

    ![][20]

7. Klicken Sie auf der **Dashboard** -Registerkarte oben Notification Hub und dann auf **Verbindungszeichenfolge anzeigen**.

    ![][21]

8. Notieren Sie **DefaultListenSharedAccessSignature** und **DefaultFullSharedAccessSignature** **Zugriff auf Informationen** im Fenster an.

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Ihre app Hub Benachrichtigung verbinden

1. In Eclipse ADT Android neues Projekt erstellen (**Datei** > **neu** > **Android-Anwendungsprojekt**).

    ![][23]

2. Geben Sie einen **Anwendungsnamen** und sicherstellen, dass die **Mindestens erforderlichen SDK** -Version soll **API 16: Android 4.1**.

    ![][24]

3. Klicken Sie auf **Weiter** und nach den Assistenten bis die **Aktivität erstellen** wird angezeigt. **Leere Aktivität** ausgewählt ist, und wählen Sie zum Erstellen einer neuen Android Anwendung **Beenden** .

    ![][25]

4. Stellen Sie sicher, dass das **Projektziel erstellen** richtig eingestellt ist.

    ![][26]

5. Herunterladen der Benachrichtigung Hubs 0.4.jar aus **dem Register [Notification-Hubs-Android-SDK Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4)** . **Bibliotheken** Ordner Ihres Eclipse-Projekts der Datei hinzu, und aktualisieren Sie *Bibliotheken* Ordner.

6. Herunterladen und Entpacken [Baidu Push Android SDK], öffnen Sie den Ordner **Libs** und kopieren **Pushservice x.y.z** JAR-Datei und **Armeabi** & **Mips** -Ordner im Ordner **Libs** Android Anwendung.

7. Öffnen Sie die Datei **AndroidManifest.xml** des Projekts Android und fügen Sie SDK Baidu erforderlichen Berechtigungen hinzu.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. Fügen Sie **Android: Name** -Eigenschaft **der Anwendungselement in **AndroidManifest.xml**, ersetzt *Ihr Projektname* (z.B. **com.example.BaiduTest hinzu**)** . Stellen Sie sicher, dass diese Projektnamen übereinstimmt, die in der Baidu-Konsole konfiguriert.

        <application android:name="yourprojectname.DemoApplication"

9. Fügen Sie die folgende Konfiguration innerhalb des Elements Application nach der **. MainActivity** Aktivitätselement ersetzt *Ihr Projektname* (z.B. **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Fügen Sie eine neue Klasse namens **ConfigurationSettings.java** für das Projekt.

    ![][28]

    ![][29]

10. Im folgenden Code hinzufügen:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Legen Sie den Wert des **API_KEY** mit was Sie Baidu Projekt für den Cloud, **NotificationHubName** mit Ihrem Hubnamen von Azure-Verwaltungsportal und **NotificationHubConnectionString** mit DefaultListenSharedAccessSignature von Azure-Verwaltungsportal entnommen.

11. Fügen Sie eine neue Klasse namens **DemoApplication.java**und fügen Sie den folgenden Code hinzu:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Fügen Sie eine weitere Klasse namens **MyPushMessageReceiver.java**und fügen Sie den folgenden Code hinzu. Die Klasse, die die Push-Benachrichtigung behandelt, die von den Baidu Push-Server empfangen wird.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Öffnen Sie **MainActivity.java**, und fügen Sie Folgendes zu der **OnCreate** -Methode:

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Import Aussagen oben zu öffnen:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Benachrichtigung für Ihre Anwendung


Sie können schnell testen Benachrichtigungen in Ihrer app benachrichtigungshub senden **Testen** über [Azure-Portal](https://portal.azure.com/) wie dem folgenden Bildschirm Benachrichtigungen senden.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Push-Benachrichtigung werden normalerweise in einem Back-End-Dienst wie Mobile Dienste oder ASP.NET eine kompatible Bibliothek gesendet. Die REST-API können direkt zum Senden von Nachrichten ist eine Bibliothek nicht für die Back-End.

In diesem Lernprogramm erstellen wir einfach und nur demonstrieren Testen der Clientanwendung Benachrichtigungen mit .NET SDK für Notification Hubs ein Konsolenanwendungsprojekt kein Back-End-Dienst. Empfohlen Lernprogramm [Verwendet Notification Hubs Push-Benachrichtigung an Benutzer](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) im nächsten Schritt für das Senden von Benachrichtigungen aus einer ASP.NET Backend. Die folgenden Ansätze können jedoch für Benachrichtigungen verwendet werden:

* **REST-Schnittstelle**: Benachrichtigung kann auf allen Back-End-Plattformen mithilfe der [REST-Schnittstelle](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)unterstützen.

* **Microsoft Azure Notification Hubs.NET SDK**: In der Nuget Paket-Manager für Visual Studio [- Installationspaket Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)ausführen.

* **Node.js** : [wie Notification Hubs von Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

* **Mobile Apps**: finden Sie ein Beispiel aus einem Backend Azure App Service Mobile Apps Benachrichtigungen, die Benachrichtigungshubs integriert [Pushbenachrichtigungen für Ihre mobilen Anwendung hinzufügen](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java / PHP**: ein Beispiel Benachrichtigungen mithilfe der REST-APIs finden Sie unter "Benachrichtigungshubs von Java-PHP verwenden" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Optional) Benachrichtigung von einer Konsole.

In diesem Abschnitt zeigen wir das Senden einer Benachrichtigung über eine Konsole app .NET.

1. Erstellen Sie eine neue Visual C#:

    ![][30]

2. Im Konsolenfenster Paket-Manager **Project Standard** auf Ihr neues Konsolenanwendungsprojekt festgelegt, und führen Sie folgenden Befehl im Konsolenfenster angezeigt:

        Install-Package Microsoft.Azure.NotificationHubs

    Einen Verweis auf Azure Notification Hubs SDK mit <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-Paket</a>hinzugefügt.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Öffnen Sie die Datei **Program.cs** , und fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.NotificationHubs;

4. In der `Program` Klasse, fügen Sie die folgende Methode und *DefaultFullSharedAccessSignatureSASConnectionString* und *NotificationHubName* mit den Werten, die Sie ersetzen.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Fügen Sie folgenden Zeilen in der **Main** -Methode hinzu:

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Testen Sie Ihre Anwendung

App mit einem Telefon testen schließen Sie Telefon an Ihrem Computer mithilfe eines USB-Kabels. Dadurch wird Ihre Anwendung auf dem angeschlossenen Telefon geladen.

Zum Testen dieser Anwendung mit dem Emulator in der oberen Eclipse-Symbolleiste auf **Ausführen**, und wählen Sie Ihre app. Startet den Emulator lädt und die Anwendung ausgeführt wird.

Die Anwendung entnimmt den Baidu Push Notification Service 'ID' und 'ChannelId' und mit der Benachrichtigung registriert.

Zum Senden einer testbenachrichtigung können Sie die Registerkarte Debuggen Azure-Verwaltungsportal. Wenn .NET Konsolenanwendungsprojekt für Visual Studio erstellt, nur drücken Sie F5 in Visual Studio zum Ausführen der Anwendung. Die Anwendung sendet eine Benachrichtigung, die im oberen Infobereich von dem Gerät oder Emulator angezeigt wird.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Dienste Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure-Verwaltungsportal]: https://manage.windowsazure.com/
[Baidu-portal]: http://www.baidu.com/
