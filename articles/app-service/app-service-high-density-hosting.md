<properties
    pageTitle="Hohe Dichte Hosten in Azure App Service | Microsoft Azure"
    description="Hohe Dichte auf Azure App Service hosten"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>Hohe Dichte auf Azure App Service hosten

Bei App Service ist die Anwendung von zwei Konzepte zugewiesene Kapazität entkoppelt:

- **Der Anwendung:** Die Anwendung und die Laufzeitkonfiguration darstellt. Beispielsweise enthält die Version von .NET, die die Common Language Runtime geladen werden soll, die app-Einstellungen usw..

- **App Service-Plan:** Definiert die Merkmale der Kapazität, verfügbare Featuresatz und Ort der Anwendung. Z. B. möglicherweise Eigenschaften Groß (vier Kerne) Computer, vier Instanzen Zusatzfunktionen im südostasiatischen US.

Eine Anwendung ist immer ein App Service-Plan verknüpft, aber App Service-Plan bietet Kapazität für apps.

Daher bietet die Plattform die Flexibilität eine einzige Anwendung isolieren oder mehrere apps Ressourcen gemeinsamen Nutzung eines App Service-Plans.

Wenn mehrere apps App Service-Plan freigeben, wird eine Instanz des app jedoch in jeder Instanz dieser App Service-Plan.

## <a name="per-app-scaling"></a>Pro Anwendung skalieren
*Pro Anwendung skalieren* ist eine Funktion, die auf Ebene der App Plan aktiviert und dann pro Anwendung verwendet werden kann.

Pro Anwendung skaliert Skalierung eine Anwendung unabhängig von der App Service-Plan, der es hostet. Auf diese Weise App Service-Plan zu 10 Instanzen konfiguriert werden jedoch nur 5 skalieren eine Anwendung eingestellt werden.

Die folgende Vorlage Azure-Ressourcen-Manager erstellt App Service-Plan, der mit 10 Instanzen und eine Anwendung pro Anwendung skalieren und auf bis zu 5 Instanzen konfiguriert skaliert wird.

App-Serviceplan **Skalierung der Site -** Eigenschaft auf True festlegen ( `"perSiteScaling": true`). Die Anwendung setzt die **Anzahl der Arbeitskräfte** auf 5 (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Empfohlene Konfiguration für high-Density-hosting

Pro Anwendung skalieren ist eine Funktion, die in öffentlichen Azure Regionen und App Service-Umgebungen aktiviert ist. Ist jedoch die empfohlene Strategie App Service-Umgebungen ihre Funktionen und größere Kapazität-Pools verwenden.  

Gehen Sie HD für Ihre apps konfigurieren

1. Konfigurieren Sie App Service-Umgebung, und wählen Sie eine Arbeitskraft, die hohe Dichte Hostingszenario zugeordnet ist.

1. Erstellen eines App Service-Plans und Skalieren die verfügbare Kapazität auf dem Arbeitspool verwenden.

1. Festlegen Sie Site-Skalierung Flag true App Service-Plan.

1. Neue Websites erstellt und zugewiesen, App Service-Plan mit der **NumberOfWorkers** -Eigenschaft auf **1**festgelegt. Mit dieser Konfiguration ergibt die höchste Dichte auf diesem Arbeitspool.

1. Anzahl von Arbeitskräften kann pro Standort nach Bedarf zusätzliche Ressourcen gewähren unabhängig voneinander konfiguriert werden. Beispielsweise kann eine Website hoch **3** , weitere Verarbeitungskapazität für die Anwendung während kaum Sites **NumberOfWorkers** **1**festgelegt werden **NumberOfWorkers** festgelegt.
