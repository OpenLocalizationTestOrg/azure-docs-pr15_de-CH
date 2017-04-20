<properties 
    pageTitle="API-Apps Einführung | Microsoft Azure" 
    description="Erfahren Sie, wie Azure App Service entwickeln, Host und RESTful-APIs verwenden." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>API-Apps (Übersicht)

API-apps in Azure App Service bieten, die leichter entwickeln, Host und APIs in der Cloud und lokalen nutzen. Mit API-apps erhalten Sie Enterprise Sicherheit, einfache Zugriffskontrolle Hybrid-Konnektivität, automatische Generierung von SDK und nahtlose Integration mit [Logik-Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Azure App Service](../app-service/app-service-value-prop-what-is.md) ist eine vollständig verwaltete Plattform für mobile Web und Integrationsszenarios. API-Apps ist eine der vier app von [Azure App Service](../app-service/app-service-value-prop-what-is.md)angeboten.

![App-Typen in Azure App Service](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Warum verwenden API-Apps?

Hier sind einige der wichtigsten Features von API-Apps:

- **Bringen Ihre vorhandenen API als-ist** -Sie müssen ändern Sie den Code in vorhandenen APIs nutzen API-Apps-Code nur an eine API-Anwendung bereitstellen. Ihre API können alle Sprachen oder Frameworks von App Service, einschließlich ASP.NET und C#, Java, PHP, Node.js und Python unterstützt.

- **Einfache Verbrauch** - integrierte Unterstützung [Swagger API-](http://swagger.io/) Metadaten stellt APIs einfach von einer Vielzahl von Clients angewendet.  Automatisch generieren von Clientcode in einer Vielzahl von Sprachen wie C#, Java und Javascript-APIs. Konfigurieren Sie [CORS](app-service-api-cors-consume-javascript.md) ohne Code problemlos. Weitere Informationen finden Sie unter [App Service API-Apps Metadaten für API-Erkennung und Code](app-service-api-metadata.md) und [verbrauchen eine API-app von JavaScript mit CORS](app-service-api-cors-consume-javascript.md). 

- **Einfache Zugriffskontrolle** - API-app von nicht authentifizierten Zugriff mit kein Code schützen. Integrierte Authentifizierung sichern APIs Zugriff von anderen Diensten oder von Clients, Benutzer darstellt. Unterstützte Identitäten zählen Active Directory Azure, Facebook, Twitter, Google und Microsoft Account. Clients können Active Directory-Authentifizierung Library (ADAL) oder Mobile Apps SDK verwenden. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung für API-Apps in Azure App Service](app-service-api-authentication.md).

- **Visual Studio Integration** - dedizierten Tools in Visual Studio optimieren die Arbeit erstellen, bereitstellen, verbraucht, Debuggen und Verwalten von API-apps. Weitere Informationen finden Sie in der [Ankündigung von Azure SDK für .NET 2.8.1](/blog/announcing-azure-sdk-2-8-1-for-net/).

- **Integration mit Logik Apps** - API-apps erstellen können von [App Service Logik Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md)genutzt werden.  Weitere Informationen finden Sie unter [Using App Service Logik Apps Ihre benutzerdefinierte API gehostet](../app-service-logic/app-service-logic-custom-hosted-api.md) und [neue Schema Version 2015-08-01-Vorschau](../app-service-logic/app-service-logic-schema-2015-08-01.md).

Darüber hinaus kann eine API-app [Web Apps](../app-service-web/app-service-web-overview.md) und [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md)Funktionen nutzen. Umgekehrt gilt: Verwenden Sie eine Web-app oder mobile Anwendung hosten eine API, sie profitieren von Funktionen wie stolz Metadaten für Client-API-Apps Codeerzeugung und CORS für domänenübergreifende Browserzugriff. Der einzige Unterschied zwischen den drei app (API, mobile Web) ist der Name und Symbol für sie in Azure-Portal.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Was ist der Unterschied zwischen API-Apps und Azure API Management?

API-Apps und [Azure API Management](../api-management/api-management-key-concepts.md) sind ergänzende Dienste:

* Management-API wird zum Verwalten von APIs. Sie beenden API Management front auf eine API mit Monitor und Gas, bearbeiten, ein- und Ausgabe, konsolidiert mehrere APIs in einen Endpunkt, usw. Die verwalteten APIs können an einer beliebigen Stelle gehostet werden.
* API-Apps ist über hosting-APIs. Der Service umfasst Features, die Entwicklung und verwendeten APIs erleichtern, jedoch nicht die Art der Überwachung, Einschränkung, bearbeiten oder Konsolidierung, Management-API ist. Wenn Sie API-Funktionen benötigen, können Sie APIs in API-apps hosten, ohne API Management.

Hier ist ein Diagramm zur Veranschaulichung gehostet in API-apps und APIs zur API-Management.

![Azure API Management und API-Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Einige Funktionen der API Management und API-Apps haben ähnliche Funktionen.  Zum Beispiel können beide CORS Unterstützung automatisieren. Wenn Sie die beiden Dienste gemeinsam verwenden, verwenden Sie API Management für CORS da als front-End für Ihre API-apps fungiert. 

## <a name="getting-started"></a>Erste Schritte

Zunächst mit API-Apps durch die Bereitstellung von Beispielcode zu finden Sie im Lernprogramm für welches Framework Sie bevorzugen:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Fragen zu API-apps im [API-Apps Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)starten Sie Thread. 
