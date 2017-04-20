<properties
    pageTitle="Konfigurieren einer Webhook auf Azure-Aktivitätsprotokoll Alerts | Microsoft Azure"
    description="Finden Sie unter Alerts Aktivitätsprotokoll, mit Webhooks aufzurufen. "
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Konfigurieren einer Webhook auf ein Azure Aktivitätsprotokoll alertS

Webhooks können eine Azure Warnmeldung an andere Systeme Nachbearbeitung oder benutzerdefinierte Aktionen weiterleiten. Können eine Webhook einer Dienste, die SMS, Fehler protokollieren, ein Team über Chat/Messagingdienste benachrichtigen oder führen Sie eine beliebige Anzahl anderer Aktionen weiterleiten. Dieser Artikel beschreibt wie eine Webhook Azure-Aktivitätsprotokoll Ausschreibung und sieht die Nutzlast für HTTP POST zu einer Webhook. Informationen zur Einrichtung und Schema für eine Azure metrische Warnung [dieser Seite erscheint](insights-webhooks-alerts.md). Sie können auch eine Warnung Aktivitätsprotokoll einrichten e-Mail aktiviert.

>[AZURE.NOTE] Diese Funktion ist zurzeit in der Vorschau, so erwarten Variable Qualität und Leistung.

Sie können ein Aktivitätsprotokoll Warnung mithilfe von [Azure PowerShell-Cmdlets](insights-powershell-samples.md#create-alert-rules), [Plattformübergreifende CLI](insights-cli-samples.md#work-with-alerts)oder [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx)einrichten.

## <a name="authenticating-the-webhook"></a>Authentifizierung der webhook
Ddie Webhook kann Authentifizierung mithilfe einer dieser Methoden:

1. **Token-basierte Autorisierung** - Webhook URI wird z.B. eine token-ID gespeichert. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Grundlegende Autorisierung** - Webhook URI werden z. B. mit einem Benutzernamen und Kennwort. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Nutzlast-schema
POST-Operation enthält die folgenden JSON-Nutzlast und das Schema für alle Aktivitätsprotokoll-basierte Alarme. Dieses Schema ähnelt einem Metrik-basierte Alarme.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Elementnamen       |Beschreibung|
|---                |---|
|Status             |Für metrische Alerts verwendet. Immer für Aktivitätsprotokoll Alerts auf "aktiviert" festgelegt.|
|Kontext            |Kontext des Ereignisses.|
|resourceProviderName|Der Ressourcenanbieter der betroffenen Ressource.|
|conditionType      |Immer "Ereignis".|
|Name               |Name der Warnregel.|
|ID                 |Ressourcen-ID der Warnung.|
|Beschreibung        |Beschreibung der Warnung wie beim Erstellen der Warnung.|
|subscriptionId     |Azure-Abonnement-ID.|
|Zeitstempel          |Uhrzeit, zu der das Ereignis von Azure Service generiert wurde, die die Anforderung verarbeitet.|
|resourceId         |Ressourcen-ID der betroffenen Ressource.|
|resourceGroupName  |Name der Ressourcengruppe für die betroffenen Ressource|
|Eigenschaften         |Set `<Key, Value>` Paare (z.B. `Dictionary<String, String>`), das Details zum Ereignis enthält.|
|Ereignis              |Element mit Metadaten über das Ereignis.|
|Autorisierung      |Die RBAC-Eigenschaften des Ereignisses. Dazu gehören normalerweise "Aktion", "Rolle" und "Bereich".|
|Kategorie           |Kategorie des Ereignisses. Gültige Werte sind: Verwaltung, Warnung, ServiceHealth, die Empfehlung.|
|Anrufer             |E-Mail-Adresse des Benutzers, der die Operation, UPN-Anspruch oder SPN Anspruch auf Verfügbarkeit ausgeführt. Kann für bestimmte Systemaufrufe null sein.|
|correlationId      |In der Regel eine GUID im Zeichenfolgenformat. Mit CorrelationId gehören zu größeren dieselbe Aktion und normalerweise eine CorrelationId freigeben.|
|eventDescription   |Statische Beschreibung des Ereignisses.|
|eventDataId        |Eindeutiger Bezeichner für das Ereignis.|
|Ereignisquelle        |Name der Azure Service oder Infrastruktur, die das Ereignis generiert hat.|
|httpRequest        |Enthält in der Regel "ClientRequestId", "ClientIpAddress" und "Methode" (HTTP-Methode z. B. PUT).|
|Ebene              |Die folgenden Werte: "Kritisch", "Fehler", "Warning", "Information" und "Verbose".|
|operationId        |In der Regel eine GUID der Ereignisse einmal verteilt.|
|operationName      |Name des Vorgangs.|
|Eigenschaften         |Eigenschaften des Ereignisses.|
|Status             |Zeichenfolge. Status des Vorgangs. Allgemeine Werte: "Gestartet", "In Bearbeitung", "Erfolgreich", "Fehlgeschlagen", "Aktiv", "Gelöst".|
|Untergeordneter          |Normalerweise enthält den HTTP-Statuscode der entsprechenden REST-Aufruf. Es kann auch andere beschreiben ein untergeordneter Zeichenfolgen enthalten. Allgemeine Unterstatus Werte: OK (HTTP-Statuscode: 200), erstellt (HTTP-Statuscode: 201) akzeptiert (HTTP-Statuscode: 202), keine Inhalte (HTTP-Statuscode: 204), ungültige Anforderung (HTTP-Statuscode: 400), wurde nicht gefunden (HTTP-Statuscode: 404), Konflikt (HTTP-Statuscode: 409), interner Serverfehler (HTTP-Statuscode: 500), Dienst nicht verfügbar (HTTP-Statuscode: 503), Gateway-Timeout (HTTP-Statuscode: 504)|

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über das Aktivitätsprotokoll](monitoring-overview-activity-logs.md)
- [Azure-Warnungen Azure Automatisierungsskripts (Runbooks) ausführen](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Verwendung Logik App SMS über Twilio von Azure Warnmeldung senden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). In diesem Beispiel ist für metrische Alerts aber konnte mit einem Aktivitätsprotokoll Warnung geändert werden.
- [Verwendung Logik App eine Pufferzeit von Azure Warnmeldung Nachricht](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). In diesem Beispiel ist für metrische Alerts aber konnte mit einem Aktivitätsprotokoll Warnung geändert werden.
- [Verwendung Logik Anwendung eine Nachricht an eine Warteschlange Azure Azure Warnung](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). In diesem Beispiel ist für metrische Alerts aber konnte mit einem Aktivitätsprotokoll Warnung geändert werden.
