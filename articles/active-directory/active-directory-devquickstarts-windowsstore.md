<properties
    pageTitle="Azure AD Windows Store Einstieg | Microsoft Azure"
    description="Erstellen eine Windows Store-Anwendung, die in Azure AD Anmelden integriert werden und ruft Azure AD geschützten APIs mit OAuth."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Azure AD in Windows Store-App integrieren

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Wenn Sie eine app für den Windows Store entwickeln, vereinfacht Azure AD und für Ihre Benutzer ihre Active Directory-Konten zu authentifizieren.  Außerdem können die Anwendung sicher alle Web API geschützt von Azure AD Azure-API oder die Office 365-APIs verwenden.

Azure AD bietet desktop Windows Store-apps, die Zugriff auf geschützte Ressourcen, Active Directory-Authentifizierung Library oder ADAL.  Zweck des ADAL im Leben ist Ihre app Zugriffstoken zu erleichtern.  Um zu veranschaulichen, wie einfach es ist, bauen hier "Verzeichnissuche" Windows Store-app, die wir:

-   Ruft zugreifen Token für das [Authentifizierungsprotokoll OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)Azure AD Graph-API aufrufen.
-   Sucht in einem Verzeichnis für Benutzer mit einem bestimmten UPN.
-   Benutzer Zeichen.

Um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Registrieren Sie Ihre Anwendung in Azure AD.
3. Installieren und Konfigurieren von ADAL.
5. ADAL von Azure AD Token zu verwenden.

Um zu beginnen, [ein Skelett Projekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) [herunterladen](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip)oder vollständiges Beispiel.  Jeder ist eine Visual Studio 2015 Lösung.  Sie benötigen auch einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Haben Sie bereits [erfahren Sie, wie man](active-directory-howto-tenant.md)Mieter.

## <a name="1-register-the-directory-searcher-application"></a>*1. Anwendung Searcher Verzeichnis registrieren*
Um Ihre app Token zu aktivieren, müssen Sie zunächst in Azure AD-Mandanten registrieren und gewährt Zugriff auf Azure AD Graph-API:

-   Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten in der Anwendung registriert.
-   Klicken Sie auf die Registerkarte **Applications** , und klicken Sie auf **Hinzufügen** in der unteren Schublade.
-   Befolgen Sie, und erstellen Sie eine neue **Systemeigene Clientanwendung**.
    -   Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.
    -   **Uri umleiten** ist eine Kombination von Schema und String, mit dem Azure AD token Antworten zurückgeben.  Geben Sie einen Platzhalter jetzt z. B. `http://DirectorySearcher`.  Wir werden diesen Wert später ersetzen.
-   Nach Abschluss Anmeldung zuweisen AAD app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten so kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Auch der Registerkarte **Konfigurieren** , suchen Sie den Abschnitt "Berechtigungen to Other Applications".  Fügen Sie für die Anwendung "Azure Active Directory" die Berechtigung **das Verzeichnis der angemeldeten Benutzer** **Delegiert**Berechtigungen hinzu  Dadurch kann die Anwendung die Graph-API für Benutzer Abfragen.

## <a name="2-install--configure-adal"></a>*2. installieren und Konfigurieren von ADAL*
Jetzt haben Sie eine Anwendung in Azure AD können Sie ADAL installieren und identitätsbezogene Code schreiben.  Damit ADAL mit Azure kommunizieren müssen Sie einige Informationen über Ihre app-Registrierung.
-   Zunächst ADAL mit der Paket-Manager-Konsole DirectorySearcher-Projekt.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Öffnen Sie im Projekt DirectorySearcher `MainPage.xaml.cs`.  Ersetzen Sie die Werte in den `Config Values` Region Werten in Azure-Portal eingegeben.  Code verweist diese Werte, wenn ADAL verwendet.
    -   Die `tenant` ist die Domäne der Azure AD-Mandanten, z.B. contoso.onmicrosoft.com
    -   Die `clientId` ClientId der Anwendung aus dem Portal kopiert wird.
-   Nun müssen den Rückruf Uri für Windows Store-Apps zu ermitteln.  Legen Sie einen Haltepunkt in dieser Zeile in der `MainPage` Methode:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Erstellen der Lösung sicherstellen, dass alle Paketverweise wiederhergestellt werden.  Wenn Pakete fehlen, Öffnen von Nuget Paket-Manager und Wiederherstellen der Pakete.
- Führen Sie die Anwendung, und kopieren Sie neben den Wert der `redirectUri` Wenn der Haltepunkt erreicht wird.  Es sieht etwas

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Ersetzen Sie auf die Registerkarte **Konfigurieren** der Anwendung in Azure-Verwaltungsportal den Wert des **RedirectUri** mit diesem Wert.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL Token AAD zu verwenden*
Das Grundprinzip hinter ADAL ist, wenn Ihre Anwendung ein Zugriffstoken benötigt, es einfach ruft `authContext.AcquireToken(…)`, und ADAL übernimmt den Rest.  

-   Der erste Schritt ist Ihre Anwendung initialisiert `AuthenticationContext` -ADAL der Klasse.  Dies ist, wo Sie ADAL Kommunikation mit Azure AD und Teilen wie cache-Token muss Koordinaten.

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- Suchen Sie nun die `Search(...)` -Methode aufgerufen wird, wenn der Benutzer die Schaltfläche "Suchen" in der app-Benutzeroberfläche klickt.  Diese Methode stellt eine GET-Anforderung Azure AD Graph API Abfrage für Benutzer, dessen UPN mit dem Suchbegriff beginnt.  Aber Abfragen Graph-API müssen Sie ein Zugriffstoken in der `Authorization` Header der Anforderung - Dadurch kommt ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Wenn Ihre app einen Token anfordert, durch Aufrufen von `AcquireTokenAsync(...)`, ADAL versucht, ein Token zurück, ohne den Benutzer Anmeldeinformationen.  ADAL fest, dass der Benutzer muss sich anmelden, ein Token abzurufen, wird ein Anmeldedialogfeld angezeigt, die Anmeldeinformationen des Benutzers erfassen und ein Token nach erfolgreicher Authentifizierung.  Wenn ADAL keinen Token aus irgendeinem Grund ist die `AuthenticationResult` Status werden Fehler.

- Nun mit dem gerade von Ihnen erworbenen Zugriffstoken.  Auch die `Search(...)` -Methode das Token auf die Graph-API Abrufen der Authorization-Header angefügt:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- Sie können auch die `AuthenticationResult` Objekt, das Informationen über den Benutzer in Ihrer Anwendung, wie die Benutzer-Id:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- ADAL können Sie schließlich den Benutzer aus der Anwendung.  Klickt der Benutzer auf die Schaltfläche "Abmelden", möchten wir sicherstellen, dass den nächsten Aufruf von `AcquireTokenAsync(...)` Zeichen wird in der Ansicht angezeigt.  ADAL ist dies so einfach wie das Löschen des Zwischenspeichers eines token:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Herzlichen Glückwunsch! Jetzt haben ein funktionierendes Windows Store-app, mit der Benutzer authentifiziert, sicheres Aufrufen von APIs mit OAuth 2.0, und grundlegende Informationen über den Benutzer.  Wenn nicht bereits geschehen ist jetzt die Zeit Ihren Mandanten mit Benutzern aufgefüllt.  Der DirectorySearcher-Anwendung ausführen und dieser Benutzer anmelden.  Suchen Sie nach anderen Benutzern basierend auf ihrem UPN.  Die Anwendung schließen und erneut ausführen.  Beachten Sie, wie die Sitzung intakt bleibt.  Melden Sie ab (Rechtsklick zum Anzeigen der unteren Leiste) und wieder als anderer Benutzer anmelden.

ADAL erleichtert diese gemeinsamen Identität Features in Ihre Anwendung integrieren.  Er übernimmt alle Prozesse laufen für Cache-Verwaltung, OAuth Unterstützung Anmeldung UI aktualisieren abgelaufenen Token und mehr Benutzer präsentiert.  Wirklich wissen müssen lediglich einen einzigen API-Aufruf `authContext.AcquireToken*(…)`.

Zu Referenzzwecken vollständiges Beispiel (ohne die Konfigurationswerte) bereitgestellt [hier](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Sie können zusätzliche Identität Szenarien nun.  Möglicherweise möchten versuchen:

[Sichere .NET Web API Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
