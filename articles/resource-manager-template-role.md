<properties
   pageTitle="Vorlage für Arbeitsaufträge für Benutzerrollen Ressourcenmanager | Microsoft Azure"
   description="Zeigt das Schema der Ressourcen-Manager für die Bereitstellung einer Zuweisung der Sicherheitsrolle über eine Vorlage."
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

# <a name="role-assignments-template-schema"></a>Rolle Arbeitsaufträge Vorlage schema

Benutzer, Gruppe oder Dienstprinzipal eine Rolle zugewiesen an einen angegebenen Bereich.

## <a name="resource-format"></a>Ressource formatieren

Erstellen Sie eine rollenzuordnung Resources-Abschnitt der Vorlage fügen Sie das folgende Schema hinzu.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Werte

Die folgenden Tabellen beschreiben die Werte im Schema müssen.

| Name | Erforderlich | Beschreibung |
| ---- | -------- | ----------- |
| Typ | Ja    | Der Ressourcentyp erstellen.<br /><br /> Ressourcengruppe:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Ressource:<br />**{Anbieternamespace} / {Ressourcentyp} / Provider RoleAssignments** |
| Anforderungsparameter |Ja | Die API-Version zum Erstellen der Ressource.<br /><br /> Verwenden Sie **2015-07-01**. | 
| Name | Ja | Ein global eindeutiger Bezeichner für die neue rollenzuweisung. |
| dependsOn | Nein | Eine durch Kommas getrennte Array einer Ressource Namen oder eindeutiger Ressourcenbezeichner.<br /><br />Die Auflistung von Ressourcen, denen Zuweisung der Sicherheitsrolle abhängig. Wenn eine Rolle zuweisen, die auf eine Ressource beschränkt und Ressource in der gleichen Vorlage bereitgestellt wird, gehören Sie dem Ressourcennamen in diesem Element, um sicherzustellen, dass die Ressource zuerst bereitgestellt wird. |
| Eigenschaften | Ja | Der Eigenschaftenobjekt, das die Funktionsdefinition Prinzipal und Bereich identifiziert. |

### <a name="properties-object"></a>Properties-Objekt

| Name | Erforderlich | Beschreibung |
| ---- | -------- | ----------- |
| roleDefinitionId | Ja |  Die Kennung einer vorhandenen Rollendefinition die Funktion Zuordnung verwendet werden.<br /><br /> Verwenden Sie das folgende Format:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Ja | Der global eindeutige Bezeichner für eine vorhandene Hauptbenutzer. Dieser Wert entspricht der Id im Verzeichnis und kann Benutzer, Dienstprinzipalnamen oder Sicherheitsgruppe. |
| Gültigkeitsbereich | Nein | Der Bereich mit dieser rollenzuordnung gilt.<br /><br />Verwenden Sie für Ressourcengruppen:<br />**/Subscriptions/ {Abonnement-Id} /resourceGroups/ {Ressource-Gruppenname}**  <br /><br />Für Ressourcen verwenden:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>Wie Sie die Funktion Zuordnung Ressource

Hinzufügen eine rollenzuweisung der Vorlage Wenn Sie einer Rolle Benutzer, Gruppe oder Dienstprinzipal während der Bereitstellung hinzufügen möchten. Arbeitsaufträge für Benutzerrollen werden von höheren Ebenen geerbt des Bereichs an, wenn Sie eine Funktion auf Ebene Abonnement bereits einen Principal hinzugefügt haben, müssen Sie nicht Ressourcengruppe oder Ressource zuweisen.

Gibt es viele Bezeichnerwerte bieten Arbeit mit Rollen zugewiesen werden müssen. Sie können die Werte über PowerShell oder Azure CLI abrufen.

### <a name="powershell"></a>PowerShell

Der Namen der rollenzuordnung erfordert einen global eindeutigen Bezeichner. Sie können eine neue Kennung für **Namen** :

    $name = [System.Guid]::NewGuid().toString()

Den Bezeichner für die Rollendefinition können abgerufen werden:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Sie können die ID für den Prinzipal mit einem der folgenden Befehle abrufen.

Eine Gruppe mit dem Namen **Prüfer**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Ein Benutzer mit dem Namen **Exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Ein Dienst mit dem Namen Principal **Exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure CLI

Den Bezeichner für die Rollendefinition können abgerufen werden:

    azure role show Reader --json | jq .[].Id -r

Sie können die ID für den Prinzipal mit einem der folgenden Befehle abrufen.

Eine Gruppe mit dem Namen **Prüfer**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Ein Benutzer mit dem Namen **Exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Ein Dienst mit dem Namen Principal **Exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Beispiele

Die folgende Vorlage erhält einen Bezeichner für eine Rolle und einen Bezeichner für Benutzer, Gruppe oder Dienstprinzipal. Die Rolle auf Ressource zugewiesen.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



Nächste Vorlage erstellt ein Speicherkonto und das Speicherkonto Rolle zugewiesen. Bezeichner für zwei Gruppen und Rolle in der Vorlage mühelos bereitgestellt wurden. Diese Werte bei der Bereitstellung über Skript abgerufen und als Parameter übergeben.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Die folgenden Vorlagen zeigen, wie die Funktion Zuordnung Ressource:

- [Ressourcengruppe integrierte Rolle zuweisen](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Vorhandenen virtuellen Computer integrierte Rolle zuweisen](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Mehrere vorhandene virtuelle Computer integrierte Rolle zuweisen](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Nächste Schritte

- Informationen über die Struktur der Vorlage finden Sie unter [Azure Ressourcenmanager erstellen Vorlagen](resource-group-authoring-templates.md).
- Weitere Informationen über rollenbasierte Zugriffskontrolle finden Sie in [Azure Active Directory Role-based Access Control](./active-directory/role-based-access-control-configure.md).
