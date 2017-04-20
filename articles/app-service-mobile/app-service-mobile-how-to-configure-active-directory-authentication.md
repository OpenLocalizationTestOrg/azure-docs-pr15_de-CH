<properties
    pageTitle="Azure Active Directory-Authentifizierung für die Anwendungsdienste Anwendung konfigurieren"
    description="Informationen Sie zum Konfigurieren von Azure Active Directory-Authentifizierung für die Anwendungsdienste Anwendung."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
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

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Wie Sie Ihre App mit Azure Active Directory-Konto konfigurieren

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema veranschaulicht die Azure App Services zur Verwendung von Azure Active Directory als Authentifizierungsanbieter konfigurieren.

## <a name="express"> </a>Azure Active Directory konfigurieren mit express-Standardeinstellungen

13. Navigieren Sie in [Azure-Portal]zur Anwendung. Klicken Sie auf **Einstellungen**und dann auf **Authentifizierung**.

14. Wenn die Authentifizierung / Autorisierung ist nicht aktiviert, Schalter **auf**.

15. Klicken Sie unter **Verwaltung**auf **Express** auf **Azure Active Directory**

16. Klicken Sie auf **OK** , um die Anwendung in Azure Active Directory registriert. Dadurch wird eine neue Registrierung erstellt. Möchten Sie stattdessen eine vorhandene Registrierung, **Wählen Sie eine vorhandene Anwendung** auf und suchen Sie nach dem Namen einer zuvor erstellten Registrierung in Ihrem Mandanten.
Klicken Sie auf die Registrierung und klicken Sie auf **OK**. **Klicken Sie auf den Azure Active Directory Settings.**

    ![][0]

    Standardmäßig App Service bietet Authentifizierung jedoch nicht den autorisierten Zugriff auf Ihre Inhalte und APIs. Sie müssen in Ihrem Anwendungscode Benutzer autorisieren.

17. (Optional) **Aktion nicht authentifizierte Anforderung** soll zum Einschränken des Zugriffs auf Ihre Website nur von Azure Active Directory authentifiziert Benutzer **Azure Active Directory einloggen**. Dies erfordert, dass alle Clientanforderungen authentifiziert werden, und alle nicht authentifizierte Anfragen in Azure Active Directory für die Authentifizierung umgeleitet werden.

17. Klicken Sie auf **Speichern**.

Sie können jetzt Azure Active Directory für die Authentifizierung in Ihrer Anwendung verwenden.

## <a name="advanced"> </a>(Alternative Methode) mit erweiterten Einstellungen Azure Active Directory manuell konfigurieren
Sie können auch eine Konfiguration manuell. Dies ist die bevorzugte Lösung AAD-Mandanten verwenden möchten von der Mieter mit dem Azure anmelden. Um die Konfiguration abzuschließen, müssen Sie zunächst eine Registrierung erstellen, in Azure Active Directory und dann geben Sie einige der Registrierungsdaten App Service.

### <a name="register"> </a>Anwendung in Azure Active Directory registrieren

1. [Azure-Portal]melden Sie an und navigieren Sie zu der Anwendung. Kopieren Sie die **URL**. Sie verwenden diese app Azure Active Directory konfigurieren.

3. Das [klassische Azure-Portal] anmelden und navigieren zu **Active Directory**.

    ![][2]

4. Wählen Sie das Verzeichnis, und wählen Sie die Registerkarte **Applications** oben. **Klicken Sie auf unten, um eine neue Registrierung einer Anwendung erstellen.**

5. Klicken Sie auf **Mein Unternehmen entwickelten Anwendung hinzufügen**.

6. Anwendung-Assistenten geben Sie einen **Namen** für die Anwendung und klicken Sie auf die **Web-Anwendung oder Web-API** . Klicken Sie dann auf Weiter.

7. Fügen Sie im Feld **URL Zeichen auf** die zuvor kopierte URL. Geben Sie im Feld **App ID URI** , denselben URL. Klicken Sie dann auf Weiter.

8. Nachdem die Anwendung hinzugefügt wurde, klicken Sie auf die Registerkarte **Konfigurieren** . Bearbeiten der **Antwort-URL** unter **auf** die URL der Anwendung mit den Pfad _/.auth/login/aad/callback_angefügt werden. Z. B. `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Stellen Sie sicher, dass Sie HTTPS-Schema verwenden.

    ![][3]

9. Klicken Sie auf **Speichern**. Kopieren Sie die **Client-ID** für die Anwendung. Sie konfigurieren die Anwendung später verwenden.

10. Klicken Sie in der Befehlsleiste unten auf **Ansicht Endpunkte**und dann kopieren Sie die URL der **Verbundmetadaten-Dokument** herunterladen Sie dieses Dokument und in einem Browser navigieren.

11. Im Stammelement **EntityDescriptor** sollte ein **%EntityID** -Attribut des Formulars `https://sts.windows.net/` gefolgt von einer bestimmten GUID Ihrem Mandanten ("Mieter ID" genannt). Kopieren Sie diesen Wert - dient als **Aussteller URL**. Sie konfigurieren die Anwendung später verwenden.

### <a name="secrets"> </a>Hinzufügen Azure Active Directory-Informationen für die Anwendung

13. Navigieren Sie in [Azure-Portal]zur Anwendung. Klicken Sie auf **Einstellungen**und dann auf **Authentifizierung**.

14. Wenn die Authentifizierung-Funktion nicht aktiviert ist, Schalter **auf**.

15. Klicken Sie auf **Erweitert** klicken Sie unter **Verwaltung**auf **Azure Active Directory** Fügen Sie in der Client-ID und Aussteller URL zuvor gewonnen. Klicken Sie auf **OK**.

    ![][1]

    Standardmäßig App Service bietet Authentifizierung jedoch nicht den autorisierten Zugriff auf Ihre Inhalte und APIs. Sie müssen in Ihrem Anwendungscode Benutzer autorisieren.

17. (Optional) **Aktion nicht authentifizierte Anforderung** soll zum Einschränken des Zugriffs auf Ihre Website nur von Azure Active Directory authentifiziert Benutzer **Azure Active Directory einloggen**. Dies erfordert, dass alle Clientanforderungen authentifiziert werden, und alle nicht authentifizierte Anfragen in Azure Active Directory für die Authentifizierung umgeleitet werden.

17. Klicken Sie auf **Speichern**.

Sie können jetzt Azure Active Directory für die Authentifizierung in Ihrer Anwendung verwenden.

## <a name="optional-configure-a-native-client-application"></a>(Optional) Konfigurieren Sie eine systemeigene Anwendung

Azure Active Directory können systemeigene Clients registrieren bietet mehr Kontrolle über Berechtigungen zuordnen. Sie benötigen eine Bibliothek wie **Active Directory Authentifizierungsbibliothek**Benutzernamen ausführen möchten.

1. Navigieren Sie in **Active Directory** im [klassischen Azure-Portal].

2. Wählen Sie das Verzeichnis, und wählen Sie die Registerkarte **Applications** oben. **Klicken Sie auf unten, um eine neue Registrierung einer Anwendung erstellen.**

3. Klicken Sie auf **Mein Unternehmen entwickelten Anwendung hinzufügen**.

4. Anwendung-Assistenten geben Sie einen **Namen** für die Anwendung und klicken Sie auf die **Systemeigene Clientanwendung** . Klicken Sie dann auf Weiter.

5. Geben Sie im **URI umleiten** Ihrer Site _/.auth/login/done_ Endpunkt mit HTTPS-Schema. Dieser Wert sollte ähnlich wie _https://contoso.azurewebsites.net/.auth/login/done_. Wenn eine Windows-Anwendung erstellen, verwenden Sie das [Paket SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als URI.

6. Sobald die ursprüngliche Anwendung hinzugefügt wurde, klicken Sie auf die Registerkarte **Konfigurieren** . Suchen Sie die **Client-ID** , und notieren Sie sich diesen Wert.

7. Einen Bildlauf zum Abschnitt **Berechtigungen für andere Programme** , und klicken Sie auf **Anwendung hinzufügen**.

8. Web-Anwendung, die Sie zuvor registriert suchen Sie, und klicken Sie auf das Pluszeichen. Klicken Sie auf das Häkchen, um das Dialogfeld zu schließen. Wenn die Anwendung gefunden und navigieren Sie zu der Registrierung hinzufügen eine neue Antwort-URL (z. B. die HTTP-Version der aktuellen URL) werden kann, klicken Sie auf Speichern, und wiederholen Sie diese Schritte - Anwendung sollte in der Liste angezeigt.

9. Öffnen Sie für den neuen Eintrag gerade hinzugefügte **Berechtigungen delegiert** Dropdown und wählen Sie **Zugriff (Anwendungsname)**. Klicken Sie auf **Speichern**.

Sie haben jetzt eine systemeigene Anwendung konfiguriert die App Service-Anwendung zugreifen können.

## <a name="related-content"> </a>Inhalte

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[Azure-Verwaltungsportal]: https://manage.windowsazure.com/
[alternative method]:#advanced
