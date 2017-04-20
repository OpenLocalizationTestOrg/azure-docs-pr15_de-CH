<properties
    pageTitle="Arbeiten mit der Back-End-Server SDK für Mobile Apps | Azure App Service"
    description="Informationen Sie zum Arbeiten mit der Back-End-Server SDK für Azure App Service Mobile Apps."
    keywords="App Service Azure app Service mobile-app, mobile Service, Skalierung, skalierbare Bereitstellung Azure app Bereitstellung"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Arbeiten Sie mit der Back-End-Server SDK Azure Mobile Apps

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Dieses Thema zeigt wie die Backend-Server SDK in Azure App Service Mobile Apps Schlüsselszenarien. Azure Mobile Apps SDK können Sie mit Ihrer Anwendung ASP.NET mobile Clients arbeiten.

>[AZURE.TIP] [.NET Server SDK für Azure Mobile Apps] [ 2] open Source auf GitHub. Das Repository enthält alle gesamten Server SDK Unit Testsuite sowie einige Beispielprojekte Quellcode.

## <a name="reference-documentation"></a>Dokumentation

Die Referenzdokumentation für den Server SDK befindet sich hier: [Azure Mobile Apps.NET Reference][1].

## <a name="create-app"></a>Gewusst wie: .NET Mobile App-Back-End erstellen

Wenn Sie ein neues Projekt beginnen, können Sie eine App Service-Anwendung mithilfe der [Azure-Portal] oder Visual Studio erstellen. Die App Service-Anwendung lokal ausführen können oder veröffentlichen Sie das Projekt auf der Cloud-basierten mobilen App Service-Anwendung.  

Wenn Sie ein vorhandenes Projekt mobile Funktionen hinzufügen, finden Sie im Abschnitt [Download und Initialisieren des SDK](#install-sdk) .

### <a name="create-a-net-backend-using-the-azure-portal"></a>Erstellen Sie .NET Backend mit Azure-portal

Erstellen Sie eine mobile App Service-Backend folgen entweder das [Schnellstart-Lernprogramm] [ 3] oder gehen Sie folgendermaßen vor:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Wählen Sie im Blatt _Erste Schritte_ unter **Erstellen eines API** **C#** als **Back-End-Sprache**. Klicken Sie auf **Download**extrahiert die komprimierten Projektdateien auf den lokalen Computer, und öffnen Sie die Projektmappe in Visual Studio.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Erstellen Sie mit Visual Studio 2013 und 2015 Visual Studio .NET backend

Installieren Sie das [Azure SDK für .NET] [ 4] (Version 2.9.0 oder höher) ein Azure Mobile Apps-Projekt in Visual Studio erstellen. Nach der Installation des SDK erstellen Sie eine ASP.NET Anwendung folgendermaßen:

1. Das Dialogfeld **Neues Projekt** öffnen ( *Datei* > **neu** > **Projekt...**).
2. **Vorlagen** > **Visual C#**, und wählen Sie **Web**.
3. Wählen Sie **ASP.NET Web-Anwendung**.
4. Geben Sie den Namen des Projekts. Klicken Sie auf **OK**.
5. Unter _ASP.NET 4.5.2 Vorlagen_, **Azure Mobile-Anwendung**auswählen. Überprüfen Sie **Host in der Cloud** erstellen mobile Backend in der Cloud, dieses Projekt zu veröffentlichen.
6. Klicken Sie auf **OK**.

## <a name="install-sdk"></a>Gewusst wie: Laden und Initialisieren des SDK

Das SDK steht unter ["NuGet.org"]. Dieses Paket enthält die Basisfunktionen zum Einstieg mit dem SDK erforderlich. Um das SDK zu initialisieren, müssen Sie Aktionen für das Objekt **HttpConfiguration** .

### <a name="install-the-sdk"></a>Installieren Sie das SDK

Um das SDK zu installieren, mit der rechten Maustaste auf den Server-Projekt in Visual Studio, **NuGet-Pakete verwalten**, [Microsoft.Azure.Mobile.Server] Paket suchen, klicken Sie auf **Installieren**.

###<a name="server-project-setup"></a>Initialisieren Sie das Serverprojekt

.NET Backend-Serverprojekt wird mit einem owin-Startklasse ähnelt anderen Projekten ASP.NET initialisiert. Sicherstellen, dass Sie das NuGet-Paket verwiesen haben `Microsoft.Owin.Host.SystemWeb`. Um diese Klasse in Visual Studio hinzufügen, mit der rechten Maustaste auf das Server-Projekt, und wählen Sie **Hinzufügen** > 
**Neu**und **Web** > **Allgemeine** > **owin-Startklasse**.  Eine Klasse ist mit dem folgenden Attribut generiert:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

In der `Configuration()` der Klasse owin-Start-Methode ein **HttpConfiguration** Objekt Azure Mobile Apps-Umgebung konfigurieren.
Im folgende Beispiel initialisiert das Serverprojekt mit keine zusätzlichen Funktionen:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Um einzelne Funktionen müssen Sie vor dem Aufruf **auf**Erweiterungsmethoden für das **MobileAppConfiguration** -Objekt aufrufen. Der folgende Code fügt beispielsweise die Standardrouten alle API-Controller mit dem Attribut `[MobileAppController]` während der Initialisierung:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Server-Schnellstart von Azure-Portal ruft **UseDefaultConfiguration()**. Diese entspricht der folgenden Einrichtung:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

Erweiterungsmethoden verwendet werden:

* `AddMobileAppHomeController()`Stellt die Standardstartseite Azure Mobile Apps.
* `MapApiControllers()`Stellt benutzerdefinierte API-Funktionen für WebAPI-Controller mit ergänzt die `[MobileAppController]` Attribut.
* `AddTables()`Stellt eine Zuordnung von der `/tables` Endpunkte Tabelle Controller.
* `AddTablesWithEntityFramework()`ist eine kurze für die Zuordnung der `/tables` Endpunkte mit Entity Framework basierten Domänencontrollern.
* `AddPushNotifications()`bietet eine einfache Methode für Benachrichtigungshubs Geräte registrieren.
* `MapLegacyCrossDomainController()`Lokale Entwicklung bereit Standardheader CORS.

### <a name="sdk-extensions"></a>SDK-Erweiterung

Folgende Erweiterung NuGet-basierte Pakete bieten verschiedene mobile-Funktionen, die von der Anwendung verwendet werden können. Extensions während der Initialisierung zu aktivieren, verwenden das **MobileAppConfiguration** -Objekt.

- [Microsoft.Azure.Mobile.Server.Quickstart] unterstützt die grundlegende Einrichtung für Mobile Apps. Die Konfiguration hinzugefügt durch Aufrufen der **UseDefaultConfiguration** -Erweiterungsmethode Initialisierung. Diese Erweiterung enthält folgende Erweiterungen: Benachrichtigungen, Authentifizierung, Entität, Tabellen, Domänen- und Home-Pakete. Dieses Paket wird von Apps Mobile Schnellstart auf Azure-Portal verwendet.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   Standard implementiert *mobile app ist Seite* für den Stamm der Website. Durch Aufrufen der   **AddMobileAppHomeController** -Erweiterungsmethode zur Konfiguration hinzufügen.

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   enthält Klassen zum Arbeiten mit Daten und Datenpipeline einrichten. Durch Aufrufen der **AddTables** -Erweiterungsmethode zur Konfiguration hinzufügen.

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   das Entity Framework auf Daten in der SQL-Datenbank zugreifen können. Durch Aufrufen der **AddTablesWithEntityFramework** -Erweiterungsmethode zur Konfiguration hinzufügen.

- [Microsoft.Azure.Mobile.Server.Authentication] ermöglicht die Authentifizierung und einrichten owin-Middleware verwendet Token zu bestätigen. Hinzufügen der Konfiguration durch Aufrufen von **AddAppServiceAuthentication**  
   und **IAppBuilder**. Erweiterungsmethoden für **UseAppServiceAuthentication** .

- [Microsoft.Azure.Mobile.Server.Notifications] Pushbenachrichtigungen und einen Push registrierungsendpunkt definiert. Durch Aufrufen der **AddPushNotifications** -Erweiterungsmethode zur Konfiguration hinzufügen.

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   erstellt einen Controller, der Mobile-App Daten auf älteren Webbrowsern dient. Durch Aufrufen der   **MapLegacyCrossDomainController** -Erweiterungsmethode zur Konfiguration hinzufügen.

- [Microsoft.Azure.Mobile.Server.Login] enthält die AppServiceLoginHandler.CreateToken()-Methode eine statische Methode während der benutzerdefinierten Authentifizierung verwendet.   

## <a name="publish-server-project"></a>Gewusst wie: Server veröffentlichen

Dieser Abschnitt veranschaulicht die .NET Backend-Projekt von Visual Studio veröffentlicht. Sie können auch Back-End-Projekt mit Git oder in der [Bereitstellungsdokumentation Azure App Service](../app-service-web/web-sites-deploy.md)behandelt Methoden bereitstellen.

1. Erstellen Sie in Visual Studio das Projekt wiederherstellen NuGet-Pakete neu.

2. Im Projektmappen-Explorer mit der rechten Maustaste des Projekts, klicken Sie auf **Veröffentlichen**. Beim ersten veröffentlichen, müssen Sie ein Veröffentlichungsprofil definieren. Wenn Sie bereits ein Profil definiert haben, können Sie es auswählen und auf **Veröffentlichen**.

2. Publish-Ziel auswählen, klicken Sie anschließend auf **Microsoft Azure App Service** > **Weiter**und (falls erforderlich) mit der Azure-Anmeldeinformationen anmelden. 
   Visual Studio-downloads und sicheres Speichern der Einstellungen direkt von Azure veröffentlichen.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Wählen Sie Ihr **Abonnement**, wählen Sie **Ressourcen** **aus**, **Mobile-Anwendung**erweitern Sie und Backend Mobile-Anwendung, klicken auf **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Überprüfen Sie die Profilinformationen veröffentlichen und klicken Sie auf **Veröffentlichen**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Wenn Mobile App Backend erfolgreich veröffentlicht wurde, sehen Sie eine Erfolgsmeldung Zielseite.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Gewusst wie: definieren einen Tabelle Controller

Definieren Sie einen Controller Tabelle um eine SQL-Tabelle für mobile Clients verfügbar zu machen.  Konfigurieren einer Tabelle Controller sind drei Schritte erforderlich:

1. Erstellen Sie eine Klasse (Data Transfer Object, DTO).
2. Konfigurieren einer Mobile DbContext-Klasse.
3. Erstellen eines Tabelle Controllers.

Data Transfer-Objekt (DTO) ist ein einfaches C# Objekt erbt `EntityData`.  Zum Beispiel:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

Das DTO dient zur Definition der Tabelle in der SQL-Datenbank.  Fügen Sie zum Erstellen des Datenbankeintrags ein `DbSet<>` -Eigenschaft können Sie mit DbContext.  In die Standardprojektvorlage für Azure Mobile Apps der DbContext heißt `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Wenn Sie Azure SDK installiert haben, können Sie jetzt einen Vorlage Tabelle Controller wie folgt erstellen:

1. Mit der rechten Maustaste auf den Ordner, und wählen Sie **Hinzufügen** > **Controller...**.
2. Wählen Sie die Option **Azure Mobile Apps Tabelle Controller** und dann auf **Hinzufügen**.
3. Im Dialogfeld **Controller hinzufügen** :
    * Wählen Sie in der Dropdownliste **Modellklasse** neues DTO.
    * Wählen Sie in der Dropdownliste **DbContext** Mobile Service DbContext-Klasse.
    * Der Name des Domänencontrollers wird erstellt.
4. Klicken Sie auf **Hinzufügen**.

Schnellstart-Serverprojekt enthält ein Beispiel für eine einfache **TodoItemController**.

### <a name="how-to-adjust-the-table-paging-size"></a>Gewusst wie: Anpassen der Tabellengröße Paging

Standardmäßig gibt Azure Mobile Apps 50 Datensätze pro Anforderung.  Paging wird sichergestellt, dass der Client nicht ihre UI-Thread noch den Server zu lange bindet einen problemlosen Betrieb sicherzustellen. Ändern Sie Größe der Auslagerungsdatei erhöhen "allowed Abfragegröße" serverseitige und clientseitige Seitengröße der Serverseite "allowed Abfragegröße" wird angepasst, mit der `EnableQuery` Attribut:

    [EnableQuery(PageSize = 500)]

Sicherzustellen, dass die PageSize gleich oder größer als die vom Client angefordert.  Kundenspezifische HOWTO Dokumentation zum Ändern der Größe der Client-Seite finden Sie unter.

## <a name="how-to-define-a-custom-api-controller"></a>Gewusst wie: definieren einen benutzerdefinierten API-Controller

Benutzerdefinierte API Controller bietet die Grundfunktionalitäten auf Mobile App Backend durch einen Endpunkt verfügbar machen. Sie können einen Mobilgerät-spezifische API-Controller mit dem Attribut [MobileAppController] registrieren. Die `MobileAppController` Attribut registriert die Route Mobile Apps JSON-Serialisierungsprogramm richtet und [Client Version überprüfen](app-service-mobile-client-and-server-versioning.md)aktiviert.

1. In Visual Studio Maustaste auf den Ordner, und klicken Sie auf **Hinzufügen** > **Controller**, wählen **Web API 2-Controller&mdash;leere** und klicken Sie auf **Hinzufügen**.

2. Geben Sie einen **Controllernamen**wie `CustomController`, und klicken Sie auf **Hinzufügen**.

3. Die neue Klassendatei Controller fügen Sie folgende Anweisung verwenden:

        using Microsoft.Azure.Mobile.Server.Config;

4. Wenden Sie das Attribut **[MobileAppController]** der API Controller-Klassendefinition wie im folgenden Beispiel:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. App_Start/Startup.MobileApp.cs-Datei fügen Sie einen Aufruf der Erweiterungsmethode **MapApiControllers** wie im folgenden Beispiel:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Sie können auch die `UseDefaultConfiguration()` Erweiterungsmethode anstelle von `MapApiControllers()`. Alle Domänencontroller, die keine **MobileAppControllerAttribute** angewendet kann Clients darauf zugreifen, aber es kann nicht ordnungsgemäß verwendet werden von Clients über eine Mobile Anwendung Client SDK.

## <a name="how-to-work-with-authentication"></a>Gewusst wie: Arbeiten mit Authentifizierung

Azure Mobile Apps verwendet App-Authentifizierung / Autorisierung mobile Backend sichern.  Dieser Abschnitt veranschaulicht die folgenden Authentifizierung bezogenen Aufgaben im Projekt .NET Back-End-Server durchführen:

+ [Gewusst wie: Authentifizierung für ein Serverprojekt hinzufügen](#add-auth)
+ [Gewusst wie: für die Anwendung benutzerdefinierte Authentifizierung verwenden](#custom-auth)
+ [Gewusst wie: Abrufen authentifizierte Benutzerinformationen](#user-info)
+ [Gewusst wie: Zugriff für autorisierte Benutzer Daten](#authorize)

### <a name="add-auth"></a>Gewusst wie: Authentifizierung für ein Serverprojekt hinzufügen

Sie können das Serverprojekt Authentifizierung hinzufügen, durch das **MobileAppConfiguration** Objekt erweitern und Konfigurieren von owin-Middleware. Wenn das [Microsoft.Azure.Mobile.Server.Quickstart] -Paket und die Erweiterungsmethode **UseDefaultConfiguration aufrufen** können Sie Schritt 3 überspringen.

1. Installieren Sie in Visual Studio das [Microsoft.Azure.Mobile.Server.Authentication] -Paket.

2. Fügen Sie in der Projektdatei Startup.cs die folgende Codezeile am Anfang **der Methode** :

        app.UseAppServiceAuthentication(config);

    Diese owin-Middleware-Komponente überprüft zugeordneten App Service-Gateway ausgestellte Token.

3. Hinzufügen der `[Authorize]` -Controller oder Methode, die eine Authentifizierung erfordert Attribut. 

Informationen zum Authentifizieren von Clients auf Mobile Apps Backend finden Sie unter [Authentifizierung für Ihre Anwendung hinzufügen](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Gewusst wie: für die Anwendung benutzerdefinierte Authentifizierung verwenden

Wenn Sie nicht eines der App Service-Authentifizierung/Autorisierung verwenden möchten, können Sie eigene Login-System implementieren. Installieren Sie das Paket [Microsoft.Azure.Mobile.Server.Login] token mit Authentifizierung unterstützen.  Bieten Sie Ihren eigenen Code für Benutzeranmeldeinformationen überprüft. Beispielsweise können Sie mit gesalzene und gehashten Kennwörter in einer Datenbank überprüfen. Im folgenden Beispiel wird die `isValidAssertion()` -Methode (an anderer Stelle definiert) ist zuständig für diese Kontrollen.

Benutzerdefinierte Authentifizierung verfügbar gemacht werden, indem eine ApiController erstellen und `register` und `login` Aktionen. Der Client sollte Informationen vom Benutzer eine benutzerdefinierte Benutzeroberfläche verwenden.  Die Informationen wird dann an die API mit einem standardmäßigen HTTP POST versendet. Nachdem der Server die Assertion überprüft, wird ein Token ausgestellt mit den `AppServiceLoginHandler.CreateToken()` Methode.  ApiController **sollte nicht** mit der `[MobileAppController]` Attribut. 

Beispiel `login` Aktion:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

Im vorangehenden Beispiel sind LoginResult und LoginResultUser serialisierbare Objekte erforderliche Eigenschaften. Der Kunde erwartet Login Antworten als JSON-Objekte des Formulars zurückgegeben werden:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

Die `AppServiceLoginHandler.CreateToken()` -Methode _Publikum_ und einen _Aussteller_ -Parameter enthält. Beide Parameter werden auf die URL der Anwendungsstamm mit HTTPS-Schema festgelegt. Ebenso sollten Sie _SecretKey_ , der Wert Ihrer Anwendung Schlüssel Signieren des festlegen. Verteilen Sie Signaturschlüssel auf einem Client nicht, wie zu mint Schlüssel Benutzer imitieren verwendet werden kann. Signaturschlüssel während in App Service referenzieren Host erhalten die _WEBSITE\_AUTH\_SIGNIERUNG\_KEY_ -Umgebungsvariable. Wenn in einem lokalen Kontext Debuggen erforderlich, gehen Sie im Abschnitt [Authentifizierung Debuggen](#local-debug) den Schlüssel abrufen und als anwendungseinstellung einer.

Das ausgestellte Token kann auch andere Ansprüche und ein Ablaufdatum enthalten.  Das ausgestellte Token muss mindestens einen Anspruch Betreff (**Sub**) enthalten.

Den standard-Client unterstützt `loginAsync()` Methode durch Überladen der Authentifizierung Route.  Wenn der Client ruft `client.loginAsync('custom');` Anmeldung Ihre Route muss `/.auth/login/custom`.  Legen Sie die Route für die benutzerdefinierte Authentifizierung Controller mit `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Mit dem `loginAsync()` wird sichergestellt, dass das Authentifizierungstoken jeder nachfolgende Aufruf an den Dienst zugeordnet ist.

###<a name="user-info"></a>Gewusst wie: Abrufen authentifizierte Benutzerinformationen

Beim Authentifizieren eines Benutzers vom App-Dienst können Sie die zugewiesene Benutzer-ID und andere Informationen in .NET Back-End-Code zugreifen. Die Benutzerinformationen kann Autorisierung Entscheidungen im Backend verwendet werden. Der folgende Code erhält eine Anforderung zugeordnete Benutzer-ID:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

Die SID-spezifischen Benutzer-ID abgeleitet ist und für einen bestimmten Benutzer und Anmeldeanbieter ist.  Die SID ist null für ungültige Authentifizierungstokens.

App Service können Sie bestimmte Ansprüche von Login-Anbieter anfordern. Jeder Identitätsanbieter bieten weitere Informationen zum Verwenden des Identitätsanbieter SDK.  Beispielsweise können Sie die Facebook-Graph-API Freunde Informationen.  Ansprüche, die angefordert werden können Anbieter Blatt im Azure-Portal. Einige Ansprüche erfordern zusätzliche Konfigurationsschritte mit der Identitätsanbieter.

Der folgende Code Ruft die Erweiterungsmethode **GetAppServiceIdentityAsync** zu den Anmeldeinformationen des Zugriffstokens erforderlichen Abfragen Facebook Graph-API umfasst:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Fügen Sie eine using-Anweisung für `System.Security.Principal` **GetAppServiceIdentityAsync** -Erweiterungsmethode bereitstellen.

### <a name="authorize"></a>Gewusst wie: Zugriff für autorisierte Benutzer Daten

Im vorherigen Abschnitt wurde wie die Benutzer-ID des authentifizierten Benutzers abgerufen. Sie können Zugriff auf Daten und andere Ressourcen basierend auf diesem Wert. Beispielsweise ist Tabellen eine Benutzer-ID-Spalte hinzufügen und Filterung der Abfrageergebnisse nach der Benutzer-ID eine einfache Möglichkeit, die zurückgegebene Daten auf autorisierte Benutzer beschränken. Der folgende Code gibt Daten nur, wenn die SID mit dem Wert in der Spalte Benutzer-ID in der TodoItem-Tabelle übereinstimmt:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

Die `Query()` -Methode gibt ein `IQueryable` von LINQ mit Filtern bearbeitet werden können.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Wie: Hinzufügen von Push Benachrichtigungen Serverprojekt

Fügen Sie Push-Benachrichtigung auf das Serverprojekt **MobileAppConfiguration** Objekt und Erstellen einer Benachrichtigungshubs hinzu

1. In Visual Studio mit der rechten Maustaste des Serverprojekts und klicken Sie auf **NuGet-Pakete verwalten**, Suchen nach `Microsoft.Azure.Mobile.Server.Notifications`, klicken Sie auf **Installieren**. 

2. Wiederholen Sie diesen Schritt Installieren der `Microsoft.Azure.NotificationHubs` Paket umfasst die Clientbibliothek Notification Hubs.

3. In App_Start/Startup.MobileApp.cs und fügen Sie einen Aufruf der **AddPushNotifications()** -Erweiterungsmethode Initialisierung:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Fügen Sie den folgenden Code, der einen Benachrichtigungshubs Client erstellt:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Den Client Benachrichtigungshubs können nun um Pushbenachrichtigungen an registrierte Geräte zu senden. Weitere Informationen finden Sie unter [Pushbenachrichtigungen für Ihre Anwendung hinzufügen](app-service-mobile-ios-get-started-push.md). Weitere Informationen zu Notification Hubs finden Sie unter [Übersicht über Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

##<a name="tags"></a>Gewusst wie: Aktivieren gezielt Push mit Tags

Benachrichtigungshubs können Sie gezielt bestimmte Registrierung Benachrichtigungen mithilfe von Tags. Mehrere Tags werden automatisch erstellt:

* Die Installations-ID identifiziert ein bestimmtes Gerät.
* Anhand der SID authentifizierten Benutzer-Id identifiziert einen bestimmten Benutzer.

Die Installations-ID aus der Eigenschaft **InstallationId** **MobileServiceClient**möglich.  Das folgende Beispiel zeigt, wie Sie eine Installations-ID einer bestimmten Installation Notification Hubs einen Tag hinzugefügt:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Während der Registrierung der Push-Benachrichtigung vom Client bereitgestellte Tags werden beim Erstellen der Installations vom Backend ignoriert. Um einen Client Installation Tags hinzufügen aktivieren, müssen Sie eine benutzerdefinierte API erstellen, Tags mit dem vorherigen Muster hinzufügt. 

Siehe [Client hinzugefügt Push Notification Tags] [ 5] in der App Service Mobile Apps abgeschlossenen Schnellstart ein Beispiel für.

##<a name="push-user"></a>Gewusst wie: Senden von Pushbenachrichtigungen für einen authentifizierten Benutzer

Wenn ein authentifizierter Benutzer für Pushbenachrichtigungen registriert, ist eine Benutzer-ID-Tag die Registrierung automatisch hinzugefügt. Mithilfe dieses Tags können Sie Push-Benachrichtigung an alle Geräte registriert diese Person senden. Der folgende Code Ruft die SID des Benutzers und sendet eine Push-vorlagenbenachrichtigung jedes Gerät Registrierung für diese Person:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Anmeldung von einem authentifizierten Client Pushbenachrichtigungen, sicherzustellen Sie, dass die Authentifizierung abgeschlossen ist, bevor Sie versuchen, die Registrierung. Weitere Informationen finden Sie unter [Benutzer Push] [ 6] in App Service Mobile Apps abgeschlossenen Schnellstart-Beispiel für .NET Backend.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Gewusst wie: Debuggen und Problembehandlung bei .NET Server SDK

Azure App Service bietet verschiedene Debuggen und Problembehandlungsverfahren für ASP.NET Applications:

- [Überwachen von Azure App Service](../app-service-web/web-sites-monitor.md)
- [Diagnoseprotokoll in Azure App Service aktivieren](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Problembehandlung bei einer Azure App Service in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Protokollierung

Das standardmäßige ASP.NET Trace schreiben können, Diagnoseprotokolle App Service schreiben. Bevor Sie die Protokolle schreiben können, müssen Sie im Backend Mobile App Diagnose aktivieren.

Diagnose aktivieren und Protokolle geschrieben:

1. Führen Sie die Schritte [zum Aktivieren der Diagnose](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Fügen Sie die folgende Anweisung in der Datei verwenden:

        using System.Web.Http.Tracing;

3. Erstellen Sie einen Trace Writer vom Back-End .NET in die Diagnoseprotokolle wie folgt schreiben:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Veröffentlichen Sie das Serverprojekt und Zugriff auf die Mobile Anwendung Backend Codepfad mit der Protokollierung ausgeführt.

5. Download und Evaluierung der Protokolle unter [wie: Download Protokolle](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Lokale Authentifizierung Debuggen

Sie können die Anwendung lokal testen Änderungen vor der Veröffentlichung in der Cloud ausführen. Die meisten Azure Mobile Apps Backends drücken Sie *F5* in Visual Studio. Es gibt jedoch einige zusätzliche Aspekte bei Authentifizierung.

Benötigen Sie eine cloudbasierte app mit App Service Authentifizierung/Autorisierung konfiguriert und der Client müssen als Alternative Anmeldehost Cloud-Endpunkt. Dokumentation für bestimmte Schritte ausführen.

Sicherstellen Sie, dass Ihre mobile Back-End- [Microsoft.Azure.Mobile.Server.Authentication] installiert. Fügen Sie dann in der Anwendung owin-Startklasse im folgenden nach `MobileAppConfiguration` wurde der `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

Im vorhergehenden Beispiel konfigurieren Einstellungen der _AuthAudience_ und _AuthIssuer_ in der Datei Web.config jedem Sie die URL des Anwendungsstamms, mit HTTPS-Schema. Ebenso sollten Sie _AuthSigningKey_ , um der Wert der Anwendung Schlüssel Signieren des festlegen. Den Schlüssel zu erhalten:

1. Navigieren Sie zu Ihrer Anwendung in [Azure-portal] 
2. Klicken Sie auf **Extras**, **Kudu** **wechseln**.
3. Kudu Management-Website klicken Sie auf **Umgebung**.
4. Der Wert für _WEBSITE\_AUTH\_SIGNIERUNG\_Schlüssel_. 

Verwenden Sie für den Parameter _AuthSigningKey_ in der lokalen Anwendung Config Signaturschlüssel.  Mobile Backend verfügt jetzt Token lokal Ausführung überprüft der Client das Token vom Endpunkt cloudbasierten abgerufen.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure-portal]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

