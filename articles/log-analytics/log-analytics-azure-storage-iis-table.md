<properties
    pageTitle="Mit BLOB-Speicher für IIS und Tabelle Ereignisse | Microsoft Azure"
    description="Protokollanalyse kann Protokolle für Azure Services, die Diagnose Tabellenspeicher schreiben oder IIS-Protokolle geschrieben BLOB-Speicher gelesen."
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
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>Verwenden von BLOB-Speicher für IIS und Tabellenspeicher für Ereignisse

Protokollanalyse können die Protokolle für die folgenden Dienste, die Diagnose Tabellenspeicher schreiben oder IIS-Protokolle geschrieben BLOB-Speicher lesen:

+ Service Fabric-Cluster (Vorschau)
+ Virtuelle Computer
+ Web-Worker-Rollen

Vor dem Sammeln von Daten für diese Ressourcen können Protokollanalyse muss Azure Diagnostics aktiviert.

Aktivieren der Diagnose Azure-Portal verwenden oder PowerShell konfigurieren Protokollanalyse Protokolle sammeln.

Azure-Diagnose ist eine Azure-Erweiterung, die Sammeln von Diagnosedaten Worker-Rolle, Webrolle oder virtuelle Computer in Azure ermöglicht. Die Daten werden in Azure Storage-Konto und können dann von Protokollanalyse erfasst werden.

Protokollanalyse diese Azure-Diagnose Protokolle sammeln muß die Protokolle an den folgenden Speicherorten:

| Protokolltyp | Ressourcentyp | Speicherort |
| -------- | ------------- | -------- |
| IIS-Protokolle | Virtuelle Computer <br> Webrollen <br> Worker-Rollen | Bündel-Iis-Protokolldateien (BLOB-Speicher) |
| Syslog | Virtuelle Computer | LinuxsyslogVer2v0 (Tabelle Speicher) |
| Service Fabric operationelle Ereignisse | Service Fabric-Knoten | WADServiceFabricSystemEventTable |
| Zuverlässige Akteur Fabric-Ereignissen | Service Fabric-Knoten | WADServiceFabricReliableActorEventTable |
| Fabric zuverlässig Dienstereignisse | Service Fabric-Knoten | WADServiceFabricReliableServiceEventTable |
| Windows-Ereignisprotokolle | Service Fabric-Knoten <br> Virtuelle Computer <br> Webrollen <br> Worker-Rollen | WADWindowsEventLogsTable (Tabellenspeicher) |
| Windows-ETW-Protokolle | Service Fabric-Knoten <br> Virtuelle Computer <br> Webrollen <br> Worker-Rollen | WADETWEventTable (Tabellenspeicher) |

>[AZURE.NOTE] IIS-Protokolle von Azure Websites werden derzeit nicht unterstützt.

Für virtuelle Maschinen können Sie der [Protokollanalyse-Agent](log-analytics-azure-vm-extension.md) in der virtuellen Maschine installieren Zusätzliche Hinweise zu aktivieren. IIS-Protokolle und die Ereignisprotokolle analysieren, sondern führen Sie zusätzliche Analyse einschließlich Konfiguration ändern, SQL Beurteilung und Bewertung aktualisieren.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Aktivieren von Azure Diagnostics auf einem virtuellen Computer für Ereignisprotokoll und IIS Auflistung protokollieren

Gehen Sie Azure Diagnostics auf einem virtuellen Computer für das Ereignisprotokoll und IIS Log mit Microsoft Azure-Portal aktivieren.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Azure-Diagnose auf einem virtuellen Computer mit dem Azure-Portal aktivieren

1. Installieren Sie den Agent VM, wenn Sie einen virtuellen Computer erstellen. Der virtuelle Computer bereits vorhanden ist, sicher, dass die VM-Agent bereits installiert ist.
    - Navigieren Sie zum virtuellen Computer in Azure-Portal wählen Sie **Optionale Konfiguration**und **Diagnose** und festlegen Sie **Status** **auf**.

    Nach Abschluss des hat der VM Azure-Diagnose-Erweiterung installiert und ausgeführt. Diese Erweiterung ist verantwortlich für die Sammlung der Diagnosedaten.

2. Aktivieren Sie die Überwachung und konfigurieren Sie Protokollierung auf einen vorhandenen virtuellen Computer. Sie können Diagnosen auf VM-Ebene. Diagnose aktivieren und konfigurieren die Protokollierung für die folgenden Schritte:
    1. Wählen Sie den virtuellen Computer.
    2. Klicken Sie auf **Überwachung**.
    3. Klicken Sie auf **Diagnose**.
    4. Legt den **Status** **auf**.
    5. Wählen Sie jede Diagnoseprotokoll, das Sie erfassen möchten.
    7. Klicken Sie auf **OK**.

Mithilfe von Azure PowerShell können Sie genauer Ereignisse angeben, die in Azure-Speicher geschrieben werden. Zum [Sammeln von Daten mithilfe von Azure-Diagnose, die Tabelle oder IIS-Protokolle geschrieben BLOB](log-analytics-azure-storage-json.md)verweisen.


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Aktivieren von Azure Diagnostics in einer Webrolle für IIS Log und Ereignis

Allgemeine Schritte zum Aktivieren der Azure-Diagnose finden Sie unter [Wie zu aktivieren Diagnose in einem Clouddienst](../cloud-services/cloud-services-dotnet-diagnostics.md) . Hinweise diese Informationen und mit Protokollanalyse anpassen.

Azure Diagnostics aktiviert:

- IIS-Protokolle werden standardmäßig mit ScheduledTransferPeriod Übertragungsintervall übertragen Daten gespeichert.
- Windows-Ereignisprotokolle werden standardmäßig nicht übertragen.

### <a name="to-enable-diagnostics"></a>Diagnose aktivieren

Windows-Ereignisprotokolle zu aktivieren oder die ScheduledTransferPeriod ändern, Azure-Diagnose mithilfe der XML-Konfigurationsdatei (diagnostics.wadcfg), wie in gezeigt konfigurieren [Schritt 4: Diagnose Konfigurationsdatei erstellen und installieren Sie die Erweiterung](../cloud-services/cloud-services-dotnet-diagnostics.md)

Die folgende Beispiel-Konfigurationsdatei sammelt IIS-Protokolle und Ereignisse aus den Protokollen Anwendung und System:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Sicherstellen Sie, dass Ihre ConfigurationSettings ein Speicherkonto, wie im folgenden Beispiel gibt:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Werte **Kontoname** und **AccountKey** finden in Azure-Portal im Dashboard Konto Speicher unter Zugriffstasten verwalten. Das Protokoll für die Verbindungszeichenfolge muss **Https**sein.

Aktualisierte Diagnose Konfiguration gilt für Cloud-Dienst und Diagnose in den Azure-Speicher geschrieben wird, sind Sie Protokollanalyse konfigurieren.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Verwenden Sie Azure-Portal, um Protokolle von Azure Storage sammeln

Azure-Portal können Sie Protokollanalyse sammeln Protokolle für Azure Folgendes konfigurieren:

+ Service Fabric-Cluster
+ Virtuelle Computer
+ Web-Worker-Rollen

Azure-Portal navigieren Sie zum Arbeitsbereich Protokollanalyse und führen Sie die folgenden Aufgaben:

1. Klicken Sie auf *Storage-Konten Protokolle*
2. Klicken Sie auf Aufgabe *Hinzufügen*
3. Wählen Sie das Speicherkonto, das die Diagnoseprotokolle enthält
  - Dieses Konto kann entweder klassische Speicherkonto oder ein Ressourcenmanager Azure storage
4. Wählen Sie die Protokolle sammeln möchten Daten
  - IIS-Protokolle sind; Ereignisse; Syslog (Linux) ETW-Protokolle; Fabric-Ereignissen
5. Der Wert für die Quelle wird basierend auf dem Datentyp automatisch ausgefüllt und kann nicht geändert werden
6. Klicken Sie auf OK, um die Konfiguration zu speichern

Wiederholen Sie die Schritte 2 bis 6 für zusätzliche Speicherkonten und Datentypen Protokollanalyse sammeln möchten.

In ca. 30 Minuten können Sie Daten aus dem Speicherkonto Protokollanalyse. Sie sehen nur Daten, die in den Speicher geschrieben, nachdem die Konfiguration angewendet wird. Protokollanalyse liest die Daten nicht aus dem Speicherkonto.

>[AZURE.NOTE] Das Portal überprüft nicht die Quelle im Speicherkonto vorhanden ist oder wenn neue Daten geschrieben werden.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Aktivieren von Azure Diagnostics auf einem virtuellen Computer für Ereignisprotokoll und IIS Auflistung PowerShell protokollieren

Gehen Sie im [Protokollanalyse Konfiguration von Azure Diagnostics indizieren](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) von PowerShell von Azure Diagnostics gelesen, die in Tabellenspeicher geschrieben werden.

Mithilfe von Azure PowerShell können Sie genauer Ereignisse angeben, die in Azure-Speicher geschrieben werden.
Weitere Informationen finden Sie unter [Aktivieren der Diagnose in Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).

Aktivieren und Aktualisieren von Azure Diagnostics mit dem folgenden PowerShell-Skript.
Sie können dieses Skript auch mit eine benutzerdefinierte Protokollierungskonfiguration verwenden.
Ändern Sie das Skript zum Festlegen der Speicherkonto Dienstname und Name des virtuellen Computers.
Das Skript verwendet Cmdlets für klassische virtuelle Maschinen.

Überprüfen Sie im folgenden Beispielskript, kopieren Sie nach Bedarf ändern Sie, speichern Sie im Beispiel als PowerShell-Skript-Datei und führen Sie das Skript.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Nächste Schritte

- [Mit JSON-Dateien im BLOB-Speicher](log-analytics-azure-storage-json.md) Lesen der Protokolle aus Azure services, Diagnose schreiben BLOB-Speicher im JSON-Format.
- [Projektmappen können](log-analytics-add-solutions.md) Einblick in die Daten.
- [Verwenden von Suchabfragen](log-analytics-log-searches.md) zum Analysieren der Daten.
