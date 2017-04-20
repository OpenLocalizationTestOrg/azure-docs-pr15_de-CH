<properties
    pageTitle="Mithilfe von PowerShell in einer virtuellen Maschine unter Windows Azure-Diagnose aktivieren | Microsoft Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    description="Erfahren Sie PowerShell verwenden, um in einer virtuellen Maschine unter Windows Azure-Diagnose aktivieren"
    authors="sbtron"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>


# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Mithilfe von PowerShell in einer virtuellen Maschine unter Windows Azure-Diagnose aktivieren

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure-Diagnose ist eine Funktion in Azure, die Sammlung der Diagnosedaten in einer bereitgestellten Anwendung ermöglicht. Die diagnoseerweiterung können Sie Diagnosedaten wie Anwendungsprotokolle oder Leistungsindikatoren aus einer Azure Virtual Machine (VM) sammeln, auf denen Windows ausgeführt wird. Dieser Artikel beschreibt, wie Windows PowerShell verwenden, um die diagnoseerweiterung für einen virtuellen Computer zu aktivieren. Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md) für die erforderlichen Komponenten für diesen Artikel benötigt.

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Aktivieren Sie diagnoseerweiterung, wenn Sie das Ressourcen-Manager-Bereitstellungsmodell verwenden

Windows VM über Azure-Ressourcen-Manager-Bereitstellungsmodell Erweiterung Konfiguration der Ressourcen-Manager-Vorlage hinzufügen zu erstellen, können Sie die diagnoseerweiterung. Siehe [Erstellen von einem Windows-Computer durch Überwachung und Diagnose mithilfe der Azure-Ressourcen-Manager-Vorlage](virtual-machines-windows-extensions-diagnostics-template.md).

Um diagnoseerweiterung auf einen vorhandenen virtuellen Computer aktivieren, die über den Ressourcen-Manager-Bereitstellungsmodell erstellt wurde, können Sie das [Set-AzureRMVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt603499.aspx) -PowerShell-Cmdlet wie unten dargestellt.


    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$Diagnosticsconfig_path* ist der Pfad zu der Datei mit der Diagnosekonfiguration im XML-Format, wie im folgenden [Beispiel](#sample-diagnostics-configuration) beschrieben.  

Wenn Diagnose-Konfigurationsdatei ein **StorageAccount** -Element mit einem Storage-Kontonamen gibt wird *Set-AzureRMVMDiagnosticsExtension* -Skript automatisch diagnoseerweiterung Diagnosedaten an diesem Storage-Konto festgelegt. Zu diesem Zweck muss das Speicherkonto in das Abonnement der VM.

Wenn keine **StorageAccount** in der Diagnosekonfiguration angegeben wurde, müssen Sie das Cmdlet in der *StorageAccountName* -Parameter übergeben. Der *StorageAccountName* -Parameter angegeben ist, wird immer das Speicherkonto im Parameter angegeben und nicht diejenige, die in der Diagnose in das Cmdlet verwenden.

Ist das Diagnose Speicherkonto in ein anderes Abonnement aus der VM müssen Sie explizit die Parameter *StorageAccountName* und *StorageAccountKey* an das Cmdlet übergeben. Der *StorageAccountKey* -Parameter ist nicht erforderlich, wenn Diagnose Speicherkonto in dieselbe Abonnement ist das Cmdlet automatisch Abfragen und setzen Sie den Wert beim Aktivieren der diagnoseerweiterung. Allerdings ist das Speicherkonto Diagnose in ein anderes Abonnement, das Cmdlet möglicherweise nicht den Schlüssel automatisch abgerufen und müssen Sie explizit den Schlüssel durch den *StorageAccountKey* -Parameter angeben.  

    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Sobald die diagnoseerweiterung auf einem virtuellen Computer aktiviert ist, erhalten Sie die Standardeinstellungen, mit dem Cmdlet [Get-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603678.aspx) .

    Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Das Cmdlet gibt *PublicSettings*, die XML-Konfiguration in eine Base64-codierte Format enthält. Zum Lesen des XML-Code müssen Sie decodiert.

    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

Das Cmdlet " [Remove-AzureRMVmDiagnosticsExtension](https://msdn.microsoft.com/library/mt603782.aspx) " kann verwendet werden, die VM diagnoseerweiterung entfernen.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Aktivieren Sie die diagnoseerweiterung, wenn Sie das klassische Bereitstellungsmodell verwenden

Das Cmdlet " [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) " können Sie diagnoseerweiterung auf einem virtuellen Computer aktivieren, die über das klassische Bereitstellungsmodell erstellen. Das folgende Beispiel veranschaulicht das Erstellen Sie eines neuen virtuellen Computer über das klassische Bereitstellungsmodell Diagnose Erweiterung aktiviert.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzureVM -Location $Location -ServiceName $Service_Name -VM $VM

Aktivieren die diagnoseerweiterung auf eine vorhandene VM, die über das klassische Bereitstellungsmodell erstellt wurde, verwenden Sie zuerst das Cmdlet " [Get-AzureVM](https://msdn.microsoft.com/library/mt589152.aspx) " VM-Konfiguration zu. Dann aktualisieren Sie die VM-Konfiguration, um die diagnoseerweiterung mithilfe des [Set-AzureVMDiagnosticsExtension](https://msdn.microsoft.com/library/mt589189.aspx) -Cmdlet. Wenden Sie anschließend aktualisierte Konfiguration der VM mit [Update-AzureVM](https://msdn.microsoft.com/library/mt589121.aspx).

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Beispielkonfiguration Diagnose

Die folgenden XML-Code kann für die öffentliche Diagnose-Konfiguration mit den oben genannten Skripts verwendet werden. Diese Beispielkonfiguration werden verschiedene Leistungsindikatoren in Speicherkonto Diagnose Fehler Anwendung, Sicherheit und Systemkanäle in den Windows-Ereignisprotokollen und Fehler in die Diagnoseprotokolle Infrastruktur übertragen.

Die Konfiguration muss aktualisiert werden, um Folgendes:

- Das *ResourceID* -Attribut des Elements **Metriken** muss mit der Ressourcen-ID für den virtuellen Computer aktualisiert werden.
    - Die Ressourcen-ID kann mit dem folgenden Muster erstellt: "Abonnements / {*Abonnement-ID für das Abonnement mit der VM*} /resourceGroups/ {*Resourcegroup-Namen für die VM*} / {*der Name*} providers/Microsoft.Compute/virtualMachines/".
    - Beispielsweise wäre wenn die Abonnement-ID für das Abonnement, auf dem die VM läuft, **11111111-1111-1111-1111-111111111111**, Gruppe Ressourcenname für die Ressourcengruppe **MyResourceGroup ist**, und der Name **MyWindowsVM**ist, dann der Wert für *ResourceID* :

        ```
        <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
        ```

    - Weitere Informationen werden Metriken generiert, basierend auf der Leistung Leistungsindikatoren und Metriken, [Azure-Diagnose Metriken](virtual-machines-windows-extensions-diagnostics-template.md#wadmetrics-tables-in-storage)Siehe im Speicher.

- Das **StorageAccount** -Element muss mit dem Namen der Diagnose Speicherkonto aktualisiert werden.

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Nächste Schritte
- Zusätzliche Hinweise auf die Azure-Diagnose und andere Techniken zur Problembehandlung finden Sie unter [Aktivieren der Diagnose in Azure Cloud Services und virtuellen Maschinen](../cloud-services/cloud-services-dotnet-diagnostics.md).
- [Diagnose-Konfigurationen Schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) erläutert verschiedene XML-Konfigurationsoptionen für die diagnoseerweiterung.
