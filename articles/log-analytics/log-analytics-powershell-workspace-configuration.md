<properties
    pageTitle="Mithilfe von PowerShell erstellen und konfigurieren einen Protokoll Analytics Arbeitsbereich | Microsoft Azure"
    description="Protokolldaten Analytics wird von Servern in der lokalen oder cloud-Infrastruktur. Sie können Daten von Azure-Speicher beim Generieren von Azure Diagnostics sammeln."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Verwalten Sie Protokollanalyse mit PowerShell

[Log Analytics PowerShell-Cmdlets] verwenden (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) Funktionen in Protokollanalyse über eine Befehlszeile oder als Teil eines Skripts ausführen.  Beispiele für Aufgaben mit PowerShell ausführen:

+ Erstellen eines Arbeitsbereichs
+ Hinzufügen oder Entfernen einer Lösung
+ Importieren und Exportieren von gespeicherten Suchen
+ Erstellen einer Computergruppe
+ Aktivieren Sie Auflistung von IIS-Protokollen auf Computern mit installiertem Windows-agent
+ Sammeln von Leistungsindikatoren von Linux und Windows
+ Sammeln von Ereignissen von Syslog auf Linux-Computern 
+ Ereignisse von Windows-Ereignisprotokolle erfassen
+ Benutzerdefinierten Ereignisprotokolle erfassen
+ Den Agent Analytics Azure virtuellen Computer hinzufügen
+ Konfigurieren der Protokollanalyse mit Indexdaten mithilfe von Azure diagnostics


Dieser Artikel enthält zwei Codebeispiele, die veranschaulichen einige der Funktionen, die von PowerShell ausführen können.  Finden Sie [Log Analytics PowerShell-Cmdlet Reference] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) für andere Funktionen.

> [AZURE.NOTE] Protokollanalyse hieß früher betriebliche Informationen daher den Namen der Cmdlets verwendet wird.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um mit Ihrem Arbeitsbereich Protokollanalyse PowerShell verwenden, benötigen Sie Folgendes:

+ Ein Azure-Abonnement und 
+ Azure-Protokollanalyse Arbeitsbereich Azure-Abonnement verknüpft.

Wenn einen OMS-Workspace erstellt haben, aber es noch nicht es zum Azure-Abonnement verknüpft ist können Sie die Verknüpfung erstellen:

+ Im Azure-portal
+ OMS-Portal oder 
+ Verwenden die Cmdlets Get-AzureRmOperationalInsightsLinkTargets und neu AzureRmOperationalInsightsWorkspace.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Erstellen und konfigurieren ein Protokoll Analytics-Arbeitsbereich

Das folgende Skript veranschaulicht wie:

1.  Erstellen eines Arbeitsbereichs
2.  Liste der Lösung
3.  Projektmappen Arbeitsbereich hinzufügen
4.  Importieren von gespeicherten Suchen
5.  Exportieren einer gespeicherten Suche
6.  Erstellen einer Computergruppe
7.  Aktivieren Sie Auflistung von IIS-Protokollen auf Computern mit installiertem Windows-agent
8.  Logischer Datenträger Leistungsindikatoren von Linux-Computer sammeln (% Inodes verwendet; MB frei; % Belegt; / Sek. übertragen; Lesevorgänge/s; Schreibvorgänge/s)
9.  Syslog-Ereignisse von Linux-Computern erfassen
10. Erfassen Sie Fehler und Warnung Ereignisse in das Anwendungsereignisprotokoll von Windows-Computern
11. Sammeln Sie Verfügbare MB Arbeitsspeicher-Leistungsindikator von Windows-Computern
12. Sammeln Sie ein benutzerdefiniertes Protokoll 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Konfigurieren der Protokollanalyse Azure Diagnostics indizieren 

Für Agents Azure Ressourcen, müssen die Ressourcen Azure Diagnostics aktiviert und konfiguriert ein Speicherkonto schreiben. Protokollanalyse kann dann konfiguriert werden, um das Speicherkonto Protokolle gesammelt. Sie müssen die vorherige Konfiguration für Ressourcen enthält:

+ Klassische Clouddienste (Web- und Workerrollen Rollen)
+ Service Fabric-Cluster
+ Netzwerk-Sicherheitsgruppen
+ Wichtige Depots und 
+ Anwendungsgateways

PowerShell können Sie ein Azure-Abonnement Protokolle von verschiedenen Azure-Abonnements sammeln Protokollanalyse Arbeitsbereich konfigurieren.

Das folgende Beispiel zeigt wie:

1.  Auflisten der vorhandenen Speicherkonten und Protokollanalyse Daten index Speicherorte
2.  Erstellen Sie eine Konfiguration ein Speicherkonto gelesen
3.  Aktualisieren Sie die neu erstellte Konfiguration Indexdaten zusätzliche Standorte
4.  Löschen Sie die neu erstellte Konfiguration

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfung Log Analytics PowerShell-Cmdlets](http://msdn.microsoft.com/library/mt188224.aspx) für Weitere Informationen zum Verwenden von PowerShell zur Konfiguration der Protokollanalyse.

