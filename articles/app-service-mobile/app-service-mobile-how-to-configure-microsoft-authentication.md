<properties
    pageTitle="Authentifizierung für die Anwendungsdienste Anwendung Microsoft Account konfigurieren"
    description="Informationen Sie zum Konfigurieren der Authentifizierung für die Anwendungsdienste Anwendung Microsoft Account."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Wie Sie Ihre App für Microsoft Account Anmeldung konfigurieren

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema veranschaulicht die Azure App Service Verwendung Microsoft Account als Authentifizierungsanbieter konfigurieren. 

## <a name="register-microsoft-account"> </a>Ihre Anwendung mit Microsoft-Konto registrieren

1. [Azure-Portal]melden Sie an und navigieren Sie zu der Anwendung. Kopieren Sie die **URL**, später mit Ihrer Anwendung mit Microsoft Account zu konfigurieren.

2. Navigieren Sie zu der Seite [Meine Programme] Microsoft Account Developer Center und ggf. mit Ihrem Microsoft-Konto anmelden.

3. Klicken Sie auf **eine Anwendung hinzufügen**, und geben Sie einen Namen und auf **Anwendung erstellen**.

4. Notieren Sie sich die **ID der Anwendung**Bedarf später wird. 

5. Klicken Sie auf **Add Platform** "Plattformen", und wählen Sie "Web".

6. Bereitstellen Sie unter "URIs umleiten" Endpunkt für die Anwendung, und klicken Sie auf **Speichern**. 
 
    >[AZURE.NOTE]Die URI-Umleitung ist der URL der Anwendung mit den Pfad _/.auth/login/microsoftaccount/callback_angefügt. Z. B. `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Stellen Sie sicher, dass Sie HTTPS-Schema verwenden.

7. Klicken Sie unter "Anwendung Geheimnisse" **Neues Kennwort generieren**. Notieren Sie sich den Wert. Verlassen die Seite wird es nicht erneut angezeigt.


    > [AZURE.IMPORTANT] Das Kennwort ist ein wichtiges Sicherheitsfeature. Mit Teilen weder einer Clientanwendung verteilen.

## <a name="secrets"> </a>Microsoft Kontoinformationen hinzufügen Ihrer App Service-Anwendung

1. Zurück zur Anwendung in [Azure-Portal]navigieren, **Klicken Sie** > **Authentifizierung / Autorisierung**.

2. Wenn die Authentifizierung / Autorisierung ist nicht aktiviert, **Einschalten**.

3. Klicken Sie auf **Microsoft-Konto**. Fügen Sie in der Anwendung-ID und Kennwort Werte zuvor gewonnen und optional aktivieren Sie alle Bereiche der Anwendung zu. Klicken Sie auf **OK**.

    ![][1]

    Standardmäßig App Service bietet Authentifizierung jedoch nicht den autorisierten Zugriff auf Ihre Inhalte und APIs. Sie müssen in Ihrem Anwendungscode Benutzer autorisieren.

4. (Optional) Legen Sie zum Einschränken des Zugriffs auf Ihre Website nur für Benutzer von Microsoft-Konto authentifiziert **Aktion nicht authentifizierte Anforderung** **Microsoft**-Konto. Dies erfordert, dass alle Clientanforderungen authentifiziert werden, und alle nicht authentifizierte Anfragen an Microsoft-Konto für die Authentifizierung umgeleitet werden.

5. Klicken Sie auf **Speichern**.

Sie können jetzt Microsoft Account zur Authentifizierung in Ihrer Anwendung verwenden.

## <a name="related-content"> </a>Inhalte

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Meine Programme]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portal]: https://portal.azure.com/
