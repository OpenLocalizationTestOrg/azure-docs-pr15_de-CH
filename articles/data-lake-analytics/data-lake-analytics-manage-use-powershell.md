<properties 
   pageTitle="Verwalten von Azure See Datenanalyse mithilfe von Azure PowerShell | Azure" 
   description="Informationen Sie zum See Datenanalyse Aufträge, Datenquellen, Benutzer verwalten. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Verwalten von Azure See Datenanalyse mithilfe von Azure PowerShell

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Informationen Sie zum Verwalten von Azure Data Lake Analytics Konten, Datenquellen, Benutzer und Azure PowerShell mit. Klicken Sie Themen mit anderen Tools die Registerkarte wählen oben.

**Erforderliche Komponenten**

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


##<a name="install-azure-powershell-10-or-greater"></a>Installieren Sie Azure PowerShell 1.0 oder höher

Siehe Abschnitt erforderliche [Mithilfe von Azure PowerShell mit Azure-Ressourcen-Manager](powershell-azure-resource-manager.md#prerequisites).
    
## <a name="manage-accounts"></a>Konten verwalten

Vor dem Ausführen von Datenanalysen See Aufträge, benötigen Sie ein Konto See Datenanalyse. Im Gegensatz zu Azure HDInsight bezahlen nicht Sie Analytics-Konto, wenn keinen Auftrag ausgeführt wird.  Sie Zahlen nur für die Zeit, wenn sie einen Auftrag ausgeführt wird.  Weitere Informationen finden Sie unter [Azure Data Lake Analytics Overview](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Erstellen von Konten

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeStoreName = "<DataLakeAccountName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $location = "<Microsoft Data Center>"
    
    Write-Host "Create a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location
    
    Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
    New-AzureRmDataLakeStoreAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeStoreName `
        -Location $location 
    
    Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
    New-AzureRmDataLakeAnalyticsAccount `
        -Name $dataLakeAnalyticsAccountName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName
    
    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsAccountName  

Sie können auch eine Vorlage Azure-Ressourcengruppe. Eine Vorlage ein Konto See Datenanalyse und abhängige See Datenspeicher-Konto ist in [Anhang A](#appendix-a). Speichern Sie die Vorlage in einer Datei mit JSON, und verwenden Sie das folgende PowerShell-Skript aufrufen:


    $AzureSubscriptionID = "<Your Azure Subscription ID>"
    
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    $DefaultDataLakeStoreAccountName = "<New Data Lake Store Account Name>"
    $DataLakeAnalyticsAccountName = "<New Data Lake Analytics Account Name>"
    
    $DeploymentName = "MyDataLakeAnalyticsDeployment"
    $ARMTemplateFile = "E:\Tutorials\ADL\ARMTemplate\azuredeploy.json"  # update the Json template path 
    
    Login-AzureRmAccount
    
    Select-AzureRmSubscription -SubscriptionId $AzureSubscriptionID
    
    # Create the resource group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    # Create the Data Lake Analytics account with the default Data Lake Store account.
    $parameters = @{"adlAnalyticsName"=$DataLakeAnalyticsAccountName; "adlStoreName"=$DefaultDataLakeStoreAccountName}
    New-AzureRmResourceGroupDeployment -Name $DeploymentName -ResourceGroupName $ResourceGroupName -TemplateFile $ARMTemplateFile -TemplateParameterObject $parameters 

 
###<a name="list-account"></a>Konto

Liste Data Lake Analytics Konten innerhalb des aktuellen Abonnements

    Get-AzureRmDataLakeAnalyticsAccount
    
Ausgabe:

    Id         : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/learn1021rg/providers/Microsoft.DataLakeAnalytics/accounts/learn1021adla
    Location   : eastus2
    Name       : learn1021adla
    Properties : Microsoft.Azure.Management.DataLake.Analytics.Models.DataLakeAnalyticsAccountProperties
    Tags       : {}
    Type       : Microsoft.DataLakeAnalytics/accounts

Liste Data Lake Analytics Konten innerhalb einer Ressourcengruppe

    Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName

Abrufen von Details eines bestimmten Kontos See Datenanalyse

    Get-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Testen einer bestimmten See Datenanalyse-Konto

    Test-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Das Cmdlet gibt **True** oder **False**zurück.

###<a name="delete-data-lake-analytics-accounts"></a>Datenanalyse See löschen

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Remove-AzureRmDataLakeAnalyticsAccount -Name $dataLakeAnalyticsAccountName 

Delete Data Lake Analytics-Konto wird nicht von dem Datenspeicher-Konto gelöscht. Das folgende Beispiel löscht See Datenanalyse und die Standard-See Datenspeicher

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount

    Remove-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName 
    Remove-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Konto-Datenquellen verwalten

Datenanalyse See unterstützt derzeit die folgenden Datenquellen:

- [Azure See Datenspeicher](../data-lake-store/data-lake-store-overview.md)
- [Azure-Speicher](storage-introduction.md)

Beim Erstellen eines Analytics-Kontos müssen Sie ein Azure See Datenspeicher Konto das Standardkonto Speicher festlegen. Das Standardkonto See Datenspeicher zum Auftrag Metadaten und Auftrag Überwachungsprotokolle speichern. Nachdem Sie ein Analytics-Konto erstellt haben, können Sie zusätzliche Daten See Speicherkonten und Azure Storage-Konto hinzufügen. 

### <a name="find-the-default-data-lake-store-account"></a>Suchen Sie das Standardkonto See Datenspeicher

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount


### <a name="add-additional-azure-blob-storage-accounts"></a>Fügen Sie zusätzliche Azure BLOB-Speicherkonten

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureStorageAccountName = "<AzureStorageAccountName>"
    $AzureStorageAccountKey = "<AzureStorageAccountKey>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -AzureBlob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

### <a name="add-additional-data-lake-store-accounts"></a>Fügen Sie zusätzliche Datenspeicher See Konten

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureDataLakeName = "<DataLakeStoreName>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -DataLake $AzureDataLakeName 

### <a name="list-data-sources"></a>Liste von Datenquellen:

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DataLakeStoreAccounts
    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.StorageAccounts
    


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-jobs"></a>Aufträge verwalten

Vor der Erstellung eines Auftrags, benötigen Sie ein Konto See Datenanalyse.  Weitere Informationen finden Sie unter [Konten verwalten See Datenanalyse](#manage-data-lake-analytics-accounts).

### <a name="list-jobs"></a>Liste Aufträge

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -State Running, Queued
    #States: Accepted, Compiling, Ended, New, Paused, Queued, Running, Scheduling, Starting
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Result Cancelled
    #Results: Cancelled, Failed, None, Successed 
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Name <Job Name>
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Submitter <Job submitter>

    # List all jobs submitted on January 1 (local time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter "2015/01/01"
        -SubmittedBefore "2015/01/02"   

    # List all jobs that succeeded on January 1 after 2 pm (UTC time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -State Ended
        -Result Succeeded
        -SubmittedAfter "2015/01/01 2:00 PM -0"
        -SubmittedBefore "2015/01/02 -0"

    # List all jobs submitted in the past hour
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter (Get-Date).AddHours(-1)

### <a name="get-job-details"></a>Job-Details abrufen

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -JobID <Job ID>
    
### <a name="submit-jobs"></a>Aufträge

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    #Pass script via path
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -ScriptPath $scriptPath

    #Pass script contents
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -Script $scriptContents

> [AZURE.NOTE] Die Standardpriorität eines Auftrags ist 1000 und der Standard-Grad der Parallelität für einen Auftrag ist 1.


### <a name="cancel-jobs"></a>Aufträge abbrechen

    Stop-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -JobID $jobID


## <a name="manage-catalog-items"></a>Katalogelemente verwalten

U-SQL Katalog dient und strukturiert, damit sie von U-SQL-Skripts verwendet werden können. Der Katalog ermöglicht die höchste Leistung mit Daten in Azure Data Lake. Weitere Informationen finden Sie unter [verwenden U-SQL-Katalog](data-lake-analytics-use-u-sql-catalog.md).

###<a name="list-catalog-items"></a>Katalogelemente

    #List databases
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database
    
    
    
    #List tables
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo"

###<a name="get-catalog-item-details"></a>Katalog Elementinformationen abrufen 

    #Get a database
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"
    
    #Get a table
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo.mytable"

###<a name="test-existence-of--catalog-item"></a>Vorhandensein Katalogelement testen

    Test-AzureRmDataLakeAnalyticsCatalogItem  `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"

###<a name="create-catalog-secret"></a>Katalog-Schlüssel erstellen
    New-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")

### <a name="modify-catalog-secret"></a>Katalog-Schlüssel ändern
    Set-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")



###<a name="delete-catalog-secret"></a>Katalog Schlüssel löschen
    Remove-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master"


## <a name="use-azure-resource-manager-groups"></a>Azure Ressourcenmanager Gruppen

Anwendung normalerweise viele Komponenten, z. B. eine Webanwendung, Datenbank Datenbank, Speicher und 3. Dienste bestehen. Azure Resource Manager (ARM) können Sie die Ressourcen in der Anwendung als Gruppe als eine Azure-Ressourcengruppe arbeiten. Sie bereitstellen, aktualisieren, überwachen oder alle Ressourcen für die Anwendung in einem einzigen koordinierten Vorgang löschen. Verwenden Sie eine Vorlage für die Bereitstellung und die Vorlage kann für verschiedene Unternehmen wie Tests, Staging und Produktion. Abrechnung für Ihr Unternehmen zu klären die mehrstufigen Kosten für die gesamte Gruppe anzeigen. Weitere Informationen finden Sie unter [Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md). 

Datenanalyse See Service kann folgenden Komponenten umfassen:

- Azure Data Lake Analytics-Konto
- Erforderliche Azure See Datenspeicher Konto
- Zusätzliche Azure Data Lake Speicherkonten
- Zusätzliche Azure-Speicherkonten

Sie können diese Komponenten einen ARM Gruppe leichter zu erstellen.

![Azure Data Lake Analytics Konto und Speicher](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Ein Konto See Datenanalyse und abhängige Speicherkonten müssen im gleichen Azure-Rechenzentrum befinden.
ARM-Gruppe kann jedoch in einem anderen Rechenzentrum befinden.  

##<a name="see-also"></a>Siehe auch 

- [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
- [Erste Schritte mit See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md)
- [Verwalten Sie Azure See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-manage-use-portal.md)
- [Überwachung und Problembehandlung von Azure Data Lake Analytics Aufträge mithilfe von Azure-Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

##<a name="appendix-a---data-lake-analytics-arm-template"></a>Anhang A - Data Lake Analytics ARM-Vorlage

Folgende ARM-Vorlage kann verwendet werden, ein Konto See Datenanalyse und die abhängigen See Datenspeicher bereitgestellt.  Speichern Sie als Json-Datei, und verwenden Sie PowerShell-Skript die Vorlage aufrufen. Weitere Informationen finden Sie unter [Bereitstellen einer Anwendung mit Azure Ressourcenmanager Vorlage](../resource-group-template-deploy.md#deploy-with-powershell) und [Erstellen von Azure Resource Manager Vorlagen](../resource-group-authoring-templates.md).

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adlAnalyticsName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Analytics account to create."
          }
        },
        "adlStoreName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Store account to create."
          }
        }
      },
      "resources": [
        {
          "name": "[parameters('adlStoreName')]",
          "type": "Microsoft.DataLakeStore/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ ],
          "tags": { }
        },
        {
          "name": "[parameters('adlAnalyticsName')]",
          "type": "Microsoft.DataLakeAnalytics/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
          "tags": { },
          "properties": {
            "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
            "dataLakeStoreAccounts": [
              { "name": "[parameters('adlStoreName')]" }
            ]
          }
        }
      ],
      "outputs": {
        "adlAnalyticsAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
        },
        "adlStoreAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
        }
      }
    }

