<properties
    pageTitle="Azure Monitor-automatische Skalierung allgemeinen Bewertungsgrundlagen. | Microsoft Azure"
    description="Erfahren Sie, welche Metriken für automatische Skalierung häufig verwendeten Ihre Cloud-Diensten und virtuellen Maschinen Web Apps."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor Skalierung allgemeinen Bewertungsgrundlagen

Automatische Skalierung Azure Monitor können Sie die Anzahl der ausgeführten Instanzen, skaliert basierend auf Daten (Metriken). Dieses Dokument Beschreibt häufige Metriken, die Sie verwenden möchten. Im Portal Azure Cloud Services und Serverfarmen können Sie die Metrik der Ressource durch skalieren. Allerdings können Sie auch eine Metrik aus einer anderen Ressource durch skalieren.

Hier werden Details zum Suchen und Auflisten der Metriken durch skalieren möchten. Gilt für Virtual Machine Maßstab legt auch skalieren.

## <a name="compute-metrics"></a>Metriken berechnen
Standardmäßig Azure VM v2 Diagnose-Erweiterung konfiguriert ist und die folgenden Metriken aktiviert haben.

- [Gast-Metriken für Windows VM-v2](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Gast-Metriken für Linux VM-v2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Sie können die `Get MetricDefinitions` API/PoSH/CLI Metriken für VMSS Ressource anzeigen. 

Wenn Sie VM Maßstab legt und eine bestimmte Metrik aufgeführt angezeigt, dann ist es wahrscheinlich *deaktiviert* in der diagnoseerweiterung.

Wenn eine bestimmte Metrik nicht aufgenommen oder Häufigkeit übertragen, können die Diagnosekonfiguration aktualisieren.

Trifft jeweils oben, überprüfen Sie [Mit PowerShell in einer virtuellen Maschine unter Windows Azure-Diagnose aktivieren](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) PowerShell konfigurieren und Aktualisieren der Azure-VM-Diagnose-Erweiterung um die Metrik zu aktivieren. Dieser Artikel enthält auch eine Beispielkonfigurationsdatei Diagnose.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Berechnen Sie Metriken für Windows VM v2 als Gast OS

Beim Erstellen einer neuen VM (v2) in Azure ist der Diagnose Erweiterung Diagnose aktiviert.

Sie können eine Liste der Messgrößen mithilfe des folgenden Befehls in PowerShell.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Sie können eine Warnung für die folgenden Metriken erstellen.

|Metrische Name|   Einheit|
|---|---|
|\Processor(_Total)\% Prozessorzeit    |Prozent|
|\Processor(_Total)\% privilegierte Zeit   |Prozent|
|\Processor(_Total)\% Benutzerzeit |Prozent|
|\Processor Informationen (_Total) \Processor Frequenz |Anzahl|
|\System\Processes| Anzahl|
|Anzahl der \Process (_Total) \Thread| Anzahl|
|Anzahl der \Process (_Total) \Handle  |Anzahl|
|\MEMORY\% Zugesicherte verwendete Bytes verwenden   |Prozent|
|\Memory\Available bytes|   Bytes|
|\Memory\Committed bytes    |Bytes|
|\Memory\Commit-Begrenzung|  Bytes|
|\Memory\Pool ausgelagerte Bytes|  Bytes|
|\Memory\Pool nicht ausgelagerte Bytes|   Bytes|
|\PhysicalDisk(_Total)\% Datenträgerzeit| Prozent|
|\PhysicalDisk(_Total)\% Datenträger Mal gelesen|    Prozent|
|\PhysicalDisk(_Total)\% Datenträgerzeit schreiben|   Prozent|
|\PhysicalDisk (_Total) \Disk/s   |CountPerSecond|
|\PhysicalDisk (_Total) \Disk/s   |CountPerSecond|
|\PhysicalDisk (_Total) \Disk/s  |CountPerSecond|
|\PhysicalDisk (_Total) \Disk Bytes/s   |BytesPerSecond|
|\PhysicalDisk (_Total) \Disk Bytes gelesen/s| BytesPerSecond|
|\PhysicalDisk (_Total) \Disk Bytes geschrieben/s |BytesPerSecond|
|\PhysicalDisk (_Total) \Avg. Warteschlangenlänge des Datenträgers|  Anzahl|
|\PhysicalDisk (_Total) \Avg. Warteschlangenlänge des Datenträgers lesen| Anzahl|
|\PhysicalDisk (_Total) \Avg. Warteschlangenlänge des Datenträgers schreiben |Anzahl|
|\LogicalDisk(_Total)\% Speicherplatz| Prozent|
|\LogicalDisk (_Total) \Free MB|   Anzahl|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Berechnen Sie Metriken für Linux VM v2 als Gast OS

Beim Erstellen einer neuen VM (v2) in Azure ist Diagnose Diagnose Erweiterung standardmäßig aktiviert.

Sie können eine Liste der Messgrößen mithilfe des folgenden Befehls in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Sie können eine Warnung für die folgenden Metriken erstellen.

|Metrische Name                            |Einheit|
|---|---|
|\Memory\AvailableMemory                |Bytes|
|\Memory\PercentAvailableMemory         |Prozent|
|\Memory\UsedMemory                     |Bytes|
|\Memory\PercentUsedMemory              |Prozent|
|\Memory\PercentUsedByCache             |Prozent|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Bytes|
|\Memory\PercentAvailableSwap           |Prozent|
|\Memory\UsedSwap                       |Bytes|
|\Memory\PercentUsedSwap                |Prozent|
|\Processor\PercentIdleTime             |Prozent|
|\Processor\PercentUserTime             |Prozent|
|\Processor\PercentNiceTime             |Prozent|
|\Processor\PercentPrivilegedTime       |Prozent|
|\Processor\PercentInterruptTime        |Prozent|
|\Processor\PercentDPCTime              |Prozent|
|\Processor\PercentProcessorTime        |Prozent|
|\Processor\PercentIOWaitTime           |Prozent|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Sekunden|
|\PhysicalDisk\AverageWriteTime         |Sekunden|
|\PhysicalDisk\AverageTransferTime      |Sekunden|
|\PhysicalDisk\AverageDiskQueueLength   |Anzahl|
|\NetworkInterface\BytesTransmitted     |Bytes|
|\NetworkInterface\BytesReceived        |Bytes|
|\NetworkInterface\PacketsTransmitted   |Anzahl|
|\NetworkInterface\PacketsReceived      |Anzahl|
|\NetworkInterface\BytesTotal           |Bytes|
|\NetworkInterface\TotalRxErrors        |Anzahl|
|\NetworkInterface\TotalTxErrors        |Anzahl|
|\NetworkInterface\TotalCollisions      |Anzahl|




## <a name="commonly-used-web-server-farm-metrics"></a>Häufig verwendete Web (Serverfarm) Metriken

Sie können auch automatisch skalieren basierend auf gemeinsamen Web servermetriken wie Http-Warteschlangenlänge ausführen. Metrische Name ist **HttpQueueLength**.  Im folgenden Abschnitt Listet verfügbare Serverfarm (Web Apps) Metriken.

### <a name="web-apps-metrics"></a>Web Apps Metriken

Sie können eine Liste der Messgrößen Web Apps mithilfe des folgenden Befehls in PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Warnen oder Skalierung durch diese Metriken.

|Metrische Name        |Einheit|
|---                |---|
|CpuPercentage      |Prozent|
|MemoryPercentage   |Prozent|
|DiskQueueLength    |Anzahl|
|HttpQueueLength    |Anzahl|
|BytesReceived      |Bytes|
|BytesSent          |Bytes|


## <a name="commonly-used-storage-metrics"></a>Häufig verwendete speichermetriken
Sie können vom Speicher Warteschlangenlänge skalieren ist die Anzahl der Nachrichten in der Speicherwarteschlange. Warteschlangenlänge Speicher ist eine spezielle und Schwellenwert angewendet werden die Anzahl der Nachrichten pro Instanz. Dies bedeutet, wenn zwei Instanzen, wenn der Schwellenwert auf 100 festgelegt ist, es skaliert wird wird die Gesamtzahl der Nachrichten in der Warteschlange 200. Beispielsweise 100 Nachrichten pro Instanz.

Sie können konfigurieren, wird der Azure-Verwaltungsportal **Einstellungen** Blatt. Bei VM Skalierung können Sie skalieren Einstellung in der Vorlage ARM *MetricName* als *ApproximateMessageCount* und übergeben Sie die ID der Speicherwarteschlange als *MetricResourceUri*aktualisieren.

Beispielsweise eine klassische Speicherkonto zählen Autoscale Einstellung MetricTrigger:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

Für ein Konto (nicht Classic) Speicherung der MetricTrigger gehören:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Häufig verwendete Service Bus Metriken

Sie können vom Service Bus Warteschlangenlänge skalieren ist die Anzahl der Nachrichten in der Warteschlange Service Bus. Warteschlangenlänge Service Bus ist eine spezielle und festgelegten angewendet werden die Anzahl der Nachrichten pro Instanz. Dies bedeutet, wenn zwei Instanzen, wenn der Schwellenwert auf 100 festgelegt ist, es skaliert wird wird die Gesamtzahl der Nachrichten in der Warteschlange 200. Beispielsweise 100 Nachrichten pro Instanz.

Bei VM Skalierung können Sie skalieren Einstellung in der Vorlage ARM *MetricName* als *ApproximateMessageCount* und übergeben Sie die ID der Speicherwarteschlange als *MetricResourceUri*aktualisieren.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Für Service Bus Konzept Gruppe Ressource existiert nicht. Azure-Ressourcen-Manager erstellt eine Standard-Ressourcengruppe pro region Die Ressourcengruppe ist normalerweise im Format "Standard - ServiceBus-[Region]". Z. B. 'Standard-ServiceBus-EastUS', 'Standard-ServiceBus-WestUS', 'Standard-ServiceBus-AustraliaEast"usw..
