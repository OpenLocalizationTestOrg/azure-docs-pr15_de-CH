<properties
   pageTitle="Vorlage für Ressourcensperren Ressourcenmanager | Microsoft Azure"
   description="Zeigt das Schema Ressourcen-Manager für die Bereitstellung von Ressourcensperren über eine Vorlage."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="resource-locks-template-schema"></a>Ressource sperren Vorlage schema

Erstellt eine Sperre auf einer Ressource und untergeordneten Ressourcen.

## <a name="schema-format"></a>Schemaformat

Erstellen Sie eine Sperre Resources-Abschnitt der Vorlage fügen Sie das folgende Schema hinzu.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Werte

Die folgenden Tabellen beschreiben die Werte im Schema müssen.

| Name | Erforderlich | Beschreibung |
| ---- | -------- | ----------- |
| Typ | Ja | Der Ressourcentyp erstellen.<br /><br />Ressourcen:<br />**{Namespace} / {type} / Anbieter Sperren**<br /><br/>Für Ressourcengruppen:<br />**Microsoft.Authorization/locks** |
| Anforderungsparameter | Ja | Die API-Version zum Erstellen der Ressource.<br /><br />Verwendung:<br />**2015-01-01**<br /><br /> |
| Name | Ja | Ein Wert, der die Ressource sperren und einen Namen für die Sperre angibt. Bis zu 64 Zeichen und keine <>, %, &,?, oder alle Zeichen.<br /><br />Ressourcen:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Für Ressourcengruppen:<br />**{Lockname}** |
| dependsOn | Nein | Eine durch Trennzeichen getrennte Liste einer Ressource Namen oder eindeutiger Ressourcenbezeichner.<br /><br />Die Auflistung von Ressourcen, denen diese Sperre abhängt. Wenn die Ressource, die Sie Sperren in derselben Vorlage bereitgestellt wird, gehören Sie dem Ressourcennamen in diesem Element, um sicherzustellen, dass die Ressource zuerst bereitgestellt wird. | 
| Eigenschaften | Ja | Ein Objekt, das die Sperre und Hinweise für die Sperre angibt.<br /><br />Siehe [Properties-Objekt](#properties-object). |  

### <a name="properties-object"></a>Properties-Objekt

| Name | Erforderlich | Beschreibung |
| ---- | -------- | ----------- |
| Ebene   | Ja | Der Typ der Sperre für den Bereich.<br /><br />**CannotDelete** - Benutzer können Ressource ändern aber nicht löschen.<br />**ReadOnly** - Benutzer können aus einer Ressource gelesen, aber nicht löschen oder Aktionen auf. |
| Notizen   | Nein | Beschreibung der Sperre. Bis zu 512 Zeichen kann enthalten. |


## <a name="how-to-use-the-lock-resource"></a>Wie Sie Ressource

Fügen Sie hier Ihre Vorlage angegebenen Aktionen für eine Ressource zu. Die Sperre gilt für alle Benutzer und Gruppen.

Erstellen oder Löschen von Management sperren, müssen Sie Zugriff auf **Microsoft.Authorization/** * oder * *Microsoft.Authorization/locks/* ** Aktionen. Die integrierten Rollen nur **Eigentümer** und **Benutzer Zugriff Administrator ** diese Aktionen gewährt werden. Informationen über rollenbasierte Zugriffskontrolle finden Sie unter [Azure Role-based Access Control](./active-directory/role-based-access-control-configure.md).

Die Sperre wird auf die angegebene Ressource und alle untergeordneten Ressourcen angewendet.

Sie können eine Sperre mit PowerShell-Befehl **AzureRmResourceLock entfernen** oder [Löschen](https://msdn.microsoft.com/library/azure/mt204562.aspx) der REST API entfernen.

## <a name="examples"></a>Beispiele

Im folgenden Beispiel wird eine Sperre löschen nicht möglich, eine Webanwendung.

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
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

Das nächste Beispiel gilt eine Sperre kann nicht löschen der Ressourcengruppe.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Nächste Schritte

- Informationen über die Struktur der Vorlage finden Sie unter [Azure Ressourcenmanager erstellen Vorlagen](resource-group-authoring-templates.md).
- Weitere Hinweise zu Sperren finden Sie unter [Sperrenressourcen mit Azure-Ressourcen-Manager](resource-group-lock-resources.md).
