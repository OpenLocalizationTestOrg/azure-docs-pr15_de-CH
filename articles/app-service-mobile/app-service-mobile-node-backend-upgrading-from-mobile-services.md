<properties
    pageTitle="Aktualisieren von mobilen Diensten Azure App Service - Node.js"
    description="Erfahren Sie, wie einfach die Anwendung Mobile Dienste auf einer App Service Mobile Anwendung aktualisieren"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Aktualisieren Sie Ihren vorhandenen Node.js Azure Mobile Service App Service

Mobile App Service ist eine neue Möglichkeit, mit Microsoft Azure-Dienste erstellen. Weitere Informationen finden Sie unter [wie Mobile Apps werden?].

Aktualisieren eine vorhandene Node.js Back-End-Anwendung von Azure Mobile Services eine neue App Service Mobile Apps beschrieben. Während diese Aktualisierung kann die bestehende Anwendung Mobile Dienste weiterhin.  Benötigen Sie eine Node.js Back-End-Anwendung aktualisieren, finden Sie in [der .NET Mobile Dienste aktualisieren](./app-service-mobile-net-upgrading-from-mobile-services.md).

Beim Aktualisieren einer mobilen Backend auf Azure App Service hat Zugriff auf alle App-Funktionen und werden nach [Anwendung Preisen]nicht Mobile Dienste Preise berechnet.

## <a name="migrate-vs-upgrade"></a>Migration und Aktualisierung

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Es wird empfohlen, dass Sie [eine Migration durchführen](app-service-mobile-migrating-from-mobile-services.md) vor einer Aktualisierung. Auf diese Weise können Sie beide Versionen der Anwendung auf der gleichen App Service und ohne zusätzliche Kosten entstehen.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Verbesserung der Mobile Apps Node.js Server SDK

Aktualisieren auf die neue [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) bietet viel verbessert:

- [Express Framework](http://expressjs.com/en/index.html), das neue Knoten SDK ist leicht und zu neuen Knoten Versionen wie sie kommen entwickelt. Sie können das Verhalten der Anwendung mit Express-Middleware.

- Beträchtliche Leistungssteigerungen im Vergleich zu Mobile Services-SDK.

- Sie können nun eine Website mit mobilen Backend hosten. Ebenso ist es Azure Mobile SDK zu einer vorhandenen Anwendung express.v4 hinzufügen.

- Erstellt für plattformübergreifende und lokalen können Mobile Apps SDK entwickelt und lokal unter Windows, Linux und OSX ausgeführt werden. Es ist benutzerfreundlich Knoten Entwicklungstechniken wie [Mocha](https://mochajs.org/) Tests vor der Bereitstellung.

## <a name="overview"></a>Upgrade-Grundlagen

Um Node.js Backend aktualisieren, hat Azure App Service Compatibility-Paket bereitgestellt.  Nach der Aktualisierung müssen Sie Niew-Website, die auf einer neuen Website App Service bereitgestellt werden können.

Der Mobile Client SDKs sind **nicht** kompatibel mit dem neuen Mobile Apps Server SDK. Um die Kontinuität des Dienstes für Ihre Anwendung sollten Sie eine Website derzeit veröffentlichten Vermögensmanagement nicht ändert veröffentlichen. Stattdessen sollten Sie eine neue mobile Anwendung erstellen, die als Duplikat dient. Diese Anwendung stellen dieselbe App Service-Plan zu zusätzlichen Kosten anfallen.

Sie haben dann zwei Versionen der Anwendung: bleibt und dient apps, und andererseits die dann aktualisieren können und mit einer neuen Client-Version veröffentlicht. Verschieben und Testen Sie den Code in Ihrem Tempo, aber stellen Sie sicher, dass alle Fehlerkorrekturen soll, beide angewendet. Wenn Sie glauben, dass die gewünschte Anzahl von Client-apps in der Natur auf die neueste Version aktualisiert haben, die ursprüngliche migrierte Anwendung löschen Wunsch. Es entstehen keine zusätzlichen finanziellen Kosten, wenn dieselbe App Service-Plan als Mobile-Anwendung gehostet.

Die vollständige Übersicht für den Aktualisierungsvorgang lautet wie folgt:

1. Die vorhandenen (migrierte) Azure Mobile Service herunterladen
2. Konvertieren Sie das Projekt mit der Kompatibilität Azure Mobile App.
3. Korrigieren Sie alle Unterschiede (wie z. B. Authentifizierung).
4. Bereitstellen Sie das konvertierte Azure Mobile-App-Projekt einen neuen App-Dienst.
4. Eine neue Version von der Clientanwendung, die neue Mobile Anwendung.
5. (Optional) Löschen der ursprünglichen migrierten Mobilfunkanbieter app.

Löschen kann auftreten, wenn Sie Datenverkehr auf Ihre ursprünglichen migrierten Mobilfunkanbieter nicht.

## <a name="install-npm-package"></a>Installieren der erforderlichen Komponenten

Sie sollten [Knoten] auf dem lokalen Computer installieren.  Sie sollten auch Compatibility-Paket installieren.  Nach Knoten installiert ist, führen Sie den folgenden Befehl von einer neuen Cmd oder PowerShell Prompt:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Azure Mobile Dienste Skripts erhalten

- [Azure-Portal]anmelden.
- Verwenden Sie **Alle Ressourcen** oder **Anwendungsdienste**, die Mobile Website finden.
- In der Website, klicken Sie auf **Extras** -> **Kudu** -> Kudu-Site öffnen**gehen** .
- Klicken Sie auf **Debug-Konsole** -> **PowerShell** Debug-Konsole öffnen.
- Navigieren Sie zu `site/wwwroot/App_Data/config` wiederum auf jedes Verzeichnis
- Klicken Sie auf das Download-Symbol neben der `scripts` Verzeichnis.

Dadurch wird die Skripts im ZIP-Format herunterladen.  Erstellen Sie ein neues Verzeichnis auf dem lokalen Computer und Entpacken die `scripts.ZIP` Datei im Verzeichnis.  Dadurch entsteht ein `scripts` Verzeichnis.

## <a name="scaffold-app"></a>Neue Azure Mobile Apps Back-End-Gerüst

Führen Sie den folgenden Befehl aus dem Verzeichnis das Skriptverzeichnis:

```scaffold-mobile-app scripts out```

Dadurch entsteht eine erstellten Azure Mobile Apps Back-End der `out` Verzeichnis.  Zwar nicht erforderlich, es empfiehlt sich, überprüfen Sie die `out` Verzeichnis in einem Quellcoderepository Ihrer Wahl.

## <a name="deploy-ama-app"></a>Bereitstellen von Azure Mobile Apps backend

Während der Bereitstellung müssen Sie Folgendes tun:

1. Erstellen Sie neue Mobile App im [Azure-Portal].
2. Führen Sie die `createViews.sql` Skript in der verbundenen Datenbank.
3. Verknüpfen Sie die Datenbank, die mit Ihrem Mobile Service zum neuen App Service verknüpft ist.
4. Verknüpfen Sie andere Ressourcen (z. B. Benachrichtigungshubs) mit der neuen App Service.
5. Bereitstellen Sie den generierten Code auf Ihre neue Site.

### <a name="create-a-new-mobile-app"></a>Erstellen Sie eine neue Mobile-App

1. [Azure-Portal]anmelden.

2. Klicken Sie auf **+ neu** > **Web + Mobile** > **Mobile-Anwendung**, geben Sie einen Namen für Ihre Mobile App-Backend.

3. Wählen Sie für die **Ressourcengruppe**eine vorhandene Ressourcengruppe aus oder erstellen Sie ein neues (mit demselben Namen als app.) 
 
    Sie wählen einen anderen App Service-Plan oder eine neue erstellen. Weitere Informationen zu App Services Pläne und erstellen ein neues Plans in unterschiedliche Preise tier und in die gewünschte Position [Azure App Service-Pläne ausführliche Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. Für **App-Serviceplan**ist Standardpläne (im [Standard-Tier](https://azure.microsoft.com/pricing/details/app-service/)) ausgewählt. Sie können auch einen anderen Plan oder [einen neuen erstellen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)auswählen. Die App Service-Plan Bestimmen der [Funktionen Kosten und compute-Ressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihrer Anwendung. 

    Nachdem Sie den Plan entscheiden, klicken Sie auf **Erstellen**. Backend Mobile-Anwendung wird erstellt. 


### <a name="run-createviewssql"></a>CreateViews.SQL ausführen

Die erstellten Anwendung enthält eine Datei namens `createViews.sql`.  Dieses Skript muss der Zieldatenbank ausgeführt werden.  Die Verbindungszeichenfolge für die Zieldatenbank die migrierten Mobilfunkanbieter aus dem Blade **Einstellungen** unter **Verbindungszeichenfolgen**entnehmen.  Es heißt `MS_TableConnectionString`.

Sie können dieses Skript in SQL Server Management Studio oder Visual Studio ausführen.

### <a name="link-the-database-to-your-app-service"></a>Verknüpfen Sie die Datenbank mit dem App-Dienst

Verknüpfen Sie die vorhandene Datenbank mit App Service:

- Öffnen Sie in [Azure-Portal]App Service.
- Wählen Sie **Alle** -> **Data Connections**.
- Klicken Sie auf **+ Hinzufügen**.
- Wählen Sie in der Dropdownliste **SQL-Datenbank**
- Wählen Sie unter **SQL-Datenbank**die vorhandene Datenbank aus und dann auf **auswählen**.
- Geben Sie unter **Verbindungszeichenfolge**Benutzernamen und Kennwort für die Datenbank ein und dann auf **OK**.
- Blade **Datenquellen hinzufügen** klicken Sie auf **OK**.

Benutzername und Kennwort finden Sie die Verbindungszeichenfolge für die Zieldatenbank in der migrierten Mobile Service anzeigen.


### <a name="set-up-authentication"></a>Authentifizierung

Azure Mobile Apps können Sie Azure Active Directory, Facebook, Google, Microsoft und Twitter-Authentifizierung innerhalb des Dienstes konfigurieren.  Benutzerdefinierter Authentifizierung müssen separat entwickelt werden.  Siehe [Authentifizierung] Dokumentation und [Authentifizierung Quickstart] -Dokumentation weitere Informationen.  

## <a name="updating-clients"></a>Mobile Clients aktualisieren

Haben Sie eine betriebliche Mobile App Back-End-können auf eine neue Version der Client-Anwendung arbeiten Sie, die verbraucht. Mobile Apps auch eine neue Version des Client-SDKs und Serverupgrade über ähnlich, Sie müssen alle Verweise auf Mobile Services SDKs entfernen, bevor Sie Mobile Apps-Versionen installieren.

Eine der wichtigsten Änderungen zwischen den Versionen ist, dass die Konstruktoren nicht mehr eine Anwendungstaste. Sie übergeben jetzt einfach die URL der Mobile-Anwendung. Auf den Clients .NET das `MobileServiceClient` Konstruktor ist:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Sie erhalten Informationen über neue SDKs installieren und verwenden die neue Struktur über die folgenden Links:

- [Android Version 2.2 oder höher](app-service-mobile-android-how-to-use-client-library.md)
- [iOS-Version 3.0.0 oder höher](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) Version 2.0.0 oder höher](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova Version 2.0 oder höher](app-service-mobile-cordova-how-to-use-client-library.md)

Wenn Ihre Anwendung Pushbenachrichtigungen verwenden, notieren Sie die spezielle Erfassung Anweisungen für jede Plattform wie einige ändert sich ebenfalls.

Wenn die neue Clientversion bereit sind, probieren Sie es gegen Ihre aktualisierten Server. Nach Überprüfung, dass es funktioniert, können Sie eine neue Version der Anwendung für Kunden freigeben. Wenn Ihre Kunden Gelegenheit, diese Updates erhalten haben, können Sie schließlich Mobile Services Version Ihrer Anwendung löschen. An diesem Punkt haben Sie App Service Mobile App mit neuesten Mobile Apps Server SDK vollständig aktualisiert.

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Was sind Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[App Servicepreise]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Authentifizierung]: ../app-service/app-service-authentication-overview.md
[Authentifizierung Schnellstart]: app-service-mobile-auth.md

[Azure-Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
