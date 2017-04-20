<properties 
    pageTitle="App Service-Umgebung | Microsoft Azure" 
    description="Was ist ein Azure App Service-Umgebung? Eine Einführung in App Service-Umgebung." 
    keywords="Azure app Service-Umgebung, virtuelles Netzwerk sichere Netzwerke"
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

# <a name="app-service-environment-documentation"></a>App Service Dokumentation

Eine App Service-Umgebung ist eine [Premium] [ PremiumTier] service-Plan Option Azure App Dienst vollständig isolierte und dedizierte Umgebung zum Ausführen von Azure App Service apps sicher auf hoher Ebene einschließlich [Web Apps][WebApps], [Mobile Apps][MobileApps], und [API-Apps][APIApps].  

App Service-Umgebung sind ideal für Arbeitslasten benötigen:

- Sehr hohe skalieren
- Isolierung und sicheren Netzwerkzugriff

Kunden können mehrere App Umgebungen in einem einzigen Azure Bereich sowie mehrere Azure Regionen erstellen.  Dadurch App Service-Umgebungen für horizontale Skalierung Zustand weniger Anwendungsebenen zur Unterstützung hoher RPS Arbeitslasten.

App Service-Umgebung isoliert nur einen einzelnen Debitor Programme ausgeführt und sind immer in einem virtuellen Netzwerk bereitgestellt.  Kunden haben eine genauere Kontrolle über beide Anwendung ein- und ausgehenden Netzwerkverkehr [Netzwerk]Sicherheitsgruppen[NetworkSecurityGroups].  Anträge können auch schnelle Verbindung über virtuelle Netzwerke, lokale Ressourcen einrichten.

Apps müssen häufig den Zugriff auf Unternehmensressourcen wie interne Datenbanken und Webdienste.  Apps auf App Service-Umgebungen Zugriff auf Ressourcen über [Zwischen Standorten] [ SiteToSite] VPN und [Azure ExpressRoute] [ ExpressRoute] Verbindung.

* [Was ist eine App Service-Umgebung?](../app-service-web/app-service-app-service-environment-intro.md)
* [Erstellen einer App Service-Umgebung](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Erstellen von Apps in einer App Service-Umgebung](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Erstellen und Verwenden einer internen Lastenausgleich mit App Service](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Konfigurieren einer App Service-Umgebung](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Skalierung von Apps in einer App Service-Umgebung](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Sicherheit und Architektur](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Wie die

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Videos
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
