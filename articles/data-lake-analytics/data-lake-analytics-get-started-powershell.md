<properties 
   pageTitle="Erste Schritte mit Azure See Datenanalyse mithilfe von Azure PowerShell | Azure" 
   description="Informationen Sie zum Datenspeicher See Konto erstellen, erstellen Sie einen See Datenanalyse Druckauftrag U SQL Azure PowerShell verwenden und senden Sie den Auftrag. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Lernprogramm: Erste Schritte mit Azure See Datenanalyse mithilfe von Azure PowerShell

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informationen Sie zum Verwenden von Azure PowerShell Azure Data Lake Analytics Konten, [U-SQL](data-lake-analytics-u-sql-get-started.md)Data Lake Analytics Aufträge definieren und Aufträge auf See analytischen Daten. Weitere Informationen über See Datenanalyse Übersicht [Azure Data Lake Analytics](data-lake-analytics-overview.md).

In diesem Lernprogramm entwickeln Sie einen Auftrag, der liest eine Wertedatei (TSV) getrennt und konvertiert ihn in eine Datei mit kommagetrennten Werten (CSV). Gleiche Lernprogramm mit unterstützten Tools klicken Sie, die Registerkarten oben in diesem Abschnitt.

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).
- **Eine Workstation mit Azure PowerShell**. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Datenanalyse See-Konto erstellen

Datenanalyse See Konto müssen vor dem Ausführen alle Aufträge verwendet werden. Zum Erstellen eines Kontos Datenanalyse See müssen Sie Folgendes angeben:

- **Azure-Ressourcengruppe**: See Datenanalyse ein Konto in einer Azure-Ressourcengruppe erstellt werden muss. [Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md) können Sie die Ressourcen in der Anwendung als Gruppe arbeiten. Bereitstellen, aktualisieren, oder löschen alle Ressourcen für die Anwendung in einer einzigen koordinierten Operation.  

    Ressourcengruppen in Ihrem Abonnement aufgelistet werden:
    
        Get-AzureRmResourceGroup
    
    So erstellen Sie eine neue Ressourcengruppe

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Data Lake Analytics-Kontoname**
- **Lage**: eine Azure Rechenzentren, die Datenanalyse See unterstützt.
- **Standardmäßig Daten See Konto**: jedes Konto See Datenanalyse ist ein Standardkonto Daten See.

    So erstellen Sie ein neues Konto Daten See

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Daten See Kontoname darf nur Kleinbuchstaben und Zahlen enthalten.



**Erstellen eines Kontos See Datenanalyse**

1. Öffnen Sie Ihrer Arbeitsstation Windows PowerShell ISE.
2. Skript:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
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
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Upload von Daten Daten See

In diesem Lernprogramm werden Sie einige Protokolle Suche verarbeitet.  Suchprotokoll kann Daten See Informationsspeicher oder Azure BLOB-Speicher gespeichert werden. 

Eine Protokolldatei für die Suche wurde eine öffentliche Azure BLOB-Container kopiert. Verwenden Sie das folgende PowerShell-Skript Datei auf Ihrer Arbeitsstation, und Laden Sie die Datei auf See Datenspeicher Standardkonto Ihres Kontos See Datenanalyse.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

PowerShell-Skript veranschaulicht zu See Datenspeicher Standardnamen für ein Konto See Datenanalyse:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] Azure-Portal bietet eine Benutzeroberfläche, um die Beispieldateien in das Standardkonto See Datenspeicher kopieren. Informationen finden Sie in [Erste Schritte mit Azure See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Datenanalyse See können auch Azure BLOB-Speicher zugreifen.  Upload von Daten in Azure BLOB-Speicher, finden Sie unter [Verwenden von Azure PowerShell mit Azure-Speicher](../storage/storage-powershell-guide-full.md).

##<a name="submit-data-lake-analytics-jobs"></a>Datenanalyse See Aufträge

Datenanalyse See Aufträge werden in der U-SQL-Sprache geschrieben. Über U-SQL finden Sie unter [Erste Schritte mit U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md) und [U-SQL Referenzhandbuch](http://go.microsoft.com/fwlink/?LinkId=691348).

**Erstellen eines Skripts See Datenanalyse Auftrag**

- Eine Datei mit folgenden U-SQL-Skript erstellen und Speichern der Datei auf Ihrer Arbeitsstation:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Dieses U-SQL-Skript liest die Quelldatei mit **Extractors.Tsv()**und erstellt eine CSV-Datei mit **Outputters.Csv()**. 
    
    Ändern Sie nicht die beiden Pfade, sofern Sie die Quelldatei in einen anderen Speicherort kopieren.  Datenanalyse See erstellt den Ausgabeordner existiert nicht.
    
    Es ist einfacher, relative Pfade für Dateien im See Konten Daten. Sie können auch absolute Pfade.  Zum Beispiel 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Zugriff auf Dateien im verknüpften Speicherkonten müssen Sie absolute Pfade verwenden.  Die Syntax für Dateien verknüpfte Azure Storage-Konto lautet:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure BLOB-Container mit öffentlichen Blobs oder öffentlichen Container Berechtigungen werden derzeit nicht unterstützt.    
    
    
**Den Auftrag senden**

1. Öffnen Sie Ihrer Arbeitsstation Windows PowerShell ISE.
2. Skript:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    Im Skript wird bei c:\tutorials\data-lake-analytics\copyFile.usql U-SQL-Skriptdatei gespeichert. Aktualisieren Sie den Pfad entsprechend.
 
Nachdem der Auftrag abgeschlossen ist, können die folgenden Cmdlets verwenden, um die Datei und Laden Sie die Datei:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Siehe auch

- Klicken Sie das gleiche Lernprogramm mit anderen Tools Registerkarte Selektoren oben auf der Seite.
- Eine komplexe Abfrage finden Sie unter [Analysieren Website Protokolle mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Finden Sie zunächst Anwendungsentwicklung U-SQL [entwickeln U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL finden Sie unter [Einstieg in Azure Data Lake Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md).
- Management-Aufgaben finden Sie unter [Verwalten von Azure See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-manage-use-portal.md).
- Um einen Überblick der Datenanalyse See Übersicht [Azure Data Lake Analytics](data-lake-analytics-overview.md).

