<properties 
    pageTitle="Wie autorisieren Developer Konten OAuth 2.0 in Azure API Management" 
    description="Erfahren Sie, wie Benutzer OAuth 2.0 API Management autorisiert." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Wie autorisieren Developer Konten OAuth 2.0 in Azure API Management

Viele APIs unterstützen [OAuth 2.0](http://oauth.net/2/) , um die API und können nur auf Ressourcen zugreifen, die sie berechtigt sind, dass nur gültige Benutzer Zugriff haben. Um interaktive Entwicklerkonsole Azure API Management diese APIs verwenden, ermöglicht der Dienst die Dienstinstanz zum Arbeiten mit Ihrem OAuth 2.0 aktiviert API konfigurieren.

## <a name="prerequisites"> </a>Komponenten

Dieses Handbuch zeigt, wie Sie die API Management Service Instanz um OAuth 2.0 Autorisierung für entwicklerkonten verwenden, aber zeigt Sie wie OAuth 2.0-Anbieter konfigurieren. Die Konfiguration für jeden Anbieter OAuth 2.0 unterscheidet, obwohl die beschriebenen Schritte und die erforderlichen Informationen konfigurieren OAuth 2.0 in Ihrem API Management-Dienstinstanz identisch sind. Dieses Thema zeigt Beispiele für die Verwendung von Azure Active Directory als Anbieter OAuth 2.0.

>[AZURE.NOTE] Weitere Informationen zum Konfigurieren von OAuth 2.0 mit Azure Active Directory finden Sie unter Beispiel [WebApp GraphAPI DotNet][] .

## <a name="step1"> </a>In API Management einen OAuth 2.0 autorisierungsserver konfigurieren

Klicken Sie zunächst auf **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst. Dadurch gelangen Sie zu Publisher API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>[AZURE.NOTE] Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

Klicken Sie auf **Sicherheit** aus der **API-** Menü auf der linken Seite und klicken Sie auf **Add Autorisierung** **OAuth 2.0**auf.

![OAuth 2.0][api-management-oauth2]

Nach dem Klicken auf **autorisierungsserver hinzufügen**, wird das neue Server Einzugsermächtigungsformular angezeigt.

![Neue server][api-management-oauth2-server-1]

Geben Sie in die Felder **Name** und **Beschreibung** einen Namen und eine Beschreibung. 

>[AZURE.NOTE] Diese Felder dienen zum Identifizieren von OAuth 2.0 autorisierungsserver innerhalb der aktuellen API Management Service und deren Werte nicht von OAuth 2.0 Server stammen.

Geben Sie die **URL der Registrierung Client**. Diese Seite ist, wo Benutzer erstellen und Verwalten ihrer Konten und variiert je nach Anbieter OAuth 2.0 verwendet. **URL der Client Registrierung** verweist auf der Seite, mit denen Benutzer erstellen und konfigurieren ihre eigenen Konten für OAuth 2.0-Provider, die Verwaltung von Konten unterstützen. Einige Organisationen nicht konfigurieren oder verwenden Sie diese Funktion, selbst wenn der Anbieter OAuth 2.0 unterstützt. Wenn Dienstanbieter OAuth 2.0 nicht Verwaltung von Konten konfiguriert haben, geben Sie eine Platzhalter-URL hier die URL des Unternehmens oder eine URL wie `https://placeholder.contoso.com`.

Der nächste Abschnitt des Formulars enthält die **Autorisierungscode gewähren Typen**, **Autorisierung Endpunkt-URL**und **Autorisierung Anforderung** .

![Neue server][api-management-oauth2-server-2]

Überprüfen die gewünschten Typen geben Sie **Autorisierungscode gewähren Typen** an. **Autorisierungscode** ist standardmäßig ausgewählt.

Geben Sie die **Autorisierung Endpunkt-URL**. Azure Active Directory diese URL werden folgende URL ähnelt dem `<client_id>` ersetzt die Client-Id, die die Anwendung OAuth 2.0 Server identifiziert.

    https://login.windows.net/<client_id>/oauth2/authorize

**Autorisierung Anforderungsmethode** gibt an, wie die Authentifizierungsanfrage OAuth 2.0-Server gesendet wird. **GET** ist standardmäßig aktiviert.

Im nächste Abschnitt wird, wobei die **Token-Endpunkt-URL**, **Clientauthentifizierungsmethoden** **Zugriffstoken Methode senden**und **Standard Bereich** angegeben werden.

![Neue server][api-management-oauth2-server-3]

Für einen Server Azure Active Directory OAuth 2.0 die **Endpunkt-URL-Token** haben das folgende Format, `<APPID>` hat das Format `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

**Die Standardeinstellung für **Clientauthentifizierungsmethoden** handelt**und **Zugriffstoken senden-Methode** ist **Authorization-Header**. Diese Werte werden in diesem Abschnitt des Formulars zusammen mit der **Bereich standardmäßig**konfiguriert.

Der **Clientanmeldeinformationen** Abschnitt enthält **Client-ID** und **geheimen**, die bei der Erstellung und Konfiguration der OAuth 2.0-Server abgerufen werden. Nachdem die **Client-ID** und **geheimen** angegeben sind, wird der **Redirect_uri** für den **Autorisierungscode** generiert. Dieser URI wird verwendet, in der Serverkonfiguration OAuth 2.0 Antworten konfigurieren.

![Neue server][api-management-oauth2-server-4]

Wenn **Autorisierungscode gewähren Typen** auf **Ressource Besitzerkennwort**festgelegt ist, wird im Abschnitt **Ressource Besitzer Anmeldeinformationen** Anmeldeinformationen angeben; Andernfalls können Sie es leer lassen.

![Neue server][api-management-oauth2-server-5]

Nach Abschluss des Formulars klicken Sie auf **Speichern** , um die API Management OAuth 2.0 Autorisierung Server-Konfiguration zu speichern. Nach der Konfiguration der Server können Sie APIs diese Konfiguration, wie im nächsten Abschnitt gezeigt konfigurieren.

## <a name="step2"> </a>API zur OAuth 2.0 Benutzer Autorisierung konfigurieren

Klicken Sie auf **APIs** der **API-** Menü auf der linken Seite, klicken Sie auf den Namen der gewünschten API klicken Sie auf **Sicherheit**und aktivieren Sie das Kontrollkästchen für **OAuth 2.0**.

![Benutzer][api-management-user-authorization]

Wählen Sie den gewünschten **autorisierungsserver** aus der Dropdown-Liste, und klicken Sie auf **Speichern**.

![Benutzer][api-management-user-authorization-save]

## <a name="step3"> </a>Im Entwicklerportal Benutzerberechtigung OAuth 2.0 testen

Nach konfigurierten Servers Autorisierung OAuth 2.0 und Konfiguration Ihrer API dieses Servers können Sie ihn testen, Entwicklerportal und Aufrufen einer API.  Klicken Sie im Menü oben rechts auf **Entwicklerportal** .

![Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie im oberen Menü auf **APIs** , und wählen Sie **Echo-API**.

![Echo-API][api-management-apis-echo-api]

>[AZURE.NOTE] Haben Sie nur eine API konfiguriert oder Ihrem Konto sichtbar, gelangen auf APIs direkt in die Operationen für diese API.

Wählen Sie die **Ressource abrufen** -Operation und wählen Sie dann aus der Dropdownliste **Autorisierungscode** auf **Geöffnete Konsole**.

![Konsole öffnen][api-management-open-console]

Wenn **Autorisierungscode** aktiviert ist, wird ein Popup-Fenster mit anmelden OAuth 2.0-Anbieter angezeigt. In diesem Beispiel wird das Formular von Azure Active Directory bereitgestellt.

>[AZURE.NOTE] Wenn Sie Popups deaktiviert haben werden Sie aufgefordert, im Browser ermöglichen. Nachdem sie aktiviert wählen erneut **Autorisierungscode** und das Formular erscheint.

![Anmelden][api-management-oauth2-signin]

Nachdem Sie sich angemeldet haben, die **Anforderungsheader** enthalten ein `Authorization : Bearer` -Header, der die Anforderung autorisiert.

![Anfrage-Header-token][api-management-request-header-token]

An diesem Punkt können Sie die gewünschten Werte für die übrigen Parameter konfigurieren und übermitteln der Anforderung. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über OAuth 2.0 und Management-API finden Sie im folgenden Video und [Artikel](api-management-howto-protect-backend-with-aad.md).

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Erste Schritte mit Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp GraphAPI DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

