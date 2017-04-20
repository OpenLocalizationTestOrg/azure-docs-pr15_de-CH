<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Wie Windows desktop-Anwendung erstellt wird, die anmelden, Anmeldung, enthält, und Verwaltung von Profilen mithilfe von Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Erstellen Sie Windows Desktop-app

Mithilfe von Azure Active Directory (Azure AD) B2C können Sie leistungsstarke Self-service-Management bietet Ihr desktop App in wenigen Schritten hinzufügen. Dieser Artikel zeigt eine .NET Windows Presentation Foundation (WPF) "Aufgabenliste" Anwendung erstellen, die Benutzer Anmeldung, Anmeldung und Verwaltung von Profilen. Die app enthalten Unterstützung für Anmeldung und melden Sie sich mit einem Benutzernamen oder e-Mail. Auch enthält Unterstützung für Anmeldung und Anmeldung mit sozialen Konten wie Facebook und Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Ein Azure AD B2C-Verzeichnis abrufen

Vor der Verwendung Azure AD B2C müssen Sie ein Verzeichnis oder Mieter.  Ein Verzeichnis ist ein Container für alle Benutzer, apps und Gruppen. Haben Sie eine bereits, [B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie fortfahren, in diesem Handbuch.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine Anwendung in Ihrem B2C-Verzeichnis erstellen. Dies informiert Azure AD, um die sichere Kommunikation mit Ihrer Anwendung. [Gehen folgendermaßen Sie vor, um eine Anwendung zu erstellen.](active-directory-b2c-app-registration.md)  Achten Sie darauf,:

- **Native Client** in der Anwendung enthalten.
- Kopieren Sie den **URI umleiten** `urn:ietf:wg:oauth:2.0:oob`. Es ist die Standard-URL für dieses Codebeispiel.
- Kopieren Sie die **ID der Anwendung** , die Ihre Anwendung zugewiesen ist. Sie werden ihn später benötigen.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen von Richtlinien

In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Dieses Codebeispiel enthält drei Identität Erfahrungen: anmelden, anmelden und Profil bearbeiten. Sie müssen zum Erstellen einer Richtlinie für jeden Typ gemäß [Richtlinie Referenzartikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wenn Sie die drei Richtlinien erstellen, müssen Sie:

- **Benutzer-ID anmelden** oder **Anmeldung E-Mail** Identität Anbieter Blatt auswählen
- Wählen Sie im die Registrierungsrichtlinie **Anzeigename** und andere Anmeldung Attribute.
- Wählen Sie **Anzeigename** und **Objekt-ID** Ansprüche Anwendung Ansprüche für jede Richtlinie. Sie können auch andere Ansprüche.
- Kopieren Sie den **Namen** jeder Richtlinie nach ihrer Erstellung. Das Präfix muss `b2c_1_`.  Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erstellt haben, können Sie Ihre Anwendung erstellen.

## <a name="download-the-code"></a>Den Code herunterladen

Der Code für dieses Lernprogramm [auf GitHub verwaltet](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Zum Beispiel wie Sie, können Sie [ein Skelett Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Sie können auch das Skelett Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Die abgeschlossene Anwendung wird auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) oder die `complete` dasselbe Repository-Zweig.

Nach dem Herunterladen des Codes öffnen Sie Visual Studio sln-Datei für den Einstieg. Die `TaskClient` Projekt ist die WPF-desktop-Anwendung, mit dem der Benutzer interagiert. Für die Zwecke dieses Lernprogramms wird eine Back-End-Aufgabe Web API gehostet in Azure, in der Aufgabenliste des Benutzers gespeichert.  Sie müssen nicht die Web-API erstellen, haben bereits für Sie ausgeführt.

Um zu erfahren, wie eine Web API sicher Anfragen authentifiziert mit Azure AD B2C, checken Sie [Web API Einstieg Artikel](active-directory-b2c-devquickstarts-api-dotnet.md)aus.

## <a name="execute-policies"></a>Ausführung von policies
Die Anwendung kommuniziert mit Azure AD B2C Authentifizierung Nachrichten, die die Richtlinie angeben, die sie als Teil der HTTP-Anforderung ausführen möchten. Platzsparend .NET können mithilfe der Vorschau Microsoft Authentifizierung Library (MSAL) Nachrichten OAuth 2.0 Authentifizierung, Ausführen von Richtlinien und Token, die Web-APIs aufrufen.

### <a name="install-msal"></a>MSAL installieren
Hinzufügen MSAL zu den `TaskClient` Projekt mit Visual Studio Paket-Manager-Konsole.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Die B2C-Details eingeben
Öffnen Sie die Datei `Globals.cs` und jeweils die Werte durch Ihren eigenen ersetzen. Diese Klasse wird verwendet, während `TaskClient` auf häufig verwendete Werte.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>Erstellen der PublicClientApplication
Die primäre MSAL ist `PublicClientApplication`. Diese Klasse stellt die Anwendung in Azure AD B2C-System. Beim Erstellen der Anwendung initialisiert einer Instanz von `PublicClientApplication` in `MainWindow.xaml.cs`. Dies kann in dem Fenster verwendet werden.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Initiieren einer Registrierungsprozesses
Entscheidet sich ein Benutzer Zeichen, wenn Sie eine Registrierungsprozesses initiieren, die die Registrierungsrichtlinie erstellte verwendet. Mithilfe von MSAL Sie rufen `pca.AcquireTokenAsync(...)`. Sie übergeben Parameter `AcquireTokenAsync(...)` bestimmen, welche Token Erhalt die Richtlinie die Authentifizierung wird immer verwendet.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a>Initiieren eines Flusses anmelden
Initiieren einer Registrierungsprozesses können Sie auf die gleiche Weise einen Fluss anmelden initiieren. Wenn sich ein Benutzer anmeldet, stellen Sie demselben Aufruf MSAL, diesmal mit Ihrer Richtlinien:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Initiieren einer fortlaufenden Profil bearbeiten
Auch können Sie eine Richtlinie Profil bearbeiten auf dieselbe Weise ausführen:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

In all diesen Fällen MSAL gibt entweder ein Token im `AuthenticationResult` oder eine Ausnahme auslöst. Bei jedem ein Tokens aus MSAL abrufen, können Sie die `AuthenticationResult.User` Objekt die Benutzerdaten in der Anwendung, wie die Benutzeroberfläche aktualisiert. ADAL speichert außerdem das Token für die Verwendung in anderen Teilen der Anwendung.


### <a name="check-for-tokens-on-app-start"></a>Überprüfen Sie auf Anwendung starten
MSAL können Sie den Status des Benutzers anmelden an.  Bei dieser app möchten wir den Benutzer angemeldet zu bleiben, auch wenn sie die Anwendung schließen und erneut öffnen.  In der `OnInitialized` überschreiben, verwenden Sie die MSAL `AcquireTokenSilent` -Methode prüfen zwischengespeicherten Tokens:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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
```

## <a name="call-the-task-api"></a>Task-API-aufrufen
Sie haben MSAL jetzt Richtlinien und Token verwendet.  Wenn Sie eine Aufgabe API Aufrufen diese Token verwenden möchten, wieder können Sie die MSAL `AcquireTokenSilent` -Methode prüfen zwischengespeicherten Tokens:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

Beim Aufruf von `AcquireTokenSilentAsync(...)` erfolgreich ist und ein Token im Cache gefunden wird, können Sie das Token der `Authorization` -Header der HTTP-Anforderung. Aufgabe Web API verwendet diesen Header zum Authentifizieren der Anforderung Aufgabenliste des Benutzers lesen:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Benutzer abmelden
Schließlich können Sie MSAL zum Beenden der Sitzung eines Benutzers mit der Anwendung beim **Abmelden**der Benutzer auswählt.  Bei MSAL muss alle Token aus dem token Cache löschen:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Führen Sie die Beispiel-app

Schließlich erstellen Sie und führen Sie des Beispiels aus.  Melden Sie sich für die Anwendung ein e-Mail-Adresse oder Benutzername verwendet. Melden Sie ab und wieder melden Sie an, als derselbe Benutzer. Bearbeiten Sie dieses Benutzerprofil Melden Sie ab, und melden Sie sich mit einem anderen Benutzer.

## <a name="add-social-idps"></a>Soziale vertriebenen hinzufügen

Die Anwendung unterstützt derzeit nur Benutzer anmelden und anmelden, die **lokale Konten**verwenden. Diese sind Konten in Ihrem B2C-Verzeichnis gespeichert, die einen Benutzernamen ein Kennwort und. Mithilfe von Azure AD B2C können Sie Unterstützung für andere Identitätsprovider (IDP) ohne Code hinzufügen.

Beginnen Sie Ihre app sozialen vertriebenen hinzu, anhand der Informationen in diesem Artikel. Für jeden IDP unterstützen möchten, müssen Sie eine Anwendung in diesem System registrieren und erhalten eine Client-ID

- [Facebook als IDP einrichten](active-directory-b2c-setup-fb-app.md)
- [Google als IDP einrichten](active-directory-b2c-setup-goog-app.md)
- [Einrichten von Amazon als IDP](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn als IDP einrichten](active-directory-b2c-setup-li-app.md)

Nach dem Hinzufügen der Identitätsanbieter zu Ihrem B2C-Verzeichnis müssen Sie jede der drei Richtlinien sollen neuen vertriebenen gemäß [Richtlinie Referenzartikel](active-directory-b2c-reference-policies.md)bearbeiten. Nach dem Speichern der Richtlinien führen Sie die Anwendung erneut. Sie sollten neuen vertriebenen als anmelden und Anmeldeoptionen aller Ihrer Identität auftritt.

Ihre Richtlinien experimentieren und beobachten Sie die Effekte für die Beispiel-app. Hinzufügen oder Entfernen von Vertriebenen, Anwendungsansprüchen bearbeiten oder Anmeldung Attribute ändern Experimentieren Sie sehen, wie Richtlinien, Authentifizierung und MSAL zusammenzufassen.

Zu Referenzzwecken vollständiges Beispiel [als ZIP-Datei bereitgestellt wird](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Sie können auch von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
