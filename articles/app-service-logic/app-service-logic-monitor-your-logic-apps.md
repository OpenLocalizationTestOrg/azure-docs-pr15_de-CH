<properties 
    pageTitle="Logik-apps in Azure App Service überwachen | Microsoft Azure" 
    description="Wie Sie also die Logik apps" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>Überwachen von Logik-apps

Nachdem Sie [eine Logik erstellen](app-service-logic-create-a-logic-app.md)die vollständige Versionsgeschichte seiner Ausführung in Azure-Portal angezeigt.  Sie können auch wie Azure-Diagnose und Azure Alerts Ereignisse in Echtzeit überwachen, und warnen Sie Ereignisse wie "bei mehr als 5 läuft innerhalb einer Stunde durchführen."

## <a name="monitor-in-the-azure-portal"></a>Monitor im Azure-Portal

Um den Verlauf anzeigen möchten, wählen Sie **Durchsuchen**und **Logik-Apps**. Eine Liste aller Logik Apps in Ihrem Abonnement wird angezeigt.  Wählen Sie die Logik-app überwachen möchten.  Sie sehen eine Liste aller Aktionen und Triggern Logik App aufgetreten sind.

![Übersicht](./media/app-service-logic-monitor-your-logic-apps/overview.png)

Es gibt einige Abschnitte auf dieser hilfreich:

- **Zusammenfassung** listet **Alle** und **Trigger Verlauf**
    - **Alle** Liste neueste Logik app ausgeführt wird.  Ausführliche Informationen zum Ausführen eine Zeile klicken oder klicken Sie auf die Kachel mehrere Testläufe aufgelistet.
    - **Trigger** Verlaufslisten alle Trigger-Aktivität für diese Anwendung Logik.  Trigger-Aktivität könnte eine "Übersprungene" Überprüfung auf neue Daten (z. B., ob eine neue Datei mit FTP hinzugefügt wurde), "Erfolgreich", was bedeutet, dass Daten zurückgegeben wurden, eine Logik-app ausgelöst, oder "Fehlgeschlagen" Fehler in der Konfiguration entspricht.
- **Diagnose** können Sie Runtime Details und Ereignisse anzeigen und Abonnieren von [Azure Alerts](#adding-azure-alerts)

>[AZURE.NOTE] Alle Details der Common Language Runtime und Ereignisse werden im Ruhezustand innerhalb der Logik-App-Dienst verschlüsselt. Sie werden nur auf Antrag anzeigen ein Benutzer entschlüsselt. Zugriff auf diese Ereignisse kann auch von Azure Role-Based Access Control (RBAC) gesteuert.

### <a name="view-the-run-details"></a>Zur Detailansicht

Diese Liste führt zeigt **Status**, die **Startzeit**und die **Dauer** des jeweiligen ausführen. Wählen Sie eine Zeile angezeigt, ausgeführt.

Die Überwachung Ansicht zeigt jeden Schritt ausführen, die Eingaben und Ausgaben und Fehlermeldungen, die möglicherweise Occurre.

![Ausführen und Aktionen](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Benötigen Sie zusätzliche Details wie die Laufzeit **Korrelations-ID** (der die REST API verwendet), können Sie die **Testlaufdetails** klicken.  Dazu gehören alle Schritte, Status und ein-und Ausgänge für die Ausführung.

## <a name="azure-diagnostics-and-alerts"></a>Azure Diagnostics und Alarme

Neben den Angaben von Azure-Portal und REST API oben können Sie Ihre Anwendung Logik Verwendung von Azure Diagnostics für umfangreiche mehr und Debuggen konfigurieren.

1. Klicken Sie auf **den Diagnoseabschnitt der Logik app blade**
1. Klicken Sie, um die **Diagnose Settings**
1. Konfigurieren einer Event Hub oder Speicherkonto Daten ausgeben

    ![Azure Diagnostics settings](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>Azure Alerts hinzufügen

Nach der Konfiguration von Diagnose können Azure Warnungen ausgelöst werden, wenn bestimmte Schwellenwerte.  Wählen Sie Blatt **Diagnose** **Alarme** Kachel und **Benachrichtigung hinzufügen**.  Dies führt Sie durch Konfigurieren einer Warnung basierend auf einer Reihe von Grenzwerten und Metriken.

![Azure Alert Metriken](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

Sie können die **Bedingung** **Schwellenwert**und **Zeitraum** wie gewünscht konfigurieren.  Schließlich können Sie konfigurieren eine e-Mail-Adresse um eine Benachrichtigung zu senden oder eine Webhook konfigurieren.  [Anforderung Trigger](../connectors/connectors-native-reqres.md) können in einer Anwendung Logik auf eine Warnung (, Dinge wie [Pufferzeit buchen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [einen Text senden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)oder [eine Nachricht an eine Warteschlange](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)).

### <a name="azure-diagnostics-settings"></a>Azure Diagnostics Settings

Jedes dieser Ereignisse enthält Details über die Logik-app und das Ereignis Status.  Hier ist ein Beispiel für ein *ActionCompleted* Ereignis:

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

Zwei Eigenschaften, die besonders für die Verfolgung und Überwachung sind *ClientTrackingId* und *TrackedProperties*.  

#### <a name="client-tracking-id"></a>Client-Tracking-ID

Der Client die tracking-ID ist ein Wert, der Ereignisse über eine Logik-app ausführen, einschließlich geschachtelter Workflows als Teil einer Anwendung Logik korrelieren.  Diese ID wird automatisch generiert, wenn nicht angegeben, aber den Client die tracking-ID von einem Trigger übergeben können manuell angeben einer `x-ms-client-tracking-id` Header mit dem ID-Wert in der Trigger-Anforderung (Anforderung Trigger, HTTP-Trigger oder Webhook Trigger).

#### <a name="tracked-properties"></a>Nachverfolgte Eigenschaften

Aktionen in der Workflowdefinition verfolgen Eingaben oder Ausgaben Diagnosedaten können nachverfolgte Eigenschaften hinzugefügt werden.  Dies ist nützlich zum Nachverfolgen von Daten wie "Bestellnummer" in der Telemetrie.  Wenn eine nachverfolgte Eigenschaft hinzufügen, die `trackedProperties` -Eigenschaft für eine Aktion.  Nachverfolgte Eigenschaften können nur Nachverfolgen einer einzelnen Aktionen Eingaben und Ausgaben, aber Sie können die `correlation` Eigenschaften der Ereignisse Aktionen in einem Testlauf korrelieren.

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>Erweitern von Projektmappen

Sie können diese Telemetrie Event Hub oder Speicher in andere Dienste wie [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)und [Power BI](https://powerbi.com) Echtzeit Überwachung Ihrer Integration Workflows nutzen.

## <a name="next-steps"></a>Nächste Schritte
- [Häufige Beispiele und Szenarien für Logik-apps](app-service-logic-examples-and-scenarios.md)
- [Erstellen eine Logik App Bereitstellungsvorlage](app-service-logic-create-deploy-template.md)
- [Enterprise-Integrationsfunktionen](app-service-logic-enterprise-integration-overview.md)
