<properties
   pageTitle="Geheime Schlüssel Depot Ressourcenmanager Vorlage | Microsoft Azure"
   description="Veranschaulicht, wie ein Geheimnis während der Bereitstellung von einem Schlüssel Depot als Parameter übergeben."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Sichere Installation übergeben

Wenn Sie während der Bereitstellung einen sicheren Wert (z. B. ein Kennwort) als Parameter übergeben müssen, können Wert als Schlüssel in einer [Azure Key Vault](./key-vault/key-vault-whatis.md) speichern und Referenzwert in anderen Ressourcenmanager Vorlagen. Sie einbeziehen der Schlüssel niemals offen gelegt und nicht den Wert für den Schlüssel jedes Mal manuell Ressourcen bereitstellen müssen lediglich einen Verweis auf den Schlüssel in Ihre Vorlage. Sie angeben, welche Benutzer oder Hauptbenutzer Service den geheimen Schlüssel zugreifen können.  

## <a name="deploy-a-key-vault-and-secret"></a>Einen Schlüssel Depot und Schlüssel bereitstellen

Schlüssel Depot erstellen, die von anderen Ressourcen-Manager Vorlagen verwiesen werden kann, muss die **EnabledForTemplateDeployment** -Eigenschaft auf **true**festgelegt, und gewähren müssen Benutzer oder Dienstprinzipalnamen, die die Bereitstellung ausgeführt wird, die den Schlüssel verweist.

Zum Bereitstellen von einen Schlüssel Depot und Schlüssel finden Sie unter [Schlüssel Vault Schema](resource-manager-template-keyvault.md) und [geheime Schlüssel Vault-Schema](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>Ein Geheimnis mit statischen Id verweisen

Sie referenzieren den Schlüssel in einer Datei die Werte an die Vorlage übergeben. Den geheime Schlüssel wird den Ressourcenbezeichner der wichtigsten Depot und den Namen des geheimen Schlüssels verweisen. In diesem Beispiel geheime Schlüssel Depot muss bereits vorhanden sein und dabei einen statischen Wert dafür Ressourcennr.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Gesamte Parameterdatei aussehen könnte:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Der Parameter, der den Schlüssel akzeptiert sollte **Securestring**. Das folgende Beispiel zeigt die relevanten Abschnitte der Vorlage, die SQL Server bereitstellt, der ein Administratorkennwort erforderlich sind.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Ein Geheimnis mit dynamischen Id verweisen

Im vorangegangenen Abschnitt erläutert wie eine statische Ressource Id für geheime Schlüssel Depot übergeben. In einigen Szenarien müssen Sie jedoch ein Schlüssel Depot Geheimnis, das variiert basierend auf der aktuellen Bereitstellung verweisen. In diesem Fall nicht die Ressourcen-Id in der Parameterdatei hartcodieren. Leider können Sie dynamisch die Ressourcen-Id in der Parameterdatei generiert Vorlagenausdrücke in der Datei nicht zulässig sind.

Um die Ressourcen-Id für ein Geheimnis Key Vault dynamisch zu generieren, müssen Sie die Ressource, die den Schlüssel in eine verschachtelte Vorlage verschieben. In der Mastervorlage verschachtelte Vorlage hinzufügen und übergeben einen Parameter, der die dynamisch generierten Ressourcen-Id enthält.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen zu wichtigen Depots finden Sie unter [Erste Schritte mit Azure Schlüssel](./key-vault/key-vault-get-started.md).
- Informationen über wichtige Tresor mit einem virtuellen Computer finden Sie unter [Sicherheitsaspekte für Azure-Ressourcen-Manager](best-practices-resource-manager-security.md).
- Vollständige Beispiele für die wichtigsten Geheimnisse auf Beispiele für [Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

