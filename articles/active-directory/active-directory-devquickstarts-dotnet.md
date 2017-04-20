<properties
    pageTitle="Azure AD .NET Einstieg | Microsoft Azure"
    description="Wie .NET Windows Desktop-Anwendung, die in Azure AD Anmelden integriert werden und ruft Azure AD geschützten APIs mit OAuth."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Integrieren Sie Azure AD in Windows Desktop WPF App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Wenn Sie eine desktop-Anwendung entwickeln, vereinfacht Azure AD und für Ihre Benutzer ihre Active Directory-Konten zu authentifizieren.  Außerdem können die Anwendung sicher alle Web API geschützt von Azure AD Azure-API oder die Office 365-APIs verwenden.

Für .NET native-Clients, die Zugriff auf geschützte Ressourcen bietet Azure AD Active Directory Authentifizierungsbibliothek oder ADAL.  Zweck des ADAL im Leben ist Ihre app Zugriffstoken zu erleichtern.  Um zu veranschaulichen, wie einfach es ist, bauen hier einer Aufgabenliste für .NET WPF-Anwendung, die wir:

-   Ruft zugreifen Token für das [Authentifizierungsprotokoll OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)Azure AD Graph-API aufrufen.
-   Sucht in einem Verzeichnis für Benutzer mit bestimmten Aliasnamen.
-   Benutzer Zeichen.

Um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Registrieren Sie Ihre Anwendung in Azure AD.
3. Installieren und Konfigurieren von ADAL.
5. ADAL von Azure AD Token zu verwenden.

Um zu beginnen, [das Skelett app](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) [herunterladen](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)oder vollständiges Beispiel.  Sie benötigen auch einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Haben Sie bereits [erfahren Sie, wie man](active-directory-howto-tenant.md)Mieter.

## <a name="1-register-the-directorysearcher-application"></a>*1. registrieren Sie die DirectorySearcher-Anwendung*
Um Ihre app Token zu aktivieren, müssen Sie zunächst in Azure AD-Mandanten registrieren und gewährt Zugriff auf Azure AD Graph-API:

-   Melden Sie sich bei Azure-Verwaltungsportal
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten in der Anwendung registriert.
-   Klicken Sie auf die Registerkarte **Applications** , und klicken Sie auf **Hinzufügen** in der unteren Schublade.
-   Befolgen Sie, und erstellen Sie eine neue **Systemeigene Clientanwendung**.
    -   Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.
    -   **Uri umleiten** ist eine Kombination von Schema und String, mit dem Azure AD token Antworten zurückgeben.  Geben Sie einen bestimmten Wert Ihrer Anwendung z. B. `http://DirectorySearcher`.
-   Nach Abschluss Anmeldung zuweisen AAD app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten so kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Auch der Registerkarte **Konfigurieren** , suchen Sie den Abschnitt "Berechtigungen to Other Applications".  Fügen Sie für die Anwendung "Azure Active Directory" die Berechtigung **Verzeichnis Zugriff auf Ihre Organisation** **Delegiert**Berechtigungen hinzu.  Dadurch kann die Anwendung die Graph-API für Benutzer Abfragen.

## <a name="2-install--configure-adal"></a>*2. installieren und Konfigurieren von ADAL*
Jetzt haben Sie eine Anwendung in Azure AD können Sie ADAL installieren und identitätsbezogene Code schreiben.  Damit ADAL mit Azure kommunizieren müssen Sie einige Informationen über Ihre app-Registrierung.
-   Zunächst ADAL mit der Paket-Manager-Konsole DirectorySearcher-Projekt.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Öffnen Sie im Projekt DirectorySearcher `app.config`.  Ersetzen Sie die Werte der Elemente in der `<appSettings>` zu den Werten in der Azure-Portal eingegeben.  Code verweist diese Werte, wenn ADAL verwendet.
    -   Die `ida:Tenant` ist die Domäne der Azure AD-Mandanten, z.B. contoso.onmicrosoft.com
    -   Die `ida:ClientId` ClientId der Anwendung aus dem Portal kopiert wird.
    -   Die `ida:RedirectUri` ist die Umleitung Url im Portal registriert.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL Token AAD zu verwenden*
Das Grundprinzip hinter ADAL ist, wenn Ihre Anwendung ein Zugriffstoken benötigt, es einfach ruft `authContext.AcquireTokenAsync(...)`, und ADAL übernimmt den Rest.  

-   In der `DirectorySearcher` Projekt, `MainWindow.xaml.cs` und die `MainWindow()` Methode.  Der erste Schritt ist Ihre Anwendung initialisiert `AuthenticationContext` -ADAL der Klasse.  Dies ist, wo Sie ADAL Kommunikation mit Azure AD und Teilen wie cache-Token muss Koordinaten.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Suchen Sie jetzt die `Search(...)` -Methode aufgerufen wird, wenn Benutzer Cliks "Suchen" in der app-Benutzeroberfläche Schaltfläche.  Diese Methode stellt eine GET-Anforderung Azure AD Graph API Abfrage für Benutzer, dessen UPN mit dem Suchbegriff beginnt.  Aber Abfragen Graph-API müssen Sie ein Zugriffstoken in der `Authorization` Header der Anforderung - Dadurch kommt ADAL.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Wenn Ihre app einen Token anfordert, durch Aufrufen von `AcquireTokenAsync(...)`, ADAL versucht, ein Token zurück, ohne den Benutzer Anmeldeinformationen.  ADAL fest, dass der Benutzer muss sich anmelden, ein Token abzurufen, wird ein Anmeldedialogfeld angezeigt, die Anmeldeinformationen des Benutzers erfassen und ein Token nach erfolgreicher Authentifizierung.  Wenn ADAL einen Token aus irgendeinem Grund zurückgeben kann, löst eine `AdalException`.
- Beachten Sie, dass die `AuthenticationResult` -Objekt enthält eine `UserInfo` Objekt, mit Informationen Ihrer app müssen.  In der DirectorySearcher `UserInfo` zum Anpassen der app-Benutzeroberfläche mit der Benutzer-Id verwendet.

- Klickt der Benutzer auf die Schaltfläche "Abmelden", möchten wir sicherstellen, dass den nächsten Aufruf von `AcquireTokenAsync(...)` fordert den Benutzer anmelden.  ADAL ist dies so einfach wie das Löschen des Zwischenspeichers eines token:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Wenn der Benutzer nicht die Schaltfläche "Abmelden" klicken, sollten Sie die Sitzung des Benutzers für die nächste auszuführende DirectorySearcher verwalten.  Beim Start der Anwendung können Sie überprüfen ADAL token Cache für einen vorhandenen Token und die Benutzeroberfläche entsprechend aktualisiert.  In der `CheckForCachedToken()` Methode rufen Sie `AcquireTokenAsync(...)`, diese Zeit der `PromptBehavior.Never` Parameter.  `PromptBehavior.Never`teilt ADAL ADAL sollte stattdessen eine Ausnahme auslösen, wenn einen Token zurückgeben kann, dass der Benutzer sollten nicht zur Anmeldung aufgefordert.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Herzlichen Glückwunsch! Jetzt haben eine funktionierende .NET WPF-Anwendung, mit der Benutzer authentifiziert, sicheres Aufrufen von APIs mit OAuth 2.0, und grundlegende Informationen über den Benutzer.  Wenn nicht bereits geschehen ist jetzt die Zeit Ihren Mandanten mit Benutzern aufgefüllt.  Der DirectorySearcher-Anwendung ausführen und dieser Benutzer anmelden.  Suchen Sie nach anderen Benutzern basierend auf ihrem UPN.  Die Anwendung schließen und erneut ausführen.  Beachten Sie, wie die Sitzung intakt bleibt.  Abmelden und als anderer Benutzer wieder anmelden.

ADAL erleichtert diese gemeinsamen Identität Features in Ihre Anwendung integrieren.  Er übernimmt alle Prozesse laufen für Cache-Verwaltung, OAuth Unterstützung Anmeldung UI aktualisieren abgelaufenen Token und mehr Benutzer präsentiert.  Wirklich wissen müssen lediglich einen einzigen API-Aufruf `authContext.AcquireTokenAsync(...)`.

Zu Referenzzwecken vollständiges Beispiel (ohne die Konfigurationswerte) bereitgestellt [hier](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Sie können zusätzliche Szenarien nun.  Möglicherweise möchten versuchen:

[Sichere .NET Web API Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
