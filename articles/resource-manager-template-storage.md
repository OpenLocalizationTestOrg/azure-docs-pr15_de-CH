<properties
   pageTitle="Vorlage für Storage Resource Manager | Microsoft Azure"
   description="Zeigt das Schema der Ressourcen-Manager für die Bereitstellung von Speicher-Konten über eine Vorlage."
   services="azure-resource-manager,storage"
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

# <a name="storage-account-template-schema"></a>Speicherschema Konto-Vorlage

Erstellt ein Speicherkonto.

## <a name="schema-format"></a>Schemaformat

Erstellen Sie ein Speicherkonto Resources-Abschnitt der Vorlage fügen Sie das folgende Schema hinzu.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Werte

Die folgenden Tabellen beschreiben die Werte im Schema müssen.

| Name | Wert |
| ---- | ---- |
| Typ | Enum<br />Erforderlich<br />**Microsoft.Storage/storageAccounts**<br /><br />Der Ressourcentyp erstellen. |
| Anforderungsparameter | Enum<br />Erforderlich<br />**2015-06-15** oder **2015-05-01-Vorschau**<br /><br />Die API-Version zum Erstellen der Ressource. | 
| Name | Zeichenfolge<br />Erforderlich<br />Zwischen 3 und 24 Zeichen, nur Zahlen und Kleinbuchstaben.<br /><br />Der Name des Speicher-Konto zu erstellen. Der Name muss auf allen Azure eindeutig sein. Sollten Sie die [UniqueString](resource-group-template-functions.md#uniquestring) -Funktion mit der Namenskonvention wie im folgenden Beispiel gezeigt. |
| Speicherort | Zeichenfolge<br />Erforderlich<br />Ein Bereich, der Speicherkonten unterstützt. Gültige Bereiche finden Sie unter [unterstützte Regionen](resource-manager-supported-services.md#supported-regions).<br /><br />Bereich für das Speicherkonto hosten. |
| Eigenschaften | Objekt<br />Erforderlich<br />[Properties-Objekt](#properties)<br /><br />Ein Objekt, das den Typ der Speicher-Konto zu erstellen. |

<a id="properties" />
### <a name="properties-object"></a>Properties-Objekt

| Name | Wert |
| ---- | ---- | 
| accountType | Zeichenfolge<br />Erforderlich<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**oder **Premium_LRS**<br /><br />Der Typ des Speicherkontos. Die zulässigen Werte entsprechen Standard lokal Redundant, Standard Zone redundante Standard Geo-Redundant, Standard Lesezugriff Geo-Redundant und Premium lokal Redundant. Informationen zu diesen Kontenarten finden Sie unter [Azure Storage Replication](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Beispiele

Im folgende Beispiel setzt ein Standard lokal redundante Speicherkonto ein eindeutiger Name für die Kennung der Ressource basiert.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Es gibt viele Schnellstart-Vorlagen, die ein Speicherkonto enthalten. Die folgenden Vorlagen veranschaulichen einige häufige Szenarien:

- [Erstellen Sie ein Konto Standardspeicher](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Einfache Bereitstellung von Windows-VM](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Einfache Bereitstellung von Linux VM](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Erstellen eines CDN-Profils CDN-Endpunkt ein Speicherkonto Ursprung](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Erstellen Sie eine hohe Verfügbarkeit SharePoint-Farm mit 9 VMs mit Powershell DSC-Erweiterung](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Einfache Bereitstellung von 5 Knoten sichern Service Fabric-Cluster mit Bündel aktiviert](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Erstellen Sie einen virtuellen Computer von einem Windows-Abbild mit 4 leere Datenträger](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen zum Speicher finden Sie unter [Einführung in Microsoft Azure-Speicher](./storage/storage-introduction.md).
- Beispielsweise Vorlagen, mit denen ein neues Speicherkonto mit einem virtuellen Computer finden Sie unter [Bereitstellen einer einfachen Linux VM](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) oder [Bereitstellen einer einfachen Windows-VM](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
