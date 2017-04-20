<properties
   pageTitle="Vorlagen mit Ressourcen-Manager verknüpft | Microsoft Azure"
   description="Beschreibt, wie mit verknüpften Vorlagen in einer Vorlage Azure-Ressourcen-Manager eine modulare Vorlage Lösung erstellen. Veranschaulicht die Parameterwerte übergeben, einer Parameterdatei und dynamisch erstellten URLs angeben."
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
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Mithilfe von verknüpften Vorlagen mit Azure-Ressourcen-Manager

Aus können in einem Ressourcenmanager Azure Vorlage Sie verknüpfen eine andere Vorlage, wodurch Sie zerlegen die Bereitstellung in eine Reihe von Ziel, zweckbestimmte Vorlagen. Wie bei eine Anwendung in verschiedene Klassen zerlegen, bietet Zerlegung Vorteile testen, Wiederverwendung und Lesbarkeit.  

Sie können Parameter übergeben von einer Hauptvorlage Vorlage verknüpft und diese Parameter können direkt zuordnen, Parameter oder Variablen, die von der aufrufenden Vorlage verfügbar gemacht werden. Die verknüpfte Vorlage kann auch eine Ausgabevariable übergeben, der Quellvorlage aktivieren einen bidirektionalen Datenaustausch zwischen Vorlagen.

## <a name="linking-to-a-template"></a>Mit einer Vorlage verknüpfen

Sie erstellen eine Verknüpfung zwischen zwei Vorlagen durch Hinzufügen einer Bereitstellung Ressource innerhalb der Hauptvorlage, die die verknüpfte Vorlage verweist. Legen Sie die Eigenschaft **TemplateLink** auf den URI der Vorlage verknüpft. Sie können Parameterwerte für die verknüpfte Vorlage oder die Werte direkt in die Vorlage mit einer Datei verknüpfen bereitstellen. Im folgenden Beispiel wird die **Parameters** -Eigenschaft direkt an einen Parameterwert verwendet.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Der Ressourcen-Manager-Dienst muss auf die verknüpfte Vorlage. Sie können keine lokale Datei oder eine Datei, die nur im lokalen Netzwerk für die verknüpfte Vorlage angeben. Sie können nur einen URI-Wert, der entweder **http** oder **Https**enthält. Eine Möglichkeit ist die verknüpfte Vorlage in ein Speicherkonto, und verwenden Sie den URI für dieses Element wie im folgenden Beispiel gezeigt.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Obwohl die verknüpfte Vorlage extern verfügbar sein muss, muss es nicht öffentlich erhältlich. Sie können private Speicherkonto, die der Kontobesitzer Speicher für Ihre Vorlage hinzufügen. Dann erstellen Sie ein Signaturtoken gemeinsamen Zugriff (SAS) um den Zugriff während der Bereitstellung zu ermöglichen. Der URI für die verknüpfte Vorlage hinzugefügt SAS-Token. Eine Vorlage in ein Speicherkonto und generieren ein SAS-Token finden Sie unter Bereitstellen mit Ressourcen-Manager und Azure PowerShell oder [Ressourcen](resource-group-template-deploy.md) [Bereitstellen mit Ressourcen-Manager und Azure-CLI](resource-group-template-deploy-cli.md). 

Das folgende Beispiel zeigt einer übergeordnete Vorlage, die mit einer anderen Vorlage verknüpft. Die verknüpfte Vorlage erfolgt mit einem SAS-Token, das als Parameter übergeben wird.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Obwohl das Token als sichere Zeichenfolge übergeben wird, wird bei der Bereitstellung dieser Ressourcengruppe URI verknüpfte Vorlage, einschließlich SAS-Token protokolliert. Um Belichtung zu beschränken, legen Sie ein Ablaufdatum für das Token.

## <a name="linking-to-a-parameter-file"></a>Verknüpfung mit einer Datei

Im nächsten Beispiel wird mithilfe der **ParametersLink** -Eigenschaft mit einer Datei verknüpft.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

Der URI-Wert für die verknüpfte Datei keine lokale Datei oder sind **http** oder **Https**. Die Datei kann auch durch ein SAS-Token auf sein.

## <a name="using-variables-to-link-templates"></a>Verwenden von Variablen Vorlagen verknüpfen

Die vorherigen Beispiele hartcodierte URL Werte für die Vorlage Links. Dieser Ansatz funktioniert für eine einfache Vorlage, aber es funktioniert nicht auch beim Arbeiten mit einer Reihe von modularen Vorlagen. Stattdessen erstellen Sie eine statische Variable, die eine base-URL für die wichtigsten Vorlage gespeichert und anschließend URLs dynamisch verknüpften Vorlagen aus der Basis-URL. Der Vorteil dieses Ansatzes ist einfach verschieben oder Vorlage Verzweigung, da Sie nur statische Variable in der Hauptvorlage ändern können. Die Hauptvorlage übergibt die richtigen URIs in zerlegte Vorlage.

Im folgenden Beispiel wird veranschaulicht, wie eine base-URL verwenden, um zwei URLs für verknüpfte Vorlagen (**SharedTemplateUrl** und **VmTemplate**) erstellen. 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Sie können auch [Freigabe deklarieren()](resource-group-template-functions.md#deployment) der Basis-URL für die aktuelle Vorlage zu verwenden, bzw. die zu der URL für die anderen Vorlagen an derselben Stelle. Dieser Ansatz ist nützlich, wenn der Vorlagenpfad (vielleicht durch Versioning) ändert oder Sie die URLs in der Vorlagendatei codieren möchten. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Bedingt Verknüpfen mit Vorlagen

Sie können verschiedene Vorlagen Parameterwert übergeben verknüpfen, mit der den URI der verknüpften Vorlage erstellen. Dieser Ansatz funktioniert gut, wenn Sie während der Bereitstellung angeben, die zu verwendende Vorlage verknüpft. Beispielsweise können Sie eine Vorlage für ein vorhandenes Speicherkonto und eine andere Vorlage für ein neues Speicherkonto angeben.

Das folgende Beispiel zeigt einen Parameter für einen speicherkontonamen und Parameter festlegen, ob das Speicherkonto neue oder vorhandene.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Erstellen Sie eine Variable für die Vorlage URI, der den Wert der neuen oder vorhandenen Parameter enthält.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Geben Sie den Variablen Wert für Bereitstellung Ressource.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

Der URI löst eine Vorlage mit dem Namen **existingStorageAccount.json** oder **newStorageAccount.json**. Erstellen Sie Vorlagen für diese URIs.

Das folgende Beispiel zeigt die Vorlage **existingStorageAccount.json** .

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

Das nächste Beispiel zeigt die Vorlage **newStorageAccount.json** . Beachten Sie, dass wie die vorhandene Vorlage Konto Speicherobjekt Ausgaben zurückgegeben wurde. Die Mastervorlage arbeitet mit einer Vorlage verknüpft.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Vollständiges Beispiel

Die folgenden Beispielvorlagen anzeigen eine vereinfachte Anordnung von verknüpften Vorlagen einige Konzepte in diesem Artikel erläutern. Es wird angenommen, dass die Vorlagen auf den gleichen Container ein Speicherkonto mit öffentlichem Zugriff deaktiviert hinzugefügt wurden. Die verknüpfte Vorlage übergibt einen Wert an die wichtigsten Vorlage im Abschnitt **gibt** .

Die **parent.json** bestehen aus:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Die **helloworld.json** bestehen aus:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
In PowerShell einen Token für den Container abrufen und Bereitstellen von Vorlagen mit:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

In Azure CLI einen Token für den Container abrufen und Bereitstellen von Vorlagen mit dem folgenden Code. Derzeit müssen Sie einen Namen für die Bereitstellung eine Vorlage URI nutzen können, die ein SAS-Token enthält.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Sie werden aufgefordert, SAS-Token als Parameter angeben. Sie müssen das Token mit **?**Vorwort.

## <a name="next-steps"></a>Nächste Schritte
- Zum Definieren der Bereitstellungsreihenfolge für Ressourcen finden Sie unter [Abhängigkeiten in Azure Ressourcenmanager Vorlagen definieren](resource-group-define-dependencies.md)
- Informationen zum Definieren einer Ressource jedoch viele Instanzen erstellen, finden Sie unter [Erstellen Sie mehrere Instanzen der Ressourcen in Azure-Ressourcen-Manager](resource-group-create-multiple.md)
