<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Wie eine Anwendung erstellen, die Anmeldung, Anmeldung und Kennwort zurückgesetzt Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure AD B2C: Anmeldung & Anmeldung in einem ASP.NET Web App

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mithilfe von Azure Active Directory (Azure AD) B2C können Sie Ihrer Anwendung in wenigen Schritten leistungsstarke Self-service-Management bietet hinzufügen. Dieser Artikel wie eine ASP.NET Web app erstellen Benutzer anmelden, Anmeldung und Kennwort zurücksetzen. Die Anwendung wird Unterstützung für Anmeldung und melden Sie sich mit einem Benutzernamen oder e-Mail- und mit sozialen Konten wie Facebook und Google enthalten.

In diesem Lernprogramm unterscheidet sich von [unserem .NET Weblernprogramm](active-directory-b2c-devquickstarts-web-dotnet.md) verwendet einen [registrieren oder anmelden, Richtlinien](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) , Registrierung und mit einem Mausklick anstelle von zwei (für Anmeldung und Anmeldung).  Kurz gesagt, eine Anmeldung oder Anmeldung Richtlinie ermöglicht Benutzern mit einem bestehenden Konto anmelden Wenn sie haben oder eine neue zu erstellen, ist die Anwendung zum ersten Mal.

## <a name="get-an-azure-ad-b2c-directory"></a>Ein Azure AD B2C-Verzeichnis abrufen

Vor der Verwendung Azure AD B2C müssen Sie ein Verzeichnis oder Mieter. Ein Verzeichnis ist ein Container für alle Benutzer, apps und Gruppen.  Haben Sie eine bereits, [B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie fortfahren, in diesem Handbuch.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine Anwendung in Ihrem B2C-Verzeichnis erstellen. Dies informiert Azure AD, um die sichere Kommunikation mit Ihrer Anwendung. [Gehen folgendermaßen Sie vor, um eine Anwendung zu erstellen.](active-directory-b2c-app-registration.md)  Achten Sie darauf,:

- **Web app/Web-API** in der Anwendung enthalten.
- Geben Sie `https://localhost:44316/` als einen **URI umleiten**. Es ist die Standard-URL für dieses Codebeispiel.
- Kopieren Sie die **ID der Anwendung** , die Ihre Anwendung zugewiesen ist.  Sie werden ihn später benötigen.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen von Richtlinien

In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Dieses Codebeispiel enthält zwei Identität Erfahrungen: **Anmeldung & Anmeldung**und **Kennwort zurücksetzen**.  Sie müssen eine Richtlinie jedes Typs erstellen gemäß [Richtlinie Referenzartikel](active-directory-b2c-reference-policies.md). Wenn Sie zwei Richtlinien erstellen, müssen Sie:

- **Benutzer-ID anmelden** oder **Anmeldung E-Mail** Identität Anbieter Blatt auswählen
- **Anzeigename** und andere Anmeldung Attribute in Ihrer Richtlinie Anmeldung & Anmeldung auswählen
- Wählen Sie **Anzeigenamen** Anspruch als Anwendung Forderung in jeder Richtlinie. Sie können auch andere Ansprüche.
- Kopieren Sie den **Namen** jeder Richtlinie nach ihrer Erstellung. Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nach dem Erstellen der zwei Richtlinien können Sie Ihre Anwendung erstellen.

## <a name="download-the-code-and-configure-authentication"></a>Den Code herunterladen und Konfigurieren der Authentifizierung

Der Code für dieses Beispiel [auf GitHub verwaltet](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI). Zum Beispiel wie Sie, können Sie [das Skelette Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Sie können auch das Skelett Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

Vollständiges Beispiel ist auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) oder die `complete` dasselbe Repository-Zweig.

Nach dem Herunterladen des Codes öffnen Sie Visual Studio sln-Datei für den Einstieg.

Die Anwendung kommuniziert mit Azure AD B2C per HTTP-Authentifizierungsanfragen, die die Richtlinie angeben, die als Teil der Anforderung ausgeführt werden sollen. Für .NET Web Applications können Sie Microsoft owin-Bibliothek senden OpenID herstellen Authentifizierungsanfragen Ausführung von Policies, anzeigenBenutzersitzungen verwalten.

Zunächst owin-Middleware NuGet-Pakete dem Projekt fügen Sie mithilfe der Verwaltungskonsole Visual Studio Paket-Manager hinzu.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Install-Package System.IdentityModel.Tokens.Jwt
```

Öffnen Sie dann die `web.config` Datei im Stammverzeichnis des Projekts, und geben Sie Werte in Ihre Anwendung die `<appSettings>` Abschnitt ersetzen die Werte unter eigene.  Lassen Sie die `ida:RedirectUri` und `ida:AadInstance` Werte ist unverändert.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Fügen Sie eine Startklasse owin-Projekt namens `Startup.cs`. Mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neue Artikel**und suchen Sie nach "OWIN." Ändern Sie die Klassendeklaration, `public partial class Startup`. Wir implementiert dieser Kurs für Sie in einer anderen Datei. Owin-Middleware Aufrufen der `Configuration(...)` -Methode, wenn die app gestartet. In dieser Methode rufen Sie `ConfigureAuth(...)`, wobei Sie für Ihre Anwendung einrichten.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und die `ConfigureAuth(...)` Methode.  Die Parameter in bieten `OpenIdConnectAuthenticationOptions` dienen als Koordinaten für Ihre Kommunikation mit Azure AD. Sie müssen auch Cookie-Authentifizierung einrichten. Die Middleware OpenID verbinden verwendet Cookies Sitzungen, unter anderem zu.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(PasswordResetPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SusiPolicyId));

    }

    private Task OnSecurityTokenValidated(SecurityTokenValidatedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        // If you wanted to keep some local state in the app (like a db of signed up users),
        // you could use this notification to create the user record if it does not already
        // exist.

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
...
```

## <a name="send-authentication-requests-to-azure-ad"></a>Authentifizierung in Azure AD senden
Ihre Anwendung ist jetzt ordnungsgemäß konfiguriert das Authentifizierungsprotokoll OpenID Verbinden mit Azure AD B2C kommunizieren.  OWIN hat alle Details Authentifizierungsnachrichten erstellen, Token von Azure AD überprüfen und Verwalten der Sitzung durchgeführt.  Bleibt jedes Benutzers initiieren.

Ein Benutzer wählt bei **Anmeldung** oder **Kennwort vergessen?** in Web app die zugeordnete Aktion aufgerufen, `Controllers\AccountController.cs`. Integrierte owin-Methoden können Sie in jedem Fall die richtige Richtlinie ausgelöst:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SusiPolicyId);
    }
}

public void ResetPassword()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties() { RedirectUri = "/" }, Startup.PasswordResetPolicyId);
    }
}
```

Während der Ausführung der Anmeldung oder Richtlinie anmelden, der Benutzer hat die Möglichkeit, auf ein **Kennwort vergessen?** verknüpfen.  In diesem Fall senden Azure AD B2C Ihre Anwendung eine Fehlermeldung, dass ein Kennwort ausführen soll Richtlinie zurückgesetzt.  Kann dieser Fehler in `Startup.Auth.cs` mit dem `AuthenticationFailed` Benachrichtigung:

```C#
// Used for avoiding yellow-screen-of-death TODO
private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        // If the user canceled the sign in, redirect back to the home page
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```


Eine Richtlinie explizit aufrufen, können Sie ein `[Authorize]` -Tag in der Domänencontroller, der eine Richtlinie ausgeführt wird, wenn der Benutzer nicht angemeldet ist. Open `Controllers\HomeController.cs` und die `[Authorize]` Tag den Ansprüchen Controller.  OWIN wählt die letzte Richtlinie konfiguriert, wenn die `[Authorize]` Tag erreicht.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

OWIN können Sie den Benutzer von der Anwendung anmelden. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Benutzerinformationen anzeigen
Wenn Sie Benutzer mithilfe von OpenID Connect authentifizieren, gibt Azure AD einen Token-ID an die Anwendung, die **Ansprüche**enthält. Dies sind Aussagen über den Benutzer. Ansprüche können Sie Ihre Anwendung anpassen.  

Öffnen der `Controllers\HomeController.cs` Datei. Zugriff Benutzeransprüche in Ihren Domänencontrollern über die `ClaimsPrincipal.Current` Security principal-Objekt.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Die möglich Ansprüche auf die gleiche Weise die Anwendung empfängt.  Eine Liste aller Ansprüche erhält die Anwendung steht Ihnen auf **der Seite** .

## <a name="run-the-sample-app"></a>Führen Sie die Beispiel-app

Schließlich können Sie erstellen und Ausführen Ihrer Anwendung. Melden Sie sich für die Anwendung ein e-Mail-Adresse oder Benutzername verwendet. Melden Sie ab und wieder melden Sie an, als derselbe Benutzer. Bearbeiten Sie dieses Benutzerprofil Abmelden und als anderer Benutzer anmelden. Beachten Sie, dass Richtlinien konfiguriert die Informationen auf der Registerkarte **Ansprüche** angezeigte Informationen entspricht.

## <a name="add-social-idps"></a>Soziale vertriebenen hinzufügen

Die Anwendung unterstützt derzeit nur Benutzer anmelden und einloggen mittels **lokaler Konten**. Diese sind Konten in Ihrem B2C-Verzeichnis gespeichert, die einen Benutzernamen ein Kennwort und. Mithilfe von Azure AD B2C können Sie andere **Identitätsprovider** (IDP) ohne Code hinzufügen.

Beginnen Sie Ihre app sozialen vertriebenen hinzu, anhand der Informationen in diesem Artikel. Für jeden IDP unterstützen möchten, müssen Sie eine Anwendung in diesem System registrieren und erhalten eine Client-ID

- [Facebook als IDP einrichten](active-directory-b2c-setup-fb-app.md)
- [Google als IDP einrichten](active-directory-b2c-setup-goog-app.md)
- [Einrichten von Amazon als IDP](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn als IDP einrichten](active-directory-b2c-setup-li-app.md)

Nach dem Hinzufügen der Identitätsanbieter zu Ihrem B2C-Verzeichnis müssen Sie jede der drei Richtlinien sollen neuen vertriebenen gemäß [Richtlinie Referenzartikel](active-directory-b2c-reference-policies.md)bearbeiten. Nach dem Speichern der Richtlinien führen Sie die Anwendung erneut.  Sie sollten neuen vertriebenen als anmelden und Anmeldeoptionen aller Ihrer Identität auftritt.

Ihre Richtlinien experimentieren und beobachten Sie die Auswirkung auf die Beispiel-app. Hinzufügen oder Entfernen von Vertriebenen, Anwendungsansprüchen bearbeiten oder Anmeldung Attribute ändern Experimentieren Sie sehen, wie Richtlinien, Authentifizierung und owin-zusammenzufassen.

Zu Referenzzwecken vollständiges Beispiel (ohne Ihre Werte) [als eine ZIP-Datei bereitgestellt wird](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip). Sie können auch von GitHub Klonen:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
