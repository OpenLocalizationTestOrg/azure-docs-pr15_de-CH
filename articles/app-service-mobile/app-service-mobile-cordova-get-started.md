<properties
    pageTitle="Erstellen eine Cordova Anwendung in Azure App Service mobiler Apps | Microsoft Azure"
    description="Folgen Sie dieser Anleitung zum Einstieg Azure mobile app-Back-Ends für Apache Cordova Entwicklung"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="Cordova, Javascript, mobile Clients" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Erstellen eines Apache Cordova-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht die Cloud-Back-End-Dienst eine Apache Cordova app ein Azure mobile app Back-End hinzufügen.  Erstellen Sie eine neue mobile app Backend und eine einfache _Aufgabenliste_ Apache Cordova Anwendung, die in Azure app-Daten gespeichert.

Dieses Lernprogramm ist eine Voraussetzung für alle anderen Apache Cordova Lehrgänge zu mithilfe des Mobile Apps in Azure App Service.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie Folgendes:

* PC mit [Visual Studio Community 2015] oder höher.
* [Visual Studio-Tools für Apache Cordova].
* Ein [Aktives Azure-Konto](https://azure.microsoft.com/pricing/free-trial/).

Sie können auch Visual Studio umgehen und Apache Cordova-Befehlszeile direkt verwenden.  Dies ist hilfreich bei der praktischen Einführung auf einem Mac.  Apache Cordova Clientanwendungen über die Befehlszeile kompilieren fällt nicht unter diesem Lernprogramm.

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen Sie ein neue Azure mobile app backend

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[In diesem Video zeigt ähnliche Schritte](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Konfigurieren Sie das Serverprojekt

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Herunterladen und Ausführen der Apache Cordova Anwendung

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Nächste Schritte

Abschluss dieses Schnellstart-Lernprogramm verschieben Sie eines der folgenden Lernprogramme:

* [Authentifizierung hinzufügen] zu Ihrem Apache Cordova Anwendung.
* [Pushbenachrichtigungen hinzufügen] zu Ihrem Apache Cordova Anwendung.

Erfahren Sie mehr über Schlüsselkonzepte Azure App Service.

* [Authentifizierung]
* [Push-Benachrichtigung]

Dazu verwenden Sie die SDKs.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio-Tools für Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Authentifizierung hinzufügen]: app-service-mobile-cordova-get-started-users.md
[Push-Benachrichtigung hinzufügen]: app-service-mobile-cordova-get-started-push.md
[Authentifizierung]: app-service-mobile-auth.md
[Push-Benachrichtigung]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
