<properties
    pageTitle="Azure AD v2. 0 .NET Web API | Microsoft Azure"
    description="Wie Sie eine Web-Api von .NET MVC erstellen, die sowohl persönliche Microsoft Account-Token und Geschäfts-oder akzeptiert."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Sichern der MVC-Web-API

Azure Active Directory v2. 0-Endpunkt eine Web-API mit [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) Zugriffstoken Anwender persönliche Microsoft-Konto sowohl Arbeit schützen oder Schule Konten sicher auf der Web-API.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

In ASP.NET Web APIs erreichen Sie dies mit Microsoft owin-Middleware in.NET Framework 4.5 aufgenommen.  OWIN verwenden wir hier eine "Aufgabenliste" MVC Web-API erstellen, durch die Clients zu Aufgaben in der Aufgabenliste des Benutzers lesen.  Web-API wird sichergestellt, dass eingehende Anfragen ein gültiges Zugangstoken enthalten und Ablehnen von Anfragen, die keine Überprüfung auf eine geschützte Route übergeben.  In diesem Beispiel wurde mit Visual Studio 2015 erstellt.

## <a name="download"></a>Herunterladen
Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet)verwaltet.  Um folgen, können Sie [die Anwendung Skelett als .zip downloaden](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) oder das Skelett Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

Skelette-app enthält den Standardcode für eine einfache API jedoch alle identitätsbezogene fehlen. Wenn Sie nachvollziehen möchten, können Sie stattdessen Klonen oder [vollständiges Beispiel herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Registrieren einer Anwendung
Erstellen Sie eine neue Anwendung auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)und befolgen Sie diese [Anleitung](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

- Kopieren Sie die **Id der Anwendung** für Ihre Anwendung zugewiesen, Sie benötigen rasch.

Visual Studio-Projektmappe enthält auch eine einfache WPF-Anwendung "TodoListClient".  Die TodoListClient wird wie ein Benutzer Zeichen in und wie ein Client fordert Web API ausstellen kann.  In diesem Fall werden die TodoListClient und die TodoListService durch die gleiche Anwendung dargestellt.  Um die TodoListClient zu konfigurieren, sollten Sie auch:

- **Mobile** Plattform für Ihre Anwendung hinzufügen.


## <a name="install-owin"></a>OWIN installieren

Registrierung eine Anwendung müssen Sie Ihre Anwendung einrichten, v2. 0-Endpunkt zu kommunizieren, um eingehende Anfragen & Token überprüft.

- Zu beginnen, öffnen Sie die Projektmappe owin-Middleware NuGet-Pakete über die Konsole Paket-Manager TodoListService-Projekt hinzufügen.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>OAuth Authentifizierung konfigurieren

- Das Projekt TodoListService einen owin-Startklasse hinzufügen `Startup.cs`.  Rechts auf das Projekt-- **Add** --> **Neues Element** --> Suche nach "OWIN".  Owin-Middleware Aufrufen der `Configuration(…)` -Methode, wenn die app gestartet.
- Ändern Sie die Klassendeklaration `public partial class Startup` – wir haben bereits Teil dieser Klasse für Sie in einer anderen Datei.  In die `Configuration(…)` -Methode ein Aufruf ConfgureAuth(...) für Ihre Web-app einrichten.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und die `ConfigureAuth(…)` -Methode Web API einrichten Token vom Endpunkt v2. 0 akzeptiert.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Sie können jetzt `[Authorize]` Attribute zum Schutz von Controllern und Aktionen mit OAuth 2.0 Träger Authentifizierung.  Ergänzen der `Controllers\TodoListController.cs` mit einem Tag autorisieren.  Dies zwingt den Benutzer vor dem Zugriff auf die Seite anmelden.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Bei autorisierter Aufrufer erfolgreich auf eines ruft der `TodoListController` -APIs in die Aktion möglicherweise Zugriff auf Informationen über den Aufrufer.  OWIN Zugriff auf die Ansprüche der trägertoken über die `ClaimsPrincpal` Objekt.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Schließlich die `web.config` im Stammverzeichnis des Projekts TodoListService Datei, und geben Sie die Werte in den `<appSettings>` Abschnitt.
  - Die `ida:Audience` ist die **Id der Anwendung** der app im Portal eingegeben.

## <a name="configure-the-client-app"></a>Konfigurieren Sie die Clientanwendung
Bevor Sie Todo Liste Dienst in Aktion sehen können, müssen Sie der Liste Todo v2. 0-Endpunkt Token erhalten und Aufrufe an den Dienst.

- Öffnen Sie im Projekt TodoListClient `App.config` , und geben Sie die Werte in den `<appSettings>` Abschnitt.
  - Die `ida:ClientId` Anwendung-Id aus dem Portal kopiert.

Schließlich bereinigen erstellen und jedes Projekt ausführen!  Sie haben nun eine .NET MVC-Web-API, das Token aus beiden persönlichen Microsoft-Konten und Geschäfts-oder akzeptiert.  Die TodoListClient anmelden, und rufen Sie Ihr Web api Aufgabenliste des Benutzers Aufgaben hinzu.

Zu Referenzzwecken können vollständiges Beispiel (ohne Ihre Werte) [als eine .zip bereitgestellt wird](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip)oder es von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Nächste Schritte
Sie können jetzt auf Weitere Themen verschieben.  Möglicherweise möchten versuchen:

[Aufrufen von Web-API von Web App >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Weitere Ressourcen zum Auschecken:
- [V2. 0-Entwicklerhandbuch >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Azure Active Directory" Tag >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen Benachrichtigungen Sicherheitsvorfälle auftreten [dieser Seite](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.
