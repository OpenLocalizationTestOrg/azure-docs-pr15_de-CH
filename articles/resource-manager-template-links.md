<properties
   pageTitle="Ressourcen-Manager-Vorlage für die Verknüpfung von Ressourcen | Microsoft Azure"
   description="Zeigt das Schema der Ressourcen-Manager für die Bereitstellung von Links zwischen verknüpften Ressourcen über eine Vorlage."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="resource-links-template-schema"></a>Ressourcenschema Links-Vorlage

Erstellt eine Verknüpfung zwischen zwei Ressourcen. Die Verknüpfung wird eine Ressource als Quelle Ressource zugewiesen. Die Ressource wird die zweite Ressource in den Link genannt.

## <a name="schema-format"></a>Schemaformat

Resources-Abschnitt der Vorlage zum Erstellen eines Links fügen Sie im folgende Schema hinzu.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Werte

Die folgenden Tabellen beschreiben die Werte im Schema müssen.

| Name | Wert |
| ---- | ---- |
| Typ | Enum<br />Erforderlich<br />**{Namespace} / {type} / Anbieter Links**<br /><br />Der Ressourcentyp erstellen. {Namespace} und {} Werte beziehen sich auf die Anbieter und Datenquellen Ressource. |
| Anforderungsparameter | Enum<br />Erforderlich<br />**2015-01-01**<br /><br />Die API-Version zum Erstellen der Ressource. |  
| Name | Zeichenfolge<br />Erforderlich<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> bis zu 64 Zeichen und keine <>, %, &,?, oder alle Zeichen.<br /><br />Ein Wert, der den Namen der Quelle Ressource und einen Namen für die Verknüpfung angibt. |
| dependsOn | Array<br />Optionale<br />Eine durch Trennzeichen getrennte Liste einer Ressource Namen oder eindeutiger Ressourcenbezeichner.<br /><br />Die Auflistung von Ressourcen hier hängt. Wenn die Ressourcen, die Sie verknüpfen in derselben Vorlage bereitgestellt werden, enthalten Sie die Ressourcennamen in diesem Element, um sicherzustellen, dass sie zunächst bereitgestellt werden. | 
| Eigenschaften | Objekt<br />Erforderlich<br />[Properties-Objekt](#properties)<br /><br />Ein Objekt, das die Ressource verknüpfen identifiziert und Hinweise zur Verknüpfung. |  

<a id="properties" />
### <a name="properties-object"></a>Properties-Objekt

| Name | Wert |
| ------- | ---- |
| targetId | Zeichenfolge<br />Erforderlich<br />**{Ressourcen-Id}**<br /><br />Die Kennung der Ressource zu verknüpfen. |
| Notizen | Zeichenfolge<br />Optionale<br />bis zu 512 Zeichen<br /><br />Beschreibung der Sperre. |


## <a name="how-to-use-the-link-resource"></a>Verwendung die Ressource verknüpfen

Eine Verknüpfung zwischen zwei Ressourcen bei Ressourcen eine Abhängigkeit, die nach der Bereitstellung weiter anwenden. Beispielsweise kann eine Anwendung mit einer Datenbank in einer anderen Ressourcengruppe verbinden. Erstellen einer Verknüpfung von der Anwendung zur Datenbank können Sie diese Abhängigkeit definieren. Links können Sie die Beziehung zwischen zwei Ressourcen dokumentieren. Später können Sie oder andere Benutzer in Ihrer Organisation Abfragen eine Ressource für Links zu der Funktionsweise der Ressourcen mit anderen Ressourcen.

Alle verknüpfte Ressourcen müssen dieselbe Abonnement gehören. Jede Ressource kann mit 50 andere Ressourcen verknüpft. Verknüpfte Ressourcen gelöscht oder verschoben werden, muss bestehende Verknüpfung Link Besitzer bereinigen.

Arbeiten mit Hyperlinks bei [Verknüpften Ressourcen](https://msdn.microsoft.com/library/azure/mt238499.aspx)angezeigt.

Verwenden Sie den folgenden Azure PowerShell-Befehl alle Hyperlinks in Ihrem Abonnement. Sie können andere Parameter, um die Ergebnisse einzuschränken.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Beispiele

Im folgenden Beispiel wird eine schreibgeschützte Sperre Web App.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Die folgenden Vorlagen Schnellstart Bereitstellen von Ressourcen mit einem Link.

- [Warnung mit Logik-app in Warteschlange](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Warnung Pufferzeit mit Logik-app](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Bereitstellen einer API-Apps mit einem vorhandenen gateway](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Bereitstellen einer API-Apps mit einem neuen gateway](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Erstellen Sie eine Logik App und API-Anwendung mithilfe einer Vorlage](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Logik-app, die eine Textnachricht sendet, wenn eine Warnung ausgelöst wird](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Nächste Schritte

- Informationen über die Struktur der Vorlage finden Sie unter [Azure Ressourcenmanager erstellen Vorlagen](resource-group-authoring-templates.md).
