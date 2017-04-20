<properties
    pageTitle="Computer lernen Arbeitsbereich Azure Ressourcenmanager Vorlage bereitstellen | Microsoft Azure"
    description="Arbeitsbereich für Azure maschinelles Lernen mit Azure Ressourcenmanager Vorlage bereitstellen"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Bereitstellen Sie Computer lernen Arbeitsbereich Azure Ressourcenmanager

## <a name="introduction"></a>Einführung
Mit einer Azure Ressourcenmanager Bereitstellungsvorlage Zeit spart und Sie können miteinander verbundenen Komponenten mit einer Validierung bereitstellen und Mechanismus wiederholen. Um Azure Machine Learning Arbeitsbereiche einzurichten, müssen Sie z. B. zunächst ein Konto Azure-Speicher konfigurieren und Bereitstellen Arbeitsbereich. Vorstellen Sie dies manuell Hunderte von Arbeitsbereichen. Eine einfachere Alternative ist Azure Ressourcenmanager Vorlage eine Azure Machine Learning-Arbeitsbereich und alle abhängigen. Dieser Artikel führt Sie durch diesen Schritt. Eine gute Übersicht der Azure-Ressourcen-Manager finden Sie unter [Übersicht über Azure Ressource-Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Schritt für Schritt: Erstellen eines Arbeitsbereichs Computer lernen
Wir Azure Ressourcengruppe erstellen, wird ein neues Azure Storage-Konto und einen neuen Azure Machine Learning-Arbeitsbereich Ressourcenmanager Vorlage bereitstellen. Nach Abschluss die Bereitstellung Drucken wir wichtige Informationen über die Arbeitsbereiche an, die (Primärschlüssel, der WorkspaceID und der URL zum Arbeitsbereich) erstellt wurden.

### <a name="create-an-azure-resource-manager-template"></a>Erstellen einer Vorlage Azure-Ressourcen-Manager
Ein Arbeitsbereich Computer lernen erfordert ein Konto Azure-Speicher zum Speichern des Datasets verknüpft.
Die folgende Vorlage verwendet den Namen der Ressourcengruppe zu Speicher-Kontonamen und dem Workspace-Namen.  Auch verwendet der speicherkontoname als Eigenschaft beim Arbeitsbereich erstellen.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Speichern Sie als Datei unter c:\temp\ mlworkspace.json.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Bereitstellen Sie die Ressourcengruppe, basierend auf der Vorlage
* PowerShell öffnen
* Installieren Sie Module für Azure-Ressourcen-Manager und Azure Service Management  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Folgendermaßen herunter und installieren die Module müssen die verbleibenden Schritte. Dies muss nur einmal in der Umgebung, die PowerShell-Befehle ausgeführt werden.   

* Authentifizierung in Azure  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Dieser Schritt muss für jede Sitzung wiederholt werden. Nach der Authentifizierung sollte Ihre Abonnementinformationen angezeigt.

![Ein Azure-Konto][1]

Jetzt haben wir Zugriff auf Azure können wir die Ressourcengruppe erstellen.

* Erstellen einer Ressourcengruppe

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Überprüfen Sie, ob die Ressource ordnungsgemäß bereitgestellt wird. **ProvisioningState** sollte "erfolgreich sein."
Der Ressourcengruppenname Vorlage dient speicherkontonamen zu. Speicher muss zwischen 3 und 24 Zeichen und Ziffern und nur Kleinbuchstaben.

![Ressourcengruppe][2]

* Mit der Bereitstellung einer Ressource, einen neuen Computer lernen Arbeitsbereich bereitstellen.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Nach Abschluss die Bereitstellung ist einfach, Eigenschaften von bereitgestellten Arbeitsbereich. Beispielsweise können Sie die primäre Schlüsseltoken zugreifen.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Können Token vorhandenen Arbeitsbereich abrufen ist der Invoke-AzureRmResourceAction verwendet. Beispielsweise können Sie die primären und sekundären Token aller Arbeitsbereiche auflisten.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Nachdem der Arbeitsbereich bereitgestellt wird, können Sie viele mit [PowerShell-Module für Azure Machine Learning](http://aka.ms/amlps)Azure Machine Learning Studio Aufgaben automatisieren.

## <a name="next-steps"></a>Nächste Schritte 
* Erfahren Sie mehr über [Authoring Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md). 
* Schauen Sie [Azure Schnellstart Vorlagen-Repository](https://github.com/Azure/azure-quickstart-templates). 
* Dieses Video über [Azure-Ressourcen-Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
