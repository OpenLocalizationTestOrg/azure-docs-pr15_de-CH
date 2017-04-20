<properties
   pageTitle="Mehrere Instanzen der Ressourcen bereitstellen | Microsoft Azure"
   description="Verwenden Sie kopieren und Arrays in einer Vorlage Azure Ressourcenmanager durchlaufen mehrere Male beim Bereitstellen von Ressourcen."
   services="azure-resource-manager"
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
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Erstellen Sie mehrere Instanzen von Ressourcen in Azure-Ressourcen-Manager

In diesem Thema veranschaulicht die Dokumentvorlage Azure-Ressourcen-Manager mehrere Instanzen einer Ressource zu durchlaufen.

## <a name="copy-copyindex-and-length"></a>Kopieren, CopyIndex und Länge

Der Ressource mehrmals erstellen definieren Sie ein Objekt **Kopieren** , die angibt, wie oft durchlaufen. Die Kopie hat das folgende Format:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Sie können den aktuelle Iterationswert mit der Funktion **copyIndex()** wie folgt in die Funktion Concat zugreifen.

    [concat('examplecopy-', copyIndex())]

Beim Erstellen mehrerer Ressourcen aus einem Array von Werten können Funktion **Länge** Sie die Anzahl angeben. Das Array wird als Parameter für die Funktion bereitstellen.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Indexwert Namen verwenden

Sie können die Kopie Vorgang mehrere Instanzen einer Ressource eindeutig anhand der inkrementellen Index erstellen. Sie möchten beispielsweise eine eindeutige Nummer an das Ende jeder Ressourcenname hinzufügen, die bereitgestellt wird. Drei Websites namens bereitgestellt:

- Examplecopy 0
- Examplecopy-1
- Examplecopy-2.

Verwenden Sie die folgende Vorlage:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Offset Indexwert

Sie sehen im vorherigen Beispiel, die der Indexwert von 0 bis 2 geht. Um den Indexwert offset können Sie einen Wert in der Funktion **copyIndex()** wie **copyIndex(1)**übergeben. Die Anzahl der Iterationen durchführen noch im Copy-Element angegeben, aber der Wert des CopyIndex mit dem angegebenen Wert versetzt. Mit derselben Vorlage wie im vorherigen Beispiel, aber **copyIndex(1)** angeben würden also drei Websites namens bereitstellen:

- Examplecopy-1
- Examplecopy 2
- Examplecopy 3

## <a name="use-copy-with-array"></a>Kopie mit Array verwenden
   
Der Kopiervorgang ist besonders hilfreich beim Arbeiten mit Arrays, da jedes Element im Array durchlaufen werden kann. Drei Websites namens bereitgestellt:

- Contoso examplecopy
- Fabrikam examplecopy
- Examplecopy Coho

Verwenden Sie die folgende Vorlage:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Natürlich legen Sie Anzahl der Exemplare auf einen anderen Wert als die Länge des Arrays. Sie konnte z. B. ein Array mit vielen Werten erstellen und übergeben Parameter-Wert, der angibt, wie viele der Arrayelemente bereitstellen. In diesem Fall legen Sie Anzahl der Exemplare, wie im ersten Beispiel gezeigt. 

## <a name="depending-on-resources-in-a-loop"></a>Je nach Ressourcen in einer Schleife

Sie können angeben, dass eine Ressource mithilfe des **DependsOn** -Elements nach einer anderen Ressource bereitgestellt werden. Wenn Sie eine Ressource bereitstellen, die die Auflistung von Ressourcen in einer Schleife hängt benötigen, können Sie den Namen der kopierschleife im **DependsOn** -Element. Im folgenden Beispiel wird veranschaulicht, wie 3 Speicherkonten bereitstellen, bevor Sie den virtuellen Computer bereitstellen. Die vollständige Definition der virtuellen Maschine wird nicht angezeigt. Beachten Sie, dass das Copy-Element **Name** , **Storagecopy** und **DependsOn** -Element für die virtuellen Computer ist auch **Storagecopy**.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Eine geschachtelte Ressource Schleifen

Eine kopierschleife können keine geschachtelten Ressource. Benötigen Sie mehrere Instanzen einer Ressource erstellen, normalerweise als geschachtelte einer anderen Ressource definieren, müssen Sie stattdessen die Ressource eine Ressource der obersten Ebene erstellen und definieren die Beziehung mit den übergeordneten Eigenschaften **Typ** und **Namen** .

Angenommen Sie, ein Dataset in der Regel als geschachtelte Ressource in Data Factory definieren.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Um mehrere Instanzen eines Datasets zu erstellen, müssen Sie die Vorlage ändern, wie unten dargestellt. Beachten Sie den voll qualifizierten Typ und den Namen der Factory-Name enthält.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Erstellen Sie mehrerer Instanzen beim Kopieren nicht funktioniert

Sie können nur **Kopie** Ressourcentypen, nicht-Eigenschaften in einen Ressourcentyp. Dies kann Probleme für Sie erstellen, wenn mehrere Instanzen eines Objekts erstellen, der eine Ressource gehört. Häufig werden mehrere Datenträger für einen virtuellen Computer erstellen. **Kopieren** können mit dem Datenträger keine **DataDisks** ist eine Eigenschaft auf dem virtuellen Computer kein eigenes Ressourcentyp. Stattdessen erstellen Sie ein Array mit beliebig viele Datenträger wird, und übergeben Sie die Anzahl der Datenträger erstellen. Definition der virtuellen Funktion Sie **nehmen** nur die Anzahl der Elemente, die Sie tatsächlich aus dem Array abrufen.

Ein vollständiges Beispiel für dieses Muster ist in der Vorlage [Erstellen Sie einen virtuellen Computer mit dynamischen Datenträgern](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) anzeigen.

Die relevanten Abschnitte der Bereitstellungsvorlage werden unten angezeigt. Ein Großteil der Vorlage wurde entfernt, um dynamisch erstellen eine Anzahl Datenträger Abschnitte hervorheben. Beachten Sie die Parameter **NumDataDisks** , der die Anzahl der Laufwerke zu übergeben kann. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Nächste Schritte
- Wenn Sie Abschnitte der Vorlage erfahren möchten, finden Sie unter [Azure Resource Manager Vorlagen erstellen](./resource-group-authoring-templates.md).
- Alle Funktionen in einer Vorlage verwenden können, finden Sie unter [Azure Ressourcenmanager Vorlage Funktionen](./resource-group-template-functions.md).
- Bereitstellen die Vorlage finden Sie unter [Bereitstellen einer Anwendung mit Azure Ressourcenmanager Vorlage](resource-group-template-deploy.md).
