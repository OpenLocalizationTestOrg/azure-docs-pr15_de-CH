<properties
    pageTitle="Übersicht über das Bereitstellungsskript Azure-Ressourcengruppe Projekt | Microsoft Azure"
    description="Beschreibt die Funktionsweise von PowerShell-Skript im Bereitstellungsprojekt Azure-Ressourcengruppe."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Übersicht über das Bereitstellungsskript Ressourcengruppe Azure-Projekt

Azure-Ressourcengruppe Bereitstellungsprojekte Hilfe bereitstellen und Dateien und andere Artefakte in Azure bereitstellen. Beim Erstellen eines Bereitstellungsprojekts Azure Resource Manager in Visual Studio dem Projekt ein Powershellskript namens **Deploy-AzureResourceGroup.ps1** hinzugefügt. Dieses Thema enthält Details zur Funktionsweise dieses Skripts und sowohl innerhalb als auch außerhalb von Visual Studio ausführen.

## <a name="what-does-the-script-do"></a>Was das Skript?

Bereitstellung AzureResourceGroup.ps1 Skripts zwei Dinge wichtig Bereitstellungsworkflow.

- Hochladen von Dateien oder Artefakte für die vorlagenbereitstellung erforderlich
- Bereitstellen der Vorlage

Der erste Teil des Skripts lädt die Dateien und Elemente für die Bereitstellung und das letzte Cmdlet im Skript die Vorlage bereitzustellen. Beispielsweise ein virtuellen Computer mit einem Skript konfiguriert werden muss, lädt das Bereitstellungsskript zuerst sicher das Konfigurationsskript Azure Storage-Konto. Dadurch verfügbar, Azure-Ressourcen-Manager für den virtuellen Computer während der Bereitstellung konfigurieren.

Da nicht alle Vorlage Bereitstellung zusätzlicher Artefakte müssen, die übertragen werden müssen, wird der Switch Parameter *UploadArtifacts* ausgewertet. Wenn Artefakte übertragen werden müssen, den Schalter *UploadArtifacts* beim Aufrufen des Skripts. Beachten Sie, dass die wichtigsten Vorlagendatei und die Datei nicht hochgeladen werden. Nur andere Dateien wie Konfigurationsskripts, geschachtelte Anwendungsvorlagen und Dateien hochgeladen werden müssen.

## <a name="detailed-script-description"></a>Ausführliches Skript Beschreibung

Es folgt eine Beschreibung welche select Abschnitten bereitstellen AzureResourceGroup.ps1 Azure PowerShell Skripts führen.

>[AZURE.NOTE] Version 1.0 des Skripts bereitstellen AzureResourceGroup.ps1 beschreibt.

1.  Deklarieren Sie Parameter von Azure Ressourcenmanager Bereitstellungsprojekt. Einige Parameter sind Standardwerte, die beim Erstellen des Projekts festgelegt wurden. Sie können diese Standardwerte im Skript oder anderen Parameterwerten hinzufügen, bevor Sie das Skript ausführen.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Parameter|Beschreibung|
  	|---|---|
  	|$ResourceGroupLocation|Der Region oder des Rechenzentrums Standort für die Ressourcengruppe **Ostasien**oder **Westen der USA** .|
  	|$ResourceGroupName|Der Name der Azure-Ressourcengruppe.|
  	|$UploadArtifacts|Ein binärer Wert, der angibt, ob Elemente aus Ihrem System in Azure hochgeladen werden müssen.|
  	|$StorageAccountName|Der Name des Kontos Azure-Speicher, Artefakte hochgeladen werden.|
  	|$StorageAccountResourceGroupName|Der Name der Azure-Ressourcengruppe, die das Speicherkonto enthält.|
  	|$StorageContainerName|Der Name des zum Uploaden Artefakte Speichercontainer.|
  	|$TemplateFile|Der Pfad zur Bereitstellungsdatei (`<app name>.json`) im Projekt Azure-Ressourcengruppe.|
  	|$TemplateParametersFile|Der Pfad zu der Datei (`<app name>.parameters.json`) im Projekt Azure-Ressourcengruppe.|
  	|$ArtifactStagingDirectory|Der Pfad in Ihrem System, Artefakte lokal, einschließlich PowerShell Script-Stammordner hochgeladen werden. Dieser Pfad kann absolut oder relativ zum Speicherort des Skripts.|
  	|$AzCopyPath|Der Pfad, mit dem AzCopy.exe die ZIP-Dateien PowerShell Script-Stammverzeichnis kopiert. Dieser Pfad kann absolut oder relativ zum Speicherort des Skripts.|
  	|$DSCSourceFolder|Der Pfad in den Quellordner DSC (gewünschte Konfiguration), einschließlich der Stammordner der PowerShell-Skript. Dieser Pfad kann absolut oder relativ zum Speicherort des Skripts. Finden Sie unter [Einführung in Azure PowerShell DSC (gewünschte Konfiguration) Erweiterung](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), ggf. Weitere Informationen.|

1.  Überprüfen Sie, ob Artefakte in Azure hochgeladen werden müssen. Wenn nicht, fahren Sie mit Schritt 11 fort. Andernfalls führen Sie die folgenden Schritte aus.

1.  Konvertieren Sie alle Variablen mit relativen Pfade in absolute Pfade. Ändern beispielsweise einen Pfad wie `..\Tools\AzCopy.exe` , `C:\YourFolder\Tools\AzCopy.exe`. Außerdem initialisieren Sie die Variablen *ArtifactsLocationName* und *ArtifactsLocationSasTokenName* auf null. *ArtifactsLocation* und *SaSToken* möglicherweise Parameter der Vorlage. Wenn die Werte nach dem Einlesen der Parameterdatei null sind, generiert das Skript Werte für sie.

    Azure Tools verwenden die Werte für die Parameter *_artifactsLocation* und *_artifactsLocationSasToken* der Vorlage Artefakte zu verwalten. Wenn PowerShell-Skript Parameter mit Namen findet, aber die Werte der Parameter nicht angegeben, wird das Skript lädt die Artefakte und gibt die entsprechenden Werte für diese Parameter. Anschließend übergibt sie an das Cmdlet über `@OptionsParameters`.

  	|Variable|Beschreibung|
  	|---|---|
  	|ArtifactsLocationName|Der Pfad zu dem Azure Artefakte befinden.|
  	|ArtifactsLocationSasTokenName|Der SAS (SAS) Name token Service Bus Authentifizieren des Skripts verwendet wird. Weitere Informationen finden Sie unter [Freigegebene Signatur Authentifizierung mit Service Bus](service-bus-shared-access-signature-authentication.md) .|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Dieser Abschnitt überprüft, ob die <app name>. parameters.json-Datei (die Datei"Parameter" genannt) hat einen übergeordneten Knoten benannte **Parameter** (in der `else` Block). Andernfalls hat keinen übergeordneten Knoten. Beide Formate sind zulässig.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  Durchlaufen Sie die Auflistung der JSON-Parameter. Wenn ein Parameterwert *_artifactsLocation* oder *_artifactsLocationSasToken*zugewiesen wurde, legen Sie die Variable *$OptionalParameters* mit diesen Werten. Dadurch wird verhindert, dass das Skript versehentlich überschreiben alle Parameterwerte, die Sie bereitstellen.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Abrufen der Speicherschlüssel Konto und Kontext Konto Speicherressourcen zu Artefakten für die Bereitstellung verwendet.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Wenn Sie einen virtuellen Computer konfigurieren PowerShell DSC verwenden, erfordert DSC Artefakte in einer einzigen Zip-Datei. So erstellen Sie eine ZIP-Archivdatei für DSC-Konfiguration. Dazu überprüfen Sie, ob $DSCSourceFolder vorhanden ist. Existiert eine DSC-Konfiguration entfernen Sie und erstellen Sie eine neue komprimierte Datei namens dsc.zip.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Sofern kein Pfad für Azure Artefakte in der Parameterdatei, legen Sie einen Pfad für das PowerShell-Skript mit Artefakten übertragen. Erstellen Sie dazu einen Pfad mit einer Kombination aus das Speicherkonto endpunktpfad plus den Containernamen Speicher. Dann aktualisieren Sie die Datei mit diesem neuen Pfad.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Verwenden Sie **AzCopy** Utility (enthalten im **Ordner des Bereitstellungsprojekts Azure-Ressourcengruppe** ) Dateien aus dem lokalen Speicher ablegen Pfad in Ihrem Onlinekonto Azure-Speicher kopieren. Wenn dieser Schritt fehlschlägt, beendet das Skript seit die Bereitstellung nicht ohne die erforderlichen Elemente erfolgreich abgeschlossen werden dürfte.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Wenn ein SAS-Token für den Standort Artefakte in der Datei enthalten ist, erstellen Sie eine temporäre nur-Lese-Zugriff auf Online-Behälter. Übergeben Sie SAS-Token an der Befehlszeile als "OptionalParameter". Beachten Sie, dass alle Parameter auf der Befehlszeile Werte in der Datei Vorrang.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Erstellen der Ressourcengruppe noch vorhanden und Vorlage und Parameter der Datei Validierungsfehler, die vor die Bereitstellung verhindern.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Stellen Sie abschließend die Vorlage. Dieser Code erstellt einen eindeutigen Namen für die Bereitstellung mit einem Zeitstempel.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Bereitstellen der Ressourcengruppe

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Bereitstellen die Ressourcengruppe in Visual Studio

1. Wählen Sie im Kontextmenü des Projekts Azure-Ressourcengruppe **Bereitstellen** > **Neue Bereitstellung**.

    ![][0]

1. Klicken Sie im Dialogfeld **Ressource bereitstellen** entweder wählen Sie eine vorhandene Ressourcengruppe im Dropdown-Listenfeld bereitzustellen oder ** &lt;neu erstellen... &gt;** Erstellen Sie eine neue Ressourcengruppe.

    ![][1]

1. Wenn Sie aufgefordert werden, geben Sie einen Namen und einen Speicherort im Dialogfeld **Ressourcengruppe erstellen** und dann **Erstellen** .

    ![][2]

1. Wählen Sie auf **Parameter bearbeiten** , zeigt das Dialogfeld **Parameter bearbeiten** , und geben alle fehlenden Parameterwerte.

    ![][3]

    >[AZURE.NOTE] Wenn alle erforderlichen Parameter Werte, wird dieses Dialogfeld beim Bereitstellen automatisch.

    ![][4]

1. Sie geben Sie Parameterwerte, wählen die Schaltfläche **Speichern** und wählen Sie dann die Schaltfläche **Bereitstellen** .

    Führt das Bereitstellungsskript (Deploy-AzureResourceGroup.ps1) und Ihre Vorlage sowie alle Artefakte in Azure bereitgestellt.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>Die Ressourcengruppe mit PowerShell bereitstellen

Möchten Sie das Skript ausführen, ohne den Befehl Visual Studio bereitstellen und die Benutzeroberfläche im Kontextmenü für das Skript, wählen Sie **mit PowerShell ISE**.

![][5]


## <a name="command-deployment-examples"></a>Befehl Bereitstellungsbeispiele

### <a name="deploy-using-default-values"></a>Mithilfe von Standardwerten

Dieses Beispiel zeigt, wie die Standardparameterwerte mit Skript. (Da Standortparameter keinen Standardwert, müssen Sie zur Verfügung.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Bereitstellen Sie Überschreiben der Standardwerte

Dieses Beispiel veranschaulicht das Skript Vorlage und Parameter-Dateien bereit, die von den Standardwerten abweichen.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Mithilfe von UploadArtifacts für die Bereitstellung

Dieses Beispiel zeigt, wie das Skript zum Hochladen aus den Releaseordner und nicht standardmäßigen Vorlagen bereitstellen.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

Dieses Beispiel zeigt, wie das Skript in einer Azure PowerShell Aufgabe in Visual Studio.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über Azure-Ressourcen-Manager [Azure Ressourcenmanager Übersicht](azure-resource-manager/resource-group-overview.md)lesen.

Weitere Beispiele für die Arbeit mit Azure-Ressourcengruppe Projekten finden Sie unter [Bereitstellen und Verwalten von Azure Ressourcen](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinden [Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Weitere Schnellstarts HealthClinic.biz Demo finden Sie unter [Azure Developer Tools Schnellstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
