<properties
    pageTitle="Azure AD v2. 0 .NET WebApp | Microsoft Azure"
    description="Wie .NET MVC Web App, ruft Web services mit persönlichen Microsoft-Konten Arbeit oder schulkonten anmelden."
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
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Aufrufen von Web-API von .NET Web app

Mit dem Endpunkt v2. 0 können schnell Ihre Web-apps Authentifizierung hinzufügen und web-APIs mit Unterstützung für beide persönlichen Microsoft-Konten und Geschäfts-oder.  Hier erstellen wir eine MVC Web app, die Benutzer mit OpenID Verbinden mit Hilfe von Microsoft owin-Middleware signiert.  Web-app abrufen OAuth 2.0 Zugriffstoken für eine Web-api OAuth 2.0, das ermöglicht gesicherte erstellen, lesen und auf einen bestimmten Benutzers "Aufgabenliste" löschen.

In diesem Lernprogramm wird Schwerpunkt mit MSAL und Zugangs-Token in einer vollständigen [hier](active-directory-v2-flows.md#web-apps)beschriebenen Web app verwenden.  Als erforderliche Komponenten sollten lernen [grundlegende anmelden Web app](active-directory-v2-devquickstarts-dotnet-web.md) hinzufügen oder Sicherung [eine Web-API](active-directory-v2-devquickstarts-dotnet-api.md).

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Beispielcode herunterladen

Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet)verwaltet.  Um folgen, können Sie [die Anwendung Skelett als .zip downloaden](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) oder das Skelett Klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Alternativ können Sie [die abgeschlossene App als .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) oder Klonen Sie die abgeschlossene Anwendung:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Registrieren einer Anwendung
Erstellen Sie eine neue Anwendung auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)und befolgen Sie diese [Anleitung](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

- Kopieren Sie die **Id der Anwendung** für Ihre Anwendung zugewiesen, Sie benötigen rasch.
- Ein **App-Schlüssel** des Typs **Kennwort** erstellen und dessen Wert später kopieren
- **Web** Plattform für Ihre Anwendung hinzufügen.
- Geben Sie den entsprechenden **URI umleiten**. Der Umleitung Uri gibt Azure AD, Authentifizierungsantworten richten - die Standardeinstellung für dieses Lernprogramm ist `https://localhost:44326/`.


## <a name="install-owin"></a>OWIN installieren
Hinzufügen der owin-Middleware NuGet-Pakete in der `TodoList-WebApp` Projekt mit der Paket-Manager-Konsole.  Owin-Middleware wird anmelden und Abmelden Anforderungen, die Sitzung des Benutzers verwalten und Informationen über den Benutzer unter anderem verwendet.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Benutzer anmelden
Konfigurieren Sie jetzt owin-Middleware das [Authentifizierungsprotokoll OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  

-   Öffnen der `web.config` Datei im Stammverzeichnis der `TodoList-WebApp` Projekt, und geben Sie Werte in Ihre Anwendung die `<appSettings>` Abschnitt.
    -   Die `ida:ClientId` ist Ihre app im registrierungsportal zugewiesene **Id der Anwendung** .
    - Die `ida:ClientSecret` der **App-Schlüssel** in der registrierungsportal erstellt.
    -   Die `ida:RedirectUri` ist im Portal eingegebene **Uri umleiten** .
- Öffnen der `web.config` Datei im Stammverzeichnis der `TodoList-Service` -Projekt, und Ersetzen Sie die `ida:Audience` mit derselben **Id Anwendung** wie oben.


- Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und `using` Anweisungen für die Bibliotheken von oben.
- Implementieren Sie in derselben Datei der `ConfigureAuth(...)` Methode.  Die Parameter in bieten `OpenIDConnectAuthenticationOptions` dienen als Koordinaten für Ihre Kommunikation mit Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>MSAL Zugriffstoken zu verwenden
In der `AuthorizationCodeReceived` Benachrichtigung, wir möchten [OAuth 2.0 zusammen mit OpenID Connect](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) Authorization_code für ein Zugriffstoken Aufgabe Liste Dienst einlösen.  MSAL kann dabei erleichtern:

- Zuerst installieren Sie die Vorabversion von MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- Und anderen `using` Anweisung die `App_Start\Startup.Auth.cs` Datei MSAL.
- Nun fügen Sie eine neue Methode der `OnAuthorizationCodeReceived` -Ereignishandler.  Dieser Handler verwenden MSAL Zugriffstoken API Liste Aufgabe zu und speichert das Token im MSALs token Cache für später:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- In Web-apps hat MSAL einen erweiterbaren token Cache, mit dem Token gespeichert werden kann.  In diesem Beispiel implementiert die `NaiveSessionCache` die HTTP-Sitzungsspeicher Cache Token verwendet.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Rufen Sie die Web-API
Jetzt ist es Zeit, das Zugriffstoken verwenden, die, das Sie in Schritt 3 erworben.  Öffnen Sie der Webanwendung `Controllers\TodoListController.cs` Datei, macht die CRUD-Anfragen an die Aufgabe Liste API.

- MSAL können wieder hier Access_tokens MSAL Cache abgerufen.  Fügen Sie zunächst ein `using` -Anweisung für MSAL auf diese Datei.

    `using Microsoft.Identity.Client;`

- In der `Index` Aktion, MSAL mit `AcquireTokenSilentAsync` Methode, um ein Zugriffstoken, das zum Lesen von Daten aus der Aufgabenliste Dienst verwendet werden kann:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Dann hinzugefügt das resultierende Token HTTP GET-Anforderung als die `Authorization` Kopf der Liste Dienst zum Authentifizieren der Anforderung verwendet.
- Kehrt die Vorgangsliste Service eine `401 Unauthorized` Antwort Access_tokens in MSAL ungültig wurden aus irgendeinem Grund.  In diesem Fall sollten Sie alle Access_tokens aus dem MSAL Cache ablegen und anzeigen eine Nachricht, die sie anmelden wieder die token Übernahme Datenfluss starten.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Wenn MSAL kein Zugriffstoken aus irgendeinem Grund ist, sollten Sie den Benutzer erneut anweisen.  Dies ist so einfach wie das Abfangen einer `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- Die genaue dieselbe `AcquireTokenSilentAsync` ist Implementd in der `Create` und `Delete` Aktionen.  Web Apps können Sie diese Methode MSAL Access_tokens zu, wenn Sie in Ihrer Anwendung benötigen.  MSAL kümmern abrufen und Zwischenspeichern Token für Sie aktualisieren.

Schließlich erstellen und Ausführen der app.  Microsoft Account oder Azure Konto melden Sie an und feststellen Sie, wie die Identität des Benutzers in der oberen Navigationsleiste wiederspiegelt.  Fügen Sie hinzu und löschen Sie einige Elemente aus der Aufgabenliste des Benutzers zu prüfen, ob das OAuth 2.0 API-Aufrufe in Aktion gesichert.  Sie haben nun ein WebApp & Web API sowohl gesicherte Standardprotokolle, die Benutzer mit ihren persönlichen und Arbeit oder Schule Konten authentifizieren können.

Zu Referenzzwecken [hier](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip)vollständiges Beispiel (ohne Ihre Werte).  

## <a name="next-steps"></a>Nächste Schritte

Weitere Ressourcen zum Auschecken:
- [V2. 0-Entwicklerhandbuch >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Azure Active Directory" Tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen Benachrichtigungen Sicherheitsvorfälle auftreten [dieser Seite](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.
