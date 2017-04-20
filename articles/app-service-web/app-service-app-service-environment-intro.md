<properties 
    pageTitle="Einführung in App Service-Umgebung" 
    description="Erfahren Sie mehr über die App Service-Umgebung Funktion, sicherer VNet verknüpft, dedizierten Maßeinheiten für alle apps ausgeführt." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="stefsch"/>

# <a name="introduction-to-app-service-environment"></a>Einführung in App Service-Umgebung

## <a name="overview"></a>Übersicht ##
Eine App Service-Umgebung ist eine [Premium] [ PremiumTier] service-Plan Option Azure App Dienst vollständig isolierte und dedizierte Umgebung zum Ausführen von Azure App Service apps sicher auf hoher Ebene einschließlich [Web Apps][WebApps], [Mobile Apps][MobileApps], und [API-Apps][APIApps].  

App Service-Umgebung sind ideal für Arbeitslasten benötigen:

- Sehr hohe skalieren
- Isolierung und sicheren Netzwerkzugriff

Kunden können mehrere App Umgebungen in einem einzigen Azure Bereich sowie mehrere Azure Regionen erstellen.  Dadurch App Service-Umgebungen für horizontale Skalierung Zustand weniger Anwendungsebenen zur Unterstützung hoher RPS Arbeitslasten.

App Service-Umgebung isoliert nur einen einzelnen Debitor Programme ausgeführt und sind immer in einem virtuellen Netzwerk bereitgestellt.  Kunden haben eine genauere Kontrolle über beide Anwendung ein- und ausgehenden Netzwerkverkehr und Applikationen können schnelle Verbindung über virtuelle Netzwerke, lokale Ressourcen einrichten.

Alle Artikel und -über die App Service-Umgebungen sind in der [Readme-Datei für Anwendung Service](../app-service/app-service-app-service-environments-readme.md)verfügbar.

Netzwerkzugriff Überblick wie App Service hoher Skalierung aktivieren und sichern, finden Sie unter der [AzureCon Deep Dive] [ AzureConDeepDive] in App Service-Umgebung.

Eine tief greifende auf horizontale Skalierung mit mehreren App Service-Umgebung finden Sie im Artikel zum Einrichten einer [geografisch verteilten Anwendung Speicherbedarf][GeodistributedAppFootprint].

Konfiguration die Sicherheitsarchitektur in der Tiefe AzureCon angezeigt, finden Sie Artikel auf eine [geschichteten Sicherheitsarchitektur](app-service-app-service-environment-layered-security.md) mit App-Dienst implementieren.

Apps auf App Service-Umgebungen können die abgegrenzten upstream Geräte wie Web Application Firewalls (AAF) zugreifen.  Zum [Konfigurieren von WAF für App Service](app-service-app-service-environment-web-application-firewall.md) werden dieses Szenario behandelt. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Dedizierte Computeressourcen ##
Alle computeressourcen in einer App Service-Umgebung gewidmet ausschließlich ein einzelnes Abonnement und App Service-Umgebung kann mit bis zu fünfzig (50) Datenverarbeitungsressourcen für die exklusive Verwendung durch eine Anwendung konfiguriert werden.

Eine App Service-Umgebung besteht aus Front-End-Compute Ressourcenpool sowie ein bis drei Worker Compute-Ressourcenpools. 

Der Front-End-Pool enthält Datenverarbeitungsressourcen für SSL-Beendigung als auch automatischen Lastausgleich der app Anfragen innerhalb einer App Service-Umgebung verantwortlich. 

Jede Arbeitspool enthält computeressourcen [App Service-Pläne][AppServicePlan], die wiederum Azure App Service apps enthalten.  Da in einer App Service-Umgebung bis zu drei verschiedene Tätigkeitsbereiche Pools verwendet werden können, haben Sie die Flexibilität, verschiedene Serverressourcen für jede Arbeitspool.  

Beispielsweise können Sie eine Arbeitspool mit weniger leistungsfähigen Serverressourcen für App Service-Pläne zur Entwicklung oder Tests apps erstellen.  Eine zweite (oder dritte) Arbeitspool können leistungsfähigere Datenverarbeitungsressourcen zur App Service-Pläne Produktion apps ausgeführt.

Weitere Informationen zu Front-End- und Arbeitskraft Pools zur Compute-Ressourcen finden Sie unter [Konfigurieren einer App Service-Umgebung][HowToConfigureanAppServiceEnvironment].  

Weitere Informationen über die verfügbaren berechnen Ressource Größen in einer App Service-Umgebung unterstützt, wenden Sie sich an die [App Servicepreise] [ AppServicePricing] Seite, und überprüfen Sie die verfügbaren Optionen für App Service Premium Tarif.

## <a name="virtual-network-support"></a>Virtuelles Netzwerk-Support ##
Eine App Service-Umgebung erstellt werden **oder** ein virtuelles Netzwerk Azure Ressourcenmanager **oder** eine klassische Modell virtuelles Netzwerk ([Weitere Informationen über virtuelle Netzwerke][MoreInfoOnVirtualNetworks]).  Da eine App Service-Umgebung in einem virtuellen Netzwerk und genauer innerhalb eines Subnetzes ein virtuelles Netzwerk besteht, können Sie die Sicherheitsfeatures von virtuellen Netzwerken ein- und ausgehende Netzwerkkommunikation steuern nutzen.  

Eine App Service-Umgebung kann entweder mit Internetzugriff mit einer öffentlichen IP-Adresse oder interne mit nur einer Adresse Azure interne Load Balancer (ILB).

[Netzwerk-Sicherheitsgruppen] können[ NetworkSecurityGroups] um eingehende Netzwerkkommunikation im Subnetz beschränken sich eine App Service-Umgebung befindet.  So können Sie apps upstream Geräte und Dienste wie Web Application Firewalls und SaaS-Netzwerkanbieter ausführen.

Apps müssen auch Zugriff auf Unternehmensressourcen wie interne Datenbanken und Webdienste.  Ein gemeinsames Konzept ist diese Endpunkte nur interne Netzwerkdatenverkehr in Azure virtuelles Netzwerk zur Verfügung.  Sobald eine App Service-Umgebung im gleichen virtuellen Netzwerk als interne Dienste hinzugefügt wird, der Umgebung apps möglich, darunter Endpunkte über [Zwischen Standorten] [ SiteToSite] und [Azure ExpressRoute] [ ExpressRoute] Verbindung.

Für Weitere Informationen zur Funktionsweise von App Service-Umgebungen mit virtuellen und lokalen Netzwerken finden Sie in den folgenden Artikeln auf [Netzwerkarchitektur][NetworkArchitectureOverview], [Eingehender Datenverkehr steuern][ControllingInboundTraffic], und eine [Sichere Verbindung]zu Backends[SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Erste Schritte

Sehen Sie mit App-Dienst zunächst [Wie zum Erstellen einer App Service-Umgebung][HowToCreateAnAppServiceEnvironment]

Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Weitere Informationen über die Plattform Azure App Service finden Sie in [Azure App Service][AzureAppService].

Übersicht der Netzwerkarchitektur App Service-Umgebung finden Sie unter [Network Architecture (Übersicht)] [ NetworkArchitectureOverview] Artikel.

Ausführliche ExpressRoute, mit einer App Service-Umgebung finden Sie im folgenden Artikel [Express]und App Service-Umgebungen[NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
