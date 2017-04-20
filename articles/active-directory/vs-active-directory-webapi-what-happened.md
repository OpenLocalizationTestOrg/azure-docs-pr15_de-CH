<properties
    pageTitle="Wo meine WebApi-Projekt (Visual Studio Azure Active Directory verbunden Service) | Microsoft Azure "
    description="Beschreibt die MVC-Projekt WebApi die Verbindung mit Visual Studio Azure AD"
  services="active-directory"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Wo meine WebApi-Projekt (Visual Studio Azure Active Directory verbunden Service)

> [AZURE.SELECTOR]
> - [Erste Schritte](vs-active-directory-webapi-getting-started.md)
> - [Was ist passiert](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Verweise wurden hinzugefügt

###<a name="nuget-package-references"></a>NuGet-Paket-Verweise

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>Auf

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Ändern von Code

###<a name="code-files-were-added-to-your-project"></a>Dateien wurden zum Projekt hinzugefügt.

Eine Startklasse Authentifizierung **App_Start/Startup.Auth.cs** Startlogik für Azure AD-Authentifizierung mit Projekt hinzugefügt wurde.

###<a name="startup-code-was-added-to-your-project"></a>Startcode wurde dem Projekt hinzugefügt.

Wenn Sie bereits eine Startklasse im Projekt, **die Methode** wurde aktualisiert einen Aufruf von `ConfigureAuth(app)`. Andernfalls wurde eine Startklasse zum Projekt hinzugefügt.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Die Datei app.config oder web.config hat neue Werte.

Die folgenden Konfigurationseinträge wurden hinzugefügt.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Azure AD App erstellt wurde

Azure AD-Anwendung wurde in dem Verzeichnis erstellt, die Sie im Assistenten ausgewählt.

[Weitere Informationen zu Active Directory Azure](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Wenn ich *einzelne Benutzerkonten Authentifizierung deaktivieren*aktiviert, wurden zusätzliche Mein Projekt Änderungen?
NuGet-Paketverweise wurden entfernt und Dateien entfernt und gesichert wurden. Je nach Zustand des Projekts müssen Sie manuell entfernen Sie weitere Referenzen oder Dateien oder Code entsprechend ändern.

###<a name="nuget-package-references-removed-for-those-present"></a>NuGet-Paket-Verweise entfernt (die)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Dateien gesichert und entfernt (die)

Jede der folgenden Dateien wurde gesichert und aus dem Projekt entfernt. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stamm Verzeichnis des Projekts.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Dateien gesichert (für die)

Jede der folgenden Dateien gesichert wurde, ersetzt. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stamm Verzeichnis des Projekts.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Wenn ich *Verzeichnisdaten lesen*aktiviert, wurden zusätzliche Mein Projekt Änderungen?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Zusätzliche Ihrer app.config oder web.config wurde geändert

Die folgenden zusätzlichen Konfigurationseinträge wurden hinzugefügt.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Ihre App Azure Active Directory aktualisiert wurde
Ihre App Azure Active Directory wurde aktualisiert, um die *Verzeichnisdaten lesen* Berechtigung und eine erstellt wurde, die dann als die *Ida: Kennwort* verwendet wurde das `web.config` Datei.

[Weitere Informationen zu Active Directory Azure](https://azure.microsoft.com/services/active-directory/)
