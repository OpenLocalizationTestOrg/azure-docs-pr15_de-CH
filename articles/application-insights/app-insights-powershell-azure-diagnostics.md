<properties
    pageTitle="Mithilfe von PowerShell Setup Einblicke in eine Azure-Anwendung | Microsoft Azure"
    description="Konfigurieren von Azure Diagnostics Pipe Anwendung Erkenntnisse zu automatisieren."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Mithilfe von PowerShell Anwendung Einblicke für Azure Web-app einrichten

[Microsoft Azure](https://azure.com) kann [Visual Studio Application Insights](app-insights-overview.md) [Senden von Azure Diagnostics konfiguriert](app-insights-azure-diagnostics.md) sein. Die Diagnose beziehen sich auf Azure Cloud Services und Azure VMs. Sie ergänzen Telemetrie, die innerhalb der app mit Application Insights SDK von senden. Als Teil des erstellen neue Ressourcen in Azure automatisieren können Sie mithilfe von PowerShell Diagnose konfigurieren.

## <a name="azure-template"></a>Azure-Vorlage

Wenn die Web-app in Azure und Ihre Ressourcen mit einer Azure-Ressourcen-Manager-Vorlage erstellen, können Sie Application Insights Ressourcenknoten hinzufügen konfigurieren:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-einen Namen für die Application Insights-Ressource
* `myWebAppName`-die Id der Web-app


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Aktivieren Sie diagnoseerweiterung als Teil der Bereitstellung einer Cloud-Dienst

Die `New-AzureDeployment` Cmdlet hat einen Parameter `ExtensionConfiguration`, der ein Array von Diagnose-Konfigurationen akzeptiert. Diese können mit erstellt werden die `New-AzureServiceDiagnosticsExtensionConfig` Cmdlet. Zum Beispiel:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Diagnoseerweiterung auf einen vorhandenen Cloud-Dienst aktivieren

Verwenden Sie einen vorhandenen Dienst `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Aktuelle Diagnose Erweiterung-Konfiguration

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Diagnoseerweiterung entfernen

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Aktiviert die diagnoseerweiterung mit `Set-AzureServiceDiagnosticsExtension` oder `New-AzureServiceDiagnosticsExtensionConfig` ohne Parameter Rolle Entfernen der Erweiterung mit `Remove-AzureServiceDiagnosticsExtension` Rolle Parameter. Rolle Parameter verwendet wurde, wenn die Erweiterung aktivieren muss es auch verwendet werden Wenn Sie die Erweiterung entfernen.

Jede einzelne Rolle diagnoseerweiterung entfernt:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Siehe auch

* [Überwachen von Azure Cloud Services apps Anwendung Einblicke](app-insights-cloudservices.md)
* [Senden von Azure Diagnostics Anwendung Einblicke](app-insights-azure-diagnostics.md)
* [Konfigurieren von Alerts automatisieren](app-insights-powershell-alerts.md)

