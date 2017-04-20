<properties
    pageTitle="Web Apps-Übersicht | Microsoft Azure"
    description="Erfahren Sie, wie Azure App Service entwickeln können und Host ASP.NET-Webanwendungen"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Web Apps-Überblick

*App Service Web Apps* ist eine vollständig verwaltete Compute-Plattform, die zum Hosten von Websites und Web optimiert ist. Diese [Plattform als Service](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) bietet Microsoft Azure können konzentrieren Sie Ihre Geschäftslogik während Azure kümmert sich die Infrastruktur ausführen und Ihre apps.

Das folgende 5-Minuten-Video führt Azure App Service Web Apps.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Was ist eine Web-app in App Service?

In App-Service ist eine *WebApp* Rechenressourcen Azure bietet für eine Website oder Anwendung.  

Die Serverressourcen möglicherweise auf gemeinsam genutzte oder dedizierte VMs (VMs) je nach den Tarif, die Sie auswählen. Anwendungscode wird in einer verwalteten VM von anderen Kunden.

Code kann beliebiger Sprache oder Rahmen von [Azure App Service](../app-service/app-service-value-prop-what-is.md)wie ASP.NET Node.js, Java, PHP oder Python unterstützt wird. Sie können auch Skripts ausführen, die [PowerShell und andere Skriptsprachen](web-sites-create-web-jobs.md#acceptablefiles) Web app verwenden.

Finden Sie Beispiele für normale Anwendungsszenarien, mit denen Sie Web-Apps für [Web app Szenarien](https://azure.microsoft.com/documentation/scenarios/web-app/) sowie die **Szenarien und Recommendations** [Azure App Service](choose-web-site-cloud-service-vm.md#scenarios), virtuelle Computer Service Fabric und Cloud Vergleich.

## <a name="why-use-web-apps"></a>Warum verwenden Web Apps?

Hier sind einige der wichtigsten Features von App Service, die für Web Apps gelten:

- **Mehrere Sprachen und Frameworks** - App unterstützt erstklassige ASP.NET Node.js, Java, PHP und Python. App Service VMs können Sie auch [PowerShell und andere Skripts oder ausführbare Dateien](../app-service-web/web-sites-create-web-jobs.md) ausführen.

- **DevOps Optimierung** - richten Sie [fortlaufende Integration und Bereitstellung](../app-service-web/app-service-continuous-deployment.md) mit Visual Studio Team Services, GitHub oder BitBucket. Fördern Sie Updates über [Test- und Stagingumgebungen](../app-service-web/web-sites-staged-publishing.md). Führen Sie [A / B-Tests](../app-service-web/app-service-web-test-in-production-get-start.md). Verwalten Sie Ihrer apps in App Service [Azure PowerShell](../powershell-install-configure.md) oder [plattformübergreifende Befehlszeilenschnittstelle (CLI)](../xplat-cli-install.md).

- **Global mit hoher Verfügbarkeit skalieren** - Skalierung [,](../app-service-web/web-sites-scale.md) oder [Sie](../monitoring-and-diagnostics/insights-how-to-scale.md) manuell oder automatisch. Hosten Ihre apps in Microsoft global Datencenter-Infrastruktur und App Service [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) verspricht hohen Verfügbarkeit.

- **Verbindung mit SaaS-Plattformen und lokalen Daten** - 50 [Connectors](../connectors/apis-list.md) für Enterprise-Systeme (z.B. SAP, Siebel und Oracle) SaaS-Dienste (wie Salesforce und Office 365) und Internetdienste (wie Facebook und Twitter) auswählen. Den lokalen Datenzugriff [Hybrid-Verbindungen](../biztalk-services/integration-hybrid-connection-overview.md) und [Azure virtuelle Netzwerke](../app-service-web/web-sites-integrate-with-vnet.md).

- **Sicherheits- und Compliance** - App ist [ISO, SOC und PCI-Compliance](https://www.microsoft.com/TrustCenter/).

- **Anwendungsvorlagen** - wählen Sie aus einer umfangreichen Liste von Anwendungsvorlagen in [Azure Marketplace](https://azure.microsoft.com/marketplace/) , mit denen Sie mithilfe eines Assistenten wie WordPress, Joomla und Drupal gängigen Open Source-Software installieren.

- **Visual Studio Integration** - dedizierten Tools in Visual Studio optimieren die Arbeit erstellen, bereitstellen und Debuggen.

Darüber hinaus kann Web app nutzen Funktionen [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) (wie CORS) und [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) (wie Push-Benachrichtigung). Weitere Informationen zu app in App Service finden Sie in [Azure App Service Overview](../app-service/app-service-value-prop-what-is.md).

Azure bietet neben Web Apps in App Service andere Dienste, die zum Hosten von Websites und Web verwendet werden können. Web Apps ist in den meisten Fällen die beste Wahl.  Microservice-Architektur berücksichtigt [Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric)und benötigen Sie mehr Kontrolle über die VMs, die auf Code ausgeführt wird, sollten Sie [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). Weitere Informationen zwischen diesen Azure Services finden Sie unter [Azure App Service, virtuellen Maschinen und Service Fabric Clouddienste Vergleich](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Erste Schritte

Einführende Beispielcode auf eine neue Web app App Service bereitzustellen, das Lernprogramm [Bereitstellen Ihrer ersten Anwendung in Azure in 5 Minuten](app-service-web-get-started.md) . Sie benötigen ein kostenloses Azure-Konto.

Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.
