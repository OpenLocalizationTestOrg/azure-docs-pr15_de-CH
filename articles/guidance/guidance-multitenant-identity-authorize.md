<properties
   pageTitle="Autorisierung in mandantenfähigen Applikationen | Microsoft Azure"
   description="Wie Sie in einer mehrinstanzenfähigen Anwendung Autorisierung"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Rollen und ressourcenbasierte Autorisierung in mandantenfähigen Applikationen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie]. Außerdem ist eine vollständige [Beispiel] , das dieser Serie begleitet.

Die [Implementierung des Verweis] ist eine ASP.NET Core 1.0-Anwendung. In diesem Artikel betrachten zwei grundsätzliche Ansätze Genehmigung wir mit der Autorisierung in ASP.NET Core 1.0 bereitgestellten APIs.

-   **Rollenbasierte Autorisierung**. Autorisierung eine Aktion basierend auf den Rollen, die einem Benutzer zugewiesen. Einige Aktionen erfordern beispielsweise eine Administratorrolle.
-   **Ressource - Autorisierung**. Eine Aktion basierend auf eine bestimmte Ressource autorisiert. Beispielsweise hat jede Ressource einen Besitzer. Der Eigentümer kann die Ressource löschen; andere Benutzer können nicht.

Eine normale Anwendung verwendet eine Kombination aus beidem. Beispielsweise um eine Ressource zu löschen, muss der Benutzer die Ressource Besitzer _oder_ Administrator


## <a name="role-based-authorization"></a>Rollenbasierte Autorisierung

[Tailspin Umfragen] [ Tailspin] Anwendung definiert die folgenden Rollen:

- Administrator. Können alle CRUD auf alle Vorgänge, die zu diesem Mandanten gehört.
- Ersteller. Können neue Umfragen erstellen
- Reader. Alle Umfragen erhalten, die diesem Mandanten angehören

Rollen übernehmen für _Benutzer_ der Anwendung. In der Anwendung Umfragen ist ein Benutzer ein Administrator, Ersteller oder Reader.

Informationen zum Definieren und Verwalten von Rollen finden Sie unter [Anwendungsrollen].

Unabhängig davon, wie Sie die Rollen verwalten zeigt Ihres Autorisierungscodes. Core ASP.NET 1.0 führt eine Abstraktion [Autorisierungsrichtlinien]aufgerufen[policies]. Mit diesem Feature Autorisierungsrichtlinien in Code definieren und Controller-Aktionen die Richtlinien zuweisen. Die Richtlinie wird vom Controller entkoppelt.

### <a name="create-policies"></a>Erstellen von Richtlinien

Definieren Sie eine Klasse, die implementiert zunächst `IAuthorizationRequirement`. Am einfachsten Ableitung `AuthorizationHandler`. In der `Handle` -Methode, die entsprechenden Ansprüche überprüfen.

Hier ist ein Beispiel aus der Tailspin Umfragen Anwendung:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] Siehe [SurveyCreatorRequirement.cs]

Diese Klasse definiert die Anforderung für einen Benutzer, eine neue Umfrage zu erstellen. Der Benutzer muss in der SurveyAdmin oder SurveyCreator sein.

Definieren Sie in Ihrer Startklasse einer benannten Richtlinie, die eine oder mehrere Anforderungen. Sind mehrere Anforderungen, muss der Benutzer _jede_ berechtigt Anforderung. Der folgende Code definiert zwei Richtlinien:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] Siehe [Startup.cs]

Dieser Code wird auch das Authentifizierungsschema, wodurch ASP.NET die Middleware Authentifizierung ausgeführt werden soll, wenn die Autorisierung fehlschlägt. In diesem Fall geben wir Middleware Authentifizierung Cookie, da Cookie Authentifizierung Middleware "Forbidden" Seite den Benutzer umgeleitet werden kann. Der Speicherort der Seite verboten wird in der AccessDeniedPath-Option für Middleware Cookie festgelegt. Siehe [Konfigurieren der Authentifizierung Middleware].

### <a name="authorize-controller-actions"></a>Autorisieren von Controller-Aktionen

Legen Sie abschließend zum Autorisieren einer Aktion in einer MVC-Controller die Richtlinie der `Authorize` Attribut:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

In früheren Versionen von ASP.NET würde **Rollen** -Eigenschaft des Attributs festlegen:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

In ASP.NET Core 1.0 weiterhin unterstützt, aber es hat einige Nachteile gegenüber Autorisierungsrichtlinien:

-   Es wird ein Anspruch angenommen. Richtlinien können für jeden Antrag überprüfen. Rollen sind nur ein Anspruch.
-   Der Rollenname ist hartcodiert Attributs. Richtlinien ist der Autorisierungslogik an einem Ort erleichtert die Aktualisierung oder sogar Laden von Konfigurationen.
-   Richtlinien können komplexere autorisierungsentscheidungen (z. B. age > = 21), können nicht durch einfache Rollenmitgliedschaft ausgedrückt.

## <a name="resource-based-authorization"></a>Ressource basierte Autorisierung

_Ressourcen-basiertes Autorisierung_ tritt auf, wenn die Autorisierung für eine bestimmte Ressource abhängt, die von einer Operation betroffen sind. In der Anwendung Tailspin Umfragen hat Umfrage Besitzer und NULL zu viele Mitwirkende

-   Der Besitzer kann lesen, aktualisieren, löschen, veröffentlichen und Veröffentlichung die Umfrage.
-   Besitzer können Contributors an der Umfrage teilnehmen.
-   Mitwirkende lesen und aktualisieren die Umfrage.

Beachten Sie, dass "Owner" und "Teilnehmer" nicht Anwendungsrollen; Sie werden pro Umfrage in der Datenbank gespeichert. Überprüfen, ob ein Benutzer eine kann prüft beispielsweise die Anwendung der Benutzer Besitzer für diese Umfrage ist.

Implementieren Sie in ASP.NET Core 1.0 Ressource Autorisierung durch Ableiten von **AuthorizationHandler** und überschreiben die Methode **behandeln** .

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Beachten Sie, dass diese Klasse Survey-Objekte stark typisiert ist.  Registrieren Sie die Klasse für DI beim Start:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Verwenden Sie zum Durchführen der Autorisierung überprüft **IAuthorizationService** -Schnittstelle, die in Ihre einfügen können. Der folgende Code überprüft, ob ein Benutzer eine Umfrage lesen kann:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Da wir übergeben einen `Survey` Objekt dieser Aufrufen der `SurveyAuthorizationHandler`.

In der Autorisierungscode ist ein guter Ansatz zum Aggregieren aller Rollen und ressourcenbasierte Berechtigungen des Benutzers und Kontrollkästchen das Aggregat für den gewünschten Vorgang festgelegt.
Hier ist ein Beispiel von app Umfragen. Die Anwendung definiert verschiedene Berechtigungstypen:

- Admin
- Teilnehmer
- Ersteller
- Besitzer
- Reader

Die Anwendung definiert außerdem eine Reihe von möglichen Vorgängen auf Umfragen:

- Erstellen
- Lesen
- Update
- Löschen
- Veröffentlichen
- Unpublsh

Der folgende Code erstellt eine Liste der Berechtigungen für einen bestimmten Benutzer und Umfrage. Beachten Sie, dass diesen auf Benutzerrollen app und Besitzer-Teilnehmer Felder in der Umfrage Code.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] [SurveyAuthorizationHandler.cs]anzeigen

In einer Multi-Tenant-Anwendung müssen Berechtigungen "des Mieters Daten Auslaufen nicht". In Umfragen app darf die Berechtigung Teilnehmer über Mieter &mdash; können Sie jemand anderen Mandanten als eine Contriubutor zuweisen. Andere Berechtigungstypen sind auf Ressourcen des Benutzers Mieter gehören. Zur Durchsetzung dieser Anforderung werden die Mandanten-ID vor dem Gewähren der Berechtigung überprüft. (Die `TenantId` Feld zugewiesen, wenn die Umfrage erstellt wird.)

Als Nächstes werden Vorgang (Read, Update, Delete usw.) anhand der Berechtigungen überprüfen. Umfragen app implementiert dadurch mithilfe einer Nachschlagetabelle Funktionen:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Nächste Schritte

- Im nächsten Artikel dieser Reihe zu lesen: [Sichern von einem Back-End-web-API in einer mehrinstanzenfähigen Anwendung][web-api]
- Über Ressource basierte Autorisierung in ASP.NET 1.0 Core finden Sie unter [Resource Autorisierung][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[Teil einer Serie]: guidance-multitenant-identity.md
[Anwendungsrollen]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[referenzimplementierung]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[Konfigurieren die Authentifizierung middleware]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[Beispiel]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
