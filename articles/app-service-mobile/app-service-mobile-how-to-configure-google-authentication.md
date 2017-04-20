<properties
    pageTitle="Google-Authentifizierung für die Anwendungsdienste Anwendung konfigurieren"
    description="Informationen Sie zum Konfigurieren der Authentifizierung für die Anwendungsdienste Anwendung Google."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Wie Sie Ihre App für Google Anmeldung konfigurieren

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema veranschaulicht die Azure App Service Verwendung Google als Authentifizierungsanbieter konfigurieren.

Die Verfahren in diesem Thema benötigen Sie ein Konto, das eine überprüft e-Mail-Adresse. Um ein neues Konto zu erstellen, gehen Sie zu [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Anwendung mit Google registrieren

1. [Azure-Portal]melden Sie an und navigieren Sie zu der Anwendung. Kopieren Sie die **URL**, später konfigurieren Google-app mit.

2. Navigieren Sie zu der Website [Google-apis](http://go.microsoft.com/fwlink/p/?LinkId=268303) , die Google-Anmeldeinformationen melden Sie an, **Projekt erstellen**, geben Sie einen **Projektnamen**klicken **Erstellen**.

3. Klicken Sie unter **Sozialen APIs** **Google-API** und **Aktivieren**.

4. In der linken Navigationsleiste **Anmeldeinformationen** > **OAuth Zustimmung Bildschirm**wählen Sie Ihre **e-Mail-Adresse**, geben Sie einen **Produktnamen**, und klicken Sie auf **Speichern**.

5. Klicken Sie auf der Registerkarte **Anmeldeinformationen** **Erstellen Anmeldeinformationen** > **OAuth-Client-ID**, und wählen Sie **Web-Anwendung**.

6. Fügen Sie die App- **URL** bereits **autorisiert JavaScript**Ursprünge kopiert und **Umleiten URI autorisiert**die URI-Umleitung einfügen. Die Umleitung URI ist der URL der Anwendung mit den Pfad _/.auth/login/google/callback_angefügt. Z. B. `https://contoso.azurewebsites.net/.auth/login/google/callback`. Stellen Sie sicher, dass Sie HTTPS-Schema verwenden. Klicken Sie auf **Erstellen**.

7. Auf dem nächsten Bildschirm Notieren der Werte der Client-ID und geheimen.


    > [AZURE.IMPORTANT]
    Clientschlüssel ist ein wichtiges Sicherheitsfeature. Jeder dieser Schlüssel freigeben oder einer Clientanwendung verteilen nicht.


## <a name="secrets"> </a>Angaben hinzufügen zur Anwendung

8. Navigieren Sie in [Azure-Portal]zur Anwendung. Klicken Sie auf **Einstellungen**und dann **Authentifizierung / Autorisierung**.

9. Wenn die Authentifizierung / Autorisierung ist nicht aktiviert, Schalter **auf**.

10. Klicken Sie auf **Google**. Fügen Sie die App-ID und Schlüssel App Werte zuvor gewonnen und optional aktivieren Sie alle Bereiche der Anwendung zu. Klicken Sie auf **OK**.

    ![][1]

    Standardmäßig App Service bietet Authentifizierung jedoch nicht den autorisierten Zugriff auf Ihre Inhalte und APIs. Sie müssen in Ihrem Anwendungscode Benutzer autorisieren.

17. (Optional) Um Zugriff auf Ihre Website nur von Google authentifiziert Benutzer festlegen Sie **Google** **Aktion Anforderung nicht authentifiziert ist** . Dies erfordert, dass alle Clientanforderungen authentifiziert werden, und alle nicht authentifizierte Anfragen an Google für die Authentifizierung umgeleitet werden.

12. Klicken Sie auf **Speichern**.

Sie können jetzt Google für die Authentifizierung in Ihrer Anwendung verwenden.

## <a name="related-content"> </a>Inhalte

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portal]: https://portal.azure.com/

