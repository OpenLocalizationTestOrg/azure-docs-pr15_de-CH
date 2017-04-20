<properties
   pageTitle="Log Analytics Alert REST-API"
   description="Log Analytics Alert REST API ermöglicht das Erstellen und Verwalten von Warnungen in Operations Management Suite (OMS).  Dieser Artikel erläutert die API und Beispiele für unterschiedliche Vorgänge."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Protokollanalyse Warnung REST-API

Log Analytics Alert REST API ermöglicht das Erstellen und Verwalten von Warnungen in Operations Management Suite (OMS).  Dieser Artikel erläutert die API und Beispiele für unterschiedliche Vorgänge.

Protokoll Analytics Suche REST API ist RESTful und über die REST-API von Azure Ressourcen-Manager zugegriffen werden. In diesem Dokument finden Sie Beispiele, die API zugegriffen wird, in einem PowerShell-Befehlszeile mit [ARMClient](https://github.com/projectkudu/ARMClient), eine open-Source-Befehlszeilentool, das vereinfacht den Azure-Ressourcen-Manager-API aufrufen. Die Verwendung von ARMClient und PowerShell ist eine von vielen Optionen auf das Protokoll Analytics Such-API zugreifen. Mit diesen Tools können Sie RESTful Azure Ressourcen-Manager-API Aufrufe OMS Arbeitsbereiche und Suche Befehle darin nutzen. Die API gibt Suche Ergebnisse im JSON-Format können Sie die Suchergebnisse programmgesteuert auf viele verschiedene Arten verwenden.

## <a name="prerequisites"></a>Erforderliche Komponenten
Alarme können derzeit nur mit einer gespeicherten Suche Protokollanalyse erstellt.  Sie können [Protokoll suchen REST API](log-analytics-log-search-api.md) Informationen verweisen.

## <a name="schedules"></a>Zeitpläne
Eine gespeicherte Suche haben einen oder mehrere Zeitpläne. Zeitplan definiert, wie oft die Suche ausführen und das Zeitintervall, die die Kriterien angegeben.
Zeitpläne haben die Eigenschaften in der folgenden Tabelle.

| Eigenschaft  | Beschreibung |
|:--|:--|
| Intervall | Wie oft die Suche ausgeführt wird. In Minuten gemessen. |
| QueryTimeSpan | Das Zeitintervall für die Kriterien ausgewertet. Muss gleich oder größer als Intervall sein. In Minuten gemessen. |
| Version | Die API-Version verwendet wird.  Derzeit sollte dies immer auf 1 festgelegt. |

Angenommen Sie, einer Ereignisabfrage eine Zeitspanne von 30 Minuten mit einem Intervall von 15 Minuten. In diesem Fall die Abfrage würde alle 15 Minuten ausgeführt und eine Warnung wird ausgelöst, wenn die Kriterien zu true aufgelöst eine Spanne von 30 Minuten.

### <a name="retrieving-schedules"></a>Abrufen von Zeitplänen
Verwenden Sie die Get-Methode, um alle Zeitpläne für gespeicherte Suche abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Verwenden Sie die Get-Methode mit eine ID ein bestimmtes Zeitplans für einer gespeicherten Suche abgerufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Folgendes zeigt Beispiel eine Antwort für einen Zeitplan.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Erstellen eines Zeitplans
Verwenden Sie die Put-Methode mit eine eindeutige ID einen neuen Zeitplan erstellen.  Beachten Sie, dass zwei Zeitpläne dieselbe ID haben können, auch wenn sie mit anderen gespeicherte Suchen.  Beim Erstellen eines Zeitplans in OMS-Konsole wird eine GUID für die Plan-ID erstellt.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Die Bearbeitung eines Zeitplans
Verwenden Sie die Put-Methode mit einer vorhandenen Zeitplan-ID für die gleiche gespeicherte Suche Zeitplans ändern.  Der Hauptteil der Anforderung muss das Etag Zeitplan enthalten.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Löschen von Zeitplänen
Verwenden Sie die Delete-Methode mit eine ID Zeitplan löschen.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Aktionen
Ein Zeitplan können mehrere Aktionen. Eine Aktion kann ein oder mehrere Prozesse oder eine e-Mail ein Runbook gestartet oder es kann einen Schwellenwert, der bestimmt, wann die Ergebnisse einer Suche einigen Kriterien entsprechen definieren.  Einige Aktionen definieren sowohl, so dass die Prozesse ausgeführt werden, wenn der Schwellenwert erreicht wurde.

Alle Aktionen haben die Eigenschaften in der folgenden Tabelle.  Unterschiedliche Warnungstypen haben verschiedene zusätzliche Eigenschaften, die nachstehend beschrieben werden.

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | Der Typ der Aktion.  Die möglichen Werte sind derzeit Warnung und Webhook. |
| Name | Name der Warnung angezeigt. |
| Version | Die API-Version verwendet wird.  Derzeit sollte dies immer auf 1 festgelegt. |

### <a name="retrieving-actions"></a>Aktionen werden abgerufen
Verwenden Sie die Get-Methode, um alle Aktionen für einen Zeitplan abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Verwenden der Get-Methode mit Aktions-ID eine bestimmte Aktion für einen Zeitplan abrufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Erstellen oder Bearbeiten von Aktionen
Verwenden Sie die Put-Methode mit einer Aktion eindeutige ID dem Plan eine neue Aktion erstellen.  Erstellen einer Aktion in der OMS-Konsole wird eine GUID für die Aktions-ID.

Verwenden der Put-Methode mit einer vorhandenen Aktions-ID für die gleiche gespeicherte Suche Zeitplans ändern.  Der Hauptteil der Anforderung muss das Etag Zeitplan enthalten.

Das Format der Anforderung zum Erstellen einer neuen Aktivität variiert je nach Aktivitätstyp so Beispielen in den folgenden Abschnitten bereitgestellt werden.

### <a name="deleting-actions"></a>Löschen von Aktionen
Verwenden Sie die Delete-Methode mit Aktions-ID eine Aktion zu löschen.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Warnaktionen
Ein Zeitplan sollte nur eine Warnaktion haben.  Warnaktionen haben eine oder mehrere Abschnitte in der folgenden Tabelle.  Jeder wird im folgenden ausführlich beschrieben.

| Abschnitt | Beschreibung |
|:--|:--|
| Schwellenwert | Kriterien für die Ausführung der Aktion. |  
| EmailNotification | Senden Sie e-Mail an mehrere Empfänger. |
| Behebung | Starten Sie ein Runbook in Azure Automation identifizierten Problems versucht. |

#### <a name="thresholds"></a>Schwellenwerte
Warnmeldungsaktion benötigen nur einen Schwellenwert.  Wenn die Ergebnisse einer gespeicherten Suche Schwellenwert in einem dieser Suche übereinstimmen, werden alle Prozesse in dieser Aktion ausgeführt.  Eine Aktion kann auch einen Schwellenwert enthalten mit anderen Typen verwendet werden, die Schwellenwerte nicht enthalten.

Schwellenwerte haben die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Operator | Operator für den Schwellenwertvergleich. <br> Gt = größer als <br> Lt = kleiner als |
| Wert | Wert für den Schwellenwert. |

Angenommen Sie, einer Ereignisabfrage mit einem Intervall von 15 Minuten, eine Zeitspanne von 30 Minuten und einen Schwellenwert von mehr als 10. In diesem Fall die Abfrage würde alle 15 Minuten ausgeführt und eine Warnung wird ausgelöst, wenn es 10 Ereignisse zurückgegeben, die über einen Zeitraum von 30 Minuten erstellt wurden.

Es folgt eine Beispielantwort für eine Aktion mit einem Schwellenwert.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Verwenden Sie verweigern eine einzigartige ID eine neuen Schwellenwertaktion für einen Zeitplan erstellen.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Verwenden Sie verweigern mit eine vorhandene Aktions-ID eine Schwellenwertaktion Zeitplan ändern.  Der Hauptteil der Anforderung muss das Etag der Aktion enthalten.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-Mail-Benachrichtigung
E-Mail-Benachrichtigungen senden e-Mail an einen oder mehrere Empfänger.  Sie gehören die in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Empfänger | Liste der e-Mail-Adressen. |
| Betreff | Der Betreff der e-Mail. |
| Anlage | Anlagen werden derzeit nicht unterstützt, damit diese immer den Wert "None". |

Folgendes zeigt Beispiel eine Antwort für eine e-Mail-Benachrichtigungsaktion mit einem Schwellenwert.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Verwenden Sie verweigern eine einzigartige ID eine neue e-Mail-Aktion für einen Zeitplan erstellen.  Das folgende Beispiel erstellt eine e-Mail-Benachrichtigung mit einem Schwellenwert, damit die e-Mail gesendet wird, wenn die Ergebnisse der gespeicherten Suche den Schwellenwert überschreiten.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Verwenden Sie verweigern mit eine vorhandene Aktions-ID eine e-Mail-Aktion für einen Zeitplan ändern.  Der Hauptteil der Anforderung muss das Etag der Aktion enthalten.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Abhilfemaßnahmen
Bereinigungsstatus starten ein Runbook in Azure-Automatisierung, die versucht, durch die Warnung angegebene Problem zu beheben.  Erstellen einer Webhook für eine Reparaturaktion verwendet Runbooks, und geben Sie den URI in der WebhookUri-Eigenschaft.  Beim Erstellen dieser Aktion mithilfe der OMS-Konsole wird eine neue Webhook für die Runbook automatisch erstellt.

Bereinigungsstatus gehören die in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| RunbookName | Name des Runbooks. Dieser muss veröffentlichten Runbook in der Projektmappe Automatisierung im Arbeitsbereich OMS konfiguriert Automation-Konto übereinstimmen. |
| WebhookUri | URI der Webhook.
| Ablauf | Das Ablaufdatum und die Uhrzeit der Webhook.  Wenn der Webhook eine Ablaufzeit hat, kann dies gültige zukünftiges Datum sein. |

Folgendes zeigt Beispiel eine Antwort für eine Reparaturaktion mit einem Schwellenwert.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Verwenden Sie verweigern eine einzigartige ID eine neue Reparaturaktion für einen Zeitplan erstellen.  Das folgende Beispiel erstellt eine Wiederherstellung mit einem Schwellenwert, sodass Runbook gestartet wird, wenn die Ergebnisse der gespeicherten Suche den Schwellenwert überschreiten.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Verwenden der Put-Methode mit einem vorhandenen Aktions-ID eine Reparaturaktion für einen Zeitplan ändern.  Der Hauptteil der Anforderung muss das Etag der Aktion enthalten.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Beispiel
Es folgt ein vollständiges Beispiel zum Erstellen einer neuen e-Mail-Benachrichtigung.  Dies erstellt einen neuen Zeitplan sowie eine Aktion mit einem Schwellenwert und e-Mail.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook Aktionen
Webhook Aktionen starten indem URL aufrufen und optional eine Nutzlast gesendet werden.  Sie ähneln Abhilfemaßnahmen, außer sie für Webhooks gedacht, die Prozesse Azure Automatisierung Runbooks aufrufen kann.  Darüber hinaus bieten die zusätzliche Möglichkeit Nutzlast Prozesses bereitgestellt werden.

Webhook Aktionen keinen Schwellenwert aber stattdessen einen Zeitplan, der eine Warnaktion Schwellenwert hinzugefügt werden.  Sie können mehrere Webhook Aktionen hinzufügen, die alle ausgeführt wird, wenn der Schwellenwert erreicht wird.

Webhook zählen die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| WebhookUri | Der Betreff der e-Mail. |
| CustomPayload | Benutzerdefinierte Nutzlast an den Webhook gesendet werden.  Das Format hängt die Webhook erwarten. |

Folgendes zeigt Beispiel eine Antwort Webhook und zugehörigen Warnmeldungsaktion mit einem Schwellenwert.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Erstellen oder Bearbeiten einer Aktion webhook
Verwenden Sie verweigern eine einzigartige ID eine neue Webhook Aktion für einen Zeitplan erstellen.  Im folgende Beispiel erstellt eine Aktion Webhook und eine Warnung mit einem Schwellenwert, sodass die Webhook ausgelöst wird, wenn die Ergebnisse der gespeicherten Suche den Schwellenwert überschreiten.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Verwenden Sie verweigern mit eine vorhandene Aktions-ID für einen Zeitplan eine Aktion Webhook ändern.  Der Hauptteil der Anforderung muss das Etag der Aktion enthalten.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie die [REST-API Protokoll Suchvorgänge](log-analytics-log-search-api.md) in Protokollanalyse.
