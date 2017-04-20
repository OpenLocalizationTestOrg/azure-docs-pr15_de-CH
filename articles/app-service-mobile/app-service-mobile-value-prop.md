<properties
    pageTitle="Was sind Mobile Apps"
    description="Erfahren Sie, welche Vorteile zu Enterprise mobile apps App bringen."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="getting-started"> </a>Was ist Mobile Apps?

Azure App Service ist eine vollständig verwaltete [Plattform als Dienst](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) für professionelle Entwickler die umfassende Funktionen für mobile Web und Integrationsszenarios bringt. *Mobile Apps* in *Azure App Service* bieten eine hoch skalierbare, global verfügbaren mobilen Plattform zur Anwendungsentwicklung für Entwickler und Systemintegratoren, die umfassende Funktionen für mobile Entwickler bereitstellt.

![Mobiler Apps](./media/app-service-mobile-value-prop/overview.png)

##<a name="why-mobile-apps"></a>Warum Mobile Apps?
*Mobile Apps* in *Azure App Service* bietet eine hochgradig skalierbare, global verfügbare mobile Plattform zur Anwendungsentwicklung für Entwickler und Systemintegratoren, die umfassende Funktionen für mobile Entwickler bereitstellt. Mit Mobile Apps können Sie:

- **Systemeigene und cross-Plattform-apps erstellen** - ob Sie systemeigene iOS, Android und Windows erstellen apps oder plattformübergreifende Xamarin oder Cordova (Phonegap) apps, profitieren Sie von App Service mit systemeigenen SDKs.
- **Mit Enterprise-Systeme** - Mobile Apps corporate Zeichen auf Minuten hinzufügen und Verbinden mit Ihrem lokalen Enterprise oder cloud-Ressourcen.
- **Entwickeln von apps mit Daten synchronisieren offline-Ready** - stellen Ihre mobilen Mitarbeiter produktiv Apps erstellen, offline arbeiten und Daten im Hintergrund synchronisieren bei Verbindung mit Ihrem Enterprise-Datenquellen oder SaaS APIs Mobile Apps mit.
- **Pushbenachrichtigungen Millionen Sekunden** - Ihre Kunden mit instant Pushbenachrichtigungen auf jedem Gerät auf ihre Bedürfnisse gesendet, wenn die Zeit angepasst.

## <a name="mobile-app-features"></a>Mobile-App-Funktionen
Die folgenden Funktionen sind wichtig für die mobile Entwicklung Cloud aktiviert:

- **Authentifizierung und Autorisierung** - wählen eine wachsende Identitätsanbieter, einschließlich Azure Active Directory für Enterprise-Authentifizierung sowie soziale Anbieter wie Facebook und Google, Twitter Microsoft Account.  Azure Mobile Apps bietet einen OAuth 2.0 für jeden Anbieter.  Sie können das SDK für Identitätsanbieter für Anbieter bestimmte Funktionen integrieren.

  Erfahren Sie mehr über unsere [Authentifizierungsfunktionen].

- **Datenzugriff** - Azure Mobile Apps bietet eine Mobilgeräte OData v3-Datenquelle mit SQL Azure oder eine lokale SQL Server verknüpft.  Dieser Service basiert auf Entity Framework ermöglicht die einfache Integration mit anderen NoSQL und SQL-Datenprovider, einschließlich [Azure-Tabellenspeicher], MongoDB [DocumentDB] und SaaS-API wie Office 365 und Salesforce.com.
- **Offline Sync** - SDKs für unsere Kunden stellen Sie stabile und reagiert Windows-Dienste erstellen, die mit offline-Daten, die Einrichtung können mit Back-End-Daten, einschließlich Unterstützung für Konflikt automatisch synchronisiert werden.

  Erfahren Sie mehr über unsere [Datenfeatures].

- **Push-Benachrichtigung** - SDKS für unsere Kunden nahtlos mit der Registrierung der Azure Benachrichtigung Sie Pushbenachrichtigungen an Millionen Benutzer gleichzeitig senden.

  Erfahren Sie mehr über unsere [Benachrichtigungsfunktionen drücken].

- **Client-SDKs** - bieten wir eine umfassende Client-SDKs, die systemeigene Entwicklung ([iOS], [Android] und [Windows]), plattformübergreifende Entwicklung ([Xamarin für IOS- und Android], [Xamarin Formen]) und Hybrid-Anwendungsentwicklung ([Apache Cordova]).  Jeder Client SDK steht MIT Lizenz und Open-Source.

## <a name="azure-app-service-features"></a>Azure App Service-Funktionen.
Die folgenden Plattformfeatures eignen sich im Allgemeinen für mobile Produktionsstandorte.

- **AutoSkalieren** - App Service können Sie schnell vergrößern oder verkleinern eingehende Kunden Last verarbeiten. Manuell die Anzahl und Größe der VMs oder mobile app Backend basierend auf Laden oder Zeitplan Skalierung Automatische Skalierung einrichten.

  Erfahren Sie mehr über [Automatische Skalierung].

- **Staging-Umgebung** - App Service führen mehrere Versionen Ihrer Website ermöglichen ein und Testen B Testen in der Produktion als Teil eines größeren DevOps direkte Staging einer neuen Backend.

  Erfahren Sie mehr über [Stagingumgebungen].

- **Kontinuierliche Bereitstellung** - App Service kann integriert mit gemeinsamen SCM automatisch eine neue Version von Ihrem Back-End bereitzustellen, indem auf einen Zweig der SCM-System.

  Erfahren Sie mehr über [Bereitstellungsoptionen].

- **Virtual Networking** - App Service eine Verbindung zum lokalen Ressourcen virtuellen Netzwerk, ExpressRoute oder Hybrid-Anschlüsse.

  Entdecken Sie mehr über [Hybrid], [virtuelle Netzwerke]und [ExpressRoute].

- **Isolierter / dedizierte Umgebung** -App Service in einem vollständig isoliert und dedizierte Umgebung zum Ausführen von Azure App Service apps sicher auf hoher Ebene ausgeführt werden.  Dies ist ideal für Arbeitslasten ohne hohe Skalierung, Isolierung oder sicheren Netzwerkzugriff.

  Erfahren Sie mehr über [App Service-Umgebungen].

## <a name="getting-started"></a>Erste Schritte ##
Zunächst mit Mobile Apps, folgen Sie der Anleitung [Beginnen] .  Dies umfasst die Grundlagen eines mobilen Backend und Client Ihrer Wahl und Integration von Authentifizierung, offline synchronisieren und Pushbenachrichtigungen.  Sie können das Lernprogramm [Beginnen] mehrmals - einmal für jede Clientanwendung folgen.

Weitere Informationen zu Azure Mobile Apps finden Sie unsere [learning Map].
Weitere Informationen zu Azure App Service-Plattform finden Sie in [Azure App Service].

>[AZURE.NOTE]Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](https://tryappservice.azure.com/?appServiceName=mobile)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

<!-- URLs. -->
[Migrate your Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Erste Schritte]: app-service-mobile-ios-get-started.md
[Azure Table Storage]: ../storage/storage-getting-started-guide.md
[DocumentDB]: ../documentdb/documentdb-get-started.md
[Funktionen]: ./app-service-mobile-auth.md
[Data-Funktionen]: ./app-service-mobile-offline-data-sync.md
[Push-Benachrichtigung-Funktionen]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin für IOS- und Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin Formulare]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[Automatische Skalierung]: ../app-service-web/web-sites-scale.md
[Stagingumgebungen]: ../app-service-web/web-sites-staged-publishing.md
[Bereitstellungsoptionen]: ../app-service-web/web-sites-deploy.md
[hybridverbindungen]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[virtuelle Netzwerke]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service-web/app-service-app-service-environment-network-configuration-expressroute.md
[App Service-Umgebung]: ../app-service-web/app-service-app-service-environment-intro.md
[Karte lernen]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/
