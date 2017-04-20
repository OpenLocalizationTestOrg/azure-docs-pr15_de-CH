<properties
    pageTitle="Facebook-Authentifizierung für die Anwendungsdienste Anwendung konfigurieren"
    description="Informationen Sie zum Konfigurieren der Authentifizierung für die Anwendungsdienste Anwendung Facebook."
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

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Wie Sie Ihre Anwendung mit Facebook Anmelden konfigurieren

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema veranschaulicht die Azure App Service Verwendung Facebook als Authentifizierungsanbieter konfigurieren.

Die Verfahren in diesem Thema benötigen Sie eine Facebook-Konto, das eine überprüft e-Mail-Adresse und Mobiltelefonnummer. Um ein neues Facebook-Konto zu erstellen, gehen Sie zu [facebook.com].

## <a name="register"> </a>Ihrer Anwendung mit Facebook anmelden

1. [Azure-Portal]melden Sie an und navigieren Sie zu der Anwendung. Kopieren Sie die **URL**. Sie verwenden diese Ihre Facebook-Anwendung konfigurieren.

2. In einem anderen Browserfenster [Facebook-Entwickler] -Website navigieren Sie und mit Ihren Facebook-Konto-Anmeldeinformationen anmelden.

3. (Optional) Wenn Sie noch nicht registriert haben, klicken Sie auf **Apps** > **als Entwickler registrieren**, übernehmen Sie die Richtlinie und führen Sie die Schritte zur Registrierung.

4. Klicken Sie auf **Meine Apps** > **Hinzufügen einer neuen Applikation** > **Website** > **überspringen und App-ID erstellen**. 

5. **Angezeigter Name**einen eindeutigen Namen für Ihre, geben Sie Ihrem **Kontakt E-Mail**, wählen Sie eine **Kategorie** für Ihre Anwendung auf **App-ID erstellen** und Sicherheitskontrolle abgeschlossen. Dadurch gelangen Sie zum Dashboard Developer für Ihre neue Facebook.

6. Klicken Sie unter "Facebook anmelden" **Beginnen**. Fügen Sie die Anwendung **URI umleiten** umleiten **gültige OAuth URIs hinzu**und dann auf **Speichern**. 

    > [AZURE.NOTE] Die URI-Umleitung ist der URL der Anwendung mit den Pfad _/.auth/login/facebook/callback_angefügt. Z. B. `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Stellen Sie sicher, dass Sie HTTPS-Schema verwenden.

6. **Klicken Sie auf in der linken Navigationsleiste.** Das Feld **App Secret** auf **Anzeigen**, geben Sie Ihr Kennwort ein, falls angefordert, notieren Sie sich die Werte der **App-ID** und **Schlüssel App**. Sie verwenden diese später zum Konfigurieren einer Anwendung in Azure.

    > [AZURE.IMPORTANT] App-Schlüssel ist ein wichtiges Sicherheitsfeature. Jeder dieser Schlüssel freigeben oder einer Clientanwendung verteilen nicht.

7. Facebook-Konto der Anwendung registriert wurde ist ein Administrator der app. Nur Administratoren können jetzt in der Anwendung anmelden. Um andere Konten Facebook authentifizieren auf **App überprüfen** und **öffentlichen Stellen < Ihr app-Name >** allgemeine Öffentlichkeit Facebook Authentifizierung aktivieren.

## <a name="secrets"> </a>Hinzufügen Facebook Informationen zur Anwendung

1. Navigieren Sie in [Azure-Portal]zur Anwendung. **Klicken Sie** > **Authentifizierung / Autorisierung**, und stellen Sie sicher, dass **App-Authentifizierung** **auf**.

2. ** **Facebook**, fügen Sie die App-ID und Schlüssel App Werte zuvor gewonnen, optional ermöglichen Sie die Anwendung erforderlichen Bereiche, klicken.**

    ![][0]

    Standardmäßig App Service bietet Authentifizierung jedoch nicht den autorisierten Zugriff auf Ihre Inhalte und APIs. Sie müssen in Ihrem Anwendungscode Benutzer autorisieren.

3. (Optional) Um Zugriff auf Ihre Website nur von Facebook authentifiziert Benutzer **Facebook**soll **Aktion Anforderung nicht authentifiziert ist** . Dies erfordert, dass alle Clientanforderungen authentifiziert werden, und alle nicht authentifizierte Anfragen an Facebook zur Authentifizierung umgeleitet werden.

4. Wenn Authentifizierung konfigurieren, klicken Sie auf **Speichern**.

Sie können jetzt Facebook für die Authentifizierung in Ihrer Anwendung verwenden.

## <a name="related-content"> </a>Inhalte

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-Entwickler]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-portal]: https://portal.azure.com/
