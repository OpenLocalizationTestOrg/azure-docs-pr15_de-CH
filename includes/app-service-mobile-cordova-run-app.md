
1. Besuchen Sie das [Azure-Portal]. Klicken Sie auf **Alle suchen** > **Mobile Apps** > Backend, die Sie gerade erstellt haben. In den Einstellungen mobile-app klicken Sie auf **Schnellstart** > **Cordova**. Erstellen Sie unter **Konfigurieren der Clientanwendung** **Einer neuen Anwendung**und klicken Sie auf **Download**. Diese downloads ein vollständiges Cordova-Projekt für eine Anwendung Ihre Back-End-Verbindung konfiguriert.

2. Entpacken Sie die heruntergeladene ZIP-Datei in ein Verzeichnis auf Ihrer Festplatte, um die Projektmappendatei (.sln) navigieren Sie und mit Visual Studio öffnen.

5. In Visual Studio Projektmappenplattform (Android, iOS oder Windows) aus der Dropdown-Pfeil Start wählen Sie und dann eine bestimmte Bereitstellungsgerät oder Emulator durch Klicken auf den Dropdown-Pfeil auf den grünen Pfeil. Beachten Sie, dass Sie Android Standardplattform und die Wellen Emulator. Weitere Tutorials benötigen Sie ein unterstütztes Gerät oder Emulator. 

6. Drücken Sie F5, oder klicken Sie auf den grünen Pfeil erstellen und und Cordova-app. Erscheint ein Sicherheitsdialog Emulator anfordernden Zugriff auf das Netzwerk akzeptieren.   

7. Nach dem Start der app auf dem Gerät oder Emulator Geben Sie einen aussagekräftigen Text **Geben Sie neuen Text**wie _das Lernprogramm abgeschlossen_ , und klicken Sie dann auf die Schaltfläche **Hinzufügen** .  
Sendet eine POST-Anforderung an den Azure Backend bereits bereitgestellt. Back-End-fügt Daten aus der Anforderung ist in der TodoItem-Tabelle in der SQL-Datenbank und gibt Informationen über die neu gespeicherten Elemente an die app. Die app Daten in der Liste angezeigt.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Wiederholen Sie die vorangegangenen drei Schritte für jede Plattform, die Sie unterstützen möchten.

[Azure-Portal]: https://portal.azure.com/
