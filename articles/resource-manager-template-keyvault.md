<properties
   pageTitle="Ressourcen-Manager-Vorlage für Schlüssel Depot | Microsoft Azure"
   description="Zeigt das Schema der Ressourcen-Manager für die Bereitstellung von wichtigen Depots über eine Vorlage."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Key Vault Vorlage schema

Erstellt einen Schlüssel Depot.

## <a name="schema-format"></a>Schemaformat

Erstellen Sie ein Key Depot Resources-Abschnitt der Vorlage fügen Sie das folgende Schema hinzu.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Werte

Die folgenden Tabellen beschreiben die Werte im Schema müssen.

| Name | Wert |
| ---- | ---- | 
| Typ | Enum<br />Erforderlich<br />**Microsoft.KeyVault/vaults**<br /><br />Der Ressourcentyp erstellen. |
| Anforderungsparameter | Enum<br />Erforderlich<br />**2015-06-01** oder **2014-12-19-Vorschau**<br /><br />Die API-Version zum Erstellen der Ressource. | 
| Name | Zeichenfolge<br />Erforderlich<br />Einen eindeutigen Namen in Azure.<br /><br />Der Name des Schlüssels Depot erstellen. Verwenden der Funktion [UniqueString](resource-group-template-functions.md#uniquestring) mit der Namenskonvention erstellen einen eindeutigen Namen wie im folgenden Beispiel gezeigt. |
| Speicherort | Zeichenfolge<br />Erforderlich<br />Ein gültiger Bereich für wichtige Depots. Gültige Bereiche finden Sie unter [unterstützte Regionen](resource-manager-supported-services.md#supported-regions).<br /><br />Der Bereich Key Vault hosten. |
| Eigenschaften | Objekt<br />Erforderlich<br />[Properties-Objekt](#properties)<br /><br />Ein Objekt, das den Typ der wichtigsten Depot erstellen. |
| Ressourcen | Array<br />Optionale<br />Werte zulässig: [geheime Schlüssel Vault-Ressourcen](resource-manager-template-keyvault-secret.md)<br /><br />Untergeordnete Ressourcen für wichtige Depot. |

<a id="properties" />
### <a name="properties-object"></a>Properties-Objekt

| Name | Wert |
| ---- | ---- | 
| enabledForDeployment | Boolescher Wert<br />Optionale<br />**true** oder **false**<br /><br />Gibt an, ob das Depot für virtuelle Computer oder Dienstfabric-Bereitstellung aktiviert ist. |
| enabledForTemplateDeployment | Boolescher Wert<br />Optionale<br />**true** oder **false**<br /><br />Gibt an, ob das Depot für Ressourcenmanager Vorlage Bereitstellung aktiviert ist. Weitere Informationen finden Sie unter [sichere Werte während der Bereitstellung](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Boolescher Wert<br />Optionale<br />**true** oder **false**<br /><br />Gibt an, ob das Depot für Verschlüsselung aktiviert ist. |
| tenantId | Zeichenfolge<br />Erforderlich<br />**Global eindeutiger Bezeichner**<br /><br />Der Mieter Bezeichner für das Abonnement. Sie können das PowerShell-Cmdlet [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) oder **Azure Konto** Azure CLI-Befehl abrufen. |
| accessPolicies | Array<br />Erforderlich<br />[AccessPolicies-Objekt](#accesspolicies)<br /><br />Ein Array von bis zu 16 Objekte, die die Berechtigungen für den Benutzer oder die Dienstprinzipalnamen an. |
| SKU | Objekt<br />Erforderlich<br />[SKU-Objekt](#sku)<br /><br />Die SKU für Schlüssel Depot. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>properties.accessPolicies-Objekt

| Name | Wert |
| ---- | ---- | 
| tenantId | Zeichenfolge<br />Erforderlich<br />**Global eindeutiger Bezeichner**<br /><br />Der Mieter Bezeichner Azure Active Directory Mieter mit **ObjectId** in dieser Richtlinie |
| objectId | Zeichenfolge<br />Erforderlich<br />**Global eindeutiger Bezeichner**<br /><br />Die Objekt-ID der Azure Active Directory-Benutzer oder Dienstprinzipalnamen, die Zugriff auf den Tresor. Sie können den Wert von [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) oder [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) -PowerShell-Cmdlets oder **Azure Ad-Benutzerobjekt** oder **Azure Ad sp** Azure-CLI-Befehle abrufen. |
| Berechtigungen | Objekt<br />Erforderlich<br />[Berechtigungen Objekt](#permissions)<br /><br />Die Berechtigungen auf dieser Active Directory-Objekt. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>properties.accessPolicies.permissions-Objekt

| Name | Wert |
| ---- | ---- | 
| Schlüssel | Array<br />Erforderlich<br />**Alle** **backup** **Erstellen**, **Entschlüsseln**, **Löschen**, **Verschlüsseln**, **Abrufen**, **Importieren**, **Liste**, **Wiederherstellen**, **Zeichen**, **Unwrapkey**, **Aktualisieren**, **Stellen Sie sicher**, **wrapkey**<br /><br />Die Berechtigungen auf Schlüssel in diesem Depot, um Active Directory-Objekt. Dieser Wert muss als Array von einem oder mehreren zulässigen Werten angegeben werden. |
| Geheimnisse | Array<br />Erforderlich<br />**Alle** **Löschen** **Abrufen**, **Liste**, **festlegen**<br /><br />Die Berechtigungen für vertrauliche Informationen in diesen Tresor zu Active Directory-Objekt. Dieser Wert muss als Array von einem oder mehreren zulässigen Werten angegeben werden. |

<a id="sku" />
### <a name="propertiessku-object"></a>Properties.SKU-Objekt

| Name | Wert |
| ---- | ---- | 
| Name | Enum<br />Erforderlich<br />**Standard**oder **premium** <br /><br />Die Dienstebene Schlüsseltresor verwenden.  Standard unterstützt Kennwörter und Schlüssel Software geschützt.  Premium bietet Unterstützung für HSM-geschützten Schlüssel. |
| Familie | Enum<br />Erforderlich<br />**EIN** <br /><br />Die Sku-Familie verwenden. |
 
    
## <a name="examples"></a>Beispiele

Das folgende Beispiel setzt einen Schlüssel Depot und Schlüssel.

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

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Die folgende Vorlage Schnellstart bereit Key vault

- [Schlüsseltresor erstellen](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen zu wichtigen Depots finden Sie unter [Erste Schritte mit Azure Schlüssel](./key-vault/key-vault-get-started.md).
- Beispielsweise auf ein Geheimnis Key Vault beim Bereitstellen von Vorlagen finden Sie unter [sichere Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).

