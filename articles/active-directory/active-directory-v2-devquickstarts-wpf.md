<properties
pageTitle="Azure Active Directory v2. 0 .NET systemeigene Anwendung | Microsoft Azure"
description="Wie eine systemeigene Anwendung .NET erstellen, die Benutzer mit beiden persönlichen Microsoft Account und Geschäfts-oder Zeichen."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Anmelden an Windows Desktop-Anwendung hinzufügen

Mit dem Endpunkt v2. 0 können Sie desktop-apps mit Unterstützung für beide Konten für persönlichen Microsoft-Authentifizierung und Geschäfts-oder schnell hinzufügen.  Außerdem kann Ihre Anwendung mit einem Back-End-Web-api sowie [Microsoft Graph](https://graph.microsoft.io) erh und [Office 365 Unified APIs](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [AZURE.NOTE] Nicht alle Szenarien Azure Active Directory (AD) und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

Azure AD bietet [.NET systemeigenen apps, die auf einem Gerät ausgeführt](active-directory-v2-flows.md#mobile-and-native-apps)Microsoft Identity Authentifizierungsbibliothek oder MSAL.  Zweck des MSAL im Leben ist Ihre app zu Token-Webdienste aufrufen zu erleichtern.  Um zu veranschaulichen, wie einfach es ist, bauen hier einer Anwendung .NET WPF-Aufgabenliste, die wir:

- Meldet sich des Benutzers bei & ruft Token mit dem [Authentifizierungsprotokoll OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow)zugreifen.
- Sichere Ruft die Vorgangsliste WebService auch OAuth 2.0 gesichert.
- Den Benutzer abmeldet.

## <a name="download-sample-code"></a>Beispielcode herunterladen

Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet)verwaltet.  Um folgen, können Sie [die Anwendung Skelett als .zip downloaden](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) oder das Skelett Klonen:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

Die abgeschlossene Anwendung wird am Ende dieses Lernprogramms bereitgestellt.

## <a name="register-an-app"></a>Registrieren einer Anwendung
Erstellen Sie eine neue Anwendung auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)und befolgen Sie diese [Anleitung](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

- Kopieren Sie die **Id der Anwendung** für Ihre Anwendung zugewiesen, Sie benötigen rasch.
- **Mobile** Plattform für Ihre Anwendung hinzufügen.

## <a name="install--configure-msal"></a>Installieren und Konfigurieren von MSAL
Jetzt haben Sie eine Anwendung mit Microsoft registriert, können Sie MSAL installieren und identitätsbezogene Code schreiben.  Damit MSAL v2. 0-Endpunkt zu kommunizieren, müssen Sie einige Informationen über Ihre app-Registrierung.

-   Zunächst MSAL TodoListClient Projekt mithilfe der Paket-Manager-Konsole.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   Öffnen Sie im Projekt TodoListClient `app.config`.  Ersetzen Sie die Werte der Elemente in der `<appSettings>` zu den Werten in einer app registrierungsportal eingeben.  Code verweist diese Werte, wenn MSAL verwendet.
    -   Die `ida:ClientId` ist die **Id der Anwendung** der app aus dem Portal kopiert.

- Öffnen Sie im Projekt Aufgabenliste Service `web.config` im Stammverzeichnis des Projekts.  
    - Ersetzen Sie die `ida:Audience` Wert mit derselben **Id der Anwendung** vom Portal aus.

## <a name="use-msal-to-get-tokens"></a>MSAL Token zu verwenden
Das Grundprinzip hinter MSAL ist, wenn Ihre Anwendung ein Zugriffstoken benötigt, Sie einfach rufen `app.AcquireToken(...)`, und MSAL erledigt den Rest.  

-   In der `TodoListClient` Projekt, `MainWindow.xaml.cs` und die `OnInitialized(...)` Methode.  Der erste Schritt ist Ihre Anwendung initialisiert `PublicClientApplication` -MSALs primären Klasse, die systemeigene Anwendung darstellt.  Dies ist, wo Sie MSAL die Koordinaten übergeben, um Kommunikation mit Azure AD und das Token Zwischenspeichern von Teilen.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Wenn die Anwendung startet, wollen wir überprüfen, ob der Benutzer in der app angemeldet ist.  Allerdings wollen wir eine Benutzeroberfläche noch aufrufen - wir machen "Anmelden" dazu klicken.  Auch die `OnInitialized(...)` Methode:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Wenn der Benutzer nicht angemeldet ist, und sie klicken Sie auf "Anmelden", wollen wir eine Anmeldung Benutzeroberfläche und die Benutzer ihre Anmeldeinformationen eingeben.  Implementieren Sie Handler Schaltfläche anmelden:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

- Wenn Benutzer erfolgreich Zeichen-in MSAL erhalten und cache einen Token und Sie fortfahren können, Aufrufen der `GetTodoList()` mit vertrauen.  Links zu Aufgaben des Benutzers lediglich zum Implementieren der `GetTodoList()` Methode.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Ausführen

Herzlichen Glückwunsch! Sie haben nun eine funktionierende .NET WPF-Anwendung, mit der Benutzer authentifiziert und sichere Web-APIs mit OAuth 2.0.  Die beiden Projekte und persönlichen Microsoft-Konto oder ein Arbeits- oder Schulcomputer Konto anmelden.  Diese Benutzer-Aufgabenliste Aufgaben hinzufügen.  Melden Sie ab und wieder melden Sie als anderer Benutzer auf ihre Aufgabenliste anzuzeigen an.  Die Anwendung schließen und erneut ausführen.  Beachten Sie, wie die Sitzung des Benutzers bleibt – denn die app Token in einer lokalen Datei speichert.

MSAL erleichtert Identity-Features in Ihre app integrieren sowohl persönliche und Konten.  Er übernimmt alle Prozesse laufen für Cache-Verwaltung, OAuth Unterstützung Anmeldung UI aktualisieren abgelaufenen Token und mehr Benutzer präsentiert.  Wirklich wissen müssen lediglich einen einzigen API-Aufruf `app.AcquireTokenAsync(...)`.

Zu Referenzzwecken können vollständiges Beispiel (ohne Ihre Werte) [als eine .zip bereitgestellt wird](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip)oder es von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt auf Themen verschieben.  Möglicherweise möchten versuchen:

- [Sichern der TodoListService Web API mit der v2. 0](active-directory-v2-devquickstarts-dotnet-api.md)

Weitere Ressourcen zum Auschecken:  

- [V2. 0-Entwicklerhandbuch >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Msal" Tag >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen Benachrichtigungen Sicherheitsvorfälle auftreten [dieser Seite](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.
