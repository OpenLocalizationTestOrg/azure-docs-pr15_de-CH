<properties
    pageTitle="Aktualisierung von mobilen Diensten auf Azure App Service"
    description="Erfahren Sie, wie einfach die Anwendung Mobile Dienste auf einer App Service Mobile Anwendung aktualisieren"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Aktualisieren Sie Ihren vorhandenen .NET Azure Mobile Service App Service

Mobile App Service ist eine neue Möglichkeit, mit Microsoft Azure-Dienste erstellen. Weitere Informationen finden Sie unter [wie Mobile Apps werden?].

Aktualisieren eine vorhandene .NET Back-End-Anwendung von Azure Mobile Services eine neue App Service Mobile Apps beschrieben. Während diese Aktualisierung kann die bestehende Anwendung Mobile Dienste weiterhin.   Benötigen Sie eine Node.js Back-End-Anwendung aktualisieren, finden Sie [Ihre Node.js Mobile Dienste aktualisieren](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Beim Aktualisieren einer mobilen Backend auf Azure App Service hat Zugriff auf alle App-Funktionen und werden nach [Anwendung Preisen]nicht Mobile Dienste Preise berechnet.

##<a name="migrate-vs-upgrade"></a>Migration und Aktualisierung

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Es wird empfohlen, dass Sie [eine Migration durchführen](app-service-mobile-migrating-from-mobile-services.md) vor einer Aktualisierung. Auf diese Weise können Sie beide Versionen der Anwendung auf der gleichen App Service und ohne zusätzliche Kosten entstehen.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>Verbesserung der Mobile Apps .NET Server SDK

Aktualisieren auf die neue [Mobile Apps SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) bietet die folgenden Vorteile:

- Mehr Flexibilität bei der NuGet abhängig. Die Hostingumgebung stellt keine eigenen Versionen von NuGet-Paketen Verwendung alternative kompatible Versionen. Gibt neue kritische Fehlerbehebungen und Sicherheitsupdates Mobile Server SDK oder Abhängigkeiten, müssen Sie den Dienst jedoch manuell aktualisieren.

- Mehr Flexibilität im mobile SDK. Sie können explizit steuern, welche Funktionen und Routen werden eingerichtet, wie Authentifizierung, Tabelle APIs und Push registrierungsendpunkt. Weitere Informationen finden Sie unter [.NET Server SDK für Azure Mobile Apps verwenden](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Unterstützung für andere Projekttypen für ASP.NET und Routen. Sie können nun MVC und Web API-Controller im selben Projekt wie das mobile Back-End-Projekt aufnehmen.

- Unterstützung für neue App Service-Authentifizierungsfunktionen eine häufige Authentifizierung auf Ihrem Web und apps verwenden können.

##<a name="overview"></a>Upgrade-Grundlagen

In vielen Fällen werden aktualisieren einfach neue Mobile Apps Server SDK wechseln und Ihr Code auf eine neue Instanz der Mobile-Anwendung veröffentlichen. Es gibt jedoch einige Szenarien einige zusätzliche Konfiguration an wie erweiterte Authentifizierungsszenarien erfordern Aufträge geplant. Jede dieser wird in den nachfolgenden Abschnitten behandelt.

>[AZURE.TIP] Es wird empfohlen, lesen und im weiteren Verlauf dieses Themas vor einer Aktualisierung vollständig verstehen. Notieren Sie Features verwenden die unter aufgerufen werden.

Der Mobile Client SDKs sind **nicht** kompatibel mit dem neuen Mobile Apps Server SDK. Um die Kontinuität des Dienstes für Ihre Anwendung sollten Sie eine Website derzeit veröffentlichten Vermögensmanagement nicht ändert veröffentlichen. Stattdessen sollten Sie eine neue mobile Anwendung erstellen, die als Duplikat dient. Diese Anwendung stellen dieselbe App Service-Plan zu zusätzlichen Kosten anfallen.

Sie haben dann zwei Versionen der Anwendung: bleibt und dient apps, und andererseits die dann aktualisieren können und mit einer neuen Client-Version veröffentlicht. Verschieben und Testen Sie den Code in Ihrem Tempo, aber stellen Sie sicher, dass alle Fehlerkorrekturen soll, beide angewendet. Wenn Sie glauben, dass die gewünschte Anzahl von Client-apps in der Natur auf die neueste Version aktualisiert haben, die ursprüngliche migrierte Anwendung löschen Wunsch.

Die vollständige Übersicht für den Aktualisierungsvorgang lautet wie folgt:

1. Erstellen Sie eine neue Mobile-App
2. Aktualisieren Sie das Projekt der neuen Server-SDKs
3. Eine neue Version der Client-Anwendung
4. (Optional) Löschen der migrierten Originalinstanz

##<a name="mobile-app-version"></a>Erstellen eine zweite Anwendungsinstanz
Der erste Schritt bei der Aktualisierung ist Ressource Mobile-Anwendung erstellen, die die neue Version der Anwendung befindet. Wenn Sie einen vorhandenen mobilen Dienst bereits migriert haben, möchten Sie diese Version auf demselben Host Plan erstellen. [Azure-Portal] zu öffnen Sie, und navigieren Sie zu der migrierten Anwendung. Notieren Sie die App Service-Plan ausgeführt wird.

Anschließend erstellen Sie die zweite Anwendungsinstanz [.NET Backend-Erstellung Anweisungen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Aufforderung auswählen wählen Sie App Service-Plan oder "hosting Plan" Plan der migrierten Anwendung.

Sie möchten wahrscheinlich die Datenbank und Benachrichtigung wie Mobile Dienste verwendet. Kopieren Sie diese Werte von [Azure-Portal] öffnen und in der ursprünglichen Anwendung navigieren, dann **Klicken Sie** > **ApplicationSettings**. Kopieren Sie unter **Verbindungszeichenfolgen**, `MS_NotificationHubConnectionString` und `MS_TableConnectionString`. Navigieren Sie zum neuen Upgrade-Website, und fügen Sie sie in vorhandene Werte überschrieben. Wiederholen Sie diesen Vorgang für jede weitere Anwendung Ihre app muss. Nicht migrierten Dienst verwenden, können Sie Verbindungszeichenfolgen und app-Einstellungen auf der Registerkarte **Konfigurieren** des Abschnitts Mobile Dienste von [Azure-Verwaltungsportal]lesen.

Eine Kopie des Projekts ASP.NET für die Anwendung und Ihre neue Site veröffentlichen. Mit einer Kopie von der Clientanwendung mit der neuen URL aktualisiert, überprüfen Sie, dass alles erwartungsgemäß funktioniert.

## <a name="updating-the-server-project"></a>Aktualisierung von Project server

Mobile Apps stellt eine neue [Mobile App Server SDK] Großteil dieselben Funktionen wie die Laufzeit Mobile Dienste bereitstellt. Entfernen Sie zunächst alle Verweise auf Mobile Services-Pakete. Suchen Sie in NuGet-Paket-Manager `WindowsAzure.MobileServices.Backend`. Die meisten apps sehen mehrere Pakete, einschließlich `WindowsAzure.MobileServices.Backend.Tables` und `WindowsAzure.MobileServices.Backend.Entity`. In diesem Fall beginnen mit der niedrigsten die Abhängigkeitsstruktur wie `Entity`, und entfernen Sie sie. Wenn Sie aufgefordert werden, entfernen Sie alle abhängigen Pakete nicht. Wiederholen Sie diesen Vorgang, bis Sie entfernt `WindowsAzure.MobileServices.Backend` selbst.

Jetzt haben Sie ein Projekt, die keine Mobile Services SDKs verweist.

Als Nächstes fügen Sie Referenzen Mobile Apps SDK. Die meisten Entwickler sollten für diese Aktualisierung herunterladen und Installieren der `Microsoft.Azure.Mobile.Server.Quickstart` Paket, wie dies in der gesamten erforderlichen Menge ziehen.

Werden einige Unterschiede zwischen den SDKs durch Compilerfehler, aber diese leicht an und werden in diesem Abschnitt behandelt.

### <a name="base-configuration"></a>Basiskonfiguration

Dann in WebApiConfig.cs, Sie ersetzen können:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

mit

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Wenn neue .NET Server SDK und Features der App Software erfahren möchten, finden Sie unter [Verwendung die Server SDK] -Thema.

Stellt Ihre Anwendung Authentifizierungsfunktionen verwenden, müssen Sie auch eine owin-Middleware registrieren. In diesem Fall sollten Sie in einem neuen owin-Startklasse des obigen Codes verschieben.

1. Fügen Sie das NuGet-Paket `Microsoft.Owin.Host.SystemWeb` Wenn es nicht bereits im Projekt enthalten ist.
2. In Visual Studio, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** -> **Neu**. **Wählen Sie aus** -> **Allgemeine** -> **owin-Startklasse**.
3. Verschieben den oben aufgeführte Code für MobileAppConfiguration von `WebApiConfig.Register()` , die `Configuration()` -Methode der neuen Startklasse.

Stellen Sie sicher, dass die `Configuration()` -Methode endet mit:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Gibt zusätzliche zur Authentifizierung die vollständige Authentifizierung Abschnitt behandelt werden.

### <a name="working-with-data"></a>Arbeiten mit Daten

In Mobile Dienste war mobile Anwendungsname den Schemanamen im Entity Framework-Setup.

Sicherstellen, dass Sie das gleiche Schema wie vor mit folgenden Schemaset in DbContext für Ihre Anwendung verwiesen:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Stellen Sie sicher MS_MobileServiceName Einstellung oben. Auch können ein anderes Schemanamens Sie, wenn Ihre Anwendung bereits angepasst.

### <a name="system-properties"></a>Systemeigenschaften

#### <a name="naming"></a>Benennen

In Azure Mobile Services Server SDK enthalten Systemeigenschaften immer einen doppelten Unterstrich (`__`) Präfix für Eigenschaften:

- __createdAt
- __updatedAt
- __deleted
- __version

Der Mobile Client SDKs haben speziellen Logik zum Analysieren von Systemeigenschaften in diesem Format.

Systemeigenschaften nicht in Azure Mobile Apps haben ein spezielles Format und die folgenden Namen:

- createdAt
- updatedAt
- gelöscht
- Version

Apps Mobile Client SDKs verwenden das neue System Eigenschaften keine Client-Code erforderlich sind. Aber wenn Sie direkt REST zu Ihrem Dienst aufrufen sollten dann Sie Abfragen entsprechend ändern.

#### <a name="local-store"></a>Lokalen Speicher

Ändert den Namen der Systemeigenschaften bedeutet, dass eine lokale Datenbank offline synchronisieren für Mobile Dienste nicht mit Mobile Apps kompatibel ist. Wenn möglich sollten Sie aktualisieren Clientcomputer apps Mobile Dienste Mobile Apps erst nach ausstehenden Änderungen an den Server gesendet wurden. Anschließend sollten die aktualisierte Anwendung einen neuen Datenbankdateinamen verwenden.

Wenn Clientanwendung von Mobile Dienste für Mobile Apps Aktualisierung während offline Änderungen in der anstehen muss die Datenbank aktualisiert werden, um die neuen Spaltennamen verwenden. IOS kann dies durch einfache Migrationen Spaltennamen ändern. Android und .NET verwalteten Client sollten Sie benutzerdefinierte SQL zum Umbenennen der Spalten für Ihre Datentabellen Objekt schreiben.

Ändern Sie auf iOS Ihre wichtigsten Daten Schema für Datenentitäten folgt an. Beachten Sie, dass die Eigenschaften `createdAt`, `updatedAt` und `version` kein `ms_` Präfix:

| Attribut |  Typ   | Hinweis                                                 |
|---------- |  ------ | -----------------------------------------------------|
| ID        | Zeichenfolge, die als erforderlich gekennzeichnet  | Primärschlüssel in remote-Speicher         |
| createdAt | Datum    | (optional) wird CreatedAt System-Eigenschaft         |
| updatedAt | Datum    | (optional) wird UpdatedAt System-Eigenschaft         |
| Version   | Zeichenfolge  | zu Konflikten, wird Version verwendet (optional) |

#### <a name="querying-system-properties"></a>Abfragen von Systemeigenschaften

In Azure Mobile Services Systemeigenschaften werden nicht gesendet, jedoch nur bei Anforderung mit der Abfragezeichenfolge `__systemProperties`. Im Gegensatz dazu sind in Azure Mobile Apps System Eigenschaften **immer aktiviert** sind Teil des Objektmodells Server SDK.

Diese Änderung wirkt sich hauptsächlich auf benutzerdefinierte Implementierung Domänenmanager wie Erweiterung `MappedEntityDomainManager`. In Mobile Dienste verlangt ein Client nie alle Systemeigenschaften kann mit einem `MappedEntityDomainManager` , die alle Eigenschaften nicht tatsächlich zugeordnet. Jedoch in Azure Mobile Apps verursacht diese nicht zugeordneten Eigenschaften einen Fehler in Abfragen abrufen.

Die einfachste Möglichkeit zur Lösung des Problems ist die DTOs ändern, sodass sie Vererben `ITableData` anstelle von `EntityData`. Fügen Sie dann die `[NotMapped]` -Attribut auf die Felder, die ausgelassen werden soll.

Definiert z. B. die folgenden `TodoItem` keine System-Eigenschaften:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Hinweis: Wenn Sie Fehler in `NotMapped`, fügen Sie einen Verweis auf die Assembly `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Mobile Dienste enthalten Unterstützung für CORS durch ASP.NET CORS Lösung einschließen. Dieser Umbruch Ebene wurde entfernt, um Entwickler mehr Kontrolle geben, sodass Sie [ASP.NET CORS Unterstützung](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)direkt nutzen können.

Sind die wichtigsten Bereiche verwenden CORS, die `eTag` und `Location` Kopfzeilen dürfen damit Client SDKs ordnungsgemäß funktioniert.

### <a name="push-notifications"></a>Push-Benachrichtigung
Für Push ist Hauptsache finden Sie möglicherweise fehlende Server SDK die PushRegistrationHandler-Klasse. Registrierung werden Mobile Apps anders behandelt, und der Registrierung sind standardmäßig aktiviert. Verwalten von Tags kann mit benutzerdefinierten APIs erfolgen. Finden Sie weitere Hinweise [für Tags registrieren](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) .

### <a name="scheduled-jobs"></a>Geplante Aufträge
Geplante Aufträge werden nicht in Mobile Apps erstellt, müssen alle Arbeitsplätze, die im .NET Backend einzeln aktualisiert werden. Eine Option ist die Erstellung eines geplanten [Webauftrag] auf der Mobile-Anwendung Code-Website. Sie können auch einen Controller, der Ihr Projekt Code enthält einrichten und konfigurieren [Azure Scheduler] diesen Endpunkt erwartet Zeitplan treffen.

### <a name="miscellaneous-changes"></a>Sonstige Änderungen
Alle ApiControllers, die von mobilen Clients verwendet werden müssen jetzt die `[MobileAppController]` Attribut. Dies ist nicht mehr standardmäßig so, dass andere ApiControllers zu mobilen Formatierungsprogramme unberührt.

Die `ApiServices` Objekt ist nicht mehr Teil des SDK. Um Mobile App Einstellungen, können Sie Folgendes verwenden:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Entsprechend erfolgt die Protokollierung verwenden des standardmäßigen ASP.NET Trace jetzt:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>Aspekte der Authentifizierung

Die Authentifizierungskomponenten von Mobile Dienste wurden jetzt in App Service Authentifizierung/Autorisierung Funktion verschoben. Sie können dies für Ihre Website aktivieren, indem Thema [Hinzufügen Authentifizierung der mobilen Anwendung](app-service-mobile-ios-get-started-users.md) kennen.

Bei einigen Anbietern wie AAD, Facebook und Google sollte Sie nutzen die Eintragung in der Anwendung kopieren. Sie müssen lediglich der Identitätsanbieter Portal navigieren und die Registrierung eine neue URL-Umleitung hinzufügen. Die Client-ID und Schlüssel konfigurieren Sie App Service Authentifizierung/Autorisierung.

### <a name="controller-action-authorization"></a>Controller-Aktion Autorisierung
Alle Instanzen der `[AuthorizeLevel(AuthorizationLevel.User)]` Attribut muss jetzt so geändert werden standardmäßige ASP.NET `[Authorize]` Attribut. Außerdem sind Domänencontroller jetzt anonym, wie andere ASP.NET Applications.
Wenn, Admin oder Anwendung AuthorizeLevel Optionen verwendet, beachten Sie, dass nicht mehr vorhanden sind. Stattdessen können Sie AuthorizationFilters für gemeinsame geheime Schlüssel einrichten oder Konfigurieren einer AAD Dienstprinzipalnamen-Dienst Aufrufe sicher aktivieren.

### <a name="getting-additional-user-information"></a>Weitere Informationen

Sie erhalten weitere Informationen, einschließlich Zugriffstoken durch die `GetAppServiceIdentityAsync()` Methode:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Außerdem nimmt die Anwendung Abhängigkeiten Benutzer-IDs, z. B. in einer Datenbank speichern, ist es wichtig zu beachten, dass die Benutzer-IDs zwischen Mobile Dienste und App Service Mobile Apps unterscheiden. Noch erhalten der Mitgliedsname Mobile Dienste jedoch. Alle Unterklassen ProviderCredentials haben eine Benutzer-ID-Eigenschaft. So fortsetzen aus dem Beispiel vor:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Wenn Ihre Anwendung Abhängigkeiten auf Benutzer-IDs verwendet werden, ist wichtig, dass die Registrierung möglichst mit Identitätsprovider nutzen. Benutzer-IDs sind normalerweise beschränkt auf die Registrierung der Anwendung, die verwendet wurde, Einführung einer neuen Registrierung Probleme mit übereinstimmenden Benutzer ihre Daten erstellen kann.

### <a name="custom-authentication"></a>Benutzerdefinierte Authentifizierung

Wenn Ihre Anwendung eine benutzerdefinierte authentifizierungslösung verwenden, sollten Sie sicherstellen, dass die aktualisierte Website auf das System zugreifen. Anweisungen Sie die neue benutzerdefinierte Authentifizierung in der [Übersicht über .NET Server SDK] die Lösung. Beachten Sie, dass die benutzerdefinierte Authentifizierungskomponenten weiterhin in der Vorschau.

##<a name="updating-clients"></a>Aktualisieren von clients
Haben Sie eine betriebliche Mobile App Back-End-können auf eine neue Version der Client-Anwendung arbeiten Sie, die verbraucht. Mobile Apps auch eine neue Version des Client-SDKs und Serverupgrade über ähnlich, Sie müssen alle Verweise auf Mobile Services SDKs entfernen, bevor Sie Mobile Apps-Versionen installieren.

Eine der wichtigsten Änderungen zwischen den Versionen ist, dass die Konstruktoren nicht mehr eine Anwendungstaste. Sie übergeben jetzt einfach die URL der Mobile-Anwendung. Auf den Clients .NET das `MobileServiceClient` Konstruktor ist:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Sie erhalten Informationen über neue SDKs installieren und verwenden die neue Struktur über die folgenden Links:

- [iOS-Version 3.0.0 oder höher](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) Version 2.0.0 oder höher](app-service-mobile-dotnet-how-to-use-client-library.md)

Wenn Ihre Anwendung Pushbenachrichtigungen verwenden, notieren Sie die spezielle Erfassung Anweisungen für jede Plattform wie einige ändert sich ebenfalls.

Wenn die neue Clientversion bereit sind, probieren Sie es gegen Ihre aktualisierten Server. Nach Überprüfung, dass es funktioniert, können Sie eine neue Version der Anwendung für Kunden freigeben. Wenn Ihre Kunden Gelegenheit, diese Updates erhalten haben, können Sie schließlich Mobile Services Version Ihrer Anwendung löschen. An diesem Punkt haben Sie App Service Mobile App mit neuesten Mobile Apps Server SDK vollständig aktualisiert.

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[Azure-Verwaltungsportal]: https://manage.windowsazure.com/
[Was sind Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Webauftrag]: ../app-service-web/websites-webjobs-resources.md
[Wie die Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[App Servicepreise]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Übersicht über .NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
