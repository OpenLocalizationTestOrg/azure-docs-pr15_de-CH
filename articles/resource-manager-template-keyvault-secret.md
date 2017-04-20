<properties
   pageTitle="Ressourcen-Manager-Vorlage für einen Schlüssel in einem Schlüssel | Microsoft Azure"
   description="Zeigt das Schema der Ressourcen-Manager für die Bereitstellung von Key Vault Geheimnisse über eine Vorlage."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Key Vault geheimen Vorlage schema

Erstellt einen Schlüssel, der in einem Schlüssel gespeichert. Dieser Ressourcentyp wird häufig als untergeordnete Ressource [schlüsseltresors](resource-manager-template-keyvault.md)bereitgestellt.

## <a name="schema-format"></a>Schemaformat

Erstellen Sie ein Geheimnis Key Vault der Vorlage fügen Sie das folgende Schema hinzu. Der Schlüssel kann als eine untergeordnete Ressource eines schlüsseltresors oder als Ressource der obersten Ebene definiert werden. Sie können es als untergeordnete Ressource beim Definieren Schlüssel Depot in der gleichen Vorlage bereitgestellt wird. Sie müssen den geheimen Schlüssel als Ressource der obersten Ebene definieren, Key Vault nicht in derselben Vorlage bereitgestellt oder mehrere Kennwörter für den Ressourcentyp Schleife erstellen müssen. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Werte

Die folgenden Tabellen beschreiben die Werte im Schema müssen.

| Name | Wert |
| ---- | ---- | 
| Typ | Enum<br />Erforderlich<br />**Geheimnisse** (wird als untergeordnete Ressource Schlüssel Depot bereitgestellt) oder<br /> **Microsoft.KeyVault/vaults/secrets** (wenn der obersten Ebene Ressource bereitgestellt)<br /><br />Der Ressourcentyp erstellen. |
| Anforderungsparameter | Enum<br />Erforderlich<br />**2015-06-01** oder **2014-12-19-Vorschau**<br /><br />Die API-Version zum Erstellen der Ressource. | 
| Name | Zeichenfolge<br />Erforderlich<br />Wort wird als untergeordnete Ressource eines schlüsseltresors oder im Format bereitgestellt **{Vault-Schlüsselname} / {Schlüssel Name}** bei der Bereitstellung der obersten Ebene Ressource vorhandenen Schlüssel Speicher hinzugefügt werden.<br /><br />Der Name des geheimen Schlüssels zu erstellen. |
| Eigenschaften | Objekt<br />Erforderlich<br />[Properties-Objekt](#properties)<br /><br />Ein Objekt, der die geheimen Schlüssel erstellen angibt. |
| dependsOn | Array<br />Optionale<br />Eine durch Trennzeichen getrennte Liste einer Ressource Namen oder eindeutiger Ressourcenbezeichner.<br /><br />Die Auflistung von Ressourcen hier hängt. Wenn Schlüssel Depot für den Schlüssel in der gleichen Vorlage bereitgestellt wird, enthalten Sie den Namen des Schlüssels Depots in diesem Element, um sicherzustellen, dass zunächst bereitgestellt. |

<a id="properties" />
### <a name="properties-object"></a>Properties-Objekt

| Name | Wert |
| ---- | ---- | 
| Wert | Zeichenfolge<br />Erforderlich<br /><br />Geheimen Wert im Schlüssel Depot gespeichert. Beim Wert für diese Eigenschaft verwendet einen Parameter des Typs **Securestring**.  |

    
## <a name="examples"></a>Beispiele

Im erste Beispiel stellt einen geheimen Schlüssel als untergeordnete Ressource eines schlüsseltresors.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Im zweite Beispiel setzt den geheimen Schlüssel als Ressource der obersten Ebene, die in einem bereits existierenden +++ Schlüssel gespeichert.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen zu wichtigen Depots finden Sie unter [Erste Schritte mit Azure Schlüssel](./key-vault/key-vault-get-started.md).
- Beispielsweise auf ein Geheimnis Key Vault beim Bereitstellen von Vorlagen finden Sie unter [sichere Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).


