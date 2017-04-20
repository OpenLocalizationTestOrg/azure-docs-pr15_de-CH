

1. Starten Sie auf Ihrem Mac **Schlüsselbund**. Öffnen Sie **Eigene Zertifikate** unter **Kategorie** auf der linken Navigationn. Das SSL-Zertifikat im vorherigen Abschnitt heruntergeladene suchen und dessen Inhalt anzugeben. Wählen Sie das Zertifikat (wählen den privaten Schlüssel nicht), und [Exportieren](https://support.apple.com/kb/PH20122?locale=en_US).

2. Klicken Sie im [Azure-Portal](https://portal.azure.com/)auf **Alle durchsuchen** > **Anwendungsdienste** > Mobile App Backend. Unter **Einstellungen**auf **App Service drücken**, klicken Ihre Hubnamen. **Apple Push Notification Services**zur > **Zertifikat hochladen**. Laden Sie die P12-Datei im richtigen **Modus** auswählen (je nachdem, ob der Client SSL Zertifikat früher ist Produktions- oder Sandbox). Speichern.

Der Dienst wird jetzt konfiguriert Pushbenachrichtigungen IOS!

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
