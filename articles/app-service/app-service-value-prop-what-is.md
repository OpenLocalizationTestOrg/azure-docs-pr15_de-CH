<properties
    pageTitle="Azure App Service für Web, Mobile und API-apps | Microsoft Azure"
    description="Erfahren Sie, wie Azure App Service entwickeln, bereitstellen und Verwalten von Web und apps."
    keywords="App Service Azure app Service app Servicekosten, Skalierung, skalierbare, Bereitstellung, Azure Bereitstellung, Paas, Plattform als Dienst, Website, Website, Web, Azure mobile"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Was ist Azure App Service?

*App* ist ein [Plattform als Service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) bietet Microsoft Azure. Erstellen Sie Web und apps für alle Plattformen und Geräte. SaaS-Lösungen apps integrieren, Verbinden mit lokalen und Automatisieren Ihrer Geschäftsprozesse. Azure führt Ihre apps auf vollständig verwaltete virtuelle Maschinen (VMs) mit dedizierten virtuellen oder VM-Ressourcen.

App Service enthält Web- und mobile Funktionen, die wir zuvor separat als Azure Websites und Azure Mobile Services bereitgestellt. Sie umfasst außerdem neue Funktionen für die Automatisierung von Geschäftsprozessen und Cloud APIs hosten. Als einen einzelnen integrierten Dienst ermöglicht App Service - Websites, mobile app-Back-Ends RESTful-APIs und Geschäftsprozesse - Komponenten in einer einzigen Projektmappe erstellen.

Das folgende 4 Minuten Video bietet eine kurze Erläuterung Beziehung App Service zu früheren Azure Angebote und neues.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Warum verwenden App Service?

Hier sind einige wichtige Features und Funktionen von App Service:

- **Mehrere Sprachen und Frameworks** - App unterstützt erstklassige ASP.NET Node.js, Java, PHP und Python. App Service VMs können Sie auch [Windows PowerShell und andere Skripts oder ausführbare Dateien](../app-service-web/web-sites-create-web-jobs.md) ausführen.

- **DevOps Optimierung** - richten Sie [fortlaufende Integration und Bereitstellung](../app-service-web/app-service-continuous-deployment.md) mit Visual Studio Team Services, GitHub oder BitBucket. Fördern Sie Updates über [Test- und Stagingumgebungen](../app-service-web/web-sites-staged-publishing.md). Führen Sie [A / B-Tests](../app-service-web/app-service-web-test-in-production-get-start.md). Verwalten Sie Ihrer apps in App Service [Azure PowerShell](../powershell-install-configure.md) oder [plattformübergreifende Befehlszeilenschnittstelle (CLI)](../xplat-cli-install.md).

- **Global mit hoher Verfügbarkeit skalieren** - Skalierung [,](../app-service-web/web-sites-scale.md) oder [Sie](../monitoring-and-diagnostics/insights-how-to-scale.md) manuell oder automatisch. Hosten Ihre apps in Microsoft global Datencenter-Infrastruktur und App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) verspricht hohen Verfügbarkeit.

- **Verbindung mit SaaS-Plattformen und lokalen Daten** - 50 [Connectors](../connectors/apis-list.md) für Enterprise-Systeme (z.B. SAP, Siebel und Oracle) SaaS-Dienste (wie Salesforce und Office 365) und Internetdienste (wie Facebook und Twitter) auswählen. Den lokalen Datenzugriff [Hybrid-Verbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure virtuelle Netzwerke](../app-service-web/web-sites-integrate-with-vnet.md).

- **Sicherheits- und Compliance** - App ist [ISO, SOC und PCI-Compliance](https://www.microsoft.com/TrustCenter/).

- **Anwendungsvorlagen** - wählen Sie aus einer umfangreichen Liste von Anwendungsvorlagen in [Azure Marketplace](https://azure.microsoft.com/marketplace/) , mit denen Sie mithilfe eines Assistenten wie WordPress, Joomla und Drupal gängigen Open Source-Software installieren.

- **Visual Studio Integration** - dedizierten Tools in Visual Studio optimieren die Arbeit erstellen, bereitstellen und Debuggen.

## <a name="app-types-in-app-service"></a>App-Typen in App Service

App Service bietet verschiedene *app-Typen*jeweils eine bestimmte Art von Arbeitslast hosten soll:

- [**Web Apps**](../app-service-web/app-service-web-overview.md) - zum Hosten von Websites und Web.

- [**Mobiler Apps**](../app-service-mobile/app-service-mobile-value-prop.md) Für die mobile Anwendung hosten back-Ends.

- [**API-Apps**](../app-service-api/app-service-api-apps-why-best-platform.md) - zum Hosten von RESTful-APIs.

- [**Logik-Apps**](../app-service-logic/app-service-logic-what-are-logic-apps.md) - Automatisierung von Geschäftsprozessen und Integration von Systemen und Daten über Wolken ohne Code zu schreiben.

Die Word- *Anwendung* Hier bezieht sich auf die Hosting-Ressourcen eine Arbeitslast ausgeführt. Aufnahme "Web app" als Beispiel, sind Sie wahrscheinlich gewöhnt sind, an eine Webanwendung als Datenverarbeitungsressourcen und Anwendungscode, der Browser gemeinsam Funktionalität bereitzustellen. Doch in App Service *WebApp* Rechenressourcen Azure bietet für den Anwendungscode hostet. Besteht die Anwendung einer front-End und RESTful API-back-End konnte bereitstellen auf einer Webanwendung und Front-End-Code konnte ein Web app und Backend-Code eine API-Anwendung bereitstellen. Die Anwendung kann mehrere App Service Apps verschiedene bestehen.

## <a name="app-service-plans"></a>App Service-Pläne

[App Service-Pläne](azure-web-sites-web-hosting-plans-in-depth-overview.md) geben die Art der Serverressourcen, die Ihre apps auf. Wenn Sie helle Datenverkehr erwarten, können Sie freigegebene VMs (**frei** und **Shared** Preise Ebenen). Für höhere Lasten können Sie aus mehreren dedizierten VMs (**Basic**, **Standard**und **Premium** -Ebenen). Mehrere App Service apps können derselben Plan, und skalieren sie nach den Tarifen zusammen im Plan.

Benötigen Sie weitere Skalierbarkeits- und Netzwerkisolation, können Sie Ihre apps in einer [App Service-Umgebung](../app-service-web/app-service-app-service-environment-intro.md)ausführen.

## <a name="pricing"></a>Preisgestaltung

Informationen zu App Service kostet finden Sie unter [App Preisen](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>App Service testen

[Erstellen einer Beispiel-Web app, mobilen Anwendung Logik app](http://go.microsoft.com/fwlink/?LinkId=523751) und Spielen mit 1 Stunde keine Kreditkartendaten erforderlich, keine Zusagen ohne Probleme.

Ein [kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)und probieren Sie unsere Lernprogramme für erste Schritte:

* [Lernprogramm: Erstellen einer Webanwendung](../app-service-web/app-service-web-get-started.md)
* [Lernprogramm: Erstellen einer Apps](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Lernprogramm: Erstellen einer API-app](../app-service-api/app-service-api-dotnet-get-started.md)
* [Lernprogramm: Erstellen einer Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
