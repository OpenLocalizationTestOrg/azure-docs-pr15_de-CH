
1. Besuchen Sie das [Azure-Portal]. Klicken Sie auf **Alle suchen** > **Mobile Apps** > Backend, die Sie gerade erstellt haben. In den Einstellungen mobile-app klicken Sie auf **Schnellstart** > **Android)**. **Konfigurieren der Clientanwendung**klicken Sie auf **herunterladen**. Diese downloads ein vollständiges Android Projekt für eine Anwendung Ihre Back-End-Verbindung konfiguriert. 

2. Öffnen Sie das Projekt mithilfe von **Android Studio**, Verwendung **importieren (Eclipse ADT, Gradle usw.)**. Sicherstellen Sie, dass diese Auswahl importieren JDK Fehler zu machen.

3. Schaltfläche **'Anwendung' ausführen** , erstellen Sie das Projekt und die app Android Simulator starten.

4. Geben Sie in der app einen aussagekräftigen Text wie _das Lernprogramm abgeschlossen_ , und klicken Sie dann auf die Schaltfläche 'Hinzufügen'. Sendet eine POST-Anforderung an den Azure Backend bereits bereitgestellt. Backend-fügt Daten aus der Anforderung ist in der TodoItem SQL-Tabelle und gibt Informationen über die neu gespeicherten Elemente an die app. Die app Daten in der Liste angezeigt. 

    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure-Portal]: https://portal.azure.com/
