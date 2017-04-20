<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Cordova-Phonegap"
    description="Informationen Sie zum Azure Mobile Engagement für Cordova-Phonegap-apps mit Analysen und Pushbenachrichtigungen verwenden."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Erste Schritte mit Azure Mobile Engagement für Cordova-Phonegap

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird veranschaulicht, wie mit Azure Mobile Engagement kennen Ihre app-Nutzung und Pushbenachrichtigungen an segmentierte für eine mobile Anwendung mit Cordova entwickelt.

In diesem Lernprogramm erstellen Sie eine leere Cordova-app mit Mac und Mobile Engagement SDK zu integrieren. Er sammelt grundlegende Analytics und empfängt Pushbenachrichtigungen Apple Push Notification System (APN) für iOS und Google Cloud Messaging (GCM) für Android verwenden. Wir werden dies ein iOS oder Android-Gerät zum Testen bereitstellen. 

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

In diesem Lernprogramm ist Folgendes erforderlich:

+ XCode Mac App Store Installation (für die Bereitstellung von IOS)
+ [Android SDK und Emulator](http://developer.android.com/sdk/installing/index.html) (für die Bereitstellung von Android)
+ Push Notification Zertifikat (p12), das von Apple Developer Center für APN erhalten können
+ GCM Nummer, die Sie in der Google-Entwickler-Konsole für GCM erhalten können
+ [Mobile Engagement Cordova Plugin](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] Der Quellcode und die Readme-Datei finden für das Plug-in Cordova [Github](https://github.com/Azure/azure-mobile-engagement-cordova) Sie

##<a id="setup-azme"></a>Mobile Engagement für Ihre Cordova Setup

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Mobile Engagement Backend herstellen Ihrer app

In diesem Lernprogramm wird eine grundlegende "Integration" ist die erforderlichen Daten sammeln und eine Push-Benachrichtigung zu senden. 

Wir erstellen eine einfache Anwendung mit Cordova die Integration zu veranschaulichen:

###<a name="create-a-new-cordova-project"></a>Erstellen eines neuen Projekts Cordova

1. Starten Sie *Terminal* -Fenster auf Ihrem Mac-Computer, und geben Sie Folgendes ein, die um ein neues Cordova Projekt aus der Standardvorlage erstellen. Sicherstellen Sie, dass Präsentationsprofil verwendeten schließlich iOS-app bereitstellen 'com.mycompany.myapp' als App-ID. verwenden 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Führen Sie zum Konfigurieren des Projekts für **iOS** und IOS Simulator ausführen Folgendes:

        $ cordova platform add ios 
        $ cordova run ios

3. Führen Sie Folgendes zum Konfigurieren des Projekts für **Android** und Android Emulator ausführen. Sicherstellen, dass Ihre Android SDK Emulatoreinstellungen Ziel als Google-APIs (Google Inc.) mit der CPU / ABI wie Google APIs ARM.  

        $ cordova platform add android
        $ cordova run android

4.  Das Plug-in Cordova Konsole hinzufügen. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Mobile Engagement Backend Ihrer app herstellen

1. Installieren des Azure Mobile Engagement Cordova Plugins gleichzeitig die Variablenwerte Plug-in konfigurieren:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Android erreichen Symbol* : muss der Namen der Ressource ohne jede Erweiterung noch zeichenbaren Präfix (ex: Mynotificationicon), die Symboldatei in android Projekt (Plattformen/Android/Res/zeichenbaren) kopiert werden muss

*iOS Symbol erreichen* : muss der Namen der Ressource mit der Erweiterung (ex: mynotificationicon.png), die Symboldatei in iOS-Projekt hinzugefügt werden muss, mit XCode (Menü hinzufügen Dateien)

##<a id="monitor"></a>Aktivieren der Überwachung in Echtzeit

1. Bearbeiten Sie Cordova Projekt - **www/js/index.js** hinzufügen, den Anruf zu Mobile eine neue Aktivität einmal *DeviceReady* Ereignis deklarieren.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Führen Sie die Anwendung:
        
    - **Für iOS**
    
        In `Terminal` Fenster starten Sie Ihre app in eine neue Instanz eines Simulator durch Folgendes ausführen:

            cordova run ios

    - **Für Android**
        
        In `Terminal` Fenster starten Ihrer Anwendung in einer neuen Emulatorinstanz durch Folgendes ausführen:

            cordova run android

3. Im Konsolenprotokoll erfasst Folgendes sehen:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging-Anwendung

Mobile Engagement können Benutzer Pushbenachrichtigungen mit app messaging im Rahmen von Kampagnen interagieren. Dieses Modul heißt REICHWEITE im Mobile Engagement-Portal.
In den folgenden Abschnitten werden Ihre Anwendung zum Empfangen einrichten.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Konfigurieren der Push-Anmeldeinformationen für Mobile

Um Mobile Engagement Pushbenachrichtigungen in Ihrem Auftrag senden können, müssen Sie Zugriff auf Ihre Apple iOS Zertifikat oder GCM Server API-Schlüssel zu gewähren. 
    
1. Navigieren Sie zum Portal Mobile Engagement. Sicherzustellen Sie, dass Sie in der app wir für dieses Projekt verwenden und klicken Sie auf **Aktivieren** unten:
    
    ![][1]
    
2. Auf der Einstellungsseite landen im Portal Engagement. Klicken Sie auf Abschnitt **Systemeigenen Push** vorhanden:
    
    ![][2]

3. IOS Zertifikat-GCM Server API-Schlüssel konfigurieren

    **[iOS]**

    ein. Wählen Sie Ihre P12 hochladen Sie und geben Sie Ihr Kennwort ein:
    
    ![][3]

    **[Android]**

    ein. Klicken Sie auf das Bearbeitungssymbol vor **API-Schlüssel** GCM Einstellungen und Popup der angezeigt wird, fügen Sie den Serverschlüssel GCM und klicken Sie auf **OK**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Push-Benachrichtigung in der Cordova-Anwendung aktivieren

Bearbeiten Sie **www/js/index.js** , um den Aufruf zu Mobile Pushbenachrichtigungen anfordern, und deklarieren Sie einen Ereignishandler hinzuzufügen:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>Führen Sie die Anwendung

**[iOS]**

1. Wir verwenden XCode erstellen und Bereitstellen der Anwendung auf dem Gerät Pushbenachrichtigungen da iOS nur Push-Benachrichtigung zu einem echten Gerät zu testen. Wechseln Sie zum Speicherort, in dem das Cordova-Projekt erstellt wurde, und navigieren Sie an **...\platforms\ios** . Die systemeigene .xcodeproj Datei in XCode öffnen. 
    
2. Erstellen Sie und Bereitstellen Sie Cordova app zu iOS-Gerät mit dem Konto das provisioning Profil mit Mobile Engagement-Portal und die App-Id des Zertifikats, die Sie gerade hochgeladen hat die Übereinstimmung der beim Erstellen der Cordova-Anwendung bereitgestellt. *Bundle-Bezeichner* in Auschecken der **Ressourcen\*-info.plist** Datei in XCode es übereinstimmen. 

3. Standard iOS Popups sehen auf Ihrem Gerät, dass die Anwendung berechtigt Benachrichtigungen anfordert. Erteilen der Berechtigung. 

**[Android]**

Den Emulator können einfach die Android führen Sie GCM Benachrichtigungen Android Emulator unterstützt. 

    cordova run android

##<a id="send"></a>Sendet eine Benachrichtigung an Ihre Anwendung

Jetzt erstellen wir eine einfache Push-Benachrichtigung-Kampagne, die einen Push für Ihre Anwendung auf dem Gerät senden:

1. Navigieren Sie zur Registerkarte **erreichen** im Portal Mobile Engagement

2. Klicken Sie auf **Neue Ankündigung** pushkampagne erstellen

    ![][6]

3. Bereitstellen von Eingaben **[Android]** Kampagne erstellen
    
    - Geben Sie einen **Namen** für die Kampagne. 
    - Wählen Sie **Liefertyp** *Benachrichtigung des Systems* *einfacher*
    - Wählen Sie die **Zeit** als *"jederzeit"*
    - Geben Sie einen **Titel** für die Benachrichtigung der ersten Zeile der Push werden.
    - Geben Sie eine **Nachricht** für die Benachrichtigung als Nachrichtentext dienen. 

    ![][11]

4. Bereitstellen von Eingaben erstellen Ihre Kampagne **[iOS]**

    - Geben Sie einen **Namen** für die Kampagne. 
    - Wählen Sie die **Lieferung** als *"aus app"*
    - Geben Sie einen **Titel** für die Benachrichtigung der ersten Zeile der Push werden.
    - Geben Sie eine **Nachricht** für die Benachrichtigung als Nachrichtentext dienen. 
 
    ![][12]

5. Bildlauf nach unten und im Abschnitt Inhalt **nur Benachrichtigung** auswählen

    ![][8]

6. [Optional] Sie können auch eine Aktions-URL bereitstellen. Sicherstellen, dass ein URL-Schema während Konfigurieren des Plug-Ins verwendet **AZME\_UMLEITEN\_URL** Variable z. B. *Myapp://test*.  

7. Sie grundlegende Kampagne Einstellung. Jetzt erneut Bildlauf aus, und klicken Sie auf **Erstellen** , um Ihre Kampagne speichern.

8. Schließlich **Aktivieren** Ihrer Kampagne
    
    ![][10]

9. Eine Push-Benachrichtigung sollte auf Ihrem Gerät oder Emulator nun im Rahmen dieser Kampagne angezeigt werden. 

##<a id="next-steps"></a>Nächste Schritte
[Übersicht über alle Methoden mit Cordova Mobile Engagement SDK](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

