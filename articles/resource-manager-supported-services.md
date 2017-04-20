<properties
   pageTitle="Ressourcen-Manager unterstützten Dienste | Microsoft Azure"
   description="Beschreibt Ressourcenprovider, die unterstützen Ressourcen-Manager, die Schemas und API-Versionen und die Bereiche, die die Ressourcen hosten können."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Ressourcen-Manager-Anbieter, Regionen, API-Versionen und schemas

Azure Resource Manager bietet eine neue Möglichkeit zur Bereitstellung und Verwaltung der Dienste, die die Anwendung bilden. Die meisten, jedoch nicht alle Dienste Ressourcenmanager und einige Dienste unterstützen Ressourcen-Manager nur teilweise. Dieses Thema enthält eine Liste der unterstützten Ressourcenprovider für Azure-Ressourcen-Manager.

Beim Bereitstellen von Ressourcen müssen Sie wissen, welche Bereiche die Ressourcen unterstützen und die API-Versionen für die Ressourcen verfügbar sind. Im Abschnitt [unterstützte Regionen](#supported-regions) veranschaulicht Bereiche arbeiten zu Ihrem Abonnement zu. Im Abschnitt [unterstützte API-Versionen](#supported-api-versions) wird die API-Versionen ermitteln Sie verwenden können.

Welche Dienste in Azure-Portal und Verwaltungsportal unterstützt werden, finden Sie unter [Azure Portal Verfügbarkeitsdiagramm](https://azure.microsoft.com/features/azure-portal/availability/). Welche Dienste verschieben Ressourcen unterstützen, finden Sie unter [Ressourcen neue Ressourcengruppe oder Abonnement](resource-group-move-resources.md).

Tabellen der Microsoft Services Unterstützung Bereitstellung und Verwaltung über Ressourcen-Manager und welche nicht. Der Link in der Spalte **Schnellstart Vorlagen** sendet eine Abfrage an Azure Schnellstart Vorlagen-Repository für die angegebene Ressource-Anbieter. Schnellstart-Vorlagen hinzugefügt und häufig aktualisiert werden. Das Vorhandensein einer Verknüpfung für einen bestimmten Dienst bedeutet nicht notwendigerweise Abfrage Vorlagen aus dem Repository gibt. Es gibt auch viele Drittanbieter-Ressourcenprovider, die Ressourcen-Manager unterstützt. Sie erfahren Ressourcenprovider [Ressourcenprovider](#resource-providers-and-types) und Typen finden Sie unter.


## <a name="compute"></a>Berechnen

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------------------------ |-------- | ------ | ------ |
| Stapelverarbeitung   | Ja     | [Batch REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [2015-12-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|Container | Ja | [Containerservice REST](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics-Lebenszyklus | Ja  |      |        |  |
| Dezimalstellen Mengen | Ja | [Skalierung festlegen REST](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Service Fabric | Ja  | [Service Fabric Rest](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Virtuelle Computer | Ja | [VM-REST](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Virtuelle Computer (klassisch) | Begrenzt | - | - | - |
| RemoteApp | Nein      | -  | - | - |
| Cloud-Dienste (klassisch) | Begrenzt (siehe unten) | - | - | - |

Virtuelle Computer (klassisch) bezieht sich auf Ressourcen über das klassische Bereitstellungsmodell statt durch den Ressourcen-Manager-Bereitstellungsmodell bereitgestellt wurden. Im Allgemeinen diese Ressourcen Vorgängen Ressourcen-Manager nicht unterstützt, aber es gibt einige Vorgänge, die aktiviert wurden. Weitere Informationen zu diesen Bereitstellungsmodellen finden Sie unter [Understanding Ressourcenmanager und klassischen Bereitstellung](resource-manager-deployment-model.md). 

Cloud-Dienste (classic) können mit anderen klassischen Ressourcen verwendet werden. classic Ressourcen jedoch nicht nutzen alle Ressourcen-Manager-Funktionen und ist eine gute Option für Lösungen. Stattdessen Sie ändern Ihre Anwendungsinfrastruktur Ressourcen von Namespaces Microsoft.Compute, Microsoft.Storage und Microsoft.Network.


## <a name="networking"></a>Netzwerk

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | -------  | -------- | ------ | ------ |
| Application Gateway | Ja | [Application Gateway REST](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS     | Ja | [DNS-REST](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | Ja  | [ExpressRoute REST](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Lastenausgleich | Ja  | [Lastenausgleich REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [LoadBalancer](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Traffic Manager | Ja | [Traffic Manager REST](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [2015-11-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtuelle Netzwerke | Ja| [Virtuelles Netzwerk REST](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| VPN-Gateway | Ja | [Netzwerk-Gateway REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[Anschlüsse](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Speicher

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Speicher | Ja  | [Storage REST](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Konto](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | Ja  |         |        |  |

## <a name="databases"></a>Datenbanken

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | Ja  | [DocumentDB REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis Cache | Ja |   | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| SQL-Datenbank | Ja | [SQL Datenbank REST](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014-04-01-Vorschau](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL Datawarehouse | Ja |   |      |


## <a name="web--mobile"></a>Web & Mobile

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------- | ------ | ------ |
| API-Apps | Ja |   | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [API-Apps](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API-Management | Ja  | [REST-API-Management](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016-07-07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Content Moderator | Ja |   |   |   |
| Funktion App | Ja |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Logik-Apps | Ja   | [Workflow Service REST-API](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobiler Apps | Ja |  | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Mobile Projekte | Ja  |  [Mobile Engagement REST](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Suche | Ja  | [Suche REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Webapps | Ja |          | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Analytics

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | -------  | -------- | ------ | ------ |
| Data Catalog | Ja  | [Data Catalog REST](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Data Factory | Ja | [Data Factory REST](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Datenanalyse See | Ja |   |   [2015-10-01-Vorschau](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Datenspeicher See | Ja  | [Datenspeicher See REST](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015-10-01-Vorschau](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | Ja | [HDInsights REST](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Computerlernen | Ja | [Lernen von anderen Computer](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016-05-01-Vorschau](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Stream Analytics | Ja  | [Stream Analytics REST](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| Power BI | Ja | [Power BI eingebettet REST](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016-01-29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Intelligenz

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------- | ------ | ------ |
| Kognitive Services | Ja |  | [2016-02-01-Vorschau](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Das Internet der Dinge

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------- | ------ | ------ |
| Ereignis-Hub | Ja  | [Ereignis-Hub REST](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | Ja | [IoT Hub REST](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Benachrichtigungshubs | Ja | [Benachrichtigungshub REST](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Medien & CDN

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------- | ------ | ------ |
| CDN | Ja | [CDN REST](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Media Service | Ja | [Media Services REST](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [2015-10-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Hybrid-Integration

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------- | ------ | ------ |
| BizTalk-Dienste | Ja |          | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Recovery-Service | Ja | [Site Recovery REST](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Servicebus | Ja | [Servicebus REST](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Identitäts- und Zugriffsmanagement 

Azure Active Directory arbeitet mit dem Ressourcen-Manager rollenbasierte Zugriffskontrolle für Ihr Abonnement aktivieren. Über rollenbasierte Zugriffskontrolle und Active Directory finden Sie unter [Azure Role-based Access Control](./active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Entwickler-Services 

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------- | ------ | ------ |
| Anwendung Einblicke | Ja  | [Einblicke REST](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.Insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Bing Maps | Ja  |          |        |  |
| DevTest Labs | Ja |   | [2016-05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Visual Studio-Konto | Ja   |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Management und Sicherheit

| Dienst | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------- | ------ | ------ |
| Automatisierung | Ja | [Automatisierung REST](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10-31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Key Vault | Ja | [Key Vault REST](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Key vault](resource-manager-template-keyvault.md)<br />[Geheime Schlüssel Depot](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Betriebliche Informationen | Ja |          |        |  |
| Planer | Ja  | [Planer REST](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [2016-03-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Sicherheit (Vorschau) | Ja | [Sicherheit REST](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Ressourcen-Manager

| Funktion | Ressourcen-Manager aktiviert | REST-API | Schema | Schnellstart-Vorlagen |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Autorisierung | Ja | [Management-Sperren](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Rollenbasierte Zugriffskontrolle](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Sperre für Ressource](resource-manager-template-lock.md)<br />[Arbeitsaufträge für Benutzerrollen](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Ressourcen | Ja | [Verknüpfte Ressourcen](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Ressourcen-links](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Ressourcen und Typen

Wenn Sie Ressourcen bereitstellen, müssen Sie häufig Informationen über Ressourcen und Typen. Sie können diese Informationen über REST-API, Azure PowerShell oder Azure CLI abrufen.

Um ein Ressourcenanbieter zu arbeiten, muss dieser Ressourcenanbieter mit Ihrem Konto registriert werden. Standardmäßig werden viele Ressourcenprovider automatisch registriert; Sie müssen jedoch einige Ressourcenprovider manuell registrieren. Die Beispiele zeigen, wie Registrierungsstatus ein Ressourcenanbieter und Registrieren des Ressourcenanbieters bei Bedarf.

### <a name="portal"></a>Portal

Sie können einfach eine Liste der unterstützten Ressourcen Anbieter mit den folgenden Schritten finden Sie unter:

1. Wählen Sie **Meine Berechtigungen**.

    ![Meine Berechtigungen](./media/resource-manager-supported-services/my-permissions.png)

2. **Ressourcenstatus-Anbieter** wählen

    ![Ressourcenstatus-Anbieter](./media/resource-manager-supported-services/resource-provider-status.png)

3. Finden Sie die vollständige Liste der Ressourcen für Ihr Abonnement.

    ![Liste der Ressourcen](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>REST-API

Um alle verfügbaren Ressourcen verwenden Anbieter, einschließlich Typen, Standorte, API-Versionen und Registrierungsstatus Vorgang [Alle Ressourcenprovider auflisten](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Einen Ressourcenanbieter registrieren, finden Sie unter [registrieren ein Abonnement mit einem Ressourcenanbieter](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>PowerShell

Im folgenden Beispiel wird veranschaulicht, wie alle verfügbaren Ressourcenanbieter.

    Get-AzureRmResourceProvider -ListAvailable
    

Das nächste Beispiel zeigt, wie die Ressourcentypen für eine bestimmte Ressourcenanbieter abgerufen.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
Die Ausgabe ähnelt:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Registrieren Sie einen Ressourcenanbieter bieten Sie den Namespace:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure CLI

Im folgenden Beispiel wird veranschaulicht, wie alle verfügbaren Ressourcenanbieter.

    azure provider list
    
Die Ausgabe ähnelt:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

Sie können die Informationen für eine bestimmte Ressourcenanbieter in eine Datei mit dem folgenden Befehl speichern.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Registrieren Sie einen Ressourcenanbieter bieten Sie den Namespace:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Unterstützten Regionen

Wenn Ressourcen bereitstellen, müssen Sie normalerweise einen Bereich für die Ressourcen festlegen. Ressourcen-Manager in allen Regionen unterstützt, aber die bereitgestellten Ressourcen möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus möglicherweise Grenzen Ihres Abonnements, die hindern Sie einige Bereiche, die die Ressource unterstützen. Diese Einschränkung im Zusammenhang mit Ihrem Land Steuerfragen oder das Ergebnis einer vom Systemadministrator Abonnement nur bestimmte Bereiche mit platziert. 

Eine vollständige Liste aller unterstützten Bereiche für alle Azure-Dienste Siehe [Dienste nach Region](https://azure.microsoft.com/regions/#services); Diese Liste kann jedoch Bereiche enthalten, die Ihr Abonnement nicht unterstützt. Sie können Regionen für einen bestimmten Ressourcentyp bestimmen, die Ihr Abonnement Portal, REST API, PowerShell oder Azure CLI unterstützt.

### <a name="portal"></a>Portal

Finden Sie unter unterstützten Regionen für einen Ressourcentyp folgende Schritte:

1. Wählen Sie **Weitere Dienste** > **Ressourcen-Explorer**.

    ![Ressourcen-explorer](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Öffnen Sie den Knoten **Anbieter** .

    ![Anbieter auswählen](./media/resource-manager-supported-services/select-providers.png)

3. Wählen Sie einen Ressourcenanbieter und zeigen Sie unterstützten Regionen und API-Versionen an.

    ![ansichtsanbieter](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>REST-API

Um zu ermitteln, welche Bereiche für einen bestimmten Ressourcentyp in Ihrem Abonnement verfügbar sind, verwenden Sie die [Liste aller Ressourcenprovider](https://msdn.microsoft.com/library/azure/dn790524.aspx) Operation. 

### <a name="powershell"></a>PowerShell

Im folgenden Beispiel wird veranschaulicht, wie Websites zu unterstützten Regionen.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
Die Ausgabe ähnelt:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure CLI

Das folgende Beispiel gibt alle unterstützten Speicherorte für jeden Ressourcentyp.

    azure location list

Sie können auch die führt mit einem JSON-Dienstprogramm wie [Jq](https://stedolan.github.io/jq/)filtern.

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Die zurückgibt:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Unterstützte API-Versionen

Beim Bereitstellen einer Vorlage müssen Sie für jede Ressource erstellt eine API-Version angeben. Die API-Version entspricht eine REST-API-Operationen, die der Ressourcenanbieter freigegeben werden. Ein Ressourcenanbieter neue Funktionen ermöglicht, veröffentlicht eine neue Version der REST-API. Daher betrifft die Version der API an die Vorlage Eigenschaften in der Vorlage können. Im Allgemeinen möchten Sie API aktuellste beim Erstellen von Vorlagen auswählen. Vorlagen können Sie entscheiden, ob weiterhin eine frühere API-Version oder die Vorlage für neue Features nutzen die neueste Version aktualisieren.

### <a name="portal"></a>Portal

Unterstützten API-Versionen bestimmt genauso Regionen bestimmt (zuvor) unterstützt.

### <a name="rest-api"></a>REST-API

Um zu ermitteln, welche API-Versionen für Typen verfügbar sind, verwenden Sie die [Liste aller Ressourcenprovider](https://msdn.microsoft.com/library/azure/dn790524.aspx) Operation. 

### <a name="powershell"></a>PowerShell

Im folgenden Beispiel wird veranschaulicht, wie zu API-Varianten für einen bestimmten Ressourcentyp.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
Die Ausgabe ähnelt:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure CLI

Sie können die Informationen (einschließlich API-Varianten) für ein Ressourcenanbieter in eine Datei mit dem folgenden Befehl speichern.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Öffnen Sie die Datei und suchen Sie das **ApiVersions** -element

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Ressourcen-Manager Vorlagen erstellen, [Erstellen von Azure Resource Manager Vorlagen](resource-group-authoring-templates.md)anzeigen
- Zum Bereitstellen von Ressourcen finden Sie unter [Bereitstellen einer Anwendung mit Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).
