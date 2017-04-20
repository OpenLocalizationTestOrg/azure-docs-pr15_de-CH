<properties
    pageTitle="Wo auf meinem MVC-Projekt (Visual Studio Azure Active Directory verbunden Service) | Microsoft Azure "
    description="Beschreibt, was dem MVC-Projekt, bei Azure AD Herstellen einer Verbindung mit Visual Studio verbunden services"
    services="active-directory"
    documentationCenter="na"
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

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Wo auf meinem MVC-Projekt (Visual Studio Azure Active Directory verbunden Service)?

> [AZURE.SELECTOR]
> - [Erste Schritte](vs-active-directory-dotnet-getting-started.md)
> - [Was ist passiert](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Verweise wurden hinzugefügt

### <a name="nuget-package-references"></a>NuGet-Paket-Verweise

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>Auf

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Code wurde hinzugefügt

### <a name="code-files-were-added-to-your-project"></a>Dateien wurden zum Projekt hinzugefügt.

Eine Startklasse Authentifizierung **App_Start/Startup.Auth.cs** Startlogik für Azure AD-Authentifizierung mit Projekt hinzugefügt wurde. Außerdem wurde eine Controllerklasse Controllers/AccountController.cs enthält Methoden **SignIn()** und **SignOut()** hinzugefügt. Schließlich eine Teilansicht, **Views/Shared/_LoginPartial.cshtml** , enthält einen Aktionslink für Anmeldung/Abmeldung hinzugefügt wurde.

### <a name="startup-code-was-added-to-your-project"></a>Startcode wurde dem Projekt hinzugefügt.

Wenn Sie bereits eine Startklasse im Projekt, wurde **die Methode** aktualisiert einen Aufruf von **ConfigureAuth(app)**. Andernfalls wurde eine Startklasse zum Projekt hinzugefügt.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>Ihrer app.config oder web.config hat neue Werte

Die folgenden Konfigurationseinträge wurden hinzugefügt.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Eine App Azure Active Directory (AD) wurde erstellt.
Azure AD-Anwendung wurde in dem Verzeichnis erstellt, die Sie im Assistenten ausgewählt.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Wenn ich *einzelne Benutzerkonten Authentifizierung deaktivieren*aktiviert, wurden zusätzliche Mein Projekt Änderungen?
NuGet-Paketverweise wurden entfernt und Dateien entfernt und gesichert wurden. Je nach Zustand des Projekts müssen Sie manuell entfernen Sie weitere Referenzen oder Dateien oder Code entsprechend ändern.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet-Paket-Verweise entfernt (die)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Dateien gesichert und entfernt (die)

Jede der folgenden Dateien wurde gesichert und aus dem Projekt entfernt. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stamm Verzeichnis des Projekts.

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Dateien gesichert (für die)

Jede der folgenden Dateien gesichert wurde, ersetzt. Sicherungsdateien befinden sich in einem Ordner "Backup" im Stamm Verzeichnis des Projekts.

- **Startup.cs**
- **App_Start\Startup.auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared ebenfalls einen\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Wenn ich *Verzeichnisdaten lesen*aktiviert, wurden zusätzliche Mein Projekt Änderungen?

Weitere Verweise wurden hinzugefügt.

###<a name="additional-nuget-package-references"></a>Weitere Verweise von NuGet-Paket

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>Zusätzliche auf

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Zusätzliche Dateien wurden dem Projekt hinzugefügt

Unterstützt das Zwischenspeichern von token wurden zwei Dateien hinzugefügt: **Models\ADALTokenCache.cs** und **Models\ApplicationDbContext.cs**.  Erläutern Sie den Zugriff auf Benutzerprofilinformationen Azure Grafik APIs wurden einen zusätzlichen Controller und Ansicht hinzugefügt.  Diese Dateien sind **Controllers\UserProfileController.cs** und **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Zusätzliche Startcode wurde dem Projekt hinzugefügt.

Ein neues **OpenIdConnectAuthenticationNotifications** -Objekt wurde in der Datei **startup.auth.cs** **Benachrichtigung** Mitglied der **OpenIdConnectAuthenticationOptions**hinzugefügt.  Dies ist das OAuth empfangen und exchange für ein Zugriffstoken.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Zusätzliche Ihrer app.config oder web.config wurde geändert

Die folgenden zusätzlichen Konfigurationseinträge wurden hinzugefügt.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Die folgenden Konfigurationsabschnitte und Verbindungszeichenfolge wurden hinzugefügt.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Ihre App Azure Active Directory aktualisiert wurde
Ihre App Azure Active Directory wurde aktualisiert, um die *Verzeichnisdaten lesen* Berechtigung und eine erstellt wurde, die dann als *Ida: ClientSecret* in der Datei **web.config** verwendet wurde.

[Weitere Informationen zu Active Directory Azure](https://azure.microsoft.com/services/active-directory/)
