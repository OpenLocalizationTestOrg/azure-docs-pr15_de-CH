<properties
   pageTitle="Logik Apps Ausnahmebehandlung | Microsoft Azure"
   description="Erfahren Sie Muster für Fehler und Ausnahmen mit Azure Logik Apps"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>Logikfehler Apps und Ausnahmebehandlung

Logik Apps bietet zahlreiche Tools und um sicherzustellen, dass Ihre Integration sind robust und stabil vor Ausfällen.  Eines der Probleme mit der Architektur der Datenintegration besteht darin, Ausfälle oder Probleme der abhängigen Systeme entsprechend behandelt werden.  Logik Apps macht Fehlerbehandlung ein erstklassiges Erlebnis der Werkzeuge müssen Sie Ausnahmen und Fehler in Ihren Workflows.

## <a name="retry-policies"></a>Wiederholen von Richtlinien

Die meisten Ausnahme- und Fehlerbehandlung ist Retry-Policy.  Diese Richtlinie legt fest, ob die Aktion wiederholen muss Anfrage überschritten oder nicht (Anfragen eine 429 führte oder 5xx-Antwort).  Alle Aktionen wiederholen standardmäßig 4 zusätzliche Zeiten über 20 Sekunden.  Wenn die erste Anforderung empfangen ein `500 Internal Server Error` Antwort das Workflowmodul hält für 20 Sekunden, und versuchen Sie die Anforderung erneut.  Ist nach Versuche die Antwort noch eine Ausnahme oder ein Fehler, der Workflow fortgesetzt und Markieren des Aktionsstatus als `Failed`.

Sie können die **Eingaben** von einem bestimmten Vorgang wiederholungsrichtlinien konfigurieren.  Wiederholen-Richtlinie kann 4 über 1 Stunde Intervalle Versuche konfiguriert werden.  Die Eingabeeigenschaften Einzelheiten entnehmen [auf MSDN][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Wenn Ihre HTTP-Aktion zu wiederholen 4 Mal 10 Minuten zwischen den einzelnen versuchen möchten müssten Sie die folgende Definition:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Weitere Informationen zu unterstützten Syntax anzeigen [Wiederholen - Abschnitt auf der MSDN-][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>RunAfter-Eigenschaft zu Fehlern

Jede logische app Aktion deklariert die Aktionen müssen abgeschlossen werden, bevor die Aktion gestartet wird.  Sie können dieses vorstellen, wie die Reihenfolge der Schritte im Workflow.  Diese Reihenfolge gilt als die `runAfter` -Eigenschaft in der Aktionsdefinition.  Es ist ein Objekt, das beschreibt, welche Aktionen und Status der Aktion die Aktion ausgeführt werden.  Alle Aktionen, die mithilfe des Designers hinzugefügt werden standardmäßig auf `runAfter` den vorherigen Schritt wurde das zuvor `Succeeded`.  Allerdings können Sie diesen Wert, um Aktionen ausgelöst werden, wenn vorherige Aktionen sind `Failed`, `Skipped`, oder mögliche Werte.  Wenn Sie einen bestimmten Service Bus-Thema nach einer bestimmten Aktion hinzufügen `Insert_Row` fehlschlägt, verwenden Sie die folgenden `runAfter` Konfiguration:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Beachten Sie die `runAfter` -Eigenschaft ausgelöst, wenn die `Insert_Row` ist `Failed`.  Zum Ausführen der Aktion wird der Status der Aktivität `Succeeded`, `Failed`, oder `Skipped` die Syntax:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Aktionen, die nach eine vorherige Aktion gescheitert ausgeführt und erfolgreich abgeschlossen markiert als `Succeeded`.  Dies bedeutet, wenn Sie erfolgreich alle Fehler in einem Workflow selbst ausführen als gekennzeichnet ist `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Bereiche und Ergebnisse auswerten Aktionen

Ähnlich wie ausführen können nach einzelnen Aktionen Sie Gruppenaktionen zusammen in einem [Bereich](app-service-logic-loops-and-scopes.md) - die als eine logische Gruppierung von Aktionen dienen.  Bereiche sind hilfreich zum Organisieren Ihrer Logik app Aktionen und aggregate Bewertungen für den Status eines Bereichs ausgeführt.  Der Bereich selbst erhalten Status nach Abschluss der Aktionen innerhalb eines Bereichs.  Die Kriterien ausführen – bestimmt der Bereich Status ist die letzte Aktion in einer Verzweigung `Failed` oder `Aborted` Status `Failed`.

Sie können `runAfter` ein Bereich markiert wurde `Failed` bestimmte Aktionen für Fehler, die innerhalb des Bereichs ausgelöst.  Nach dem Ausfall eines Bereichs können Sie eine einzelne Aktion um Fehler abzufangen, wenn *keine* Aktionen im Rahmen erstellen.

### <a name="getting-the-context-of-failures-with-results"></a>Abrufen des Kontexts von Fehlern mit Ergebnissen

Sie sollten auch den Kontext verstehen genau welche Aktionen fehlgeschlagen und Fehler oder zurückgegebenen Statuscodes Abfangen von Fehlern aus einem Bereich sehr nützlich ist.  Die `@result()` Workflow-Funktion bietet Kontext in das Ergebnis aller Aktionen innerhalb eines Bereichs.

`@result()`Gibt ein Array aller Aktion Ergebnisse in diesem Bereich hat einen einzelnen Parameter, Namen  Diese Aktionsobjekte enthalten die gleichen Attribute wie der `@actions()` -Objekts, einschließlich Aktion Startzeit, Endzeit der Aktivität Aktionsstatus, Aktion Eingaben, Aktion Korrelations-IDs und Aktion gibt.  Kann einfach aufgebaut ein `@result()` funktioniert mit einer `runAfter` an Kontext fehlgeschlagene Vorgänge innerhalb eines Bereichs.

Wenn eine Aktion *für jede* Aktion in einem Bereich auszuführen, `Failed`, Koppeln Sie `@result()` eine Aktion **[Filtern](../connectors/connectors-native-query.md)** mit einer **[ForEach](app-service-logic-loops-and-scopes.md)** -Schleife.  Dadurch können Sie das Array mit Ergebnissen Aktionen filtern, die nicht.  Sie können gefilterte Ergebnisarray und Aktion beim Fehlschlagen die **ForEach** -Schleife verwenden.  Hier ist ein Beispiel folgt eine ausführliche Erläuterung.  In diesem Beispiel sendet eine HTTP POST-Anforderung mit der Antwort fehlgeschlagene Vorgänge im Rahmen `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Hier wird eine detaillierte Anleitung geschieht:

1. **Filter-Array** -Aktion zum Filtern der `@result('My_Scope')` das Ergebnis aller Aktivitäten in`My_Scope`
1. Das **Filtern** ist eine `@result()` Artikel mit dem Status gleich `Failed`.  Filtert das Array alle Aktionsergebnisse aus `My_Scope` in ein Array der fehlgeschlagenen Aktion.
1. Durchführen Sie **für jede** Aktion auf dem **Array gefiltert**  Dies führt eine Aktion *für jede* Aktionsergebnis wir über gefiltert.
    - Wäre eine einzelne Aktion, die nicht die Aktivitäten im Bereich der `foreach` wird nur einmal ausgeführt.  Viele fehlgeschlagene Aktionen würde eine Aktion pro Fehler.
1. Senden einer HTTP-POST auf der `foreach` Antworttexts, Artikel oder `@item()['outputs']['body']`.  Die `@result()` Element Form entspricht der `@actions()` Form und dieselbe Weise analysiert werden können.
1. Ebenfalls zwei benutzerdefinierte Header mit dem Namen fehlgeschlagene Aktion `@item()['name']` und fehlgeschlagenen Client tracking-ID `@item()['clientTrackingId']`.

Zu Referenzzwecken hier ist ein Beispiel eines `@result()` Element.  Sie sehen die `name`, `body`, und `clientTrackingId` Eigenschaften im obigen Beispiel analysiert.  Anzumerken, außerhalb von einer `foreach`, `@result()` gibt ein Objektarray zurück.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Die obigen Ausdrücke können unterschiedliche Muster für die Ausnahmebehandlung ausführen.  Sie können zum Ausführen einer einzelnen Ausnahmebehandlung außerhalb des Bereichs, den gesamte gefilterte Array entfernen und akzeptiert die `foreach`.  Sie können auch andere nützlichen Eigenschaften der `@result()` oben angezeigte Antwort.

## <a name="azure-diagnostics-and-telemetry"></a>Azure Diagnostics und Telemetrie

Die oben genannten sind hervorragende Möglichkeit zum Behandeln von Fehlern und Ausnahmen innerhalb eines jedoch können auch und Fehler unabhängig von der Ausführung selbst.  [Azure-Diagnose](app-service-logic-monitor-your-logic-apps.md) bietet eine einfache Möglichkeit, alle Workflowereignisse (einschließlich aller ausführen und Aktion Status) ein Azure Storage-Konto oder ein Azure Event Hub zu senden.  Sie können die Protokolle und Metriken überwachen oder in jede gewünschte ausführen Status auswerten Überwachungstool veröffentlichen.  Eine mögliche Option ist die Ereignisse durch Azure Event Hub in [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)stream.  Im Stream Analytics können Sie live von Anomalien, Durchschnittswerte oder Fehler in die Diagnoseprotokolle Abfragen.  Stream Analytics können problemlos mit anderen Datenquellen wie Warteschlangen Themen, SQL, DocumentDB und Power BI ausgeben.

## <a name="next-steps"></a>Nächste Schritte
- [Wie Kunden finden Sie robuste Fehlerbehandlung mit Logik Apps erstellt](app-service-logic-scenario-error-and-exception-handling.md)
- [Weitere Logik Apps Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Erstellen Sie automatisierte Bereitstellung von Logik-apps](app-service-logic-create-deploy-template.md)
- [Entwerfen und Bereitstellen von Logik apps aus Visual Studio](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9