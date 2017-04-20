<properties
    pageTitle="Azure AD Windows Phone Einstieg | Microsoft Azure"
    description="Erstellen eine Windows Phone-Anwendung, die in Azure AD Anmelden integriert werden und ruft Azure AD geschützten APIs mit OAuth."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>



# <a name="integrate-azure-ad-with-a-windows-phone-app"></a>Azure AD in eine Windows Phone App integrieren

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Wenn die Entwicklung einer Windows Phone 8.1 Anwendung vereinfacht Azure AD und für Ihre Benutzer ihre Active Directory-Konten zu authentifizieren.  Außerdem können die Anwendung sicher alle Web API geschützt von Azure AD Azure-API oder die Office 365-APIs verwenden.

> [AZURE.NOTE] Dieses Codebeispiel verwendet ADAL v2. 0.  Für die neueste Technologie sollten Sie versuchen [universelle Windows-Lernprogramm ADAL v3. 0 verwenden](active-directory-devquickstarts-windowsstore.md).  Tatsächlich eine app für Windows Phone 8.1 erstellen, ist richtig.  ADAL v2. 0 wird weiterhin unterstützt, und die empfohlene Methode zum Entwickeln von apps Agianst Windows Phone 8.1 Azure AD verwendet.

Für .NET native-Clients, die Zugriff auf geschützte Ressourcen bietet Azure AD Active Directory Authentifizierungsbibliothek oder ADAL.  Zweck des ADAL im Leben ist Ihre app Zugriffstoken zu erleichtern.  Um zu veranschaulichen, wie einfach es ist, bauen hier ein "Verzeichnissuche" app für Windows Phone 8.1, die wir:

-   Ruft zugreifen Token für das [Authentifizierungsprotokoll OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)Azure AD Graph-API aufrufen.
-   Sucht in einem Verzeichnis für Benutzer mit einem bestimmten UPN.
-   Benutzer Zeichen.

Um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Registrieren Sie Ihre Anwendung in Azure AD.
3. Installieren und Konfigurieren von ADAL.
5. ADAL von Azure AD Token zu verwenden.

Um zu beginnen, [ein Skelett Projekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) [herunterladen](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip)oder vollständiges Beispiel.  Jeder ist eine Visual Studio 2013-Lösung.  Sie benötigen auch einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Haben Sie bereits [erfahren Sie, wie man](active-directory-howto-tenant.md)Mieter.

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
- Auch der Registerkarte **Konfigurieren** , suchen Sie den Abschnitt "Berechtigungen to Other Applications".  Fügen Sie für die Anwendung "Azure Active Directory" die Berechtigung **Verzeichnis Zugriff auf Ihre Organisation** **Delegiert**Berechtigungen hinzu.  Dadurch kann die Anwendung die Graph-API für Benutzer Abfragen.

## <a name="2-install--configure-adal"></a>*2. installieren und Konfigurieren von ADAL*
Jetzt haben Sie eine Anwendung in Azure AD können Sie ADAL installieren und identitätsbezogene Code schreiben.  Damit ADAL mit Azure kommunizieren müssen Sie einige Informationen über Ihre app-Registrierung.
-   Zunächst ADAL mit der Paket-Manager-Konsole DirectorySearcher-Projekt.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Öffnen Sie im Projekt DirectorySearcher `MainPage.xaml.cs`.  Ersetzen Sie die Werte in den `Config Values` Region Werten in Azure-Portal eingegeben.  Code verweist diese Werte, wenn ADAL verwendet.
    -   Die `tenant` ist die Domäne der Azure AD-Mandanten, z.B. contoso.onmicrosoft.com
    -   Die `clientId` ClientId der Anwendung aus dem Portal kopiert wird.
-   Nun müssen den Rückruf Uri für Ihre Windows Phone entdecken.  Legen Sie einen Haltepunkt in dieser Zeile in der `MainPage` Methode:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
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

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

- Suchen Sie jetzt die `Search(...)` -Methode aufgerufen wird, wenn Benutzer Cliks "Suchen" in der app-Benutzeroberfläche Schaltfläche.  Diese Methode stellt eine GET-Anforderung Azure AD Graph API Abfrage für Benutzer, dessen UPN mit dem Suchbegriff beginnt.  Aber Abfragen Graph-API müssen Sie ein Zugriffstoken in der `Authorization` Header der Anforderung - Dadurch kommt ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
- Wenn interaktive Authentifizierung erforderlich, ADAL verwendet Windows Phone Web Authentication Broker (WAB) und [Fortsetzung Modell](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) Azure AD Anmeldeseite.  Wenn sich der Benutzer anmeldet, muss Ihre Anwendung ADAL übergeben die Ergebnisse der Aktivität, die WAB.  Diese Implementierung ist die `ContinueWebAuthentication` Schnittstelle:

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

- Nun mit den `AuthenticationResult` , die ADAL an Ihre Anwendung zurückgegeben.  In der `QueryGraph(...)` -Rückruf die GET-Anforderung in der Authorization-Header Sie erworben Zugriffstoken zuordnen:

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
- Sie können auch die `AuthenticationResult` -Objekts Informationen über den Benutzer in der app. In der `QueryGraph(...)` Methode anhand des Ergebnisses der Benutzer-Id auf der Seite angezeigt:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- ADAL können Sie schließlich den Benutzer aus der Anwendung.  Klickt der Benutzer auf die Schaltfläche "Abmelden", möchten wir sicherstellen, dass den nächsten Aufruf von `AcquireTokenSilentAsync(...)` fehl.  ADAL ist dies so einfach wie das Löschen des Zwischenspeichers eines token:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Herzlichen Glückwunsch! Jetzt haben ein funktionierendes Windows Phone-app, mit der Benutzer authentifiziert, sicheres Aufrufen von APIs mit OAuth 2.0, und grundlegende Informationen über den Benutzer.  Wenn nicht bereits geschehen ist jetzt die Zeit Ihren Mandanten mit Benutzern aufgefüllt.  Der DirectorySearcher-Anwendung ausführen und dieser Benutzer anmelden.  Suchen Sie nach anderen Benutzern basierend auf ihrem UPN.  Die Anwendung schließen und erneut ausführen.  Beachten Sie, wie die Sitzung intakt bleibt.  Abmelden und als anderer Benutzer wieder anmelden.

ADAL erleichtert diese gemeinsamen Identität Features in Ihre Anwendung integrieren.  Er übernimmt alle Prozesse laufen für Cache-Verwaltung, OAuth Unterstützung Anmeldung UI aktualisieren abgelaufenen Token und mehr Benutzer präsentiert.  Wirklich wissen müssen lediglich einen einzigen API-Aufruf `authContext.AcquireToken*(…)`.

Zu Referenzzwecken vollständiges Beispiel (ohne die Konfigurationswerte) bereitgestellt [hier](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Sie können zusätzliche Identität Szenarien nun.  Möglicherweise möchten versuchen:

[Sichere .NET Web API Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]