<properties
    pageTitle="Azure AD v2. 0 .NET WebApp | Microsoft Azure"
    description="Wie .NET MVC Web App erstellen, die Benutzer mit beiden persönlichen Microsoft Account und Geschäfts-oder Zeichen."
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

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Hinzufügen einer .NET MVC Web app anmelden

Mit dem Endpunkt v2. 0 können Sie schnell Ihre Web Apps mit Unterstützung für beide Konten für persönlichen Microsoft-Authentifizierung und Geschäfts-oder hinzufügen.  In ASP.NET webapps erreichen Sie dies mit Microsoft owin-Middleware in.NET Framework 4.5 aufgenommen.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

 Hier erstellen wir eine Webanwendung, die OWIN den Benutzer anmelden und einige Informationen über den Benutzer abmelden des Benutzers von der Anwendung verwendet.
 
 ## <a name="download"></a>Herunterladen
Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet)verwaltet.  Um folgen, können Sie [die Anwendung Skelett als .zip downloaden](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) oder das Skelett Klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Die abgeschlossene Anwendung wird am Ende dieses Lernprogramms bereitgestellt.

## <a name="register-an-app"></a>Registrieren einer Anwendung
Erstellen Sie eine neue Anwendung auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)und befolgen Sie diese [Anleitung](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

- Kopieren Sie die **Id der Anwendung** für Ihre Anwendung zugewiesen, Sie benötigen rasch.
- **Web** Plattform für Ihre Anwendung hinzufügen.
- Geben Sie den entsprechenden **URI umleiten**. Der Umleitung Uri gibt Azure AD, Authentifizierungsantworten richten - die Standardeinstellung für dieses Lernprogramm ist `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Installieren und Konfigurieren von owin-Authentifizierung
Konfigurieren Sie hier owin-Middleware um das Authentifizierungsprotokoll OpenID verbinden verwenden.  Anmelden und Abmelden Anforderungen, die Sitzung des Benutzers verwalten und Informationen über den Benutzer unter anderem wird OWIN verwendet werden.

-   Öffnen Sie zunächst das `web.config` -Datei im Stammverzeichnis des Projekts, und geben Sie Werte in Ihre Anwendung die `<appSettings>` Abschnitt.
    -   Die `ida:ClientId` ist Ihre app im registrierungsportal zugewiesene **Id der Anwendung** .
    -   Die `ida:RedirectUri` ist im Portal eingegebene **Uri umleiten** .

-   Fügen Sie owin-Middleware NuGet-Pakete über die Konsole Paket-Manager hinzu.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Hinzufügen einer "owin-Startklasse" zum Projekt namens `Startup.cs` rechts auf das Projekt-- **Add** --> **Neues Element** --> Suche nach "OWIN".  Owin-Middleware Aufrufen der `Configuration(...)` -Methode, wenn die app gestartet.
-   Ändern Sie die Klassendeklaration `public partial class Startup` – wir haben bereits Teil dieser Klasse für Sie in einer anderen Datei.  In die `Configuration(...)` -Methode ein Aufruf ConfigureAuth(...) für Ihre Web-app einrichten  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und die `ConfigureAuth(...)` Methode.  Die Parameter in bieten `OpenIdConnectAuthenticationOptions` dienen als Koordinaten für Ihre Kommunikation mit Azure AD.  Müssen auch Cookie-Authentifizierung einrichten - Middleware OpenID verbinden verwendet Cookies hinter den Kulissen.

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
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Authentifizierung senden
Ihre app ist jetzt ordnungsgemäß mit der v2. 0 mit dem Authentifizierungsprotokoll OpenID verbinden Kommunikation konfiguriert.  OWIN hat alle hässlich Details Authentifizierungsnachrichten erstellen, Token von Azure AD überprüfen und Verwalten der Sitzung durchgeführt.  Bleibt zu den Benutzern eine Möglichkeit zum Anmelden und Abmelden.

- Sie können Tags auf Ihren Domänencontrollern muss vor dem Zugriff auf eine bestimmte Seite anmeldet, Benutzer autorisieren.  Open `Controllers\HomeController.cs`, und die `[Authorize]` Tag über Controller.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   OWIN können Authentifizierungsanfragen von direkt im Code ausgeben.  Open `Controllers\AccountController.cs`.  Geben Sie in die SignIn() und SignOut() Aktionen Herausforderung OpenID verbinden und Abmelden Anfragen.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Öffnen Sie jetzt `Views\Shared\_LoginPartial.cshtml`.  Dies ist, wo Sie dem Benutzer Ihrer Anwendung anmelden und Abmelden Links anzeigen und drucken Sie den Namen des Benutzers in einer Ansicht.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="display-user-information"></a>Benutzerinformationen anzeigen
Beim Authentifizieren von Benutzern mit OpenID Connect gibt v2. 0-Endpunkt ein ID-Token zu der Anwendung, die [Ansprüche](active-directory-v2-tokens.md#id_tokens)bzw. Aussagen über den Benutzer enthält.  Diese Ansprüche können Sie Ihre app anpassen:

- Öffnen der `Controllers\HomeController.cs` Datei.  Zugriff des Benutzers Ansprüche in Ihren Domänencontrollern über die `ClaimsPrincipal.Current` Security principal-Objekt.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Ausführen

Schließlich erstellen und Ausführen der app.   Persönliche Microsoft Account oder Arbeits- oder Schulcomputer Konto melden Sie an und feststellen Sie, wie die Identität des Benutzers in der oberen Navigationsleiste wiederspiegelt.  Sie haben nun eine Web app gesichert Standardprotokolle, die Benutzer mit ihren persönlichen und Arbeit oder Schule Konten authentifizieren können.

Zu Referenzzwecken können vollständiges Beispiel (ohne Ihre Werte) [als eine .zip bereitgestellt wird](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip)oder es von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt auf Themen verschieben.  Möglicherweise möchten versuchen:

[Secure Web API mit dem Endpunkt v2. 0 >>](active-directory-devquickstarts-webapi-dotnet.md)

Weitere Ressourcen zum Auschecken:
- [V2. 0-Entwicklerhandbuch >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Azure Active Directory" Tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen Benachrichtigungen Sicherheitsvorfälle auftreten [dieser Seite](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.
