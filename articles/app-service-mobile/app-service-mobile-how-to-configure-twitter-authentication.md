<properties
    pageTitle="Twitter-Authentifizierung für die Anwendungsdienste Anwendung konfigurieren"
    description="Informationen Sie zum Konfigurieren der Authentifizierung für die Anwendungsdienste Anwendung Twitter."
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

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Wie Sie Ihre App Twitter Login verwenden konfigurieren

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema veranschaulicht die Azure App Service Verwendung Twitter als Authentifizierungsanbieter konfigurieren.

Die Verfahren in diesem Thema benötigen Sie einen Twitter-Konto, die eine überprüft e-Mail-Adresse und Anzahl. Gehen Sie zum Erstellen eines neuen Twitter-Kontos <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Die Anwendung bei Twitter registrieren


1. [Azure-Portal]melden Sie an und navigieren Sie zu der Anwendung. Kopieren Sie die **URL**. Sie verwenden diese app Twitter konfigurieren.

2. Navigieren Sie zu der Website [Twitter Entwickler] Ihre Twitter-Anmeldeinformationen melden Sie an und klicken Sie auf **Neue Anwendung erstellen**.

3. Geben Sie den **Namen** und eine **Beschreibung** für Ihre neue Anwendung. Fügen Sie in der Anwendung **URL** für die **Website** -Wert. Fügen Sie dann für die **Callback-URL**bereits kopierten **Callback-URL** . Dies ist die Mobile Anwendung Gateway mit den Pfad _/.auth/login/twitter/callback_angefügt. Z. B. `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Stellen Sie sicher, dass Sie HTTPS-Schema verwenden.

3.  Am unteren Rand der Seite lesen und annehmen. Klicken Sie auf **der Twitter-Anwendung erstellen**. Dadurch wird die app zeigt Details zur jeweiligen Anwendung registriert.

4. Klicken Sie auf **die Registerkarte** , überprüfen Sie **diese Anwendung mit Twitter anmelden**und klicken Sie **Aktualisieren**.

5. Wählen Sie die Registerkarte **Schlüssel und Zugriffstoken** . Notieren Sie sich die Werte der **Verbraucher Taste (API)** und **Verbrauchergeheimnis (API-Schlüssel)**.

    > [AZURE.NOTE] Das Verbrauchergeheimnis ist ein wichtiges Sicherheitsfeature. Teilen Sie diesen Schlüssel mit weder mit Ihrer Anwendung verteilen.


## <a name="secrets"> </a>Informationen zu Ihrer Anwendung hinzufügen Twitter

13. Navigieren Sie in [Azure-Portal]zur Anwendung. Klicken Sie auf **Einstellungen**und dann **Authentifizierung / Autorisierung**.

14. Wenn die Authentifizierung / Autorisierung ist nicht aktiviert, Schalter **auf**.

15. Klicken Sie auf **Twitter**. Fügen Sie die App-ID und App Werte, die Sie zuvor erworben. Klicken Sie auf **OK**.

    ![][1]

    Standardmäßig App Service bietet Authentifizierung jedoch nicht den autorisierten Zugriff auf Ihre Inhalte und APIs. Sie müssen in Ihrem Anwendungscode Benutzer autorisieren.

17. (Optional) Um Zugriff auf Ihre Website nur Twitter authentifizierte Benutzer auf **Twitter**festgelegt **Aktion Anforderung nicht authentifiziert ist** . Dies erfordert, dass alle Clientanforderungen authentifiziert werden, und alle nicht authentifizierte Anfragen werden auf Twitter Authentifizierung umgeleitet.

17. Klicken Sie auf **Speichern**.

Sie können nun Twitter für die Authentifizierung in Ihrer Anwendung verwenden.

## <a name="related-content"> </a>Inhalte

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter-Entwickler]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure-portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
