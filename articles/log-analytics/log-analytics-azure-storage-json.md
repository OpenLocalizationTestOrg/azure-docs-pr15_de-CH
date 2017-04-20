<properties
    pageTitle="Analysieren von Azure Diagnoseprotokolle Protokollanalyse mit | Microsoft Azure"
    description="Protokollanalyse erhalten die Protokolle von Azure Services, die Azure Diagnoseprotokolle BLOB-Speicher im JSON-Format schreiben."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Analysieren von Azure Diagnoseprotokolle Protokollanalyse verwenden

Protokollanalyse können die Protokolle für die folgenden Azure Dienste sammeln, die [Diagnoseprotokolle Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) BLOB-Speicher im JSON-Format schreiben:

+ Automatisierung (Vorschau)
+ Key Vault (Vorschau)
+ Application Gateway (Vorschau)
+ Netzwerk-Sicherheitsgruppe (Vorschau)

In den folgenden Abschnitten gehen Sie mithilfe von PowerShell:

+ Konfigurieren der Protokollanalyse von Speicher für jede Ressource die Protokolle sammeln  
+ Der Protokollanalyse Lösung für Azure service

Vor dem Sammeln von Daten für diese Ressourcen können Protokollanalyse muss Azure Diagnostics aktiviert. Verwenden der `Set-AzureRmDiagnosticSetting` -Cmdlet zum Aktivieren der Protokollierung.

Finden Sie in folgendem Artikel Weitere Informationen zum Aktivieren des Diagnoseprotokolls:

+ [Key Vault](../key-vault/key-vault-logging.md)
+ [Application Gateway](../application-gateway/application-gateway-diagnostics.md)
+ [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-network-nsg-manage-log.md)

Diese Dokumentation enthält auch Details auf:

+ Problembehandlung bei der Datenerfassung
+ Sammlung beenden

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurieren der Protokollanalyse Azure Diagnoseprotokolle sammeln

Erfassung von Protokollen für diese Dienste und die Lösung die Protokolle visualisieren wird mit PowerShell-Skripts ausgeführt.

Das folgende Beispiel aktiviert die Protokollierung für alle unterstützten Ressourcen

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Bei einigen Ressourcen ist die vorangegangenen Konfiguration von Azure-Portal mithilfe der Schritte im [Überblick Azure Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)durchgeführt.

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurieren der Protokollanalyse Azure Diagnoseprotokolle sammeln

Wir haben ein PowerShell-Skriptmodul bereitgestellt, die zwei Cmdlets beim Konfigurieren der Protokollanalyse exportiert:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`fordert Sie zur Eingabe und kann einfache Konfigurationen einrichten
2. `Add-AzureDiagnosticsToLogAnalytics`die Ressourcen als Eingabe überwacht und konfiguriert dann Protokollanalyse  

### <a name="pre-requisites"></a>Erforderliche Komponenten

1. Azure PowerShell Version 1.0.8 oder neuer Operational Insights-Cmdlets.
  - [Zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)
  - Überprüfen Sie die Version von Cmdlets:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Diagnoseprotokolle ist für Azure Ressource konfiguriert, die Sie überwachen möchten. Mit `Set-AzureRmDiagnosticSetting` oder [Verwendung Protokollanalyse Sammeln von Daten von Azure-Speicherkonten](log-analytics-azure-storage.md) für Diagnose aktivieren.
3. [Protokollanalyse](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS) -Arbeitsbereich  
4. AzureDiagnosticsAndLogAnalytics-PowerShell-Modul
  - [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) -Modul von PowerShell Gallery herunterladen

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Option 1: Führen Sie das Skript für die interaktive

PowerShell öffnen und ausführen:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

Eine Liste von Auswahlmöglichkeiten angezeigt und erhalten eine Aufforderung zur Auswahl.
Sie müssen eine Auswahl für die folgenden:

+ Ressourcentypen (Azure Services) Protokolle sammeln
+ Ressourceninstanzen Protokolle sammeln
+ Log Analytics Arbeitsbereich zum Sammeln der Daten

Nach dem Ausführen dieses Skripts sollten Sie Datensätze Protokollanalyse sehen etwa 30 Minuten nach neue Diagnosedaten in den Speicher geschrieben werden. Wenn keine Datensätze verfügbar sind, nach diesem Zeitpunkt finden Sie unter Problembehandlung Abschnitt.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Option 2: Erstellen Sie eine Liste der Ressourcen und die Konfiguration Cmdlet übergeben

Sie können eine Liste der Ressourcen erstellen, die Azure Diagnostics aktiviert und die Ressourcen Konfiguration Cmdlet übergeben.

Finden Sie weitere Informationen über das Cmdlet mit `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


Weitere Details auf OMS- [Protokoll Analytics PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt188224.aspx) gefunden

>[AZURE.NOTE] Wenn Ressourcen und Arbeitsbereich in verschiedenen Azure-Abonnements, wechseln Sie mit Bedarf`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Nach dem Ausführen dieses Skripts sollten Sie Datensätze Protokollanalyse sehen etwa 30 Minuten nach neue Diagnosedaten in den Speicher geschrieben werden. Wenn keine Datensätze verfügbar sind, nach diesem Zeitpunkt finden Sie unter Problembehandlung Abschnitt.  

>[AZURE.NOTE] Die Konfiguration wird nicht im Azure-Portal angezeigt. Sie können überprüfen, Konfiguration der `Get-AzureRmOperationalInsightsStorageInsight` Cmdlet.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Sammeln von Azure Diagnoseprotokolle Protokollanalyse verhindern

Verwenden Sie zum Löschen der Protokollanalyse-Konfigurations für eine Ressource der `Remove-AzureRmOperationalInsightsStorageInsight` Cmdlet.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Problembehandlung bei der Konfiguration für Azure Diagnoseprotokolle

*Problem*

Der folgende Fehler wird angezeigt, wenn eine Ressource konfigurieren, die klassische Speicher anmelden:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Auflösung*

Die API für das klassische Bereitstellungsmodell mit anmelden`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Datensammlung für Azure Diagnoseprotokolle Problembehandlung

Wenn Sie Daten für Ihre Azure Ressource Protokollanalyse nicht sehen, können Sie die folgenden Problembehandlungsschritte aus:

+ Daten Sie an das Speicherkonto
+ Überprüfen Sie, ob die Protokollanalyse-Lösung für den Dienst aktiviert ist
+ Protokollanalyse konfiguriert ist, aus dem Speicher gelesen
+ Den Speicherschlüssel Konto aktualisieren

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Überprüfen Sie, ob Daten an das Speicherkonto übertragen

Sie können überprüfen, ob Daten an das Speicherkonto ein Tool durchsuchen Azure-Speicher (z. B. Visual Studio) oder mithilfe von PowerShell fließt.

Das Speicherkonto zu ist die Ressource konfiguriert protokollieren, um die folgenden PowerShell verwenden:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

Die Behälter von Azure Diagnostics verwendet hat ein Format:  

`insights-logs-<Category>`

Finden Sie weitere Informationen zum Anzeigen des Inhalts des Speicherkontos [mit Speicher-Cmdlets Container für aktuelle Daten überprüft](../storage/storage-powershell-guide-full.md) .

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Überprüfen Sie, ob die Protokollanalyse-Lösung für den Dienst aktiviert ist

Verwenden Sie `Add-AzureDiagnosticsToLogAnalyticsUI`, die richtige Protokollanalyse automatisch für Sie aktiviert ist.

Prüfen, ob eine Lösung aktiviert ist, führen Sie die folgende PowerShell:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Wenn die Lösung nicht aktiviert ist, können Sie mit dem folgenden PowerShell aktivieren:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Verwenden Sie der Name der Lösung für jeden Ressourcentyp aktivieren, um die folgenden PowerShell (diese Variable ist verfügbar, nachdem Sie das Modul importiert haben):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Protokollanalyse konfiguriert ist, aus dem Speicher gelesen

Wenn zusätzlicher Azure Ressourcen hinzufügen müssen Sie aktivieren das Diagnoseprotokoll für sie und Protokollanalyse für sie konfigurieren.
Welche Ressourcen und Speicherkonten sieht Protokollanalyse Protokolle für die Verwendung der folgenden PowerShell erfassen:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Aktualisieren Sie den Speicherschlüssel

Ändern Sie den Schlüssel für das Speicherkonto Protokollanalyse Konfiguration auch mit dem neuen Schlüssel aktualisiert werden muss.
Sie können die Protokollanalyse-Konfiguration mit einem neuen Schlüssel mit dem folgenden PowerShell aktualisieren:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Storage Insight Name verwenden, um die `Get-AzureRmOperationalInsightsStorageInsight` -Cmdlet, wie in den vorherigen Beispielen gezeigt.

## <a name="next-steps"></a>Nächste Schritte

- [Mit BLOB-Speicher für IIS und Tabellenspeicher für Ereignisse](log-analytics-azure-storage-iis-table.md) die Protokolle für Azure services, Schreiben Diagnose Tabelle oder IIS-Protokolle auf BLOB-Speicher geschrieben.
- [Projektmappen können](log-analytics-add-solutions.md) Einblick in die Daten.
- [Verwenden von Suchabfragen](log-analytics-log-searches.md) zum Analysieren der Daten.
