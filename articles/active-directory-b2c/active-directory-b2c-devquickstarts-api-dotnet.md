<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Erstellen einer Web-API von .NET mit Azure Active Directory B2C, OAuth 2.0 Zugriffstoken für die Authentifizierung mit gesichert."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Erstellen Sie .NET Web API

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mit Azure Active Directory (Azure AD) B2C sichern Sie eine Web-API mit OAuth 2.0 Zugriffstoken. Diese Token können Ihre Clientanwendungen, die Azure AD B2C verwendet die API authentifizieren. Dieser Artikel beschreibt, wie eine .NET Model-View-Controller (MVC) "Aufgabenliste" API erstellen, die Benutzern, CRUD-Vorgänge ermöglicht. Web-API mit Azure AD B2C gesichert ist und nur authentifizierten Benutzern ihrer Aufgabenliste verwalten.

## <a name="create-an-azure-ad-b2c-directory"></a>Ein Azure AD B2C-Verzeichnis erstellen

Vor der Verwendung Azure AD B2C müssen Sie ein Verzeichnis oder Mieter. Ein Verzeichnis ist ein Container für alle Benutzer, apps und Gruppen. Haben Sie eine bereits, [B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie fortfahren, in diesem Handbuch.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine Anwendung in Ihrem B2C-Verzeichnis erstellen. Dies informiert Azure AD, um die sichere Kommunikation mit Ihrer Anwendung. [Gehen folgendermaßen Sie vor, um eine Anwendung zu erstellen.](active-directory-b2c-app-registration.md) Achten Sie darauf,:

- Fügen Sie **WebApp** oder **Web-API** in der Anwendung.
- Verwenden Sie **Umleiten uniform Resource Identifier** `https://localhost:44316/` für Web app. Dies ist der Standardspeicherort des Webclients app dieses Codebeispiel.
- Kopieren Sie die **ID der Anwendung** , die Ihre Anwendung zugewiesen ist. Sie werden ihn später benötigen.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen von Richtlinien

In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Der Client in diesem Codebeispiel enthält drei Identität Erfahrungen: anmelden, anmelden und Profil bearbeiten. Sie müssen zum Erstellen einer Richtlinie für jeden Typ gemäß [Richtlinie Referenzartikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wenn Sie die drei Richtlinien erstellen, müssen Sie:

- **Benutzer-ID anmelden** oder **Anmeldung E-Mail** Identität Anbieter Blatt auswählen
- Wählen Sie im die Registrierungsrichtlinie **Anzeigename** und andere Anmeldung Attribute.
- Wählen Sie **Anzeigename** und **Objekt-ID** Ansprüche Anwendung Ansprüche für jede Richtlinie. Sie können auch andere Ansprüche.
- Kopieren Sie den **Namen** jeder Richtlinie nach ihrer Erstellung. Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erstellt haben, können Sie Ihre Anwendung erstellen.

## <a name="download-the-code"></a>Den Code herunterladen

Der Code für dieses Lernprogramm [auf GitHub verwaltet](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Zum Beispiel wie Sie, können Sie [ein Skelett Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Sie können auch das Skelett Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Die abgeschlossene Anwendung wird auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) oder die `complete` dasselbe Repository-Zweig.

Nach dem Herunterladen des Codes öffnen Sie Visual Studio sln-Datei für den Einstieg. Die Projektmappendatei enthält zwei Projekte: `TaskWebApp` und `TaskService`. `TaskWebApp`ist ein MVC, mit dem der Benutzer interagiert. `TaskService`ist die app Backend-Web-API, die Aufgabenliste des Benutzers speichert.

## <a name="configure-the-task-web-app"></a>Konfigurieren der Task Web app

Bei einer Benutzerinteraktion mit `TaskWebApp`, der Client sendet Anfragen an Azure AD und wieder Token verwendet werden können, rufen Sie die `TaskService` web-API. Benutzer anmelden und Token müssen bereitstellen `TaskWebApp` Informationen über Ihre app. In der `TaskWebApp` Projekt, der `web.config` -Datei im Stammverzeichnis des Projekts, und Ersetzen Sie die Werte in der `<appSettings>` Abschnitt.  Lassen Sie die `AadInstance`, `RedirectUri`, und `TaskServiceUrl` als Werte-ist.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

Dieser Artikel behandelt nicht Gebäude die `TaskWebApp` Client.  Erstellen eine Web-app mit Azure AD B2C finden Sie in [unserer .NET Web app-Lernprogramm](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Sichern der API

Bei einem Client, der für Benutzer der API-Aufrufe Sichern `TaskService` mit OAuth 2.0 trägertoken. Ihre API kann akzeptieren und Token mithilfe von Microsoft Open Web-Schnittstelle für .NET (owin-) Bibliothek überprüfen.

### <a name="install-owin"></a>OWIN installieren
Beginnen Sie mit owin-OAuth Authentifizierung Pipeline:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Die B2C-Details eingeben
Öffnen der `web.config` Datei im Stammverzeichnis der `TaskService` Projekt, und Ersetzen Sie die Werte in den `<appSettings>` Abschnitt. Diese Werte werden in der API und owin-Bibliothek verwendet.  Lassen Sie die `AadInstance` Wert unverändert.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Fügen Sie eine owin-Start-Klasse
Hinzufügen eine owin-Start-Klasse die `TaskService` mit dem Namen `Startup.cs`.  Mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neue Artikel**und OWIN suchen.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>OAuth 2.0 Authentifizierung konfigurieren
Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und implementieren Sie die `ConfigureAuth(...)` Methode:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Sichern des Task-Controllers
Nach die Anwendung OAuth 2.0-Authentifizierung konfiguriert ist, Sichern Sie Ihre Web-API durch Hinzufügen einer `[Authorize]` zu Task-Controller. Dies ist der Controller, alle Aufgabe Liste Bearbeitung stattfindet sollten Sie den gesamten Controller auf Klassenebene sichern. Sie können auch die `[Authorize]` zu einzelnen Aktionen für eine genauere Kontrolle.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Abrufen von Informationen aus dem token
`TasksController`Aufgaben in einer Datenbank jeden Vorgang zugeordneten Benutzer Besitzer hat "Aufgabe" gespeichert. Der Besitzer ist der Benutzer **Objekt-ID**identifiziert. (Daher mussten Sie die Objekt-ID als Anwendung hinzufügen alle Richtlinien Anspruch.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Führen Sie die Beispiel-app

Schließlich erstellen und führen beide `TaskWebApp` und `TaskService`. Melden Sie sich mit einer e-Mail-Adresse oder Benutzername für die Anwendung. Einige Aufgaben in der Aufgabenliste des Benutzers erstellen und wie sie in der API beibehalten werden auch nach dem Beenden und den Client Neustarten beachten.

## <a name="edit-your-policies"></a>Bearbeiten von Richtlinien

Nachdem Sie eine API mit Azure AD B2C gesichert haben, können Sie experimentieren mit Ihrer Anwendung und die Effekte anzeigen (oder deren Fehlen) auf die API. Sie können Anwendung Ansprüche in den Richtlinien bearbeiten und ändern die Benutzerinformationen in der Web-API verfügbar ist. Ansprüche, die Sie hinzufügen werden die MVC .NET Web-API in der `ClaimsPrincipal` -Objekts, wie weiter oben in diesem Artikel beschrieben.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
