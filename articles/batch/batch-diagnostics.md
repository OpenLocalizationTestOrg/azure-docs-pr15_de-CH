<properties
   pageTitle="Azure Batch Diagnoseprotokoll | Microsoft Azure"
   description="Erfassen Sie und analysieren Sie Diagnoseprotokoll Ereignisse für Azure Batch kontoressourcen-Pools und Aufgaben."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure Batch-Diagnoseprotokolls

Wie bei vielen Azure Services gibt der Batch-Dienst Protokollieren von Ereignissen für bestimmte Ressourcen während der Lebensdauer der Ressource. Sie können Azure Batch Diagnoseprotokolle Ereignisse protokolliert Ressourcen-Pools und Aufgaben aktivieren und die Protokolle zur Diagnose und Überwachung verwenden. Ereignisse wie Pool löschen, Anfangs- und Aufgabe abgeschlossen in Batch Diagnoseprotokolle enthalten;

>[AZURE.NOTE] Protokollieren von Ereignissen für Batch-kontoressourcen erläutert, keine Arbeit und Ausgabedaten Aufgabe. Ausführliche Informationen zum Speichern der Ausgabedaten Ihrer Projekte und Aufgaben finden Sie unter [beibehalten Azure Batch Projekt und Ausgabe](batch-task-output.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Azure Batch-Konto](batch-account-create-portal.md)

* [Azure Storage-Konto](../storage/storage-create-storage-account.md#create-a-storage-account)

  Behalten Sie Diagnoseprotokolle Batch müssen Sie Azure Storage-Konto erstellen, Azure die Protokolle gespeichert werden. Geben Sie dieses Konto Speicher beim [Diagnoseprotokoll aktivieren](#enable-diagnostic-logging) für Batch-Konto. Das Speicherkonto angeben, wenn Sie Protokollsammlung aktivieren ist kein verknüpftes Speicherkonto gemäß Artikel [Anwendungspakete](batch-application-packages.md) und [Aufgabe Ausgabe Dauerhaftigkeit](batch-task-output.md) identisch.

  >[AZURE.WARNING] Sie können Daten in Azure Storage-Konto **belastet** . Dies schließt die Diagnoseprotokolle in diesem Artikel beschrieben. Beachten Sie dies beim Entwerfen der [Beibehaltungsrichtlinie](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).

## <a name="enable-diagnostic-logging"></a>Aktivieren des Diagnoseprotokolls

Diagnoseprotokolle ist für Batch-Konto standardmäßig nicht aktiviert. Sie müssen explizit das Diagnoseprotokoll für jeden Batch-Konto zu überwachen:

[Aktivieren der Sammlung von Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Wir empfehlen Sie lesen vollständige [Übersicht Azure Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) um Verständnis nicht nur wie Protokollierung, aber die Kategorien protokollieren von verschiedenen Azure Services unterstützt. Azure Batch unterstützt beispielsweise derzeit eine Kategorie: **Dienstprotokollen**.

## <a name="service-logs"></a>Dienst protokolliert

Azure Batch-Dienstprotokolle enthalten Ereignisse von Azure Batch-Dienst während der Lebensdauer einer Stapelverarbeitung Ressource wie ein Pool oder eine Aufgabe. Jedes Ereignis Batch wird im angegebenen Speicherkonto im JSON-Format gespeichert. Dies ist z. B. der Text ein Beispiel **Pool Ereignis erstellen**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Jede Ereignistext befindet sich in einer JSON-Datei im angegebenen Azure Storage-Konto. Wenn Sie Protokolle direkt zugreifen möchten, können Sie [Diagnoseprotokolle im Speicherkonto Schema](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account)überprüfen.

## <a name="service-log-events"></a>Protokoll-Ereignisse

Der Batch-Dienst gibt momentan Protokoll Folgendes. Diese Liste kann nicht vollständig, da zusätzliche Ereignisse hinzugefügt wurden seit der letzten dieser Artikel Aktualisierung.

| **Protokoll-Ereignisse** |
| ------------------ |
| [Pool erstellen][pool_create] |
| [Pool löschen starten][pool_delete_start] |
| [Pool löschen abgeschlossen][pool_delete_complete] |
| [Start der Pool-Größe][pool_resize_start] |
| [Pool-Größe abgeschlossen][pool_resize_complete] |
| [Aufgabe starten][task_start] |
| [Vorgang abgeschlossen][task_complete] |
| [Vorgang fehlgeschlagen][task_fail] |

## <a name="next-steps"></a>Nächste Schritte

Neben Diagnoseprotokoll Ereignisse in Azure Storage-Konto speichern, können Sie auch stream Batch-Protokoll Ereignisse ein [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), und [Azure Protokollanalyse](../log-analytics/log-analytics-overview.md)an.

* [Stream Azure Diagnoseprotokolle an Ereignis Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Stream Batch Diagnoseereignissen hochskalierbare Eindringen Service Event Hubs. Event Hubs können Millionen Ereignisse pro Sekunde aufnehmen, die Sie transformieren und speichern mit jedem Anbieter Echtzeitanalysen.

* [Analysieren von Azure Diagnoseprotokolle Protokollanalyse verwenden](../log-analytics/log-analytics-azure-storage-json.md)

  Senden Sie die Diagnoseprotokolle an Protokollanalyse können im Portal Operations Management Suite (OMS) analysieren, oder für die Analyse in Power BI oder Excel exportieren.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
