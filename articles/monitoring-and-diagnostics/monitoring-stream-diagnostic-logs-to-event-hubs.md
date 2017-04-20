<properties
    pageTitle="Stream Azure Diagnoseprotokolle an Ereignis Hubs | Microsoft Azure"
    description="Enthält Informationen zum Streamen von Azure Diagnoseprotokolle an Ereignis Hubs."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Stream Azure Diagnoseprotokolle an Ereignis Hubs

**[Azure Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md)** können nahezu in Echtzeit für jede Anwendung mit der integrierten "Exportieren, Ereignis Hubs" Option im Portal oder aktivieren Sie die Bus Regel-Id in einer Diagnose über Azure PowerShell-Cmdlets oder Azure CLI übertragen.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Diagnoseprotokolle mit Event Hubs möglich
Hier sind ein paar Methoden Streaming-Funktion für Diagnoseprotokolle verwenden können:

- **Stream Protokolle 3rd Party Protokollierung und telemetriesysteme** – werden im Laufe der Zeit Ereignis Hubs streaming Mechanismus leiten die Diagnoseprotokolle in Drittanbieter SIEMs und analyselösungen.

- **Dienststatus durch streaming "Langsamster Pfad" Daten PowerBI Ansicht** verwenden Ereignis Hubs, Stream Analytics und PowerBI können die Diagnosedaten in Echtzeit Einblicke in leicht Azure Services umwandeln. [Diese Dokumentation Artikel bietet einen groben Überblick über das Event Hubs einrichten, Prozessdaten mit Stream Analytics und PowerBI als Ausgabe verwenden](../stream-analytics/stream-analytics-power-bi-dashboard.md). Hier ist einige Tipps für die Einrichtung mit Diagnoseprotokolle:
    - Ereignis Hubs für eine Kategorie von Diagnoseprotokollen wird automatisch erstellt, wenn das Kontrollkästchen im Portal oder über PowerShell aktivieren, sollten Sie Ereignis-Hubs in Service Bus-Namespace mit dem Namen auswählen, die mit"Einblicke" beginnt
    - Hier ist eine Beispielabfrage Stream Analytics, mit einfach alle Protokolldaten in einer PowerBI Tabelle analysieren:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Erstellen einer benutzerdefinierten Telemetrie und Protokollierung Plattform** – Wenn Sie bereits eine benutzerdefinierte Telemetrie Plattform oder zu eine hochgradig skalierbare Veröffentlichen-Abonnieren Art Event Hubs können Sie flexibel Diagnoseprotokolle aufnehmen. [Siehe Dan Rosanova Anleitung zur Verwendung von Ereignis-Hubs in einem weltweit Telemetrie Plattform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen streaming
Sie können streaming Diagnoseprotokolle programmgesteuert über das Portal oder [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx)verwenden. In beiden Fällen wählen Sie einen Service Bus-Namespace und ein Ereignis Hubs im Namespace für jedes Protokollkategorie, die Sie aktivieren erstellt. Diagnostische **Kategorie** ist ein Protokoll, das eine Ressource erheben. Sie können auswählen, welche Kategorien protokollieren für eine bestimmte Ressource in Azure-Portal unter Diagnose Blade sammeln möchten.

![Kategorien protokollieren im Portal](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Aktivieren und streaming Diagnoseprotokolle von Compute-Ressourcen (z. B. VMs oder Service Fabric) [erfordert andere Schritte](../event-hubs/event-hubs-streaming-azure-diags-data.md).

### <a name="via-powershell-cmdlets"></a>Über PowerShell-Cmdlets
Damit streaming über [Azure PowerShell-Cmdlets](insights-powershell-samples.md)können Sie die `Set-AzureRmDiagnosticSetting` Cmdlet mit folgenden Parametern:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

Service Bus Regel-ID ist eine Zeichenfolge mit diesem Format: `{service bus resource ID}/authorizationrules/{key name}`, z. B. `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Über Azure-CLI
Damit streaming über die [Azure-Befehlszeilenschnittstelle](insights-cli-samples.md)können Sie die `insights diagnostic set` Befehl wie folgt:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Verwenden Sie dasselbe Format wie für das PowerShell-Cmdlet für Service Bus Regel-ID.

###<a name="via-azure-portal"></a>Azure-Portal
Damit streaming über das Azure-Portal auf die Diagnose einer Ressource navigieren Sie, und wählen Sie "Export an Event Hub".

![Ereignis-Hubs im Portal exportieren](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Wählen Sie zum Konfigurieren einer vorhandenen Service Bus-Namespace. Ausgewählten Namespace werden dem Ereignis Hubs erstellt (ist dies der erste Diagnoseprotokolle der Time streaming) oder zum Streaming (wenn es bereits Ressourcen, die dieser Kategorie auf streaming), und die Richtlinie definiert die Berechtigungen der Streaming-Mechanismus. Heute muss auf ein Ereignis Hubs Berechtigungen verwalten, lesen und senden. Erstellen oder ändern Service Bus-Namespace freigegebene Richtlinien im Verwaltungsportal Registerkarte "Konfigurieren" für Ihr Service Bus-Namespace. Um diese Diagnose zu aktualisieren, muss der Client die ListKey auf Service Bus-Autorisierungsregel Berechtigung.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Wie wird verwenden die Protokolldaten von Ereignis?
Beispieldaten aus der Ereignis-Hubs Ausgabe lautet

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Elementnamen | Beschreibung                                            |
|--------------|--------------------------------------------------------|
|Datensätze       | Ein Array von alle Protokollereignisse Diese Nutzlast.            |
|Zeit          | Uhrzeit des Ereignisses.                      |
|Kategorie      | Kategorie für dieses Ereignis.                           |
|resourceId    | Ressourcen-ID der Ressource, die dieses Ereignis generiert. |
|operationName | Name des Vorgangs.                                 |
|Ebene         | Optional. Gibt die Ereignisebene an.               |
|Eigenschaften    | Eigenschaften des Ereignisses.                               |


Eine Liste der Ressourcenanbieter, die an Event Hub unterstützen anzeigen [hier](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen über Azure Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md)
- [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
