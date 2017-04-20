<properties
    pageTitle="Azure Stapel Web Apps Übersicht | Microsoft Azure"
    description="Übersicht über Web-Apps in Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Übersicht über Azure Stack-Apps
    
> [AZURE.NOTE] Folgendes gilt nur für Azure Stapel TP1 Bereitstellungen.

Azure Stapel Web Apps ist das erste Element ins Azure Stapel Azure App Serviceangebot. Azure Stapel Web Apps Installer eine Instanz jeder der fünf erforderliche Rolle erstellen und auch einen Dateiserver erstellen. Weitere Instanzen für jede Rollentypen hinzugefügt werden können, denken Sie daran, dass es nicht viel Platz für VMs im Technical Preview 1. Die aktuellen Funktionen für Azure Stapel Web Apps sind hauptsächlich Foundation Funktionen, die Web-apps und Hostserver verwalten.

![Stapel Stapel Azure App Service Web Apps in Azure Portal][1]

## <a name="limitations-of-the-technical-preview"></a>Grenzen der technischen Vorschau

Es unterstützt keine Azure Stapel App Service Preview-Versionen. Setzen Sie Produktionsarbeitslasten auf diese Vorabversion. Es gibt auch kein Upgrade zwischen Azure Stapel App Service Preview-Versionen. Der Hauptzweck der Vorschau Versionen sind zeigen, was wir bereitstellen und Feedback zu erhalten. 

## <a name="what-is-an-app-service-plan"></a>Was ist eine App Service-Plan

Der Ressourcenanbieter Azure Stapel Web Apps verwendet denselben Code, den die Azure Web Apps in Azure App Service verwendet. Daher sind einige allgemeine Konzepte zu beschreiben. In Web-Apps heißt Preise Container für Web-apps App Service-Plan. Es stellt die dedizierte virtuelle Computer verwendet, um speichern Ihre apps dar. Innerhalb eines Abonnements können Sie mehrere App Service-Pläne. Dies gilt auch in Azure Stapel Web Apps. 

In Azure sind freigegebene und engagierte Mitarbeiter. Eine freigegebene Arbeitskraft unterstützt HD-mandantenfähigen app Webhosting und gibt nur eine freigegebene Arbeitskräfte. Dedizierte Server nur von einem Mandanten verwendet und sind in drei Größen: klein, Mittel und groß. Für lokale Kunden können immer mit diesen Begriffen beschrieben werden. In Azure Stapel Web Apps können Anbieter Ressourcenadministratoren Arbeitskraft Ebenen wollen sie zur Verfügung stellen, so dass sie mehrere Sätze von freigegebenen Arbeitskräfte oder unterschiedliche dedizierte Arbeitskräften basierend auf ihrer eindeutigen Webhosting-Bedürfnisse. Mithilfe dieser Arbeitskraft Definitionen können sie dann die SKUs definieren.

## <a name="portal-features"></a>Leistungsmerkmale des Portals

Wie auch mit dem Back-End verwendet Azure Stapel Web Apps dieselbe Benutzeroberfläche Azure Web Apps verwendet. Einige Funktionen sind deaktiviert und nicht noch Azure Stack funktionsfähige. Deswegen Azure-spezifische erwartet wird oder Dienste, die diese Features erfordern noch nicht verfügbar in Azure Stapel sind. 

## <a name="next-steps"></a>Nächste Schritte

- [Vor dem Einstieg in Azure Stapel Web Apps](azure-stack-webapps-before-you-get-started.md)
- [Web Apps Ressourcenanbieter installieren](azure-stack-webapps-deploy.md)

Sie können auch andere [Plattform als Dienst (PaaS)](azure-stack-tools-paas-services.md), wie [SQL Server Ressourcenanbieter](azure-stack-sql-rp-deploy-short.md) und [MySQL Ressourcenanbieter](azure-stack-mysql-rp-deploy-short.md)ausprobieren.

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
