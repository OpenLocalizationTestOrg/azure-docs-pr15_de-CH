<properties
    pageTitle="Azure AD .NET Einstieg | Microsoft Azure"
    description="Wie .NET MVC Web App erstellen, die in Azure AD Anmelden integriert."
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

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>ASP.NET Web App anmelden und Abmelden bei Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD macht es einfach und unkompliziert Ihre Web app Identitätsmanagement Auslagern mit einzelnen an- und Abmeldung mit nur wenigen Zeilen Code.  In Asp.NET Web apps erreichen Sie dies mithilfe der Microsoft Implementierung von.NET Framework 4.5 enthalten communitygestützte owin-Middleware.  Hier verwenden wir OWIN an:
-   Die app Azure AD als Identitätsanbieter melden Sie Benutzer an.
-   Einige Informationen über den Benutzer angezeigt.
-   Abmelden des Benutzers von der Anwendung.

Dazu benötigen Sie:

1. Registrieren einer Anwendung in Azure AD
2. Richten Sie Ihre app Pipeline owin-Authentifizierung verwenden.
3. Verwenden Sie OWIN Azure AD anmelden und Abmelde Anfragen erteilen.
4. Daten über die Benutzer ausgegeben.

Um zu beginnen, [das Skelett app](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) [herunterladen](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip)oder vollständiges Beispiel.  Sie benötigen auch einen Azure AD-Mandanten, Ihre Anwendung registrieren.  Haben Sie eine bereits [erfahren Sie, wie man](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. Registrieren einer Anwendung in Azure AD*
Aktivieren Sie Ihre app zum Authentifizieren von Benutzern müssen Sie zuerst eine neue Anwendung in Ihrem Mandanten registrieren.

- Melden Sie sich bei Azure-Verwaltungsportal.
- Klicken Sie im linken Navigationsbereich auf **Active Directory**.
- Wählen Sie Mieter wo Sie registrieren möchten.
- Klicken Sie auf die Registerkarte **Programme** , und klicken Sie auf Hinzufügen in der unteren Schublade.
- Befolgen Sie, und erstellen Sie eine neue **Anwendung oder WebAPI**.
    - Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.
    -   **Zeichen-URL** ist der Basis-URL der Anwendung.  Das Skelett-Standardwert ist `https://localhost:44320/`.
    - **URI der App-ID** ist eine eindeutige Kennung für die Anwendung.  Die Konvention ist mit `https://<tenant-domain>/<app-name>`, z.B.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Nach Abschluss Anmeldung zuweisen AAD app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten so kopieren Sie sie von der Registerkarte konfigurieren.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. richten Sie Ihre app Pipeline owin-Authentifizierung*
Konfigurieren Sie hier owin-Middleware um das Authentifizierungsprotokoll OpenID verbinden verwenden.  Anmelden und Abmelden Anforderungen, die Sitzung des Benutzers verwalten und Informationen über den Benutzer unter anderem wird OWIN verwendet werden.

-   Fügen Sie zunächst owin-Middleware NuGet-Pakete über die Konsole Paket-Manager hinzu.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Das Projekt eine owin-Startklasse hinzufügen `Startup.cs` rechts auf das Projekt-- **Add** --> **Neues Element** --> Suche nach "OWIN".  Owin-Middleware Aufrufen der `Configuration(...)` -Methode, wenn die app gestartet.
-   Ändern Sie die Klassendeklaration `public partial class Startup` – wir haben bereits Teil dieser Klasse für Sie in einer anderen Datei.  In die `Configuration(...)` -Methode ein Aufruf ConfgureAuth(...) für Ihre Web-app einrichten  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und die `ConfigureAuth(...)` Methode.  Die Parameter in bieten `OpenIDConnectAuthenticationOptions` dienen als Koordinaten für Ihre Kommunikation mit Azure AD.  Müssen auch Cookie-Authentifizierung einrichten - Middleware OpenID verbinden verwendet Cookies hinter den Kulissen.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Schließlich die `web.config` Datei im Stammverzeichnis des Projekts, und geben Sie die Werte in den `<appSettings>` Abschnitt.
    -   Ihre Anwendung `ida:ClientId` ist die Guid von Azure Portal in Schritt 1 kopiert.
    -   Die `ida:Tenant` ist der Name der Azure AD-Mandanten, z. B. "contoso.onmicrosoft.com".
    -   Die `ida:PostLogoutRedirectUri` gibt an, wo ein Benutzer umgeleitet werden sollten nach dem erfolgreichen einer Abmeldung Anforderung Abschluss, Azure AD.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. OWIN Azure AD anmelden und Abmelde Anfragen erteilen*
Ihre Anwendung wird jetzt ordnungsgemäß mit Azure mithilfe des OpenID verbinden Kommunikation konfiguriert.  OWIN hat alle hässlich Details Authentifizierungsnachrichten erstellen, Token von Azure AD überprüfen und Verwalten der Sitzung durchgeführt.  Bleibt zu den Benutzern eine Möglichkeit zum Anmelden und Abmelden.

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
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Öffnen Sie jetzt `Views\Shared\_LoginPartial.cshtml`.  Dies ist, wo Sie dem Benutzer Ihrer Anwendung anmelden und Abmelden Links anzeigen und drucken Sie den Namen des Benutzers in einer Ansicht.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
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

## <a name="4--display-user-information"></a>*4. Benutzerinformationen anzeigen*
Beim Authentifizieren von Benutzern mit OpenID Connect Azure AD ein ID-Token der Anwendung zurückgegeben, die "Ansprüche" oder Aussagen über den Benutzer enthält.  Diese Ansprüche können Sie Ihre app anpassen:

- Öffnen der `Controllers\HomeController.cs` Datei.  Zugriff des Benutzers Ansprüche in Ihren Domänencontrollern über die `ClaimsPrincipal.Current` Security principal-Objekt.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Schließlich erstellen und Ausführen der app.  Wenn nicht bereits geschehen, jetzt ist die Zeit zum Erstellen eines neuen Benutzers in Ihrem Mandanten mit einer *..onmicrosoft.com-Domäne.  Dieser Benutzer melden Sie an und feststellen Sie, wie die Identität des Benutzers in der oberen Navigationsleiste wiederspiegelt.  Melden Sie ab und wieder melden Sie an, als ein anderer Benutzer in Ihrem Mandanten.  Wenn Sie vor fühlen, registrieren und für eine andere Instanz dieser Anwendung (mit eigenen ClientId) und Überwachung Siehe Single-Sign-Aktion auszuführen.

Zu Referenzzwecken [hier](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip)vollständiges Beispiel (ohne Ihre Werte).  

Sie können jetzt auf Themen verschieben.  Möglicherweise möchten versuchen:

[Secure Web API Azure Ad >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
