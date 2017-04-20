<properties
    pageTitle="Azure AD .NET Einstieg | Microsoft Azure"
    description="Wie Sie eine Web-API von .NET MVC erstellen, die in Azure AD für Authentifizierung und Autorisierung integriert."
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


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Schützen Sie eine Web-API trägertoken von Azure AD verwenden

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Wenn Sie eine Anwendung erstellen, die Zugriff auf geschützte Ressourcen müssen Sie wissen, wie diese Ressourcen vor ungerechtfertigten Zugriffen zu schützen.
Azure AD macht es einfach und unkompliziert eine API mit OAuth Träger 2.0 Zugriffstoken mit nur wenigen Zeilen Code schützen.

In Asp.NET Web apps erreichen Sie dies mithilfe der Microsoft Implementierung von.NET Framework 4.5 enthalten communitygestützte owin-Middleware.  Hier verwenden wir OWIN, "Aufgabenliste" Web API erstellen:
-   Legt fest, welche APIs geschützt sind.
-   Überprüft, dass Web-API-Aufrufe eine gültige Zugriffstoken enthalten.

Dazu benötigen Sie:

1. Registrieren einer Anwendung in Azure AD
2. Richten Sie Ihre app Pipeline owin-Authentifizierung verwenden.
3. Konfigurieren einer Clientanwendung zu tun Liste Web API aufrufen

Um zu beginnen, [das Skelett app](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) [herunterladen](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)oder vollständiges Beispiel.  Jeder ist eine Visual Studio 2013-Lösung.  Sie benötigen auch einen Azure AD-Mandanten, Ihre Anwendung registrieren.  Haben Sie eine bereits [erfahren Sie, wie man](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. Registrieren einer Anwendung in Azure AD*
Zur Sicherung Ihrer Anwendung müssen Sie zunächst eine Anwendung in Ihrem Mandanten erstellen und Azure AD mit ein paar wichtige Informationen bereitstellen.

-   Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten in der Anwendung registriert.
-   Klicken Sie auf die Registerkarte **Applications** , und klicken Sie auf **Hinzufügen** in der unteren Schublade.
-   Befolgen Sie, und erstellen Sie eine neue **Anwendung oder WebAPI**.
    -   Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.  Geben Sie "Aufgabenliste Service".
    -   **Uri umleiten** ist eine Kombination von Schema und Zeichenfolge, die Azure AD verwenden alle Token angeforderte app zurückgegeben. Geben Sie `https://localhost:44321/` für diesen Wert.
-   Sobald die Registrierung abgeschlossen haben, navigieren Sie zur Registerkarte **Konfigurieren** und suchen Sie Feld **App ID URI** .  Geben Sie eine mandantenspezifische Kennung für diesen Wert z.B.`https://contoso.onmicrosoft.com/TodoListService`
- Speichern Sie die Konfiguration.  Das Portal offen - müssen auch kurz Ihre Client-Anwendung registrieren.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. richten Sie Ihre app Pipeline owin-Authentifizierung*

Eine Anwendung in Azure AD registriert haben, müssen Sie eine Anwendung in Azure AD kommunizieren, um eingehende Anfragen & Token überprüfen einrichten.

-   Zu beginnen, öffnen Sie die Projektmappe owin-Middleware NuGet-Pakete über die Konsole Paket-Manager TodoListService-Projekt hinzufügen.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Das Projekt TodoListService einen owin-Startklasse hinzufügen `Startup.cs`.  Rechts auf das Projekt-- **Add** --> **Neues Element** --> Suche nach "OWIN".  Owin-Middleware Aufrufen der `Configuration(…)` -Methode, wenn die app gestartet.
-   Ändern Sie die Klassendeklaration `public partial class Startup` – wir haben bereits Teil dieser Klasse für Sie in einer anderen Datei.  In die `Configuration(…)` -Methode ein Aufruf ConfgureAuth(...) für Ihre Web-app einrichten.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und die `ConfigureAuth(…)` Methode.  Die Parameter in bieten `WindowsAzureActiveDirectoryBearerAuthenticationOptions` dienen als Koordinaten für Ihre Kommunikation mit Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Sie können jetzt `[Authorize]` Attribute zum Schutz von Controllern und Aktionen mit JWT Träger Authentifizierung.  Ergänzen der `Controllers\TodoListController.cs` mit einem Tag autorisieren.  Dies zwingt den Benutzer vor dem Zugriff auf die Seite anmelden.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Bei autorisierter Aufrufer erfolgreich auf eines ruft der `TodoListController` -APIs in die Aktion möglicherweise Zugriff auf Informationen über den Aufrufer.  OWIN Zugriff auf die Ansprüche der trägertoken über die `ClaimsPrincpal` Objekt.  
- Eine allgemeine Anforderung für Web-APIs "Bereiche" im Token überprüft wird sichergestellt, dass der Endbenutzer die Todo Liste Dienst erforderlichen Berechtigungen zugestimmt hat:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Schließlich die `web.config` im Stammverzeichnis des Projekts TodoListService Datei, und geben Sie die Werte in den `<appSettings>` Abschnitt.
  - Die `ida:Tenant` ist der Name der Azure AD-Mandanten, z. B. "contoso.onmicrosoft.com".
  - Die `ida:Audience` ist der App-ID-URI der Anwendung in Azure-Portal eingegeben.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. konfigurieren Sie eine Clientanwendung und führen Sie des Dienstes aus*
Bevor Sie Todo Liste Dienst in Aktion sehen können, müssen Sie der Liste Todo AAD Token erhalten und Aufrufe an den Dienst.

- Navigieren Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com)
- Erstellen einer neuen Anwendung in Azure AD-Mandanten und **Anwendungsspezifischen Client** in der resultierenden Meldung auswählen.
    -   Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.
    -   Geben Sie `http://TodoListClient/` für die **Umleitung Uri** -Wert.
- Nach Abschluss Anmeldung zuweisen AAD app eine eindeutige **Client-Id**. Sie benötigen diesen Wert in den nächsten Schritten so kopieren Sie sie von der Registerkarte konfigurieren.
- Auch der Registerkarte **Konfigurieren** , suchen Sie den Abschnitt "Berechtigungen to Other Applications". Klicken Sie auf "Anwendung hinzufügen". Wählen Sie "Alle Apps" in der Dropdownliste "Anzeigen", und klicken Sie auf das obere Kontrollkästchen. Klicken Sie auf die zu tun Liste Dienst, und klicken Sie auf das Häkchen unten die Anwendung hinzufügen. Wählen Sie "Zu tun Liste Dienst" aus "Delegiert Berechtigungen", und speichern Sie die Konfiguration.


- Öffnen Sie in Visual Studio `App.config` in den TodoListClient und geben Sie die Werte in den `<appSettings>` Abschnitt.
  - Die `ida:Tenant` ist der Name der Azure AD-Mandanten, z. B. "contoso.onmicrosoft.com".
  - Die `ida:ClientId` app-ID aus dem Azure-Portal kopiert.
  - Die `todo:TodoListResourceId` ist der App-ID URI führen Liste Service-Anwendung, die in Azure-Portal eingegeben.

Schließlich bereinigen erstellen und jedes Projekt ausführen!  Wenn nicht bereits geschehen, jetzt ist die Zeit zum Erstellen eines neuen Benutzers in Ihrem Mandanten mit einer *..onmicrosoft.com-Domäne.  Liste Client mit Benutzer anmelden und des Benutzers Liste Aufgaben hinzufügen.

Zu Referenzzwecken vollständiges Beispiel (ohne die Konfigurationswerte) bereitgestellt [hier](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Sie können weitere zusätzliche Identität Szenarien nun.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
