<properties
    pageTitle="Webhooks Azure metrische Warnungen konfigurieren | Microsoft Azure"
    description="Azure-Warnungen auf anderen Systemen nicht Azure umgeleitet."
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
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurieren einer Webhook Azure metrische Ausschreibung

Webhooks können eine Azure Warnmeldung an andere Systeme Nachbearbeitung oder benutzerdefinierte Aktionen weiterleiten. Können eine Webhook einer Dienste, die SMS, Fehler protokollieren, ein Team über Chat/Messagingdienste benachrichtigen oder führen Sie eine beliebige Anzahl anderer Aktionen weiterleiten. Dieser Artikel beschreibt wie eine Webhook Azure metrische Ausschreibung und sieht die Nutzlast für HTTP POST zu einer Webhook. Informationen über die Installation und das Schema für Azure Aktivitätsprotokoll Warnung (Alert auf Ereignisse) [dieser Seite erscheint](./insights-auditlog-to-webhook-email.md).

Azure-Warnungen HTTP POST alert Inhalt in JSON format Schema definiert, um ein Webhook URI, der beim Erstellen der Warnung bereitgestellt. Dieser URI muss eine gültige HTTP oder HTTPS-Endpunkt. Azure stellen einen Eintrag pro Anforderung eine Warnung aktiviert ist.

## <a name="configuring-webhooks-via-the-portal"></a>Webhooks über das Portal konfigurieren

Sie können hinzufügen oder Aktualisieren der Webhook URI im Fenster erstellen/aktualisieren im [Portal](https://portal.azure.com/).

![Hinzufügen einer Regel](./media/insights-webhooks-alerts/Alertwebhook.png)

Konfigurieren Sie eine Warnung an Webhook URI mit [Azure PowerShell-Cmdlets](./insights-powershell-samples.md#create-alert-rules), [Plattformübergreifende CLI](./insights-cli-samples.md#work-with-alerts)oder [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Authentifizierung der webhook

Die Webhook kann mit einer dieser Methoden authentifizieren:

1. **Token-basierte Autorisierung** - Webhook URI wird z.B. eine token-ID gespeichert. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Grundlegende Autorisierung** - Webhook URI werden z. B. mit einem Benutzernamen und Kennwort. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Nutzlast-schema

POST-Operation enthält die folgenden JSON-Nutzlast und das Schema für alle Metrik-basierte Alarme.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Feld | Obligatorisch | Feste Gruppe von Werten | Notizen |
| :-------------| :-------------   | :-------------   | :-------------   |
|Status|Y|"Aktiviert", "Gelöst"|Status für die Warnung basiert auf die Bedingung, die Sie festgelegt haben.|
|Kontext| Y | | Der Kontext der Warnung.|
|Zeitstempel| Y | | Der Zeitpunkt, an dem die Warnung ausgelöst wurde.|
|ID | Y | | Jede Regel hat eine eindeutige Kennung.|
|Name               |Y                  |                   | Der Name der Warnung.|
|Beschreibung        |Y                  |                           |Beschreibung der Warnung.|
|conditionType      |Y                  |"Metrisch", "Ereignis"          |Zwei Arten von Warnungen werden unterstützt. Basierend auf einer metrischen Bedingung und basierend auf einem Ereignis im Aktivitätsprotokoll. Verwenden Sie diesen Wert um zu überprüfen, ob die Warnung Metrik oder Ereignis basiert.|
|Bedingung          |Y                  |                           | Felder für die ConditionType überprüfen.|
|metricName         |für metrische Alarme  |                           |Der Name der Metrik, die definiert, was die Regel überwacht.|
|metricUnit         |für metrische Alarme  |"Bytes", "BytesPerSecond", "Anzahl", "CountPerSecond", "Prozent", "Sekunden"|     Die Einheit, in der Metrik zulässig. [Zulässige Werte sind hier aufgeführt](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |für metrische Alarme  |                           |Der tatsächliche Wert der Metrik, die die Warnung verursacht hat.|
|Schwellenwert          |für metrische Alarme  |                           |Der Schwellenwert, an dem die Warnung aktiviert ist.|
|windowSize         |für metrische Alarme  |                           |Der Zeitraum, mit der Warnung Aktivität basierend auf den Schwellenwert überwachen. Muss zwischen 5 Minuten und 1 Tag. Dauer ISO 8601-Format.|
|timeAggregation    |für metrische Alarme  |"Durchschnitt", "Last", "Maximum", "Minimum", "None", "insgesamt" | Wie die gesammelten Daten mit der Zeit kombiniert werden soll. Der Standardwert ist Durchschnitt. [Zulässige Werte sind hier aufgeführt](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|Operator           |für metrische Alarme  |                           |Der Operator zum Vergleichen der aktuellen Messdaten Schwellenwert festlegen.|
|subscriptionId     |Y                  |                           |Azure-Abonnement-ID.|
|resourceGroupName  |Y                  |                           |Name der Ressourcengruppe für die betroffenen Ressource.|
|resourceName       |Y                  |                           |Ressourcenname der betroffenen Ressource.|
|resourceType       |Y                  |                           |Ressourcentyp der betroffenen Ressource.|
|resourceId         |Y                  |                           |Ressourcen-ID der betroffenen Ressource.|
|resourceRegion     |Y                  |                           |Region oder der betroffenen Ressource.|
|portalLink         |Y                  |                           |Direkter Link auf die Zusammenfassungsseite Portal Ressource.|
|Eigenschaften         |N                  |Optionale                   |Set `<Key, Value>` Paare (z.B. `Dictionary<String, String>`), das Details zum Ereignis enthält. Das Eigenschaften-Feld ist optional. In einem benutzerdefinierten UI oder Logik app-basierte Workflow können Benutzer Schlüssel eingeben, die über die Nutzdaten übergeben werden können. Andere geht benutzerdefinierte Eigenschaften an die Webhook übergeben über den Webhook-Uri selbst (als Abfrageparameter)|


>[AZURE.NOTE] Das Feld Eigenschaften kann nur in [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx)festgelegt werden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure-Warnungen und Webhooks video [Integrieren Azure Warnungen PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
- [Azure-Warnungen Azure Automatisierungsskripts (Runbooks) ausführen](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Verwenden Sie Logik App SMS über Twilio von Azure Warnung senden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Verwenden Sie Logik App eine Pufferzeit von Azure Warnmeldung Nachricht](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Verwenden Sie Logik-App, um eine Nachricht an eine Warteschlange Azure von Azure Warnmeldung](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
