<properties
    pageTitle="Aktivieren der Diagnose in Azure-Cloud-Diensten mit PowerShell | Microsoft Azure"
    description="Aktivieren Sie Diagnose für Cloud-Diensten mit PowerShell"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Aktivieren Sie Diagnose in Azure-Cloud-Diensten mit PowerShell

Erfassen von Diagnosedaten wie Anwendungsprotokolle, Leistungsindikator aus einem Clouddienst Azure-Diagnose-Erweiterung verwenden. Dieser Artikel beschreibt die Erweiterung Azure Diagnostics für einen Cloud-Dienst mit PowerShell aktivieren.  Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md) für die erforderlichen Komponenten für diesen Artikel benötigt.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Aktivieren Sie diagnoseerweiterung als Teil der Bereitstellung einer Cloud-Dienst

Dieser Ansatz für fortlaufende Integrationstyp Szenarien, die diagnoseerweiterung als Teil der Bereitstellung von Cloud-Dienst aktiviert werden kann. Beim Erstellen einer neuen Bereitstellung von Cloud-Dienst können Sie die diagnoseerweiterung *ExtensionConfiguration* Parameter [Neu-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) -Cmdlet übergeben. Der *ExtensionConfiguration* -Parameter akzeptiert ein Array von Diagnose-Konfigurationen, die mit dem [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) -Cmdlet erstellt werden können.

Das folgende Beispiel zeigt, wie Sie Diagnostics für einen Cloud-Dienst mit WebRole WorkerRole mit einer anderen Diagnosekonfiguration aktivieren können.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Wenn Diagnose-Konfigurationsdatei ein StorageAccount-Element mit einem Storage-Kontonamen gibt verwendet das New-AzureServiceDiagnosticsExtensionConfig-Cmdlet automatisch diesem Storage-Konto. Zu diesem Zweck muss das Speicherkonto in dieselbe Abonnement als Cloud-Dienst bereitgestellt wird.

Von Azure SDK 2.6 weiter von MSBuild generiert Erweiterungskonfigurationsdateien veröffentlichen einschließen Zielausgabe des Speicher-Kontonamens basierend auf Diagnose Konfiguration angegebenen Zeichenfolge in der Service-Konfigurationsdatei (.cscfg). Das folgende Skript veranschaulicht zu analysieren, die Erweiterungskonfigurationsdateien Zielausgabe veröffentlichen diagnoseerweiterung für jede Rolle bei der Bereitstellung von Cloud-Dienst konfigurieren.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online verwendet einen ähnlichen Ansatz für automatisierte Deploymnts von Clouddiensten Diagnose-Erweiterung. Ein vollständiges Beispiel finden Sie unter [AzureCloudDeployment.ps1 veröffentlichen](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) .

Wenn keine StorageAccount in der Diagnosekonfiguration angegeben wurde, müssen Sie das Cmdlet in der StorageAccountName-Parameter übergeben. Wenn der StorageAccountName-Parameter angegeben ist, wird in das Cmdlet immer das Speicherkonto verwenden, das Parameters und nicht das Element, das in der Diagnose-Konfigurationsdatei angegebene angegeben.

Wenn Diagnose Speicherkonto in ein anderes Abonnement aus Cloud-Dienst ist, müssen Sie explizit die Parameter StorageAccountName und StorageAccountKey an das Cmdlet übergeben. Der StorageAccountKey-Parameter ist nicht erforderlich, wenn Diagnose Speicherkonto in dieselbe Abonnement ist das Cmdlet automatisch Abfragen und setzen Sie den Wert beim Aktivieren der diagnoseerweiterung. Allerdings ist das Speicherkonto Diagnose in ein anderes Abonnement, das Cmdlet möglicherweise nicht den Schlüssel automatisch abgerufen und müssen Sie explizit den Schlüssel durch den StorageAccountKey-Parameter angeben.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Diagnoseerweiterung auf einen vorhandenen Cloud-Dienst aktivieren

Verwenden Sie das Cmdlet [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) aktivieren oder update Diagnosekonfiguration auf einen Cloud-Dienst bereits ausgeführt wird.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Aktuelle Diagnose Erweiterung-Konfiguration
Verwenden Sie das Cmdlet " [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) " die aktuelle Diagnose Konfiguration für einen Clouddienst.

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Diagnoseerweiterung entfernen
Diagnose für einen Cloud-Dienst deaktivieren können Sie das Cmdlet " [Remove-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx) ".

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Wenn diagnoseerweiterung mit *Set-AzureServiceDiagnosticsExtension* oder *Neu AzureServiceDiagnosticsExtensionConfig* ohne Parameter *Rolle* aktiviert können Sie die Erweiterung *Entfernen AzureServiceDiagnosticsExtension* ohne Parameter *Rolle* entfernen. *Rolle* Parameter verwendet wurde, wenn die Erweiterung aktivieren muss es auch verwendet werden Wenn Sie die Erweiterung entfernen.

Jede einzelne Rolle diagnoseerweiterung entfernt:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Nächste Schritte

- Zusätzliche Hinweise zur Verwendung von Azure Diagnostics und andere Probleme finden Sie unter [Aktivieren der Diagnose in Azure Cloud Services und virtuellen Maschinen](cloud-services-dotnet-diagnostics.md).
- Das [Konfigurationsschema Diagnose](https://msdn.microsoft.com/library/azure/dn782207.aspx) erläutert verschiedene XML-Konfigurationsoptionen für die diagnoseerweiterung.
- Aktivieren der diagnoseerweiterung für virtuelle Computer finden Sie unter [Erstellen einer Windows Virtual Machine bei der Überwachung und Diagnose mit Azure Ressourcenmanager Vorlage](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
