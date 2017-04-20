<properties
    pageTitle="Erste Schritte mit Android Apps Azure Mobile Engagement"
    description="Informationen Sie zum Azure Mobile Engagement für apps mit Analysen und Push Notifications verwenden."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Erste Schritte mit Azure Mobile Engagement für apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird wie mit Azure Mobile Engagement die app Verwendung verstehen und Pushbenachrichtigungen segmentierten Benutzer Android Anwendung senden.
Dieses Lernprogramm demonstriert das einfache Übertragung Szenario mit Mobile Engagement. Erstellen Sie eine leere Android, die Daten sammelt und Pushbenachrichtigungen mit Google Cloud Messaging (GCM) erhält.

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Lernprogramm erfordert [Android Entwicklertools](https://developer.android.com/sdk/index.html), Android Studio-IDE und die neuesten Android-Plattform.

Außerdem muss der [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Mobile Engagement für Ihre Android einrichten

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Mobile Engagement Backend Ihrer app herstellen

In diesem Lernprogramm wird eine "einfache Integration", ist minimal erforderlichen Daten sammeln und eine Push-Benachrichtigung zu senden. Erstellen eine einfache Anwendung mit Android Studio Integration zu veranschaulichen.

Die vollständige Integration Dokumentation finden in der [Mobile Engagement Android SDK-Integration](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Ein Android-Projekt erstellen

1. Starten Sie **Android Studio**, und wählen Sie im Popupfenster **Starten eines neuen Projekts Android Studio**.

    ![][1]

2. Bereitstellen einer Anwendungsdomäne und Firmenname. Notieren von was Sie füllen, da später benötigen. Klicken Sie auf **Weiter**.

    ![][2]

3. Wählen Sie die Ziel-Formfaktor und API-Ebene, und klicken Sie auf **Weiter**.

    >[AZURE.NOTE] Mobile Engagement erfordert API mindestens 10 (Android 2.3.3).

    ![][3]

4. Wählen Sie **Leere Aktivität** hier nur Bildschirm für diese Anwendung und klicken Sie auf **Weiter**.

    ![][4]

5. Schließlich Standardwerte ist, und klicken Sie auf **Fertig stellen**.

    ![][5]

Android Studio erstellt jetzt demoApp, die wir Mobile Engagement integrieren.

### <a name="include-the-sdk-library-in-your-project"></a>SDK-Bibliothek in Ihr Projekt aufnehmen

1. [Mobile Engagement Android SDK](https://aka.ms/vq9mfn)herunterladen.
2. Extrahieren Sie die Archivdatei zu einem Ordner auf Ihrem Computer.
3. Identifizieren der JAR-Bibliothek für die aktuelle Version des SDK und in die Zwischenablage kopieren.

      ![][6]

4. Navigieren Sie zum **Abschnitt** (1) und fügen Sie die JAR in Bibliotheken Ordner (2).

      ![][7]

5. Synchronisieren Sie zum Laden der Bibliothek des Projekts.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Mobile Engagement Backend mit der Verbindungszeichenfolge Ihrer app herstellen

1. Kopieren Sie die folgenden Codezeilen in Aktivität erstellen (nur an einer Stelle der Anwendung, in der Regel die Hauptaktivität erfolgt). Für dieses Beispiel-app-öffnen Sie unter Src MainActivity > Main -> Java und fügen Sie Folgendes hinzu:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Lösen Sie die Verweise, indem Sie Alt + Eingabetaste drücken oder Import Folgendes:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Zurück zur Azure-Verwaltungsportal auf Ihre app- **Verbindungsinformationen** und die **Verbindungszeichenfolge**kopieren.

      ![][9]

4. Fügen Sie ihn in den `setConnectionString` Parameter ersetzen die gesamte Zeichenfolge im folgenden Code dargestellt:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Berechtigungen und Service-Deklaration hinzufügen

1. ' Manifest.Xml ' des Projekts Berechtigungen hinzufügen, unmittelbar vor oder nach dem `<application>` Tag:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Den Agent-Dienst zu deklarieren, fügen Sie diesen Code zwischen die `<application>` und `</application>` Tags:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Ersetzen Sie den eingefügten Code `"<Your application name>"` die Bezeichnung der im **Einstellungen** finden können Sie Dienste auf dem Gerät angezeigt. Sie können das Wort "Service" in der Beschriftung hinzufügen.

### <a name="send-a-screen-to-mobile-engagement"></a>Senden Sie einen Bildschirm zu Mobile

Sendevorgang starten und sicherstellen, dass der Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm (Aktivität) Mobile Engagement Backend senden.

Gehen Sie zu **MainActivity.java** , und fügen Sie Folgendes zum Ersetzen der Basisklasse **MainActivity** , **EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Wenden Sie die Basisklasse nicht *Aktivität*ist, [Erweiterte Android Reporting](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) , wie von anderen Klassen erben.


Kommentieren Sie die folgende Zeile für dieses einfache Szenario:

    // setSupportActionBar(toolbar);

Wenn Sie möchten das `ActionBar` in Ihrer Anwendung finden Sie unter [Erweiterte Android Reporting](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Push-Benachrichtigung und messaging-Anwendung aktivieren

Während der Kampagne können Mobile Engagement interagieren und erreichen die Benutzer mit Pushbenachrichtigungen messaging-app. Dieses Modul heißt REICHWEITE im Mobile Engagement-Portal.
Im folgenden Abschnitt richtet Ihre app erhalten.

### <a name="copy-sdk-resources-in-your-project"></a>SDK-Ressourcen in Ihrem Projekt kopieren

1. Navigieren Sie zu der SDK-Downloads und kopieren Sie den Ordner **Res** .

    ![][10]

2. Gehen Sie auf Android Studio wählen **das Hauptverzeichnis der Projektdateien** , und fügen Sie dem Projekt Ressourcen hinzufügen.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

[Android SDK](mobile-engagement-android-sdk-overview.md) detaillierte Kenntnisse über die SDK-Integration zu wechseln.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
